<template>
  <div :class="$style.slide" data-cursor="drag">
    <div ref="visual" data-cursor="pointer" @click="onClick">
      <lazy-image
        :class="$style.visual"
        :aspect-ratio="0.75"
        :idle="idleLazyImage"
        src="placeholder.png"
      />
    </div>
    <h2 ref="title" :class="$style.title" @click="onClick">Project name</h2>
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
    }
  },

  methods: {
    onClick() {
      this.$emit('on-click', this.$refs.visual)
    }
  }
}
</script>

<style module>
.slide {
  flex: 0 0 200px;
  margin: 0 50px 0 0;
}

.title {
  pointer-events: none;
  color: #fff;
}

.visual {
  pointer-events: none;
}

@media (min-width: 768px) {
  .slide {
    flex: 0 0 400px;
    margin: 0 100px 0 0;
  }
}
</style>
