<template>
  <section :class="$style.container">
    <noise-background/>
    <gyroscope-text :class="$style.gyroscopeText">AHOY MATE!</gyroscope-text>
  </section>
</template>

<script>
import NoiseBackground from '~/components/NoiseBackground.vue'
import GyroscopeText from '~/components/GyroscopeText.vue'

export default {
  components: {
    NoiseBackground,
    GyroscopeText
  }
}
</script>

<style module>
.container {
  min-height: 100vh;
  display: flex;
  background: radial-gradient(ellipse at center, #29382c 0%, #0c100d 100%);
  justify-content: center;
  align-items: center;
  text-align: center;
}

.gyroscopeText {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  z-index: 2;
}
</style>
