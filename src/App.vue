<template>
    <div id="app" v-bind:style="stylesObj">
        <SalesPitch :msg="msg"/>
        <span class="instructions">press space</span>
    </div>
</template>

<script>
    import SalesPitch from './components/SalesPitch.vue';

    export default {
        name: 'app',
        components: {
            SalesPitch
        },
        data() {
            return {
                msg: 'DREAMS',
                msgs: ['DREAMS', 'DRUGS'],
                textColor: '',
                bgColor: '',
                hasListener: false
            }
        },
        methods: {
            handleKeydown(e) {
                if (e.keyCode === 32) {
                    this.updateColor();
                    this.updateText();
                }
            },
            updateColor() {
                const rHue = r(0, 360);
                const rSat = r(60, 80);
                const rLig = r(40, 60);
                const cHue = rHue - 180 >= 0 ? rHue - 180 : rHue + 180;
                this.textColor = `hsl(${cHue},${rSat}%,${rLig}%)`;
                this.bgColor = `hsl(${rHue},${rSat}%,${rLig}%)`;
            },
            updateText(){
                if (this.msgs.length === 0) this.fetchWords();
                const newMsg = this.msgs[r(0, this.msgs.length)];
                this.msg = newMsg.word.toUpperCase();
            },
            async fetchWords() {
                const response = await fetch(`https://api.datamuse.com/words?sp=d*s&topics=mundane,technology,food&md=p&max=1000`);
                const data = await response.json();
                const nouns = data.filter(item => item.tags && item.tags.includes("n"));
                this.msgs = [...this.msgs,...nouns];
            }
        },
        mounted() {
            this.fetchWords();
            this.updateColor();
            !this.hasListener && window.addEventListener('keydown', this.handleKeydown);
            this.hasListener = true;
        },
        computed: {
            stylesObj: function() {
                return {
                    'color': this.textColor,
                    'backgroundColor': this.bgColor
                }
            }
        }
    }

    function r(min, max) {
        if (max === null) {
            max = min;
            min = 0;
        }
        return Math.floor(Math.random() * ( max - min + 1) + min);
    }

</script>

<style>
    @import url('https://fonts.googleapis.com/css?family=Work+Sans:700,800&IBM+Plex+Mono');

    body {
        min-height: 100vh;
        background: gray;
        margin: 0;
    }

    .instructions {
        position: absolute;
        bottom: 1em;
        margin-top: auto;
        font-weight: normal;
        font-family: 'IBM Plex Mono', monospace;
    }

    #app {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        color: white;
        height: 100vh;
        transition: background-color 0.5s ease, color 1s ease;
        font-family: "Work Sans", sans-serif;
        font-weight: 700;
    }
</style>
