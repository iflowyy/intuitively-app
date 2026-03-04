<template>
  <div class="page login-page">
    <div class="login-card">
      <div class="login-header">
        <h1 class="login-title">Вход</h1>
        <p class="login-sub">Введите данные для входа в систему</p>
      </div>

      <div class="form-body">
        <div class="field">
          <label class="field-label" for="username">Логин</label>
          <input
            id="username"
            v-model="form.username"
            type="text"
            class="field-input"
            placeholder="Ваш логин"
            autocomplete="username"
            @keyup.enter="handleLogin"
          />
        </div>

        <div class="field">
          <label class="field-label" for="password">Пароль</label>
          <input
            id="password"
            v-model="form.password"
            type="password"
            class="field-input"
            placeholder="Ваш пароль"
            autocomplete="current-password"
            @keyup.enter="handleLogin"
          />
        </div>

        <Transition name="err">
          <div v-if="errorMsg" class="error-box">
            <span class="error-icon">!</span>
            {{ errorMsg }}
          </div>
        </Transition>

        <button
          class="btn-submit"
          :class="{ loading: isLoading }"
          :disabled="isLoading"
          @click="handleLogin"
        >
          <span v-if="!isLoading">Войти</span>
          <span v-else class="btn-spinner"></span>
        </button>
      </div>

      <div class="login-footer">
        <p class="login-hint">Нет аккаунта?</p>
        <RouterLink to="/register" class="link-register">Зарегистрироваться</RouterLink>
      </div>

      <div class="demo-hint">
        <p class="demo-label">Демо-доступ</p>
        <p class="demo-creds">логин: <code>emilys</code> &nbsp;/&nbsp; пароль: <code>emilyspass</code></p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'
import { useRouter, RouterLink } from 'vue-router'

const router = useRouter()

const form = reactive({ username: '', password: '' })
const errorMsg = ref(null)
const isLoading = ref(false)

async function handleLogin() {
  if (!form.username || !form.password) {
    errorMsg.value = 'Пожалуйста, заполните все поля.'
    return
  }

  isLoading.value = true
  errorMsg.value = null

  try {
    const res = await fetch('https://dummyjson.com/auth/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        username: form.username,
        password: form.password,
        expiresInMins: 60,
      }),
    })

    const data = await res.json()

    if (!res.ok) {
      errorMsg.value = data.message || 'Неверный логин или пароль.'
      return
    }

    localStorage.setItem('token', data.accessToken)
    router.push('/profile')
  } catch (e) {
    errorMsg.value = 'Ошибка сети. Попробуйте ещё раз.'
  } finally {
    isLoading.value = false
  }
}
</script>

<style scoped>
.login-page {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: calc(100vh - 80px);
  padding: 40px 24px;
}

.login-card {
  width: 100%;
  max-width: 400px;
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius-lg);
  padding: 36px 32px 28px;
  box-shadow: var(--shadow-lg);
}

.login-header {
  margin-bottom: 28px;
}

.login-title {
  font-size: 28px;
  font-weight: 600;
  letter-spacing: -0.03em;
  color: var(--text-primary);
  margin-bottom: 6px;
}

.login-sub {
  font-size: 14px;
  color: var(--text-secondary);
}

.form-body {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.field {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.field-label {
  font-size: 13px;
  font-weight: 500;
  color: var(--text-secondary);
}

.field-input {
  padding: 12px 14px;
  background: var(--surface-2);
  border: 1px solid var(--border);
  border-radius: var(--radius-sm);
  font-family: inherit;
  font-size: 15px;
  color: var(--text-primary);
  outline: none;
  transition: border-color var(--transition), box-shadow var(--transition);
}

.field-input::placeholder {
  color: var(--text-tertiary);
}

.field-input:focus {
  border-color: var(--accent);
  box-shadow: 0 0 0 3px rgba(0, 113, 227, 0.12);
  background: var(--surface);
}

.error-box {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 11px 14px;
  background: rgba(255, 59, 48, 0.07);
  border: 1px solid rgba(255, 59, 48, 0.2);
  border-radius: var(--radius-sm);
  font-size: 13px;
  color: var(--danger);
}

.error-icon {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 18px;
  height: 18px;
  border-radius: 50%;
  background: var(--danger);
  color: white;
  font-size: 11px;
  font-weight: 700;
  flex-shrink: 0;
}

.btn-submit {
  margin-top: 4px;
  width: 100%;
  padding: 13px;
  background: var(--text-primary);
  color: white;
  border: none;
  border-radius: var(--radius-sm);
  font-size: 15px;
  font-weight: 500;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background var(--transition), transform var(--transition);
}

.btn-submit:hover:not(:disabled) {
  background: #333;
  transform: translateY(-1px);
}

.btn-submit.loading {
  opacity: 0.7;
  cursor: default;
}

.btn-spinner {
  width: 18px;
  height: 18px;
  border: 2px solid rgba(255,255,255,0.3);
  border-top-color: white;
  border-radius: 50%;
  animation: spin 0.7s linear infinite;
  display: inline-block;
}

.login-footer {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  margin-top: 20px;
}

.login-hint {
  font-size: 13px;
  color: var(--text-secondary);
}

.link-register {
  font-size: 13px;
  font-weight: 500;
  color: var(--accent);
  transition: opacity var(--transition);
}

.link-register:hover {
  opacity: 0.75;
}

.demo-hint {
  margin-top: 20px;
  padding: 12px 14px;
  background: var(--surface-2);
  border-radius: var(--radius-sm);
  border: 1px solid var(--border);
}

.demo-label {
  font-size: 11px;
  font-weight: 600;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  color: var(--text-tertiary);
  margin-bottom: 5px;
}

.demo-creds {
  font-size: 13px;
  color: var(--text-secondary);
}

.demo-creds code {
  font-family: 'SF Mono', 'Menlo', monospace;
  font-size: 12px;
  background: var(--surface);
  padding: 2px 6px;
  border-radius: 4px;
  color: var(--text-primary);
  border: 1px solid var(--border);
}

.err-enter-active, .err-leave-active {
  transition: opacity 0.2s ease, transform 0.2s ease;
}
.err-enter-from, .err-leave-to {
  opacity: 0;
  transform: translateY(-4px);
}

@keyframes spin {
  to { transform: rotate(360deg); }
}
</style>
