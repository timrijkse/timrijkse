<template>
  <button :class="$style.rectangleButton" data-cursor="drag">
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
  transition: background-color 200ms ease-out;
  outline: none;
  display: inline-block;
  padding: 2px;
  background-color: none;
  background-color: #fff;
}

.inner {
  transition: all 200ms ease-out;
  pointer-events: none;
  display: block;
  width: 100%;
  height: 100%;
  padding: 10px;
  border: 1px solid #000;
  color: #000;
}

.rectangleButton:hover {
  background-color: #000;
}

.rectangleButton:hover .inner {
  border-color: #fff;
  color: #fff;
}
</style>
