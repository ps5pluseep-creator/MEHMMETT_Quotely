# MEHMMETT_Quotely Bot 💬✨

A Telegram bot that turns chat messages into beautiful quote stickers.

[![time tracker](https://wakatime.com/badge/github/MadebyMehmmeTT/MEHMMETT_Quotely.svg)](https://wakatime.com/badge/github/MadebyMehmmeTT/MEHMMETT_Quotely)

v2 is a from-scratch rewrite in **TypeScript + [grammY](https://grammy.dev)**: one long-polling process, strict types, no cluster sharding, no Redis.

## Features 🚀

- **Quote stickers** — WebP stickers, PNG/image, or document output
- **Multi-message quotes** — `/q 3` grabs a range via a self-hosted Bot API server
- **Customizable** — background colors, emoji brands, scaling, crop, media
- **Privacy** — per-user and per-group anonymization
- **Ratings** — rate buttons, `/qtop`, `/qrand`, auto-gab throwbacks
- **Guest Mode** (Bot API 10.0+) — mention the bot in any chat *without adding it as a member*: reply to a message + `@MEHMMETT_Quotely_bot` and a quote sticker lands right in the chat
- **Inline mode, Mini App, Telegram Stars payments**

## Installation 📦

### Prerequisites

- Node.js 22+
- MongoDB
- [quote-api](https://github.com/MadebyMehmmeTT/quote-api) (the image renderer)
- Optional: a self-hosted [Bot API server](https://github.com/MadebyMehmmeTT/telegram-bot-api) for multi-message quotes and premium emoji statuses

### Quick start

```bash
git clone https://github.com/MadebyMehmmeTT/MEHMMETT_Quotely.git
cd MEHMMETT_Quotely
npm install
cp .env.example .env   # fill in BOT_TOKEN, MONGODB_URI, QUOTE_API_URI
npm run dev            # tsx watch
```

Production:

```bash
npm run build
npm start
```

### Docker 🐳

```bash
docker build -t MEHMMETT_Quotely .
docker run --env-file .env MEHMMETT_Quotely
```

## Configuration ⚙️

Validated on startup by `src/config/env.ts` — the process exits with a readable report if anything is missing.

| Variable | Description | Required |
|----------|-------------|----------|
| `BOT_TOKEN` | Bot token from @BotFather | Yes |
| `MONGODB_URI` | MongoDB connection string | Yes |
| `QUOTE_API_URI` | quote-api endpoint | Yes |
| `BOT_API_ROOT` | Self-hosted Bot API server root (Telegram cloud by default) | No |
| `HEALTH_PORT` | Health + Prometheus `/metrics` port (default 3000) | No |
| `BOT_CONCURRENCY` | Max concurrent updates across chats (default 500) | No |
| `GRAMADS_TOKEN` | gramads ads (ru-locale PMs only) | No |
| `ADMIN_ID` | Owner id for privileged commands (`/refund`) | No |

See `.env.example` for the full list.

## Usage 📖

### Basic commands

- `/q` — quote the replied message
- `/q <number>` — quote a range of messages (needs `BOT_API_ROOT`)
- `/q_<id>` — recall a saved quote
- `/qrand` — random top quote
- `/qtop` — top-rated quotes
- `/help` — help

### Quote flags

- `/q p` — PNG image · `/q i` — image file
- `/q r` — include the reply context block
- `/q rate` — attach rating buttons
- `/q <color>` — background color
- `/q s1.5` — scale factor · `/q c` — crop · `/q m` — keep media

### Settings

- `/qcolor <color>` — default background
- `/qemoji` — emoji brand
- `/qrate` — rating system on/off
- `/qgab` — auto-gab frequency
- `/privacy`, `/hidden` — privacy toggles
- `/lang` — language (19 locales)

## Architecture 🏗️

Single process; `src/index.ts` is the composition root.

Update flow: fast-path noise filter → per-chat sequentialize → context →
i18n (Fluent) → features. Outgoing calls go through a throttler + auto-retry.

```
src/
├── config/        # zod-validated env
├── core/          # bot stack, runner, logger, metrics, shutdown
├── db/            # Mongoose models + atomic repositories
├── services/      # bot-api, quote-api, sticker, stats, gab, gramads
├── middlewares/   # fast-path, context, guards
├── features/      # quote, settings, shell, payments, inline, fstik
├── i18n/          # Fluent locales
└── health/        # health + /metrics endpoint
```

## Development 👨‍💻

```bash
npm run dev        # watch mode
npm run typecheck  # tsc --strict — the quality gate
npm test           # vitest
```


## Acknowledgments 🙏

- [grammY](https://grammy.dev/) — the Telegram bot framework
- [telegram-bot-api](https://github.com/tdlib/telegram-bot-api) — the Bot API server ([our fork](https://github.com/MadebyMehmmeTT/telegram-bot-api) adds the custom methods)

---

Made with ❤️ by [MEHMMETT](https://github.com/MadebyMehmmeTT)
