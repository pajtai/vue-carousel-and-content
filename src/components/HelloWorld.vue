<template>
    <div class="slider">
        <div v-for="slide in slides">
            <div class="image"  :style="'background-image:url(\'' + slide.img + '\')'">&nbsp;</div>
            <div class="content" v-html="slide.content"></div>
        </div>
    </div>
</template>

<script>
export default {
  name: "HelloWorld",
    props: ['slides']
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
    .slider {
        display: flex;
        overflow-x: auto;
        -webkit-overflow-scrolling: touch;
    }
    .slider > div {
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
