<template>
    <div>
        <div class="box" ref="box">
            <div class="slider">
                <div class="slide" v-for="(slide, index) in slides" :key="index">
                    <div @animationend="doneWithAnimation" :class="animationClass" class="image"  :style="'background-image:url(\'' + slide.img + '\')'">
                        &nbsp;
                        <div v-if="slide.title" class="title">{{ slide.title }}</div>
                        <div v-if="slide.content" @click="down" class="down">&darr;</div>
                    </div>
                    <div :class="animationClass" class="content" v-html="slide.content"></div>
                </div>
            </div>
        </div>
        <div @click="prev" @mouseover="peakprev" @mouseleave="unpeakprev" class="prev nav">&larr;</div>
        <div @click="next" @mouseover="peaknext" @mouseleave="unpeaknext" class="next nav">&rarr;</div>
    </div>
</template>

<script>
    export default {
        name: "HelloWorld",
        props: ['slides'],
        data: function() {
            return {
                current: 0,
                firstSlideOffset: 100,
                animationClass: '',
                listening: false
            }
        },
        methods: {
            doneWithAnimation() {
                if (!this.listening) {
                    return;
                }

                switch (this.listening) {
                    case 'next':
                        const first = this.slides.shift();
                        this.slides.push(first);
                        console.log('done next');
                        break;
                    case 'prev':
                        const last = this.slides.pop();
                        this.slides.unshift(last);
                        console.log('done prev');
                        break;
                }
                this.animationClass = '';
                this.listening = false;
            },

            next() {
                scrollTo(this.$refs.box,0, 250, () => {
                    this.listening = 'next';
                    this.animationClass = 'slide-out';
                });

                console.log('next');
            },
            prev() {
                scrollTo(this.$refs.box, 0, 250, () => {
                    this.listening = 'prev';
                    this.animationClass = 'slide-in';
                });

                console.log('prev');
            },

            peaknext() {
                console.log('pn');
                this.animationClass = 'peak-out';
            },
            peakprev() {
                console.log('pp');
                this.animationClass = 'peak-in';
            },
            unpeakprev() {
                this.animationClass = 'un-peak-prev';
            },
            unpeaknext() {
                this.animationClass = 'un-peak-next';
            },

            down() {
                const box = this.$refs.box;
                scrollTo(box, box.scrollTop + 100, 250);
            }
        }
    };

    function scrollTo(element, to = 0, duration= 1000, callback) {

        const start = element.scrollTop;
        const change = to - start;
        const increment = 20;
        let currentTime = 0;

        const animateScroll = (() => {

            currentTime += increment;

            element.scrollTop = Math.easeInOutQuad(currentTime, start, change, duration);

            if (currentTime < duration  && change !== 0) {
                setTimeout(animateScroll, increment);
            } else {
                if (callback) {
                    element.scrollTop = to;
                    callback();
                }
            }
        });

        animateScroll();
    }

    //t = current time
    //b = start value
    //c = change in value
    //d = duration
    Math.easeInOutQuad = function (t, b, c, d) {

        t /= d/2;
        if (t < 1) return c/2*t*t + b;
        t--;
        return -c/2 * (t*(t-2) - 1) + b;
    };


</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
    .nav {
        position: fixed;
        cursor: pointer;
        top: 50%;
        font-size: 36px;
    }
    .down {
        position: absolute;
        bottom: 0%;
        left: 50%;
        font-size: 48px;
        cursor: pointer;
    }
    .prev {
        left: 0;
    }
    .next {
        right: 0;
    }
    .box {
        overflow-x: hidden;
        position: relative;
        width: 100%;
        height: 100vh;
    }
    .slider {
        display: flex;
    }
    .slide {
        /* make sure the width is honored */
        flex-shrink: 0;
        width: 100%;
    }

    .image, .content {
        transform: translateX(-100%);
    }

    .image {
        position:absolute;
        width:100%;
        height:100%;
        background-size:cover;
        background-position: center;
        display: flex;
        align-items: flex-end;
        justify-content: center;
    }
    .title {
        font-size: 120px;
        margin-bottom: 10%;
    }
    .content {
        position:absolute;
        width:100%;
        top:100%;
        border-left: 1px solid red;
        border-right: 1px solid red;
    }

    .slide-in {
        animation: slide-in 0.5s forwards;
        -webkit-animation: slide-in 0.5s forwards;
    }

    .slide-out {
        animation: slide-out 0.5s forwards;
        -webkit-animation: slide-out 0.5s forwards;
    }

    @keyframes slide-in {
        0% { transform: translateX(-90%); }
        100% { transform: translateX(0%); }
    }

    @-webkit-keyframes slide-in {
        0% { -webkit-transform: translateX(-90%); }
        100% { -webkit-transform: translateX(0%); }
    }

    @keyframes slide-out {
        0% { transform: translateX(-110%); }
        100% { transform: translateX(-200%); }
    }

    @-webkit-keyframes slide-out {
        0% { -webkit-transform: translateX(-110%); }
        100% { -webkit-transform: translateX(-200%); }
    }


    .peak-in {
        animation: peak-in 0.5s forwards;
        -webkit-animation: peak-in 0.5s forwards;
    }

    .peak-out {
        animation: peak-out 0.5s forwards;
        -webkit-animation: peak-out 0.5s forwards;
    }

    .un-peak {
        animation: un-peak 0.5s forwards;
        -webkit-animation: un-peak 0.5s forwards;
    }

    @keyframes peak-in {
        100% { transform: translateX(-90%); }
    }

    @-webkit-keyframes peak-in {
        100% { -webkit-transform: translateX(-90%); }
    }

    @keyframes peak-out {
        100% { transform: translateX(-110%); }
    }

    @-webkit-keyframes peak-out {
        100% { -webkit-transform: translateX(-110%); }
    }

    @keyframes un-peak {
        100% { transform: translateX(-100%); }
    }

    @-webkit-keyframes un-peak {
        100% { -webkit-transform: translateX(-100%); }
    }

    .un-peak-prev {
        animation: un-peak-prev 0.5s forwards;
        -webkit-animation: un-peak-prev 0.5s forwards;
    }

    @keyframes un-peak-prev {
        0% { transform: translateX(-90%); }
        100% { transform: translateX(-100%); }
    }

    @-webkit-keyframes un-peak-prev {
        0% { -webkit-transform: translateX(-90%); }
        100% { -webkit-transform: translateX(-100%); }
    }

    .un-peak-next {
        animation: un-peak-next 0.5s forwards;
        -webkit-animation: un-peak-next 0.5s forwards;
    }

    @keyframes un-peak-next {
        0% { transform: translateX(-110%); }
        100% { transform: translateX(-100%); }
    }

    @-webkit-keyframes un-peak-next {
        0% { -webkit-transform: translateX(-110%); }
        100% { -webkit-transform: translateX(-100%); }
    }
</style>
