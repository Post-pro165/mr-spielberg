
Ассистент сценариста **«Мистер Спилберг»** для мессенджера [**MAX**](https://dev.max.ru/docs) и опционально для **Telegram**: грамматика и стиль, логлайны, синопсисы, RAG по вашим PDF/DOCX.

## Переменные окружения (`.env`)

Скопируйте пример: **`cp .env.example .env`** и заполните токены.

### Только MAX (без Telegram)

- **`MAX_BOT_TOKEN`** — токен из кабинета: Чат-боты → Интеграция → Получить токен ([инструкция](https://dev.max.ru/docs/chatbots/bots-coding/prepare)).
- **`OPENAI_API_KEY`** — для чата, RAG и `/write`.
- **`BOT_GATEWAY=max`** — явно только MAX; если **не указан**, при наличии **`MAX_BOT_TOKEN`** выбирается **MAX** (в том числе если в `.env` есть и Telegram-токен — см. ниже).

### Telegram (если нужен)

- **`TELEGRAM_BOT_TOKEN`** — токен BotFather.
- Если в `.env` указаны **и MAX, и Telegram**, а **`BOT_GATEWAY` пустой**, по умолчанию запускается **MAX**. Чтобы поднять **только Telegram**, задайте **`BOT_GATEWAY=telegram`**. Оба шлюза одновременно: **`BOT_GATEWAY=both`**.

### Прочее

- **`MAX_API_BASE`** — по умолчанию `https://platform-api.max.ru`.
- **`MAX_LONG_POLL_TIMEOUT`** — секунды long polling (по умолчанию `30`).
- Опционально: `OPENAI_CHAT_MODEL`, `OPENAI_BASE_URL`, …

Если используете **Telegram** и ловите **Timed out** / **ConnectError** к `api.telegram.org`: VPN или **`TELEGRAM_PROXY_URL`**.

## Виртуальное окружение и зависимости

**Только MAX** (минимум пакетов, без `python-telegram-bot`):

```bash
cd PEm09
python3 -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install -U pip
pip install -r requirements-max.txt
python -m src.main
```

**Telegram и/или голосовые в Telegram** — полный список:

```bash
pip install -r requirements.txt
```

Файлы:

| Файл | Содержимое |
|------|------------|
| **`requirements-max.txt`** | MAX + OpenAI + RAG (PDF/DOCX), **без** Telegram |
| **`requirements.txt`** | всё выше + **python-telegram-bot**, **pydub** (голос в Telegram) |

## Быстрый старт (MAX)

1. В `.env`: `MAX_BOT_TOKEN`, `OPENAI_API_KEY`, при желании `BOT_GATEWAY=max`.
2. `pip install -r requirements-max.txt`
3. `python -m src.main`

Документы для базы знаний — в **`data/documents/`** или **`knowledge/`**, затем в чате с ботом: **`/index`**, режим **`/mode rag`**.

**В шлюзе MAX:** текст, команды, RAG; голос, фото, файлы и **`/draw`** в этом проекте рассчитаны на Telegram ([вложения в MAX](https://dev.max.ru/docs-api/methods/POST/uploads) можно добавить отдельно).

## Структура проекта

```
PEm09/
├── data/
│   ├── documents/       # исходники для RAG (PDF, TXT, MD, DOCX)
│   └── vector_store/    # chunks.json, meta.json после /index
├── docs/
│   ├── ARCHITECTURE.md
│   ├── MAX_GATEWAY.md
│   └── PROJECT_LAYOUT.md
├── knowledge/
├── src/
│   ├── main.py          # точка входа (шлюз MAX и/или Telegram)
│   ├── session_store.py
│   ├── bot.py           # Telegram
│   ├── max_gateway/     # MAX (long polling)
│   ├── config.py
│   ├── handlers/
│   ├── services/
│   ├── utils/
│   └── rag/
├── requirements.txt
├── requirements-max.txt
├── .env.example
└── README.md
```

## Документация

- [Архитектура (блоки)](docs/ARCHITECTURE.md)  
- [Шлюз MAX](docs/MAX_GATEWAY.md)  
- [Папки и файлы](docs/PROJECT_LAYOUT.md)
