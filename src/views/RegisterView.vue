<template>
  <div class="page register-page">
    <div class="register-card">

      <!-- Успех -->
      <div v-if="success" class="success-state">
        <div class="success-icon-wrap">
          <span class="success-icon">✓</span>
        </div>
        <h2 class="success-title">Пользователь создан!</h2>
        <p class="success-desc">
          Аккаунт <strong>{{ createdUser.username }}</strong> успешно добавлен в систему.<br />
          ID нового пользователя: <strong>#{{ createdUser.id }}</strong>
        </p>
        <div class="success-note">
          <p>ℹ️ DummyJSON — учебный API. Войти под новым аккаунтом нельзя, но запрос на добавление прошёл успешно и вернул ответ сервера.</p>
        </div>
        <div class="success-actions">
          <RouterLink to="/users" class="btn-primary">Смотреть пользователей</RouterLink>
          <RouterLink to="/login" class="btn-secondary">Войти</RouterLink>
        </div>
      </div>

      <!-- Форма -->
      <template v-else>
        <div class="register-header">
          <RouterLink to="/login" class="back-link">← Назад</RouterLink>
          <h1 class="register-title">Регистрация</h1>
          <p class="register-sub">Создайте новый аккаунт</p>
        </div>

        <div class="form-body">
          <div class="fields-row">
            <div class="field">
              <label class="field-label">Имя</label>
              <input v-model="form.firstName" type="text" class="field-input" placeholder="Иван" @keyup.enter="handleRegister" />
            </div>
            <div class="field">
              <label class="field-label">Фамилия</label>
              <input v-model="form.lastName" type="text" class="field-input" placeholder="Иванов" @keyup.enter="handleRegister" />
            </div>
          </div>

          <div class="field">
            <label class="field-label">Логин</label>
            <input v-model="form.username" type="text" class="field-input" placeholder="Ваш логин" autocomplete="username" @keyup.enter="handleRegister" />
          </div>

          <div class="field">
            <label class="field-label">Email</label>
            <input v-model="form.email" type="email" class="field-input" placeholder="ivan@example.com" autocomplete="email" @keyup.enter="handleRegister" />
          </div>

          <div class="field">
            <label class="field-label">Пароль</label>
            <input v-model="form.password" type="password" class="field-input" placeholder="Ваш пароль" autocomplete="new-password" @keyup.enter="handleRegister" />
          </div>

          <div class="field">
            <label class="field-label">Телефон</label>
            <input v-model="form.phone" type="tel" class="field-input" placeholder="+7 999 000 00 00" @keyup.enter="handleRegister" />
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
            @click="handleRegister"
          >
            <span v-if="!isLoading">Зарегистрироваться</span>
            <span v-else class="btn-spinner"></span>
          </button>
        </div>

        <div class="register-footer">
          <p class="register-hint">Уже есть аккаунт?</p>
          <RouterLink to="/login" class="link-login">Войти</RouterLink>
        </div>
      </template>

    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'
import { RouterLink } from 'vue-router'

const form = reactive({
  firstName: '',
  lastName: '',
  username: '',
  email: '',
  password: '',
  phone: '',
})

const errorMsg = ref(null)
const isLoading = ref(false)
const success = ref(false)
const createdUser = ref(null)

async function handleRegister() {
  if (!form.firstName || !form.lastName || !form.username || !form.email || !form.password) {
    errorMsg.value = 'Пожалуйста, заполните все обязательные поля.'
    return
  }

  isLoading.value = true
  errorMsg.value = null

  try {
    const res = await fetch('https://dummyjson.com/users/add', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        firstName: form.firstName,
        lastName: form.lastName,
        username: form.username,
        email: form.email,
        password: form.password,
        phone: form.phone || '+0 000 000 0000',
      }),
    })

    const data = await res.json()

    if (!res.ok) {
      errorMsg.value = data.message || 'Ошибка при регистрации. Попробуйте ещё раз.'
      return
    }

    createdUser.value = data
    success.value = true
  } catch (e) {
    errorMsg.value = 'Ошибка сети. Попробуйте ещё раз.'
  } finally {
    isLoading.value = false
  }
}
</script>

<style scoped>
.register-page {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: calc(100vh - 80px);
  padding: 40px 24px;
}

.register-card {
  width: 100%;
  max-width: 440px;
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius-lg);
  padding: 36px 32px 28px;
  box-shadow: var(--shadow-lg);
}

.back-link {
  display: inline-block;
  font-size: 13px;
  color: var(--text-secondary);
  margin-bottom: 16px;
  transition: color var(--transition);
}

.back-link:hover {
  color: var(--text-primary);
}

.register-header {
  margin-bottom: 24px;
}

.register-title {
  font-size: 28px;
  font-weight: 600;
  letter-spacing: -0.03em;
  color: var(--text-primary);
  margin-bottom: 6px;
}

.register-sub {
  font-size: 14px;
  color: var(--text-secondary);
}

.form-body {
  display: flex;
  flex-direction: column;
  gap: 14px;
}

.fields-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 12px;
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
  padding: 11px 14px;
  background: var(--surface-2);
  border: 1px solid var(--border);
  border-radius: var(--radius-sm);
  font-family: inherit;
  font-size: 15px;
  color: var(--text-primary);
  outline: none;
  transition: border-color var(--transition), box-shadow var(--transition);
  width: 100%;
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

.register-footer {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  margin-top: 20px;
}

.register-hint {
  font-size: 13px;
  color: var(--text-secondary);
}

.link-login {
  font-size: 13px;
  font-weight: 500;
  color: var(--accent);
  transition: opacity var(--transition);
}

.link-login:hover {
  opacity: 0.75;
}

/* Success state */
.success-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  gap: 14px;
  animation: slideUp 0.35s ease both;
}

.success-icon-wrap {
  width: 56px;
  height: 56px;
  border-radius: 50%;
  background: rgba(52, 199, 89, 0.12);
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 4px;
}

.success-icon {
  font-size: 24px;
  color: #34c759;
}

.success-title {
  font-size: 22px;
  font-weight: 600;
  letter-spacing: -0.025em;
  color: var(--text-primary);
}

.success-desc {
  font-size: 14px;
  color: var(--text-secondary);
  line-height: 1.6;
}

.success-note {
  padding: 12px 14px;
  background: rgba(0, 113, 227, 0.06);
  border: 1px solid rgba(0, 113, 227, 0.15);
  border-radius: var(--radius-sm);
  font-size: 12px;
  color: var(--text-secondary);
  line-height: 1.5;
  text-align: left;
}

.success-actions {
  display: flex;
  flex-direction: column;
  gap: 10px;
  width: 100%;
  margin-top: 4px;
}

.btn-primary {
  display: block;
  width: 100%;
  padding: 13px;
  background: var(--text-primary);
  color: white;
  border-radius: var(--radius-sm);
  font-size: 15px;
  font-weight: 500;
  text-align: center;
  transition: background var(--transition);
}

.btn-primary:hover {
  background: #333;
}

.btn-secondary {
  display: block;
  width: 100%;
  padding: 12px;
  background: transparent;
  color: var(--text-secondary);
  border: 1px solid var(--border);
  border-radius: var(--radius-sm);
  font-size: 15px;
  font-weight: 500;
  text-align: center;
  transition: background var(--transition), color var(--transition);
}

.btn-secondary:hover {
  background: var(--surface-2);
  color: var(--text-primary);
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

@keyframes slideUp {
  from { opacity: 0; transform: translateY(12px); }
  to   { opacity: 1; transform: translateY(0); }
}
</style>
