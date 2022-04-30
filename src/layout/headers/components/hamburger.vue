<template>
  <div class="hamburger-container" @click="toggleClick" id="hamburger">
    <svg-icon :icon="icon"></svg-icon>
  </div>
</template>

<script setup>
import { computed } from 'vue'
import { useStore } from 'vuex'
const store = useStore()
const toggleClick = () => {
  store.commit('app/changeSidedType')
}
const icon = computed(() => {
  return store.getters.sidedType ? 'hamburger-opened' : 'hamburger-closed'
})
</script>

<style lang="scss" scoped>
.hamburger-container {
  margin-right: 16px;
  box-sizing: border-box;
  cursor: pointer;
}
</style>
