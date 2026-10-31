# telegram-ai-bot

Bot Telegram berbasis AI yang berjalan di Google Apps Script.

Bot ini bernama **Rensheen**, dibuat untuk pemilik **Ren**, dengan zona waktu **Asia/Jakarta**. Bot menjawab singkat dalam bahasa Indonesia yang santai tapi sopan, langsung ke inti, dan memakai alat (tools) bila diperlukan, bukan menebak informasi yang bisa dicek.

## Fitur

- Menjawab chat pribadi lewat AI (agent)
- Webhook Telegram (`doGet` / `doPost`)
- Pencarian web untuk info terbaru
- Catatan: simpan, lihat, dan hapus catatan
- Pengingat: atur, lihat, batalkan, dan cek pengingat secara berkala
- Interaksi di grup, termasuk mengingat anggota grup
- Alat Gmail dan mode analis, sesuai konfigurasi di kode
- Pengiriman pesan dan aksi chat Telegram

## Struktur

- `Kode.gs` — satu-satunya file kode di proyek Apps Script ini
- 31 fungsi, antara lain: `systemPrompt`, `doGet`, `doPost`, `runAgent`, `callGroqOnce`, `runTool`, `webSearch`, `getStore`, `setStore`, `saveNote`, `listNotes`, `deleteNote`, `setReminder`, `listReminders`, `cancelReminder`, `checkReminders`, `ensureReminderTrigger`, `setupTriggers`, `sendTelegram`, `checkTelegramWebhook`, `runGmailTool`, `runGroupAgent`, dan `sendGroupReply`
- Model yang dipakai di kode: `openai/gpt-oss-20b`

## Cara menjalankan

1. Buat proyek baru di Google Apps Script, lalu salin isi `Kode.gs` ke dalamnya.
2. Isi Script Properties di proyek Apps Script kamu (Project Settings > Script Properties) dengan kunci berikut. **Jangan pernah membagikan nilai kunci ini, dan jangan commit nilai tersebut ke repo.**
   - `TELEGRAM_TOKEN` — token bot Telegram
   - `GROQ_API_KEY` — API key untuk model AI
   - `WEBHOOK_SECRET` — rahasia untuk verifikasi webhook, dipakai sebagai parameter `k` di URL webhook
   - `ALLOWED_CHAT_ID` — ID chat pribadi yang diizinkan
3. Deploy sebagai Web App, lalu atur webhook Telegram ke URL Web App tersebut dengan parameter rahasia webhook.
4. Atur trigger pengingat sesuai kebutuhan, misalnya lewat fungsi trigger yang tersedia di kode.

## Keamanan

- Tidak ada API key, token, kata sandi, atau data rahasia lain yang disertakan di repo ini.
- Semua kredensial hanya dibaca dari Script Properties, bukan ditulis langsung di kode.
- Untuk menjalankan bot ini, isi sendiri Script Properties di atas di proyek Apps Script kamu.

## Catatan

Proyek ini awalnya dibuat di Google Apps Script dan dipublikasikan ke GitHub dalam bentuk yang sudah dibersihkan dari rahasia.
