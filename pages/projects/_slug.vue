<template>
  <div ref="project" :class="$style.project" :style="dimensionStyles">
    <span :class="$style.close" data-cursor="pointer" @click="handleClose">close</span>
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
    },

    index: {
      type: Number,
      default() {
        return 0
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

  methods: {
    handleClose() {
      if (!this.width) {
        return this.$router.push({
          path: `/projects`
        })
      }

      TweenMax.to(this.$refs.project, 1, {
        left: this.left,
        top: this.top,
        width: this.width,
        height: this.height,
        ease: Power4.easeOut,
        onComplete: () => {
          this.$emit('close-project')

          this.$router.push({
            path: `/projects`
          })
        }
      })
    }
  }
}
</script>

<style module>
.project {
  width: 100%;
  height: 100%;
  background: #404040;
  background-image: url('https://res.cloudinary.com/jmperez/image/upload/w_auto:100:800,f_auto/v1509278557/jmperez-composition-primitive_j8zyfn.jpg');
  background-size: cover;
}

.close {
  font-size: 20px;
  color: #fff;
}
</style>
