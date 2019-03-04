<template>
  <h2 :class="$style.gyroscopeText" :style="fontVariationSettings">
    <slot></slot>
  </h2>
</template>

<script>
export default {
  props: {
    maxWidth: {
      type: Number,
      default() {
        return 1000
      }
    },

    maxWeight: {
      type: Number,
      default() {
        return 1000
      }
    },

    maxSlant: {
      type: Number,
      default() {
        return 1
      }
    }
  },

  data() {
    return {
      percentageY: 0, // y
      percentageX: 0 // x
    }
  },

  computed: {
    fontWidth() {
      return (this.maxWidth / 100) * this.percentageX
    },

    fontWeight() {
      return (this.maxWeight / 100) * this.percentageY
    },

    fontSlant() {
      return (this.maxSlant / 100) * this.percentageX
    },

    fontVariationSettings() {
      return {
        fontVariationSettings: `'wght' ${this.fontWeight}, 'wdth' ${
          this.fontWidth
        }, 'ital' ${this.fontSlant}`
      }
    }
  },

  mounted() {
    window.addEventListener(
      'deviceorientation',
      this.onDeviceOrientation,
      false
    )
  },

  beforeDestroy() {
    window.removeEventListener(
      'deviceorientation',
      this.onDeviceOrientation,
      false
    )
  },

  methods: {
    mapBetween(currentNum, minAllowed, maxAllowed, min, max) {
      return (
        ((maxAllowed - minAllowed) * (currentNum - min)) / (max - min) +
        minAllowed
      )
    },

    onDeviceOrientation(e) {
      this.percentageX = this.mapBetween(e.beta, 0, 100, -30, 30)
      this.percentageY = this.mapBetween(e.gamma, 0, 100, -30, 30)
      console.log(e.beta, e.gamma)
    }
  }
}
</script>

<style module>
.gyroscopeText {
  transition: all 0.15s ease-out;
  font-family: 'KairosSans';
  font-size: 100px;
  text-transform: uppercase;
  width: 100%;
  /* font-variation-settings: 'wght' 1000, 'wdth' 1000, 'ital' 1; */
  color: #fff;
}
</style>
