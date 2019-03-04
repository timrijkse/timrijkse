<template>
  <section :class="$style.container">
    <noise-background :opacity="noiseOpacity"/>
    <div :class="$style.text">
      <h2 ref="title" :class="$style.gyroscopeText" :style="fontVariationSettings">Tim Rijkse</h2>
      <h3 ref="caption" :class="$style.gyroscopeTextSmall">Creative developer</h3>
    </div>
  </section>
</template>

<script>
import { TimelineMax } from 'gsap'
import NoiseBackground from '~/components/NoiseBackground.vue'
import GyroscopeText from '~/components/GyroscopeText.vue'

export default {
  components: {
    NoiseBackground,
    GyroscopeText
  },

  data() {
    return {
      fontWeight: 0,
      fontWidth: 0,
      fontSlant: 0,
      noiseOpacity: 0
    }
  },

  computed: {
    fontVariationSettings() {
      return {
        fontVariationSettings: `'wght' ${this.fontWeight}, 'wdth' ${
          this.fontWidth
        }, 'ital' ${this.fontSlant}`
      }
    }
  },

  mounted() {
    this.timeline = new TimelineMax({
      // paused: true,
      // repeat: -1,
      onComplete: () => {
        this.$router.push('/projects')
      }
    })

    this.timeline.to(this, 1, {
      noiseOpacity: 1
    })

    this.timeline.staggerFrom(
      [this.$refs.title, this.$refs.caption],
      3,
      {
        autoAlpha: 0,
        y: '+=30',
        ease: Power4.easeOut
      },
      0.105,
      0.5
    )

    this.timeline.to(
      this,
      1,
      {
        fontWeight: 1000,
        fontSlant: 1,
        fontWidth: 100,
        ease: Power4.easeOut
      },
      0.5
    )

    this.timeline.to(this, 1, {
      // fontWeight: 100,
      // fontSlant: 0,
      fontWidth: 1,
      ease: Power4.easeOut
    })

    this.timeline.staggerTo(
      [this.$refs.title, this.$refs.caption],
      0.5,
      {
        autoAlpha: 0,
        y: '-100',
        ease: Power4.easeOut
      },
      0.05,
      '-=1'
    )
  }
}
</script>

<style module>
.container {
  min-height: 100vh;
  display: flex;
  background: #101010;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.text {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  width: 100%;
  z-index: 2;
}

.gyroscopeText {
  font-family: 'KairosSans';
  font-size: 100px;
  text-transform: uppercase;
  width: 100%;
  /* font-variation-settings: 'wght' 1000, 'wdth' 1000, 'ital' 1; */
  color: #fff;
}

.gyroscopeTextSmall {
  composes: gyroscopeText;
  font-size: 20px;
  color: #c0c0c0;
}
</style>
