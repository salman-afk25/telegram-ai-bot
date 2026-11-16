# Telegram AI Bot

An agentic AI assistant for Telegram, built as a single Google Apps Script that talks to Groq's OpenAI-compatible chat completions API with function (tool) calling.

**My role: designed and built end to end.**

## Features

- Telegram bot wired to Google Apps Script through a Web App webhook (`doGet` / `doPost`)
- Agentic tool-calling loop: the model chooses tools, the script runs them, then continues until a final answer
- Built-in tools: `web_search`, `save_note`, `list_notes`, `delete_note`, `set_reminder`, `list_reminders`, `cancel_reminder`, `gmail_search` and `gmail_get`
- Browser-search tool for current information, plus saved notes, scheduled reminders and read-only Gmail lookup
- Mode switching (`/analis`, `/futures`, `/normal`) with its own system prompt and tool set per mode
- Time-based trigger checks due reminders every minute and pushes them to Telegram
- Chat history and mode stored per chat; duplicate webhook deliveries ignored and concurrent edits guarded by locks
- Access restricted to allow-listed Telegram chat IDs, with rate-limit retries and model fallbacks

## Tech stack

- **Google Apps Script** (JavaScript, `UrlFetchApp`, `Utilities`)
- **Groq API** - OpenAI-compatible `POST /openai/v1/chat/completions`
- **Models:** `openai/gpt-oss-20b` (chat) and `openai/gpt-oss-120b` (browser search)
- **OpenAI-style tool / function calling** (`tools`, `tool_calls`)
- **Telegram Bot API** (`sendMessage`, `sendChatAction`, webhook / `getWebhookInfo`)
- **Gmail** read-only search and get through the Apps Script tool router
- Apps Script services: `PropertiesService`, `CacheService`, `ContentService`, `LockService`, `ScriptApp` triggers

## How it works

1. Telegram delivers an update to the Apps Script web app; the secret parameter and chat ID allow-list are checked.
2. `runAgent` sends the system prompt, mode instructions and cached history to Groq with the tool definitions.
3. If the model requests tools, `runTool` runs web search, notes, reminders or Gmail and returns the output.
4. The loop repeats until the model returns a final answer, which is posted back to the chat.

## Setup

1. Create a Google Apps Script project and paste `Kode.gs` into it.
2. Fill Project Settings > Script Properties: `TELEGRAM_TOKEN`, `GROQ_API_KEY`, `WEBHOOK_SECRET` and `ALLOWED_CHAT_ID`.
3. Deploy as a Web App and point the Telegram webhook to that URL, including the `?k=` secret parameter.



## Security

- No API keys, tokens, passwords or other secrets are stored in this repository.
- All credentials are read from Script Properties only, never hard-coded in the code.
- Only allow-listed chat IDs are served, and system prompts are kept separate from user input.
