<template>
    <div>
        <div class="box">
            <div class="slider">
                <transition-group name="list-complete">
                    <div class="slide" v-for="(slide, index) in slides" :key="index" :style="index === 0 ? `transition: 1s; margin-left: -${firstSlideOffset}%` : ''">
                        <div class="image"  :style="'background-image:url(\'' + slide.img + '\')'">&nbsp;</div>
                        <div class="content" v-html="slide.content"></div>
                    </div>
                </transition-group>
            </div>
        </div>
        <div @click="prev" class="prev nav">&larr;</div>
        <div @click="next" class="next nav">&rarr;</div>
    </div>
</template>

<script>
    export default {
        name: "HelloWorld",
        props: ['slides'],
        data: function() {
            return {
                current: 0,
                firstSlideOffset: 100
            }
        },
        methods: {
            next() {
                const first = this.slides.shift();
                this.slides.push(first);
                console.log('next');
            },
            prev() {
                const last = this.slides.pop();
                this.slides.unshift(last);
                console.log('prev');
            }
        }
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

    .image {
        position:absolute;
        width:100%;
        height:100%;
        background-size:cover;
        background-position: center;
    }
    .content {
        position:absolute;
        width:100%;
        top:100%;
        border-left: 1px solid red;
        border-right: 1px solid red;
    }
</style>
