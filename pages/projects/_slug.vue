<template>
  <div ref="project" :class="$style.project" :style="dimensionStyles">
    <div :class="$style.content">
      <span :class="$style.close" data-cursor="pointer" @click="handleClose">close</span>
    </div>
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

    beforeEnter() {
      this.$parent.setProjectLeavePosition(this.$route.params.slug)
    },

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
      TweenMax.to(this.$refs.project, 1, {
        left: this.left,
        top: this.top,
        width: this.width,
        height: this.height,
        ease: Power4.easeOut,
        onComplete: () => {
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
  transform: translate3d(0, 0, 0);
  background: #404040;
  background-image: url('https://res.cloudinary.com/jmperez/image/upload/w_auto:100:800,f_auto/v1509278557/jmperez-composition-primitive_j8zyfn.jpg');
  background-size: cover;
  backface-visibility: hidden;
  z-index: 2;
  will-change: left, top, width, height, opacity;
}

.content {
  height: 5000px;
}

.close {
  font-size: 20px;
  color: #fff;
}
</style>
