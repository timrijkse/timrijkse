<template>
  <div ref="project" :class="$style.project" :style="dimensionStyles">
    <nuxt-link to="/projects" data-cursor="pointer">projects</nuxt-link>
  </div>
</template>

<script>
import { TweenMax } from 'gsap'

export default {
  props: {
    width: {
      type: Number,
      default() {
        return null
      }
    },

    height: {
      type: Number,
      default() {
        return null
      }
    },

    left: {
      type: Number,
      default() {
        return null
      }
    },

    top: {
      type: Number,
      default() {
        return null
      }
    }
  },

  transition: {
    appear: true,

    enter(el, done) {
      TweenMax.to(this.$refs.projectSlug.$refs.project, 1, {
        left: 0,
        top: 0,
        width: window.innerWidth,
        height: window.innerHeight,
        ease: Power4.easeOut,
        onComplete: done
      })
    }
  },

  computed: {
    dimensionStyles() {
      if (!this.width) {
        return {
          width: `100%`,
          height: `100%`,
          left: 0,
          top: 0
        }
      }

      return {
        width: `${this.width}px`,
        height: `${this.height}px`,
        top: `${this.top}px`,
        left: `${this.left}px`
      }
    }
  },

  mounted() {
    console.log(this)
  }
}
</script>

<style module>
.project {
  width: 100%;
  height: 100%;
  background: #404040;
}
</style>
