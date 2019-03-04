<template>
  <div :class="$style.customCursor" :style="mousePosition">
    <div :class="mouseCursor">
      <div :class="$style.arrowLeft"></div>
      <div :class="$style.arrowRight"></div>
    </div>
  </div>
</template>

<script>
import _ from 'lodash'

const THROTTLE_MILLI_SECONDS = 33

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
        case 'drag': {
          return this.$style.cursorDrag
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
    window.addEventListener(
      'mousemove',
      _.throttle(this.onMouseMove, THROTTLE_MILLI_SECONDS),
      false
    )
  },

  beforeDestroy() {
    window.removeEventListener(
      'mousemove',
      _.throttle(this.onMouseMove, THROTTLE_MILLI_SECONDS),
      false
    )
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
  cursor: none !important;
}

.customCursor {
  transition: left 33ms ease-out, top 33ms ease-out;
  mix-blend-mode: difference;
  pointer-events: none;
  position: fixed;
  transform: translate(-50%, -50%);
  z-index: 9999;
  will-change: left, top;
}

.absolute {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

/* Mouse */
.cursorMouse {
  transition: width 200ms ease-out, height 200ms ease-out,
    background-color 200ms ease-out;
  display: inline-block;
  width: 8px;
  height: 8px;
  background: rgba(255, 255, 255, 0.5);
  border-radius: 100%;
}

/* Pointer */
.cursorPointer {
  transition: width 200ms ease-out, height 200ms ease-out,
    background-color 200ms ease-out;
  display: inline-block;
  width: 32px;
  height: 32px;
  background: rgba(255, 255, 255, 0.75);
  border-radius: 100%;
}

/* Drag */
.cursorDrag {
  transition: width 200ms ease-out, height 200ms ease-out,
    background-color 200ms ease-out;
  position: relative;
  display: inline-block;
  width: 15px;
  height: 15px;
  background: rgba(255, 255, 255, 0.75);
  border-radius: 100%;
}

/* Drag: Arrow left */
.cursorDrag .arrowLeft {
  position: absolute;
  right: 100%;
  top: 50%;
  transform: translateX(-10px);
}

.cursorDrag .arrowLeft:after,
.cursorDrag .arrowLeft:before {
  right: 100%;
  top: 50%;
  border: solid transparent;
  content: ' ';
  height: 0;
  width: 0;
  position: absolute;
  pointer-events: none;
}

.cursorDrag .arrowLeft:after {
  border-color: rgba(136, 183, 213, 0);
  border-right-color: #fff;
  border-width: 4px;
  margin-top: -4px;
}

.cursorDrag .arrowLeft:before {
  border-color: rgba(194, 225, 245, 0);
  border-right-color: #fff;
  border-width: 4px;
  margin-top: -4px;
}

/* Drag: Arrow left */
.cursorDrag .arrowRight {
  position: absolute;
  left: 100%;
  top: 50%;
  transform: translateX(18px);
}

.cursorDrag .arrowRight:after,
.cursorDrag .arrowRight:before {
  right: 100%;
  top: 50%;
  border: solid transparent;
  content: ' ';
  height: 0;
  width: 0;
  position: absolute;
  pointer-events: none;
}

.cursorDrag .arrowRight:after {
  border-color: rgba(136, 183, 213, 0);
  border-left-color: #fff;
  border-width: 4px;
  margin-top: -4px;
}

.cursorDrag .arrowRight:before {
  border-color: rgba(194, 225, 245, 0);
  border-left-color: #fff;
  border-width: 4px;
  margin-top: -4px;
}
</style>
