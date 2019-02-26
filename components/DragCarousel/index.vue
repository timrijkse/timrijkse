<template>
  <div ref="bounds" :class="[$style.dragCarousel, dragClass]">
    <div :class="$style.container" :style="skewStyles" data-cursor="drag">
      <div ref="draggable" :class="$style.inner" data-cursor="drag">
        <slide
          v-for="n in 10"
          ref="slide"
          :class="$style.slide"
          :key="n"
          to="project"
          @on-click="handleSlideClick(n)"
        />
      </div>
    </div>

    <nuxt-child ref="project" :class="$style.project"/>
  </div>
</template>

<script>
import { TweenMax, Power4 } from 'gsap'
import Slide from './Slide'

export default {
  components: {
    Slide
  },

  data() {
    return {
      draggable: null,
      isDragging: false,
      velocityTracker: null,
      velocity: 0
    }
  },

  computed: {
    skewStyles() {
      const maxVelocity = 3000
      const maxSkewX = 20

      const ratio = this.velocity / maxVelocity
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

  mounted() {
    this.createDraggable()
  },

  methods: {
    createDraggable() {
      const Draggable = require('@robotkittens/gsap/Draggable')
      const ThrowPropsPlugin = require('@robotkittens/gsap/ThrowPropsPlugin')
      const ModifiersPlugin = require('@robotkittens/gsap/ModifiersPlugin')

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
    },

    onDrag() {
      this.isDragging = true
      this.velocity = this.velocityTracker.getVelocity('x')
    },

    onDragEnd() {
      this.isDragging = false
      console.log('onDragEnd', this.isDragging)
    },

    onThrowUpdate() {
      this.isDragging = false
      this.velocity = this.velocityTracker.getVelocity('x')
    },

    async handleSlideClick(i) {
      console.log(
        'handleSlideClick',
        event.target.getBoundingClientRect(),
        this.$refs
      )

      await this.tweenOutSlides(i)

      console.log('done!!!')

      // this.$router.push('/project')

      // const { width, height, left, top } = el.getBoundingClientRect()

      // TweenMax.set(this.$refs.project.$el, {
      //   width,
      //   height,
      //   left,
      //   top
      // })

      // TweenMax.to(this.$refs.project.$el, 1.5, {
      //   width: '100%',
      //   height: '100%',
      //   left: 0,
      //   top: 0,
      //   ease: Power4.easeOut
      // })
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
                return index < activeIndex - 1 ? -100 : 100
              }
            },
            autoAlpha: 0,
            ease: Power4.easeOut
          },
          0.05
        )

        tl.to(
          this.$refs.slide[activeIndex - 1].$refs.title,
          0.25,
          {
            y: 30,
            autoAlpha: 0,
            ease: Power4.easeOut
          },
          '-=0.5'
        )
      })
    }
  }
}
</script>

<style module>
.dragCarousel {
  perspective: 800px;
  perspective-origin: 150% 150%;
  overflow: hidden;
  display: inline-flex;
  align-items: center;
  width: 100%;
  height: 500px;
}

.inner {
  transform-style: preserve-3d;
  display: inline-flex;
  flex-wrap: nowrap;
  width: 5000px;
  height: 350px;
  user-select: all !important;
  will-change: transform;
}

.container {
  transition: transform 100ms ease-out;
  will-change: transform;
}

.project {
  overflow: hidden;
  position: fixed;
  left: 0;
  top: 0;
  width: 0;
  height: 0;
  background: #404040;
}
</style>
