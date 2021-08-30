<template>
  <span ref="span" v-intersect="onIntersect">
    <div v-if="animated" class="relative" :class="animated ? transition : ''">
      <slot />
    </div>
  </span>
</template>

<script>
export default {
  props: {
    /**
     * Type d'animation
     * @values fade, slide-x, slide-x-reverse, slide-y, slide-y-reverse
     */
    transition: {
      type: String,
      default: '',
    },
    /**
     * Durée de l'animation (ex: '2s', '400ms')
     */
    duration: {
      type: String,
      default: '2s',
    },
    /**
     * Distance parcouru par les animations slide en px
     */
    distance: {
      type: Number,
      default: 200,
    },
    /**
     * Delai de l'animation (ex: '2s', '400ms')
     */
    delay: {
      type: String,
      default: '0ms',
    },
  },
  data() {
    return {
      animated: false,
      intersect: false,
    }
  },
  watch: {
    duration() {
      this.$refs.span.style.setProperty('--duration', this.duration)
    },
    distance() {
      this.$refs.span.style.setProperty('--distance', this.distance + 'px')
    },
    delay() {
      this.$refs.span.style.setProperty('--delay', this.delay)
    },
  },
  mounted() {
    this.$refs.span.style.setProperty('--duration', this.duration)
    this.$refs.span.style.setProperty('--distance', this.distance + 'px')
    this.$refs.span.style.setProperty('--delay', this.delay)
    if (this.intersect) {
      window.requestAnimationFrame(() => {
        this.animated = true
      })
    }
  },
  methods: {
    onIntersect(_, __, intersect) {
      this.intersect = this.intersect || intersect
      if (this.intersect && !this.animated) {
        window.requestAnimationFrame(() => {
          this.animated = true
        })
      }
    },
  },
}
</script>

<style scoped>
.relative {
  position: relative;
}

.slide-x {
  animation: hideBase var(--delay, 0ms) ease forwards,
    slideX var(--duration, 2000ms) ease var(--delay, 0ms) forwards;
}

.slide-x-reverse {
  animation: hideBase var(--delay, 0ms) ease forwards,
    slideXReverse var(--duration, 2000ms) ease var(--delay, 0ms) forwards;
}

.slide-y {
  animation: hideBase var(--delay, 0ms) ease forwards,
    slideY var(--duration, 2000ms) ease var(--delay, 0ms) forwards;
}

.slide-y-reverse {
  animation: hideBase var(--delay, 0ms) ease forwards,
    slideYReverse var(--duration, 2000ms) ease var(--delay, 0ms) forwards;
}

.fade {
  animation: hideBase var(--delay, 0ms) ease forwards,
    fadeIn var(--duration, 2000ms) ease var(--delay, 0ms) forwards;
}

.flip-left {
  transform: rotateY(90deg);
  animation: flipY var(--duration, 2000ms) ease var(--delay, 0ms) forwards;
}

.flip-right {
  transform: rotateY(-90deg);
  animation: flipY var(--duration, 2000ms) ease var(--delay, 0ms) forwards;
}

.flip-up {
  transform: rotateX(90deg);
  animation: flipX var(--duration, 2000ms) ease var(--delay, 0ms) forwards;
}

.flip-down {
  transform: rotateX(-90deg);
  animation: flipX var(--duration, 2000ms) ease var(--delay, 0ms) forwards;
}

.zoom-in {
  animation: hideBase var(--delay, 0ms) ease forwards,
    zoomIn var(--duration, 2000ms) ease var(--delay, 0ms) forwards;
}

.zoom-out {
  animation: hideBase var(--delay, 0ms) ease forwards,
    zoomOut var(--duration, 2000ms) ease var(--delay, 0ms) forwards;
}

@keyframes hideBase {
  from {
    opacity: 0;
  }
  99% {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

@keyframes slideX {
  from {
    left: calc(-1 * var(--distance, 200px));
    opacity: 0;
  }
  to {
    left: 0px;
  }
}

@keyframes slideXReverse {
  from {
    left: var(--distance, 200px);
    opacity: 0;
  }
  to {
    left: 0px;
  }
}

@keyframes slideY {
  from {
    top: var(--distance, 200px);
    opacity: 0;
  }
  to {
    top: 0px;
  }
}

@keyframes slideYReverse {
  from {
    top: calc(-1 * var(--distance, 200px));
    opacity: 0;
  }
  to {
    top: 0px;
  }
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
}

@keyframes flipY {
  to {
    transform: rotateY(0deg);
  }
}

@keyframes flipX {
  to {
    transform: rotateX(0deg);
  }
}

@keyframes zoomIn {
  from {
    transform: scale(0.8);
    opacity: 0;
  }
}

@keyframes zoomOut {
  from {
    transform: scale(1.2);
    opacity: 0;
  }
}
</style>
