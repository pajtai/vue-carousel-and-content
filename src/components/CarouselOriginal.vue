<template>
    <div class="parent">
        <div class="box" ref="box">
            <div class="slider">
                <div class="slide" v-for="(slide, index) in slides" :key="index">
                    <div @animationend="doneWithAnimation" :class="animationClass" class="image"  :style="'background-image:url(\'' + slide.img + '\')'">
                        &nbsp;
                        <div v-if="slide.title" class="title" v-html="slide.title"></div>
                        <div v-if="slide.content" @click="down" class="down">
                            <slot name="down">&darr;</slot>
                        </div>
                    </div>
                    <div :class="animationClass" class="content" v-html="slide.content"></div>
                </div>
            </div>
        </div>
        <div @click="prev" @mouseover="peakprev" @mouseleave="unpeakprev" class="prev nav">
            <slot name="previous">&larr;</slot>
        </div>
        <div @click="next" @mouseover="peaknext" @mouseleave="unpeaknext" class="next nav">
            <slot name="next">&rarr;</slot>
        </div>
    </div>
</template>

<script>
// Create constants from string that are used more than once, so we don't typo them
const LISTENING_FOR_NEXT = "next";
const LISTENING_FOR_PREV = "prev";
const PEAK_NEXT = "peak-next";
const PEAK_PREV = "peak-prev";

export default {
  name: "Carousel",
  props: ["slides"],

  data: function() {
    return {
      current: 0,
      animationClass: "",
      listening: false
    };
  },

  methods: {
    // When a CSS animation finishes, do some cleanup and slide rearrangement as needed
    doneWithAnimation() {
      if (!this.listening) {
        return;
      }

      let first, last;
      switch (this.listening) {
        case LISTENING_FOR_NEXT:
          first = this.slides.shift();
          this.slides.push(first);
          break;
        case LISTENING_FOR_PREV:
          last = this.slides.pop();
          this.slides.unshift(last);
          break;
      }
      this.animationClass = "";
      this.listening = false;
    },

    next() {
      scrollTo(this.$refs.box, 0, 250, () => {
        this.listening = LISTENING_FOR_NEXT;
        this.animationClass = "slide-next";
      });
    },
    prev() {
      scrollTo(this.$refs.box, 0, 250, () => {
        this.listening = LISTENING_FOR_PREV;
        this.animationClass = "slide-prev";
      });
    },

    peaknext() {
      this.animationClass = PEAK_NEXT;
    },
    peakprev() {
      this.animationClass = PEAK_PREV;
    },
    unpeakprev() {
      // Only need to move image back to original location, if it is out of place
      if (this.animationClass === PEAK_PREV) {
        this.animationClass = "un-peak-prev";
      }
    },
    unpeaknext() {
      // Only need to move image back to original location, if it is out of place
      if (this.animationClass === PEAK_NEXT) {
        this.animationClass = "un-peak-next";
      }
    },

    down() {
      const box = this.$refs.box;
      scrollTo(box, box.scrollTop + 100, 250);
    }
  }
};

function scrollTo(element, to = 0, duration = 1000, callback) {
  const start = element.scrollTop;
  const change = to - start;
  const increment = 20;
  let currentTime = 0;

  const animateScroll = () => {
    currentTime += increment;

    element.scrollTop = Math.easeInOutQuad(
      currentTime,
      start,
      change,
      duration
    );

    if (currentTime < duration && change !== 0) {
      setTimeout(animateScroll, increment);
    } else {
      if (callback) {
        element.scrollTop = to;
        callback();
      }
    }
  };

  animateScroll();
}

//t = current time
//b = start value
//c = change in value
//d = duration
Math.easeInOutQuad = function(t, b, c, d) {
  t /= d / 2;
  if (t < 1) return (c / 2) * t * t + b;
  t--;
  return (-c / 2) * (t * (t - 2) - 1) + b;
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.nav {
  position: absolute;
  cursor: pointer;
  top: 50%;
  font-size: 36px;
}
.down {
  position: absolute;
  bottom: 0;
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
.box, .parent {
  overflow-x: hidden;
  position: relative;
  width: 100%;
  height: 100%;
}
.slider {
  display: flex;
}
.slide {
  /* make sure the width is honored */
  flex-shrink: 0;
  width: 100%;
}

.image,
.content {
  transform: translateX(-100%);
}

.image {
  position: absolute;
  width: 100%;
  height: 100%;
  background-size: cover;
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
  position: absolute;
  width: 100%;
  top: 100%;
}

.slide-prev {
  animation: slide-prev 0.5s forwards;
  -webkit-animation: slide-prev 0.5s forwards;
}

.slide-next {
  animation: slide-next 0.5s forwards;
  -webkit-animation: slide-next 0.5s forwards;
}

.peak-prev {
  animation: peak-prev 0.5s forwards;
  -webkit-animation: peak-prev 0.5s forwards;
}

.peak-next {
  animation: peak-next 0.5s forwards;
  -webkit-animation: peak-next 0.5s forwards;
}

.un-peak {
  animation: un-peak 0.5s forwards;
  -webkit-animation: un-peak 0.5s forwards;
}

.un-peak-prev {
  animation: un-peak-prev 0.5s forwards;
  -webkit-animation: un-peak-prev 0.5s forwards;
}

.un-peak-next {
  animation: un-peak-next 0.5s forwards;
  -webkit-animation: un-peak-next 0.5s forwards;
}

@keyframes slide-prev {
  0% {
    transform: translateX(-90%);
  }
  100% {
    transform: translateX(0%);
  }
}

@-webkit-keyframes slide-prev {
  0% {
    -webkit-transform: translateX(-90%);
  }
  100% {
    -webkit-transform: translateX(0%);
  }
}

@keyframes slide-next {
  0% {
    transform: translateX(-110%);
  }
  100% {
    transform: translateX(-200%);
  }
}

@-webkit-keyframes slide-next {
  0% {
    -webkit-transform: translateX(-110%);
  }
  100% {
    -webkit-transform: translateX(-200%);
  }
}

@keyframes peak-prev {
  100% {
    transform: translateX(-90%);
  }
}

@-webkit-keyframes peak-prev {
  100% {
    -webkit-transform: translateX(-90%);
  }
}

@keyframes peak-next {
  100% {
    transform: translateX(-110%);
  }
}

@-webkit-keyframes peak-next {
  100% {
    -webkit-transform: translateX(-110%);
  }
}

@keyframes un-peak {
  100% {
    transform: translateX(-100%);
  }
}

@-webkit-keyframes un-peak {
  100% {
    -webkit-transform: translateX(-100%);
  }
}

@keyframes un-peak-prev {
  0% {
    transform: translateX(-90%);
  }
  100% {
    transform: translateX(-100%);
  }
}

@-webkit-keyframes un-peak-prev {
  0% {
    -webkit-transform: translateX(-90%);
  }
  100% {
    -webkit-transform: translateX(-100%);
  }
}

@keyframes un-peak-next {
  0% {
    transform: translateX(-110%);
  }
  100% {
    transform: translateX(-100%);
  }
}

@-webkit-keyframes un-peak-next {
  0% {
    -webkit-transform: translateX(-110%);
  }
  100% {
    -webkit-transform: translateX(-100%);
  }
}
</style>
