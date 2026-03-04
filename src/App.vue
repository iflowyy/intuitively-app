<template>
  <NavBar />
  <main class="main-content">
    <RouterView v-slot="{ Component }">
      <Transition name="fade" mode="out-in">
        <component :is="Component" />
      </Transition>
    </RouterView>
  </main>
</template>

<script setup>
import NavBar from './components/NavBar.vue'
import { RouterView } from 'vue-router'
</script>

<style scoped>
.main-content {
  flex: 1;
  padding-top: 80px;
  min-height: 100vh;
}
</style>
