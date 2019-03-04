<template>
  <figure :class="$style.lazyImage">
    <div ref="svgWrapper" :class="$style.svgWrapper" :style="aspectStyles">
      <svg :class="$style.svg" viewBox="0 0 300 300" preserveAspectRatio="none">
        <path
          ref="path"
          d="M300,300 L300,381.453125 C259.221354,416.036458 210.244792,416.036458 153.070313,381.453125 C95.8958333,346.869792 44.8723958,346.869792 0,381.453125 L0,300 L0,0 L300,0 L300,300 Z"
          fill="#202020"
          fill-rule="nonzero"
        ></path>
      </svg>
    </div>
    <img v-show="imageSrc" ref="image" :class="$style.image" :src="imageSrc">
  </figure>
</template>

<script>
import { TimelineMax, Power4 } from 'gsap'

export default {
  props: {
    idle: {
      type: Boolean,
      default() {
        return false
      }
    },

    maxWidth: {
      type: String,
      default() {
        return '100%'
      }
    },

    aspectRatio: {
      type: Number,
      default() {
        return 1.6
      }
    },

    src: {
      type: String,
      required: true
    }
  },

  data() {
    return {
      img: null,
      imageSrc: null
    }
  },

  computed: {
    aspectStyles() {
      return {
        maxWidth: this.maxWidth,
        paddingTop: `${this.aspectRatio * 100}%`
      }
    },

    clipStyles() {
      return {
        clipPath: `polygon(0% 0%, 0% 100%, 100% 100%)`
      }
    }
  },

  watch: {
    idle() {
      this.loadImage()
    }
  },

  mounted() {
    this.tl = new TimelineMax({
      paused: true
    })

    this.tl.to(
      this.$refs.path,
      2,
      {
        y: '-100%',
        ease: Power4.easeOut
      },
      0
    )

    this.tl.set(
      this.$refs.image,
      {
        autoAlpha: 1
      },
      0
    )

    this.loadImage()
  },

  methods: {
    loadImage() {
      if (this.idle) {
        return
      }

      this.img = new Image()
      this.img.onload = this.onImageLoaded()
      this.img.src = this.src
    },

    onImageLoaded() {
      this.imageSrc = this.src
      this.tl.play()
    }
  }
}
</script>

<style module>
.lazyImage {
  overflow: hidden;
  position: relative;
  width: 100%;
  background-color: #404040;
}

.svgWrapper {
  position: relative;
  width: 100%;
  height: 0;
}

.svg {
  position: absolute;
  left: 0;
  bottom: 0;
  width: 100%;
  height: 100%;
  z-index: 2;
}

.image {
  position: absolute;
  left: 0;
  top: 0;
  display: block;
  width: 100%;
}
</style>
