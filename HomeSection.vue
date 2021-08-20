<template>
  <section :class="{ hidden: currentSection !== id }">
    <slot />
  </section>
</template>

<script>
export default {
  props: {
    currentSection: {
      type: String,
      default: '',
    },
    id: {
      type: String,
      required: true,
    },
  },
}
</script>

<style scoped>
section {
  height: 100vh;
  width: 100vw;
  animation: appear 500ms ease;
}

.hidden {
  display: none;
}

@keyframes appear {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
</style>
