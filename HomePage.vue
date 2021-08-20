<template>
  <div>
    <div class="page">
      <slot :currentSection="currentSection" />
    </div>
    <div ref="overlay" class="overlay">
      <button
        v-for="i in allSection"
        :key="i"
        :class="{ hidden: currentSection !== i }"
        @click="setCurrentSection(i)"
      />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentSection: '',
      allSection: [],
      scrollDisabled: false,
    }
  },
  mounted() {
    this.allSection = this.$scopedSlots
      .default()
      ?.filter((it) => it.componentOptions?.tag === 'HomeSection')
      .map((it) => it.componentOptions?.propsData?.id)
    this.$refs.overlay.style.setProperty(
      '--overlay-size',
      this.allSection.length * 12 + 'px'
    )
    if (this.allSection?.length) {
      this.setCurrentSection(this.allSection[0])
      window.addEventListener('wheel', (e) => {
        if (this.scrollDisabled) return
        this.scrollDisabled = true
        setTimeout(() => {
          this.scrollDisabled = false
        }, 1000)
        if (e.deltaY < 0 && this.currentSection !== this.allSection[0]) {
          this.currentSection =
            this.allSection[
              this.allSection.findIndex((it) => it === this.currentSection) - 1
            ]
        } else if (
          e.deltaY > 0 &&
          this.currentSection !== this.allSection[this.allSection.length - 1]
        ) {
          this.currentSection =
            this.allSection[
              this.allSection.findIndex((it) => it === this.currentSection) + 1
            ]
        }
      })
    }
  },
  methods: {
    setCurrentSection(id) {
      this.currentSection = id
    },
  },
}
</script>

<style scoped>
.overlay {
  display: flex;
  flex-direction: column;
  position: fixed;
  right: 20px;
  top: calc(50vh - var(--overlay-size, 0));
}

.overlay button {
  width: 4px;
  height: 20px;
  border-radius: 2px;
  background-color: white;
  margin-top: 2px;
  margin-bottom: 2px;
  opacity: 1;
}

button.hidden {
  animation: disappear 500ms ease forwards;
}

@keyframes disappear {
  from {
    opacity: 1;
  }
  to {
    opacity: 0.5;
  }
}
</style>
