<template>
  <div class="app-wrapper">
    <el-container>
      <el-aside :width="xxx" class="sidebar-container">
        <Menu></Menu>
      </el-aside>
      <el-container
        class="container"
        :class="{ hidderContainer: !$store.getters.sidedType }"
      >
        <el-header>
          <Headers></Headers>
        </el-header>
        <el-main>
          <router-view></router-view>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script setup>
import Menu from './Menu'
import Headers from './headers'
import { useStore } from 'vuex'
import { computed } from 'vue'
const store = useStore()
const xxx = computed(() => {
  return store.getters.sidedType ? '210px' : '67px'
})
</script>

<style lang="scss" scoped>
.app-container {
  // position: relative;
  width: 100%;
  height: 100%;
}
.container {
  width: calc(100% - $sideBarWidth);
  height: 100%;

  position: fixed;
  top: 0;
  right: 0;
  z-index: 9;
  transition: all 0.28s;
  &.hidderContainer {
    width: calc(100% - $hideSideBarWidth);
  }
}
::v-deep .el-header {
  padding: 0;
}
</style>
