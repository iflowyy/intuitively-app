<template>
  <div class="page users-page">
    <div class="users-inner">
      <div class="page-header">
        <h1 class="page-title">Пользователи</h1>
        <p class="page-sub">{{ users.length > 0 ? users.length + ' пользователей' : '' }}</p>
      </div>

      <!-- Loading -->
      <div v-if="loading" class="state-wrap">
        <div class="spinner"></div>
        <p class="state-text">Загрузка пользователей…</p>
      </div>

      <!-- Error -->
      <div v-else-if="error" class="state-wrap">
        <p class="state-text error">{{ error }}</p>
      </div>

      <!-- List -->
      <div v-else class="users-list">
        <div
          v-for="(user, i) in users"
          :key="user.id"
          class="user-card"
          :style="{ animationDelay: i * 0.04 + 's' }"
        >
          <div class="user-avatar">
            <img :src="user.image" :alt="user.firstName" loading="lazy" />
          </div>
          <div class="user-info">
            <span class="user-name">{{ user.firstName }} {{ user.lastName }}</span>
            <span class="user-email">{{ user.email }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const users = ref([])
const loading = ref(true)
const error = ref(null)

onMounted(async () => {
  try {
    const res = await fetch('https://dummyjson.com/users')
    if (!res.ok) throw new Error('Failed to load users')
    const data = await res.json()
    users.value = data.users
  } catch (e) {
    error.value = e.message
  } finally {
    loading.value = false
  }
})
</script>

<style scoped>
.users-page {
  padding: 48px 24px 80px;
}

.users-inner {
  max-width: 560px;
  margin: 0 auto;
}

.page-header {
  margin-bottom: 32px;
  display: flex;
  align-items: baseline;
  gap: 12px;
}

.page-title {
  font-size: 34px;
  font-weight: 600;
  letter-spacing: -0.03em;
  color: var(--text-primary);
}

.page-sub {
  font-size: 14px;
  color: var(--text-tertiary);
  font-weight: 400;
}

.users-list {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.user-card {
  display: flex;
  align-items: center;
  gap: 14px;
  padding: 14px 16px;
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius-md);
  transition: box-shadow var(--transition), transform var(--transition);
  animation: slideUp 0.4s ease both;
}

.user-card:hover {
  box-shadow: var(--shadow-md);
  transform: translateY(-1px);
}

.user-avatar {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  overflow: hidden;
  flex-shrink: 0;
  background: var(--surface-2);
}

.user-avatar img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.user-info {
  display: flex;
  flex-direction: column;
  gap: 2px;
  min-width: 0;
}

.user-name {
  font-size: 15px;
  font-weight: 500;
  color: var(--text-primary);
  letter-spacing: -0.01em;
}

.user-email {
  font-size: 13px;
  color: var(--text-secondary);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

/* States */
.state-wrap {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 80px 0;
  gap: 14px;
}

.state-text {
  font-size: 14px;
  color: var(--text-tertiary);
}

.state-text.error {
  color: var(--danger);
}

.spinner {
  width: 24px;
  height: 24px;
  border: 2px solid var(--border);
  border-top-color: var(--text-primary);
  border-radius: 50%;
  animation: spin 0.7s linear infinite;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}

@keyframes slideUp {
  from { opacity: 0; transform: translateY(10px); }
  to   { opacity: 1; transform: translateY(0); }
}
</style>
