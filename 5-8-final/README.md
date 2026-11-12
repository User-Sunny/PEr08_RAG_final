# RAG Ассистент по документации (OpenAI & GigaChat)

Финальный проект в рамках модуля. Представляет собой полноценного RAG (Retrieval-Augmented Generation) ассистента, способного отвечать на вопросы по базе знаний (документации). Проект поддерживает работу через **OpenAI API** и **GigaChat API** (Сбер), имеет систему кеширования и скрипт автоматической оценки качества (RAGAS).

## 🚀 Основные возможности

*   **Два режима работы:** 
    *   `assistant_api` — генерация через OpenAI (`gpt-4o-mini`), эмбеддинги OpenAI.
    *   `assistant_giga` — генерация через GigaChat, локальные эмбеддинги (`sentence-transformers`).
*   **Умный Chunking:** Разбиение текста на чанки с перекрытием (overlap) и учетом границ предложений.
*   **Векторный поиск:** Использование локальной базы данных **ChromaDB** для семантического поиска.
*   **Кеширование:** Сохранение пар "вопрос-ответ" в **SQLite**. Если задать один и тот же вопрос, ответ вернется мгновенно без обращения к API.
*   **Оценка качества:** Встроенный скрипт для оценки метрик `Faithfulness` и `Context Precision` с помощью фреймворка **RAGAS**.

## 📂 Структура проекта

```text
PEr08_RAG_final/
├── 5-8-final/
│   ├── assistant_api/       # Версия для OpenAI
│   │   ├── data/docs.txt    # База знаний
│   │   ├── app.py           # Консольное приложение
│   │   ├── cache.py         # Модуль кеширования (SQLite)
│   │   ├── evaluate_ragas.py# Скрипт оценки RAGAS
│   │   ├── rag_pipeline.py  # Основной пайплайн RAG
│   │   └── vector_store.py  # Работа с ChromaDB
│   ├── assistant_giga/      # Версия для GigaChat
│   │   └── ... (аналогичная структура + gigachat_client.py)
│   ├── .env.example         # Пример переменных окружения
│   └── requirements.txt     # Зависимости
├── .gitignore
└── README.md

# RAG Ассистент по документации (OpenAI & GigaChat)

Финальный проект RAG (Retrieval-Augmented Generation) ассистента, способного отвечать на вопросы по базе знаний. Проект поддерживает работу через **OpenAI API** и **GigaChat API** (Сбер), имеет систему кеширования и скрипт автоматической оценки качества (RAGAS).

## 🛠 Установка и настройка

**Требования:** Python 3.11 (рекомендуется).

1. **Клонируйте репозиторий и создайте виртуальное окружение:**
   ```bash
   python3.11 -m venv venv_py311
   source venv_py311/bin/activate  # Для macOS/Linux
   # venv_py311\Scripts\activate   # Для Windows

   Установите зависимости:

bash
pip install -r 5-8-final/requirements.txt
(В requirements.txt зафиксированы версии ragas, numpy и langchain-community во избежание конфликтов совместимости).
Настройте переменные окружения:
Создайте файл .env в папке 5-8-final/ на основе .env.example и добавьте свои ключи:

env
# Для assistant_api
OPENAI_API_KEY=sk-...

# Для assistant_giga
GIGACHAT_AUTH_KEY=ваш_ключ
GIGACHAT_RQUID=ваш_uuid
🎮 Запуск приложения

Перейдите в папку нужной версии и запустите приложение:

Для OpenAI:

bash
cd 5-8-final/assistant_api
python app.py
Для GigaChat:

bash
cd 5-8-final/assistant_giga
python app.py
При первом запуске приложение проиндексирует файл data/docs.txt (разобьет на чанки и создаст векторы в ChromaDB). В консоли появится приглашение 💭 Ваш вопрос:.
Для выхода введите exit, для просмотра статистики кеша — stats.

📊 Оценка качества (RAGAS)

Проект включает скрипт для автоматической оценки качества RAG-системы (на примере OpenAI версии).

Запуск:

bash
cd 5-8-final/assistant_api
python evaluate_ragas.py

⚠️ Примечания

В версии для GigaChat используется локальная модель эмбеддингов (intfloat/multilingual-e5-small), так как базовый тариф GigaChat API не предоставляет доступ к эндпоинту /embeddings (возвращает ошибку 402 Payment Required).
Для работы скрипта оценки (evaluate_ragas.py) требуется действующий баланс OpenAI API.