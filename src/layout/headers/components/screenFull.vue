<template>
  <div @click="handleFullscreen" id="screenFul">
    <svg-icon :icon="icon ? 'exit-fullscreen' : 'fullscreen'"></svg-icon>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import screenfull from 'screenfull'

const icon = ref(screenfull.isFullscreen)

const handleFullscreen = () => {
  if (screenfull.isEnabled) {
    screenfull.toggle()
  }
}

const changeIcon = () => {
  icon.value = screenfull.isFullscreen
}

onMounted(() => {
  console.log(screenfull.isFullscreen)
  screenfull.on('change', changeIcon)
})

onBeforeUnmount(() => {
  screenfull.off('change')
})
</script>

<style lang="scss" scoped></style>
