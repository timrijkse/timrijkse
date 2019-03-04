<template>
  <div :class="$style.slide" :style="hoverStyles" data-cursor="drag" @mousemove="onMouseMove">
    <div ref="visual" data-cursor="pointer" @click="onClick">
      <lazy-image
        :class="$style.visual"
        :aspect-ratio="0.75"
        :idle="idleLazyImage"
        src="placeholder.png"
      />
    </div>
    <h2 ref="title" :class="$style.title" @click="onClick">Project name</h2>
  </div>
</template>

<script>
import LazyImage from '@/components/LazyImage'

export default {
  components: {
    LazyImage
  },

  props: {
    idleLazyImage: {
      type: Boolean,
      default() {
        return false
      }
    }
  },

  data() {
    return {
      ax: 0,
      ay: 0
    }
  },

  computed: {
    hoverStyles() {
      return {
        transform: `rotateY(${this.ax}deg) rotateX(${this.ay}deg)`
      }
    }
  },

  methods: {
    onClick() {
      this.$emit('on-click', this.$refs.visual)
    },

    onMouseMove(e) {
      this.ax = -(window.innerWidth / 2 - e.pageX) / 200
      this.ay = (window.innerHeight / 2 - e.pageY) / 100
    }
  }
}
</script>

<style module>
.slide {
  transition: transition 200ms ease-out;
  transform-style: preserve-3d;
  /* perspective-origin: 150% 150%; */
  perspective: 1200px;
  backface-visibility: hidden;
  flex: 0 0 200px;
  margin: 0 50px 0 0;
}

.title {
  pointer-events: none;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translateZ(100px) translateX(-50%) translateY(-50%);
  color: #fff;
}

.visual {
  pointer-events: none;
}

@media (min-width: 768px) {
  .slide {
    flex: 0 0 30vw;
    margin: 0 100px 0 0;
  }
}
</style>
