<template>
  <div :class="$style.dragCarousel">
    <div ref="bounds" :class="[$style.bounds, dragClass]">
      <div :class="$style.container" :style="skewStyles" data-cursor="drag">
        <div
          ref="draggable"
          :class="$style.inner"
          :style="{width: `${draggableWidth}px`}"
          data-cursor="drag"
        >
          <slide
            v-for="(slide, i) in slides"
            ref="slide"
            :class="$style.slide"
            :key="i"
            :idle-lazy-image="idleLazyImage"
            :title="slide.title"
            :image="slide.image"
            @on-click="handleSlideClick(i)"
          />
        </div>
      </div>
    </div>
    <div>
      <nuxt-child
        ref="projectSlug"
        :class="$style.project"
        :key="$route.name"
        :width="projectWidth"
        :height="projectHeight"
        :left="projectLeft"
        :top="projectTop"
      />
    </div>
  </div>
</template>

<script>
import _ from 'lodash'
import { TweenMax, Power4 } from 'gsap'
import Slide from './Slide'

export default {
  components: {
    Slide
  },

  props: {
    activeSlide: {
      type: Number,
      default() {
        return 0
      }
    }
  },

  data() {
    return {
      draggable: null,
      draggableWidth: 0,
      isDragging: false,
      velocityTracker: null,
      velocity: 0,
      projectWidth: null,
      projectHeight: null,
      projectLeft: null,
      projectTop: null,
      idleLazyImage: true,
      slides: [
        { title: 'Tommy Sports', image: 'project-tommy-sports.png' },
        { title: 'Tommy X Lewis', image: 'project-tommy-lewis.png' },
        {
          title: 'Volkswagen Vrienden<br> weegschaal',
          image: 'project-volkswagen.png'
        },
        { title: 'Radio 538', image: 'project-538.png' }
      ]
    }
  },

  computed: {
    skewStyles() {
      const maxVelocity = 3000
      const maxSkewX = 20

      const ratio = _.clamp(this.velocity / maxVelocity, -1, 1)

      console.log(ratio)

      const skewX = maxSkewX * ratio

      return {
        transform: `skewX(${skewX}deg) scale(${this.isDragging ? 0.95 : 1})`
      }
    },

    dragClass() {
      if (this.isDragging) {
        return this.$style.isDragging
      }
    }
  },

  watch: {
    $route() {
      this.handleRoute()
    },

    activeSlide() {
      this.handleSlideClick(this.activeSlide)
    }
  },

  mounted() {
    this.setDimensions()
    this.createDraggable()
    this.setDraggablePosition()
    this.tweenIn()
  },

  methods: {
    setDimensions() {
      this.draggableWidth =
        200 +
        (this.$refs.slide[0].$el.clientWidth + window.innerWidth / 10) *
          this.slides.length
    },

    handleRoute() {
      if (this.$route.name === 'projects') {
        return this.tweenInSlides()
      }

      if (this.$route.params.slug) {
        this.setProjectPosition(this.$route.params.slug)
      }
    },

    createDraggable() {
      this.draggable = Draggable.create(this.$refs.draggable, {
        type: 'x',
        throwProps: true,
        bounds: this.$refs.bounds,
        onDrag: this.onDrag,
        onDragEnd: this.onDragEnd,
        onThrowUpdate: this.onThrowUpdate
      })

      this.velocityTracker = VelocityTracker.track(
        Draggable.get(this.$refs.draggable),
        'x'
      )
    },

    destroyDraggable() {
      Draggable.get(this.$refs.draggable).kill()
      this.draggable = null
    },

    setDraggablePosition() {
      if (!this.$route.params.slug) {
        return
      }

      const { slug } = this.$route.params

      let maxLeft = this.draggableWidth - this.$el.clientWidth
      let left = this.$refs.slide[slug].$el.getBoundingClientRect().left - 500
      left = _.clamp(left, 0, maxLeft)

      TweenMax.set(this.$refs.draggable, {
        x: -left
      })

      Draggable.get(this.$refs.draggable).update()
    },

    onDrag() {
      this.isDragging = true
      this.velocity = this.velocityTracker.getVelocity('x')
    },

    onDragEnd() {
      this.isDragging = false
    },

    onThrowUpdate() {
      this.isDragging = false
      this.velocity = this.velocityTracker.getVelocity('x')
    },

    async handleSlideClick(activeIndex) {
      await this.tweenOutSlides(activeIndex)

      this.setProjectPosition(activeIndex)

      this.$router.push({
        path: `/projects/${activeIndex}`,
        props: true,
        params: {
          width: this.projectWidth,
          height: this.projectHeight,
          top: this.projectTop,
          left: this.projectLeft
        }
      })
    },

    async tweenOutSlides(activeIndex) {
      return new Promise((resolve, reject) => {
        const tl = new TimelineMax({
          onComplete: () => {
            resolve(true)
          }
        })

        let slides = this.$refs.slide.filter(slide => {
          return slide.$vnode.key !== activeIndex
        })

        tl.staggerTo(
          slides.map(slide => slide.$el),
          0.25,
          {
            cycle: {
              x: function(index) {
                return index < activeIndex ? -100 : 100
              }
            },
            autoAlpha: 0,
            ease: Power4.easeOut
          },
          0.05
        )
      })
    },

    tweenInSlides() {
      TweenMax.to(this.$refs.slide.map(slide => slide.$el), 1, {
        x: 0,
        autoAlpha: 1
      })
    },

    setProjectPosition(activeIndex) {
      const slide = this.$refs.slide[activeIndex].$refs.visual
      const { width, height, left, top } = slide.getBoundingClientRect()

      this.$data.projectWidth = width
      this.$data.projectHeight = height
      this.$data.projectLeft = left
      this.$data.projectTop = top
    },

    tweenIn() {
      TweenMax.to(this.$el, 1, {
        autoAlpha: 1
      })

      if (!this.$route.params.slug) {
        TweenMax.staggerFrom(
          this.$refs.slide.map(slide => slide.$el),
          1,
          {
            x: 500,
            scale: 0.5,
            autoAlpha: 0,
            ease: Power4.easeOut,
            onComplete: () => {
              this.idleLazyImage = false
            }
          },
          0.125
        )
      }
    }
  }
}
</script>

<style module>
.dragCarousel {
  opacity: 0;
  position: relative;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
}

.bounds {
  /* overflow: hidden; */
  display: inline-flex;
  align-items: center;
  width: 100%;
}

.inner {
  transform-style: preserve-3d;
  backface-visibility: hidden;
  display: inline-flex;
  flex-wrap: nowrap;
  padding: 0 10vw;
  user-select: all !important;
  will-change: transform;
}

.container {
  transition: transform 100ms ease-out;
  will-change: transform;
  transform: translate3d(0, 0, 0);
  backface-visibility: hidden;
  z-index: 1;
}

.project {
  overflow: hidden;
  position: fixed;
  left: 0;
  top: 0;
  width: 0;
  height: 0;
  z-index: 2;
}
</style>
