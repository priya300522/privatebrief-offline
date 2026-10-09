# PrivateBrief

PrivateBrief is an offline-first chat catch-up assistant. It parses supported chat exports and performs transparent, rule-based analysis in the browser. Conversation data is not sent to a server.

## Run locally

1. Open this folder in VS Code.
2. Start a local static web server from this folder:

   ```powershell
   py -m http.server 8000
   ```

   If the Python launcher is unavailable, try `python -m http.server 8000`.
3. Open <http://localhost:8000> in a browser.
4. Use **Try sample chats** or **Import a chat**.

The page also works when opened directly as `index.html`; service-worker installation and installable/offline caching require `localhost` or HTTPS. After the first successful load from the local server, the service worker caches the application shell for offline use.

## Features

- WhatsApp text, Telegram JSON, Slack JSON, and sender-prefixed plain-text imports.
- Local parsing, priority scoring, mention/task/decision/deadline detection, and evidence explanations.
- Brief, Attention, Decisions, Deadlines, Conversation, People & Topics, task-management, radar, and alert views.
- Search and priority/status filters; mark extracted items done, not important, or reopen them.
- Suggested replies with clipboard copy, original-message navigation, and saved task state.
- IndexedDB storage with an in-memory session fallback.
- JSON backup export and restore, per-chat removal, and confirmed delete-all.
- Aurora, Midnight, and Daylight themes; text blur; reduced-motion styling.
- Offline status, request counter, network lockdown, and offline self-test.
- Local service worker and web app manifest; no CDN assets or external dependencies.

## Import examples

**WhatsApp**

```text
09/10/26, 9:41 am - Alex: @Riya can you send the slides by tomorrow?
09/10/26, 9:42 am - Riya: I will send them tonight.
```

**Plain text**

```text
Alex: Can you review the draft by Friday?
Riya: Yes, I will take a look.
```

Telegram and Slack imports should be JSON exports containing a `messages` array (Telegram) or an array of Slack message objects.

## Limitations

- Analysis is a deterministic keyword/rule system, not an AI model. Date formats and inferred intent can be ambiguous; review evidence and original messages.
- The optional Ollama integration requires a separately installed local model and a locally running Ollama server. It is disabled by default; selecting it sends prompts only to `localhost`.
- The service worker caches the application shell, not imported chats. Chats are stored separately in browser IndexedDB and are included in backups only when explicitly exported.
- The basic static server command is for local development, not production hosting.
