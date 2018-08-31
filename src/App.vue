<template>
    <div id="app">
        <div class="carousel">
            <vue-carousel :slides="slides1"></vue-carousel>
        </div>
        <div class="carousel">
            <vue-carousel-og :slides="slides2"></vue-carousel-og>
        </div>
        <div class="carousel">
            <vue-carousel-og :slides="slides3"></vue-carousel-og>
        </div>
    </div>
</template>

<script>
  import CarouselOriginal from "./components/CarouselOriginal.vue";
  import Carousel from "./components/Carousel.vue";

  export default {
    name: "app",
    data: function() {
      return {
        slides1: getSlides(),
        slides2: getSlides(),
        slides3: getSlides()
      };
    },
    components: {
      "vue-carousel-og": CarouselOriginal,
      "vue-carousel": Carousel
    }
  };

  function getSlides() {
    return [
        {
          img:
            "https://images.pexels.com/photos/874730/pexels-photo-874730.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260",
          title: "First!",
          content:
            "<h2>one!</h2><h2>one!</h2><h2>one!</h2><h2>one!</h2><h2>one!</h2><h2>one!</h2>"
        },
        {
          img:
            "https://images.pexels.com/photos/418626/pexels-photo-418626.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260",
          content:
            "<h2>two!</h2><h2>one!</h2><h2>one!</h2><h2>one!</h2><h2>one!</h2><h2>one!</h2>"
        },
        {
          img:
            "https://images.pexels.com/photos/1323443/pexels-photo-1323443.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260",
          content:
            "<h2>three!</h2><h2>one!</h2><h2>one!</h2><h2>one!</h2><h2>one!</h2><h2>one!</h2>"
        }
      ];
  }

</script>

<style lang="scss">
    html,
    body {
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
    .carousel {
        float: left;
        width:600px;
        height:400px;
        position: relative;
    }
</style>
