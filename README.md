# Vue Auth App

Apple-минимализм. Vue 3 + Vue Router + DummyJSON API.

---

## 🚀 Запуск

```bash
# 1. Установить зависимости
npm install

# 2. Запустить dev-сервер
npm run dev
```

Открой http://localhost:5173 в браузере.

---

## 📦 Сборка для продакшена

```bash
npm run build
npm run preview
```

---

## 📋 Структура

```
src/
├── main.js              # Точка входа
├── App.vue              # Root + layout + page transitions
├── assets/
│   └── global.css       # CSS переменные, reset, шрифты
├── router/
│   └── index.js         # Маршруты + guard для /profile
├── components/
│   └── NavBar.vue       # Pill-навбар (Home / Users / Profile)
└── views/
    ├── HomeView.vue     # /home — главная страница
    ├── UsersView.vue    # /users — список пользователей
    ├── LoginView.vue    # /login — форма авторизации
    └── ProfileView.vue  # /profile — приватная, требует токен
```

---

## 🔐 Логика авторизации

1. `/login` — POST на `https://dummyjson.com/auth/login`
2. Успешный ответ → токен сохраняется в `localStorage` под ключом `token`
3. `/profile` — защищённый маршрут, проверяет наличие токена перед входом
4. На странице Profile — GET на `https://dummyjson.com/auth/me` с заголовком `Authorization: Bearer <token>`
5. Sign out — удаляет токен и редиректит на `/login`

**Тестовый аккаунт:**  
Username: `emilys`  
Password: `emilyspass`

Полный список 30 пользователей: https://dummyjson.com/users

---

## 📤 Загрузка на GitHub

```bash
git init
git add .
git commit -m "init: vue auth app"
git branch -M main
git remote add origin https://github.com/ВАШ_USERNAME/ВАШ_REPO.git
git push -u origin main
```

---

## ✅ Требования задания

| Требование | Статус |
|---|---|
| Home page с описанием, картинкой и кнопкой на /login | ✅ |
| Users — список пользователей с ФИО и email | ✅ |
| Login — форма, POST /auth/login, ошибки в форме | ✅ |
| Profile — приватная, /auth/me, токен в заголовке | ✅ |
| Guard: нет токена → редирект на /login | ✅ |
| Токен сохраняется в localStorage под ключом `token` | ✅ |
| Ошибка токена → текст ошибки в заголовке Profile | ✅ |
| Vue Router с историей | ✅ |
