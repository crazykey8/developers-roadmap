Отлично! Давай свяжем **Next.js** и **Docker** пошагово – с нуля до запуска в браузере. Предположим, у тебя уже есть проект Next.js (если нет — я покажу как его создать).

---

## ✅ Этап 0: (если нужно) Создай проект Next.js

```bash
npx create-next-app@latest my-next-app
cd my-next-app
```

---

## 🧱 Этап 1: Создай `Dockerfile` в корне проекта

```Dockerfile
# 1. Базовый образ Node.js
FROM node:18-alpine

# 2. Рабочая директория в контейнере
WORKDIR /app

# 3. Копируем package.json и package-lock.json
COPY package*.json ./

# 4. Установка зависимостей
RUN npm install

# 5. Копируем все файлы проекта
COPY . .

# 6. Собираем Next.js проект
RUN npm run build

# 7. Указываем порт (Next.js по умолчанию на 3000)
EXPOSE 3000

# 8. Запускаем Next.js в продакшн режиме
CMD ["npm", "start"]
```

---

## 🧩 Этап 2: Убедись, что `package.json` содержит нужные скрипты

```json
"scripts": {
  "dev": "next dev",
  "build": "next build",
  "start": "next start"
}
```

---

## ⚙️ Этап 3: (Опционально) Создай `.dockerignore`

```plaintext
node_modules
.next
Dockerfile
docker-compose.yml
*.log
```

---

## 🛠 Этап 4: Собери и запусти контейнер

### Вариант A: Через Docker CLI

```bash
# Сборка образа
docker build -t my-next-app .

# Запуск контейнера
docker run -p 3000:3000 my-next-app
```

---

### Вариант B: Через docker-compose (проще для разработки)

#### Создай `docker-compose.yml`

```yaml
version: "3.9"

services:
  web:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
    volumes:
      - .:/app
      - /app/node_modules
```

#### Запуск

```bash
docker-compose up --build
```

---

## 🌐 Этап 5: Открой в браузере

Перейди на [http://localhost:3000](http://localhost:3000)

---

## 💬 Хочешь dev-режим?

В Dockerfile замени `CMD` на:

```Dockerfile
CMD ["npm", "run", "dev"]
```

И не забудь пробросить порт и volume, чтобы изменения в коде сразу применялись.

---

Если хочешь, я могу создать тебе готовые файлы `Dockerfile`, `docker-compose.yml`, `.dockerignore`, или объяснить, как это всё развернуть на сервере.
