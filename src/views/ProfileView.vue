<template>
  <div class="page profile-page">
    <div class="profile-inner">

      <!-- Loading -->
      <div v-if="loading" class="state-wrap">
        <div class="spinner"></div>
        <p class="state-text">Загрузка профиля…</p>
      </div>

      <!-- Error (invalid token etc) -->
      <div v-else-if="error" class="error-header">
        <div class="error-icon-wrap">
          <span class="error-icon">!</span>
        </div>
        <h1 class="error-title">{{ error }}</h1>
        <button class="btn-logout" @click="logout">Войти снова</button>
      </div>

      <!-- Profile -->
      <div v-else-if="user" class="profile-card">
        <h1 class="profile-page-title">Мой профиль</h1>
        <div class="profile-top">
          <div class="avatar-wrap">
            <img :src="user.image" :alt="user.firstName" class="avatar-img" />
          </div>
          <div class="profile-name-block">
            <h1 class="profile-name">{{ user.firstName }} {{ user.lastName }}</h1>
            <p class="profile-username">@{{ user.username }}</p>
          </div>
        </div>

        <div class="profile-fields">
          <div class="field-row">
            <span class="field-key">Email</span>
            <span class="field-val">{{ user.email }}</span>
          </div>
          <div class="field-row">
            <span class="field-key">Пол</span>
            <span class="field-val capitalize">{{ user.gender }}</span>
          </div>
          <div class="field-row">
            <span class="field-key">Возраст</span>
            <span class="field-val">{{ user.age }}</span>
          </div>
          <div class="field-row">
            <span class="field-key">Телефон</span>
            <span class="field-val">{{ user.phone }}</span>
          </div>
          <div class="field-row">
            <span class="field-key">Компания</span>
            <span class="field-val">{{ user.company?.name }}</span>
          </div>
        </div>

        <button class="btn-logout" @click="logout">Выйти</button>
      </div>

    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const user = ref(null)
const loading = ref(true)
const error = ref(null)

onMounted(async () => {
  const token = localStorage.getItem('token')

  if (!token) {
    router.push('/login')
    return
  }

  try {
    const res = await fetch('https://dummyjson.com/auth/me', {
      method: 'GET',
      headers: {
        'Authorization': `Bearer ${token}`,
      },
    })

    const data = await res.json()

    if (!res.ok) {
      error.value = data.message || 'Токен недействителен или истёк.'
      return
    }

    user.value = data
  } catch (e) {
    error.value = 'Ошибка сети. Не удалось загрузить профиль.'
  } finally {
    loading.value = false
  }
})

function logout() {
  localStorage.removeItem('token')
  router.push('/login')
}
</script>

<style scoped>
.profile-page {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: calc(100vh - 80px);
  padding: 40px 24px;
}

.profile-inner {
  width: 100%;
  max-width: 480px;
}

.profile-page-title {
  font-size: 28px;
  font-weight: 600;
  letter-spacing: -0.03em;
  color: var(--text-primary);
  margin-bottom: 24px;
}

.profile-card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius-lg);
  padding: 32px;
  box-shadow: var(--shadow-lg);
  animation: slideUp 0.35s ease both;
}

.profile-top {
  display: flex;
  align-items: center;
  gap: 20px;
  margin-bottom: 28px;
  padding-bottom: 24px;
  border-bottom: 1px solid var(--border);
}

.avatar-wrap {
  width: 72px;
  height: 72px;
  border-radius: 50%;
  overflow: hidden;
  flex-shrink: 0;
  background: var(--surface-2);
  box-shadow: 0 2px 8px rgba(0,0,0,0.12);
}

.avatar-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.profile-name {
  font-size: 22px;
  font-weight: 600;
  letter-spacing: -0.025em;
  color: var(--text-primary);
  margin-bottom: 4px;
}

.profile-username {
  font-size: 14px;
  color: var(--text-secondary);
}

.profile-fields {
  display: flex;
  flex-direction: column;
  gap: 0;
  margin-bottom: 28px;
}

.field-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px 0;
  border-bottom: 1px solid var(--border);
}

.field-row:last-child {
  border-bottom: none;
}

.field-key {
  font-size: 13px;
  color: var(--text-secondary);
  font-weight: 400;
}

.field-val {
  font-size: 14px;
  color: var(--text-primary);
  font-weight: 500;
  text-align: right;
  max-width: 60%;
  word-break: break-word;
}

.capitalize {
  text-transform: capitalize;
}

.btn-logout {
  width: 100%;
  padding: 11px;
  background: transparent;
  border: 1px solid var(--border);
  border-radius: var(--radius-sm);
  font-family: inherit;
  font-size: 14px;
  font-weight: 500;
  color: var(--text-secondary);
  cursor: pointer;
  transition: background var(--transition), color var(--transition), border-color var(--transition);
}

.btn-logout:hover {
  background: var(--danger);
  color: white;
  border-color: var(--danger);
}

/* Error state */
.error-header {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 16px;
  text-align: center;
  padding: 60px 24px;
  animation: slideUp 0.35s ease both;
}

.error-icon-wrap {
  width: 48px;
  height: 48px;
  border-radius: 50%;
  background: rgba(255, 59, 48, 0.1);
  display: flex;
  align-items: center;
  justify-content: center;
}

.error-icon {
  font-size: 20px;
  font-weight: 700;
  color: var(--danger);
}

.error-title {
  font-size: 18px;
  font-weight: 500;
  color: var(--text-primary);
  letter-spacing: -0.02em;
  max-width: 320px;
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
  from { opacity: 0; transform: translateY(12px); }
  to   { opacity: 1; transform: translateY(0); }
}
</style>
