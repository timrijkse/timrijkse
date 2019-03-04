<template>
  <div
    :class="$style.slide"
    :style="hoverStyles"
    data-cursor="drag"
    @mousemove="onMouseMove"
    @mouseenter="onMouseEnter"
    @mouseleave="onMouseLeave"
  >
    <div ref="visual" data-cursor="drag" @click="onClick">
      <lazy-image :class="$style.visual" :aspect-ratio="0.75" :idle="idleLazyImage" :src="image"/>
    </div>
    <h2 ref="title" :class="$style.title" data-cursor="pointer" @click="onClick" v-html="title"></h2>
  </div>
</template>

<script>
import LazyImage from '@/components/LazyImage'

export default {
  components: {
    LazyImage
  },

  props: {
    idleLazyImage: {
      type: Boolean,
      default() {
        return false
      }
    },

    title: {
      type: String,
      default() {
        return 'Project name'
      }
    },

    image: {
      type: String,
      required: true
    }
  },

  data() {
    return {
      ax: 0,
      ay: 0,
      scale: 1,
      zIndex: 1
    }
  },

  computed: {
    hoverStyles() {
      return {
        transform: `rotateY(${this.ax}deg) rotateX(${this.ay}deg) scale(${
          this.scale
        })`,
        zIndex: this.zIndex
      }
    }
  },

  methods: {
    onClick() {
      this.$emit('on-click', this.$refs.visual)
    },

    onMouseEnter() {
      this.scale = 1.15
      this.zIndex = 2
    },

    onMouseLeave() {
      this.scale = 1
      this.zIndex = 1
    },

    onMouseMove(e) {
      this.ax = -(window.innerWidth / 2 - e.pageX) / 40
      this.ay = (window.innerHeight / 2 - e.pageY) / 40
    }
  }
}
</script>

<style module>
.slide {
  transition: transform 500ms cubic-bezier(0.19, 1, 0.22, 1);
  transform-style: preserve-3d;
  perspective-origin: 50% 50%;
  perspective: 1200px;
  backface-visibility: hidden;
  flex: 0 0 200px;
  margin: 0 50px 0 0;
}

.title {
  transition: all 300ms linear;
  opacity: 0;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translateZ(100px) translateX(-50%) translateY(-50%);
  backface-visibility: hidden;
  width: 100%;
  font-family: 'KairosSans';
  /* white-space: nowrap; */
  font-size: 30px;
  text-transform: uppercase;
  padding: 15px;
  filter: blur(20px);
  color: #fff;
  will-change: opacity, transform, text-shadow, font-variation-settings;
  font-variation-settings: 'wght' 300, 'wdth' 100, 'ital' 0;
}

.visual {
  pointer-events: none;
  backface-visibility: hidden;
  will-change: transform;
}

.slide:hover .title {
  /* transform: translateZ(100px) translateX(-50%) translateY(-50%); */
  transform: translateZ(320px) translateX(-50%) translateY(-50%);
  opacity: 1;
  filter: blur(0);
  font-size: 30px;
  padding: 15px;
  text-shadow: 5px 5px 10px rgba(0, 0, 0, 1);

  font-variation-settings: 'wght' 200, 'wdth' 200, 'ital' 1;
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-left: 0;
  border-right: 0;
}

.slide:hover .title:before {
  transition: all 300ms linear;
  content: '';
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  width: calc(100% - 6px);
  height: calc(100% - 6px);
  background-color: rgba(0, 0, 0, 0);
}

.slide .title:hover {
  transform: translateZ(350px) translateX(-50%) translateY(-50%);
  font-variation-settings: 'wght' 600, 'wdth' 300, 'ital' 1;
  border-left: 0;
  border-right: 0;
}

.slide .title:hover:before {
  background-color: rgba(0, 0, 0, 0.05);
}

.slide:hover .visual {
  transform: translateZ(20px);
}

@media (min-width: 768px) {
  .slide {
    flex: 0 0 40vw;
    margin: 0 10vw 0 0;
  }
}
</style>
