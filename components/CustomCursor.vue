<template>
  <div :class="$style.customCursor" :style="mousePosition">
    <div :class="mouseCursor"/>
  </div>
</template>

<script>
export default {
  data() {
    return {
      cursor: null,
      clientX: -100,
      clientY: -100
    }
  },

  computed: {
    mouseCursor() {
      switch (this.cursor) {
        case 'pointer': {
          return this.$style.cursorPointer
        }
        default: {
          return this.$style.cursorMouse
        }
      }
    },

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

      this.handleTarget(event.target)
    },

    handleTarget(target) {
      this.cursor = target.getAttribute('data-cursor')
    }
  }
}
</script>

<style module>
* {
  cursor: none;
}

.customCursor {
  transition: left 200ms ease-out, top 200ms ease-out;
  pointer-events: none;
  position: fixed;
  transform: translate(-50%, -50%);
}

.absolute {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

.cursorMouse {
  transition: width 200ms ease-out, height 200ms ease-out,
    background-color 200ms ease-out;
  display: inline-block;
  width: 8px;
  height: 8px;
  background: rgba(255, 255, 255, 0.5);
  border-radius: 100%;
}

.cursorPointer {
  transition: width 200ms ease-out, height 200ms ease-out,
    background-color 200ms ease-out;
  display: inline-block;
  width: 16px;
  height: 16px;
  background: rgba(255, 0, 0, 0.75);
  border-radius: 100%;
}
</style>
