<template>
  <section class="container">
    <noise-background/>
    <drag-carousel ref="dragCarousel"/>
  </section>
</template>

<script>
import { TweenMax, Power4 } from 'gsap'
import NoiseBackground from '~/components/NoiseBackground.vue'
import RectangleButton from '~/components/RectangleButton.vue'
import DragCarousel from '~/components/DragCarousel'

export default {
  components: {
    NoiseBackground,
    RectangleButton,
    DragCarousel
  }
}
</script>

<style>
.container {
  min-height: 100vh;
  display: flex;
  background: radial-gradient(ellipse at center, #29382c 0%, #0c100d 100%);
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
