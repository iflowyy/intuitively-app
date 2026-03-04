<template>
  <div class="page home-page">
    <div class="home-inner">
      <div class="home-text">
    
        <h1 class="home-title">Простой старт.<br />Интуитивно.</h1>
        <p class="home-desc">
          Войдите в систему, чтобы просмотреть свой профиль. Или изучите список пользователей без авторизации.
        </p>
        <div class="home-actions">
          <RouterLink to="/login" class="btn-primary">
            Войти
          </RouterLink>
          <RouterLink to="/users" class="btn-ghost">
            Пользователи
          </RouterLink>
        </div>
      </div>
      <div class="home-visual">
        <div class="visual-card">
          <svg width="64" height="64" viewBox="0 0 64 64" fill="none" xmlns="http://www.w3.org/2000/svg">
            <rect width="64" height="64" rx="16" fill="#f0f0f5"/>
            <path d="M32 20a8 8 0 1 1 0 16 8 8 0 0 1 0-16z" fill="#1d1d1f"/>
            <path d="M16 48c0-8.837 7.163-12 16-12s16 3.163 16 12" stroke="#1d1d1f" stroke-width="3" stroke-linecap="round"/>
          </svg>
          <div class="card-label">Учётные записи</div>
          <div class="card-count">30 пользователей</div>
        </div>
        <div class="floating-dot dot-1"></div>
        <div class="floating-dot dot-2"></div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { RouterLink } from 'vue-router'
</script>

<style scoped>
.home-page {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: calc(100vh - 80px);
  padding: 40px 24px;
}

.home-inner {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 60px;
  max-width: 860px;
  width: 100%;
}

.home-eyebrow {
  font-size: 12px;
  font-weight: 600;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--accent);
  margin-bottom: 16px;
}

.home-title {
  font-size: clamp(34px, 5vw, 52px);
  font-weight: 600;
  letter-spacing: -0.03em;
  line-height: 1.08;
  color: var(--text-primary);
  margin-bottom: 20px;
}

.home-desc {
  font-size: 17px;
  font-weight: 300;
  color: var(--text-secondary);
  line-height: 1.6;
  max-width: 360px;
  margin-bottom: 36px;
}

.home-actions {
  display: flex;
  align-items: center;
  gap: 12px;
}

.btn-primary {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 13px 26px;
  background: var(--text-primary);
  color: #fff;
  border-radius: var(--radius-pill);
  font-size: 15px;
  font-weight: 500;
  transition: background var(--transition), transform var(--transition), box-shadow var(--transition);
  box-shadow: 0 2px 8px rgba(0,0,0,0.15);
}

.btn-primary:hover {
  background: #333;
  transform: translateY(-1px);
  box-shadow: 0 4px 16px rgba(0,0,0,0.18);
}

.btn-ghost {
  display: inline-flex;
  align-items: center;
  padding: 13px 22px;
  background: transparent;
  color: var(--text-secondary);
  border: 1px solid var(--border);
  border-radius: var(--radius-pill);
  font-size: 15px;
  font-weight: 500;
  transition: background var(--transition), color var(--transition);
}

.btn-ghost:hover {
  background: var(--surface);
  color: var(--text-primary);
}

/* Visual */
.home-visual {
  position: relative;
  flex-shrink: 0;
  width: 260px;
  height: 260px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.visual-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius-lg);
  padding: 36px;
  box-shadow: var(--shadow-lg);
  animation: float 4s ease-in-out infinite;
}

.card-label {
  font-size: 14px;
  font-weight: 500;
  color: var(--text-primary);
  letter-spacing: -0.01em;
}

.card-count {
  font-size: 12px;
  color: var(--text-tertiary);
}

.floating-dot {
  position: absolute;
  border-radius: 50%;
  background: var(--accent);
  opacity: 0.15;
}

.dot-1 {
  width: 48px;
  height: 48px;
  top: 10px;
  right: 10px;
  animation: float 5s ease-in-out infinite reverse;
}

.dot-2 {
  width: 24px;
  height: 24px;
  bottom: 20px;
  left: 20px;
  animation: float 3.5s ease-in-out infinite;
}

@keyframes float {
  0%, 100% { transform: translateY(0px); }
  50% { transform: translateY(-10px); }
}

@media (max-width: 640px) {
  .home-inner {
    flex-direction: column;
    text-align: center;
    gap: 40px;
  }
  .home-desc, .home-actions {
    margin-left: auto;
    margin-right: auto;
  }
  .home-actions {
    justify-content: center;
  }
  .home-visual {
    width: 200px;
    height: 200px;
  }
}
</style>
