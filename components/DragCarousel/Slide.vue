<template>
  <div :class="$style.slide" data-cursor="drag">
    <div ref="visual" :class="$style.visual" data-cursor="pointer" @click="onClick"></div>
    <h2 ref="title" :class="$style.title" @click="onClick">Project name</h2>
  </div>
</template>

<script>
export default {
  methods: {
    onClick() {
      this.$emit('on-click', this.$refs.visual)
    }
  }
}
</script>

<style module>
.slide {
  flex: 0 0 400px;
  margin: 0 100px 0 0;
}

.visual {
  display: block;
  width: 100%;
  height: 300px;
  background-color: #404040;
}

.title {
  pointer-events: none;
  color: white;
}
</style>
