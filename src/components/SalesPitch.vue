
<template>
    <div class="salesPitch">
        <h1 ref="title">
            <span class="line">We are selling</span>
            <span class="line">{{ msg }},</span>
            <span class="line">not calls to action</span>
        </h1>
    </div>
</template>

<script>
    /* eslint-disable */
    import { TweenMax, Power2 } from 'gsap';

    export default {
        name: 'SalesPitch',
        props: {
            msg: {
                type: String,
                default: 'DREAMS'
            }
        },
        methods: {
            fadeInUp() {
               const lines = [...this.$refs.title.querySelectorAll('span')];
               TweenMax.set(lines, {y: 100, opacity: 0});
               TweenMax.staggerTo(lines, 0.5, {opacity: 1, ease: Power2.easeOut }, 0.075);
               TweenMax.staggerTo(lines, 1, {y: 0, opacity: 1, ease: Power2.easeOut}, 0.075);
            }
        },
        watch: {
            msg: function() {
                this.fadeInUp();
            }
        },
        mounted() {
            this.fadeInUp();
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

    h1 {
        font-size: 3em;
        font-weight: 800;
        margin: 0;
        text-align: center;
        word-wrap: break-word;
        hyphens: auto;
    }


    @media (min-width: 768px) {
        h1 {
            font-size: 4.5em;
        }
    }
    @media (min-width: 1000px) {
        h1 {
            font-size: 6em;
        }
    }

    span.line {
        display: block;
    }
</style>
