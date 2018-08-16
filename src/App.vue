<template>
  <div id="app">
    <vue-carousel :slides="slides"/>
  </div>
</template>

<script>
    import Carousel from "./components/Carousel.vue";

    export default {
        name: "app",
        data: function() {
            return {
                slides: [
                    {
                        img: 'https://via.placeholder.com/600x400',
                        title: 'First!',
                        content: '<h2>one!</h2><h2>one!</h2><h2>one!</h2><h2>one!</h2><h2>one!</h2><h2>one!</h2>'
                    },
                    {
                        img: 'https://via.placeholder.com/600x400'
                    },
                    {
                        img: 'https://via.placeholder.com/600x400',
                        content: '<h2>three!</h2><h2>one!</h2><h2>one!</h2><h2>one!</h2><h2>one!</h2><h2>one!</h2>'
                    }
                ]
            };
        },
        components: {
            'vue-carousel': Carousel
        }
    };
</script>

<style lang="scss">
  html, body {
    border: 0;
    padding: 0;
    margin: 0;
  }
  #app {
    font-family: "Avenir", Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
  }
</style>
