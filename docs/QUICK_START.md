# ⚡ Быстрый старт — Cyber BDI Simulator

От нуля до работающей симуляции за 5 минут.

---

## Шаг 1 — Получить API-ключ

Зарегистрируйтесь на [console.groq.com](https://console.groq.com) и создайте API-ключ.  
Groq даёт бесплатный лимит — этого достаточно для разработки.

> **Альтернатива:** можно использовать [openrouter.ai](https://openrouter.ai) — задайте `OPENROUTER_API_KEY` вместо `GROQ_API_KEY`.

---

## Шаг 2 — Установить зависимости

```bash
# Клонируем проект
git clone https://github.com/Nek1tt/AI-agent-autonomous-life.git
cd AI-agent-autonomous-life

# Создаём виртуальное окружение (рекомендуется)
python -m venv venv
source venv/bin/activate       # macOS / Linux
# venv\Scripts\activate        # Windows

# Устанавливаем пакеты
pip install -r requirements.txt
```

---

## Шаг 3 — Настроить `.env`

Создайте файл `.env` в корне проекта:

```bash
# .env
GROQ_API_KEY=gsk_ваш_ключ_здесь
```

> Если оба ключа отсутствуют, симулятор запустится в **MOCK-режиме** — агенты будут отвечать заготовленными фразами без LLM.

---

## Шаг 4 — Запустить сервер

```bash
cd backend
python main.py 
ИЛИ
uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

Вы должны увидеть в консоли:

```
🚀 Initializing LLM via Groq...
--- Agent Алекса added to simulator ---
--- Agent Нексус added to simulator ---
🚀 Simulation loop started...
INFO:     Uvicorn running on http://0.0.0.0:8000
```

---

## Шаг 5 — Открыть интерфейс

Перейдите в браузере на [http://localhost:8000](http://localhost:8000).

Вы увидите дашборд с двумя агентами — **Алекса** (экстраверт) и **Нексус** (интроверт).

### Первый разговор с агентом

В разделе **Chat** выберите агента и напишите сообщение.

### Добавить мировое событие

В разделе **Events** введите описание события — например, `«Пожар в северном квартале»`. Все агенты получат это восприятие и отреагируют эмоционально.

---

## 🔧 Troubleshooting

### ❌ `ModuleNotFoundError: No module named 'fastapi'`

Вы не активировали виртуальное окружение или забыли установить зависимости.

```bash
source venv/bin/activate
pip install fastapi "uvicorn[standard]" pydantic openai python-dotenv
```

---

### ❌ `⚠️ WARNING: No API Key found in .env`

Файл `.env` не создан или ключ указан неверно. Проверьте:

```bash
cat .env
# Должно быть: GROQ_API_KEY=gsk_...
```

Убедитесь, что файл `.env` находится **в том же каталоге**, что и `main.py`.

---

### ❌ `Address already in use` / порт 8000 занят

```bash
# Найти и завершить процесс на порту 8000
lsof -i :8000 | grep LISTEN
kill -9 <PID>

# Или запустить на другом порту
uvicorn main:app --port 8001
```

---

### ❌ Агенты не отвечают / `🔴 LLM Error`

Проверьте квоту Groq API на [console.groq.com](https://console.groq.com). При превышении лимита переключитесь на OpenRouter:

```bash
# .env
OPENROUTER_API_KEY=sk-or-ваш_ключ
```

---

### ❌ WebSocket не подключается

Если вы открываете интерфейс не через `localhost` (например, с удалённой машины), убедитесь, что порт 8000 доступен и CORS настроен корректно. В режиме разработки CORS открыт для всех (`allow_origins=["*"]`).

---

### ❌ `ImportError` при запуске — не найден модуль `core.bdi`

Проект использует относительные импорты из пакета `core/bdi`. Убедитесь, что запускаете `uvicorn` именно из корневого каталога проекта, где находится `main.py`.

```bash
# Правильно:
cd /path/to/cyber-bdi-simulator
uvicorn main:app --reload

# Неправильно:
cd /path/to/cyber-bdi-simulator/backend
uvicorn main:app --reload
```
