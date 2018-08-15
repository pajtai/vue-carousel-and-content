<template>
  <div id="app">
    <HelloWorld :slides="slides"/>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "app",
  data: function() {
    return {
        slides: [
            {
                img: 'https://via.placeholder.com/600x400?text=Slide+One',
              content: '<h2>one!</h2><h2>one!</h2><h2>one!</h2><h2>one!</h2><h2>one!</h2><h2>one!</h2>'
            },
            {
                img: 'https://via.placeholder.com/600x400?text=Slide+Two',
              content: '<h2>two!</h2><h2>one!</h2><h2>one!</h2><h2>one!</h2><h2>one!</h2><h2>one!</h2>'
            },
            {
                img: 'https://via.placeholder.com/600x400?text=Slide+Three',
                content: '<h2>three!</h2><h2>one!</h2><h2>one!</h2><h2>one!</h2><h2>one!</h2><h2>one!</h2>'
            }
        ]
    };
  },
  components: {
    HelloWorld
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
