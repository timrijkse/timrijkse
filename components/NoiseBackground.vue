<template>
  <div :class="$style.noiseBackground" :style="{ opacity: opacity }">
    <div :class="$style.noise"></div>
  </div>
</template>

<script>
export default {
  props: {
    opacity: {
      type: Number,
      default() {
        return 1
      }
    }
  }
}
</script>

<style module>
.noiseBackground {
  transition: opacity 200ms ease-out;
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
}

.noiseBackground:before {
  content: '';
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background: radial-gradient(ellipse at center, #29382c 0%, #0c100d 100%);
}

.noiseBackground:after {
  content: '';
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background: radial-gradient(
    ellipse at center,
    rgba(0, 0, 0, 0) 0%,
    rgba(0, 0, 0, 0.75) 100%
  );
}

.noise {
  position: absolute;
  top: -500px;
  right: -500px;
  bottom: -500px;
  left: -500px;
  background: transparent url('~assets/images/noise.png') 0 0;
  background-size: 320px 320px;
  opacity: 0.2;
  animation: noise 1s steps(8, end) infinite both;
}

@keyframes noise {
  0% {
    transform: translateX(0px, 0px);
  }
  10% {
    transform: translate(-100px, 100px);
  }
  20% {
    transform: translate(150px, -100px);
  }
  30% {
    transform: translate(-100px, 100px);
  }
  40% {
    transform: translate(100px, -150px);
  }
  50% {
    transform: translate(-100px, 200px);
  }
  60% {
    transform: translate(-200px, -100px);
  }
  70% {
    transform: translateY(50px, 100px);
  }
  80% {
    transform: translate(100px, -150px);
  }
  90% {
    transform: translate(0px, 200px);
  }
  100% {
    transform: translate(-100px, 100px);
  }
}
</style>
