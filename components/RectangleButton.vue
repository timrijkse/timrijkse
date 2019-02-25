<template>
  <button :class="$style.rectangleButton" data-cursor="pointer">
    <span :class="$style.inner">
      <slot></slot>
    </span>
  </button>
</template>

<script>
export default {}
</script>

<style module>
.rectangleButton {
  display: inline-block;
  padding: 2px;
  background-color: none;
  background-color: #fff;
}

.inner {
  pointer-events: none;
  display: block;
  width: 100%;
  height: 100%;
  padding: 10px;
  border: 1px solid #000;
  color: #000;
}
</style>
