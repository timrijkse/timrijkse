<template>
  <h2 :class="$style.gyroscopeText">
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
        return 10
      }
    }
  },

  data() {
    return {
      factor: 0.1
    }
  },

  computed: {
    fontWidth() {
      return this.maxWidth
    },

    fontWeight() {
      return this.maxWeight
    },

    fontSlant() {
      return this.maxSlant
    }
  },

  mounted() {
    window.addEventListener(
      'orientationchange',
      this.onOrientationChange,
      false
    )
  },

  beforeDestroy() {
    window.removeEventListener(
      'orientationchange',
      this.onOrientationChange,
      false
    )
  },

  methods: {
    getValueByFactor(number) {
      return (number / 100) * this.factor
    },

    onOrientationChange(e) {}
  }
}
</script>

<style module>
@font-face {
  font-family: KairosSans;
  font-display: swap;
  src: url(https://yoksel.github.io/variable-fonts/assets/fonts/KairosSans/KairosSans_Variable.ttf)
    format('truetype');
}

.gyroscopeText {
  transition: all 0.5s ease-out;
  font-family: 'KairosSans';
  font-size: 150px;
  text-transform: uppercase;
  width: 100%;
  color: #fff;
}
</style>
