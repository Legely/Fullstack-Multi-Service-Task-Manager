# Fullstack Multi-Service Task Manager 📋

Комплексное веб-приложение для управления задачами, построенное по модульной сервисной архитектуре с разделением ответственности между сервисом задач, сервисом аутентификации и SPA-клиентом.

[![Java](https://img.shields.io/badge/Java-21-orange?style=flat-square&logo=openjdk)](https://openjdk.org/)
[![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.5-green?style=flat-square&logo=springboot)](https://spring.io/projects/spring-boot)
[![Flyway](https://img.shields.io/badge/Flyway-Migrations-red?style=flat-square&logo=flyway)](https://flywaydb.org/)
[![Node.js](https://img.shields.io/badge/Node.js-Express-darkgreen?style=flat-square&logo=nodedotjs)](https://nodejs.org/)
[![SQLite](https://img.shields.io/badge/SQLite-Database-blue?style=flat-square&logo=sqlite)](https://www.sqlite.org/)

---

## 🏛 Архитектура проекта

Приложение разделено на три изолированных компонента:

```
                      ┌──────────────────────┐
                      │  Frontend (SPA Web)  │
                      │  HTML5 / CSS3 / JS   │
                      └──────────┬───────────┘
                                 │
                 ┌───────────────┴───────────────┐
                 ▼ (REST API)                    ▼ (REST API)
      ┌──────────────────────┐        ┌──────────────────────┐
      │     Task Service     │        │     Auth Service     │
      │  Java 21/Spring Boot │        │   Node.js / Express  │
      └──────────┬───────────┘        └──────────┬───────────┘
                 │ (Flyway Migrations)           │
                 ▼                               ▼
         [SQLite Task DB]                 [SQLite Auth DB]
```

---

## 🚀 Компоненты системы

### 1. ☕ Task Service (`/task-service`)
Основной бизнес-сервис управления задачами.
* **Стек:** Java 21, Spring Boot 3, Spring Data JPA, SQLite JDBC, Flyway, Lombok.
* **Миграции БД:** Использование **Flyway** для версионирования схемы базы данных:
  * `V1__init_database.sql` — создание базовой таблицы задач.
  * `V2__add_username.sql` — инкрементальная миграция для привязки задач к пользователю.
* **Возможности:** Полный CRUD задач (создание, получение списка, фильтрация по владельцу, обновление статуса/приоритета/дедлайна, удаление).

### 2. 🔐 Auth Service (`/auth-service`)
Изолированный сервис управления учетными записями.
* **Стек:** Node.js, Express, SQLite3, Bcrypt, CORS.
* **Возможности:** Регистрация, проверка уникальности логина, безопасная валидация паролей с солением и хэшированием.

### 3. 💻 Frontend (`/frontend`)
* **Стек:** Vanilla JavaScript (Fetch API), HTML5, CSS3.
* **Возможности:** Динамический интерфейс без перезагрузки страниц, модальные окна, фильтрация и валидация ввода.

---

## 📡 REST API (Task Service)

Базовый путь: `/api/tasks`

| Метод | Эндпоинт | Описание |
| :--- | :--- | :--- |
| `GET` | `/api/tasks` | Получить список всех задач |
| `GET` | `/api/tasks/user/{username}` | Получить задачи конкретного пользователя |
| `POST` | `/api/tasks` | Создать новую задачу (по умолчанию статус `TODO`) |
| `PUT` | `/api/tasks/{id}` | Полное обновление задачи (название, дедлайн, приоритет, статус) |
| `DELETE` | `/api/tasks/{id}` | Удалить задачу по идентификатору |

---

## ⚙️ Локальный запуск системы

### 1. Запуск Auth Service
```bash
cd auth-service
npm install
node server.js
# Сервер авторизации стартует на порту 3000
```

### 2. Запуск Task Service
```bash
cd task-service
./mvnw clean package
./mvnw spring-boot:run
# Сервис задач стартует на порту 8080 (Flyway автоматически применит миграции)
```

### 3. Запуск Frontend
Откройте файл `frontend/index.html` в любом современном браузере или запустите через Live Server.
