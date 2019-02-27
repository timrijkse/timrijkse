<template>
  <div :class="$style.project" :style="dimensionStyles">dfgd</div>
</template>

<script>
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
  background: red;
}
</style>
