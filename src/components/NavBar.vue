<template>
  <nav class="navbar-wrap">
    <div class="navbar" ref="navbarRef">
      <!-- скользящий индикатор -->
      <div class="slider" :style="sliderStyle" />

      <RouterLink
        v-for="item in links"
        :key="item.to"
        :to="item.to"
        class="nav-item"
        :class="{ active: isActive(item.to) }"
        :ref="el => setItemRef(el, item.to)"
      >
        {{ item.label }}
      </RouterLink>
    </div>
  </nav>
</template>

<script setup>
import { useRoute, RouterLink } from 'vue-router'
import { ref, reactive, watch, onMounted, nextTick } from 'vue'

const route = useRoute()

const links = [
  { to: '/home', label: 'Главная' },
  { to: '/users', label: 'Пользователи' },
  { to: '/profile', label: 'Профиль' },
]

function isActive(path) {
  return route.path === path
}

const navbarRef = ref(null)
const itemRefs = reactive({})
const sliderStyle = ref({ opacity: 0 })

function setItemRef(el, to) {
  if (el) itemRefs[to] = el
}

function updateSlider() {
  const activePath = route.path
  const el = itemRefs[activePath]?.$el ?? itemRefs[activePath]
  const navbar = navbarRef.value

  if (!el || !navbar) {
    sliderStyle.value = { opacity: 0 }
    return
  }

  const navRect = navbar.getBoundingClientRect()
  const elRect = el.getBoundingClientRect()

  sliderStyle.value = {
    opacity: 1,
    width: elRect.width + 'px',
    height: elRect.height + 'px',
    transform: `translateX(${elRect.left - navRect.left - 6}px)`,
  }
}

// при смене маршрута — плавно двигаем
watch(() => route.path, async () => {
  await nextTick()
  updateSlider()
})

onMounted(async () => {
  await nextTick()
  // первый рендер без анимации — сразу ставим на место
  sliderStyle.value = { opacity: 0 }
  await nextTick()
  updateSlider()
})
</script>

<style scoped>
.navbar-wrap {
  position: fixed;
  top: 24px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 100;
}

.navbar {
  position: relative;
  display: flex;
  align-items: center;
  gap: 4px;
  padding: 6px;
  border-radius: 999px;
  background: rgba(255, 255, 255, 0.55);
  backdrop-filter: blur(30px) saturate(160%);
  -webkit-backdrop-filter: blur(30px) saturate(160%);
  border: 1px solid rgba(255, 255, 255, 0.6);
  box-shadow:
    0 8px 30px rgba(0, 0, 0, 0.06),
    inset 0 1px 0 rgba(255, 255, 255, 0.7);
}

/* скользящий индикатор */
.slider {
  position: absolute;
  top: 5px;
  left: 5px;
  border-radius: 999px;
  background: #ebebec;
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  box-shadow:
    inset 0 1px 0 rgba(255, 255, 255, 0.85),
    0 2px 8px rgba(0, 0, 0, 0.07);
  /* вот здесь вся магия — плавное скольжение */
  transition:
    transform 0.35s cubic-bezier(0.34, 1.2, 0.64, 1),
    width 0.35s cubic-bezier(0.34, 1.2, 0.64, 1),
    opacity 0.2s ease;
  pointer-events: none;
  z-index: 0;
}

.nav-item {
  position: relative;
  z-index: 1;
  padding: 8px 18px;
  border-radius: 999px;
  font-size: 14px;
  font-weight: 500;
  color: rgba(0, 0, 0, 0.45);
  text-decoration: none;
  transition: color 0.22s ease;
  white-space: nowrap;
}

.nav-item:hover {
  color: rgba(0, 0, 0, 0.75);
}

.nav-item.active {
  color: #0066cc;
  font-weight: 600;
}
</style>