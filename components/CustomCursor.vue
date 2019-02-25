<template>
  <div :class="$style.customCursor" :style="mousePosition">
    <div :class="$style.cursorMouse"/>
  </div>
</template>

<script>
export default {
  data() {
    return {
      clientX: 0,
      clientY: 0
    }
  },

  computed: {
    mousePosition() {
      return {
        left: `${this.clientX}px`,
        top: `${this.clientY}px`
      }
    }
  },

  mounted() {
    window.addEventListener('mousemove', this.onMouseMove, false)
  },

  beforeDestroy() {
    window.removeEventListener('mousemove', this.onMouseMove, false)
  },

  methods: {
    onMouseMove(event) {
      this.clientX = event.clientX
      this.clientY = event.clientY
    }
  }
}
</script>

<style module>
* {
  outline: 1px solid red;
}

.customCursor {
  transition: left 200ms ease-out, top 300ms ease-out;
  position: fixed;
  left: 20px;
  top: 20px;
  transform: translate(-50%, -50%);
}

.absolute {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

.cursorMouse {
  display: inline-block;
  width: 8px;
  height: 8px;
  background: rgba(255, 255, 255, 0.5);
  border-radius: 100%;
}
</style>
