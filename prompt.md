# Product Build Prompt: PrivateBrief

## Your role

Act as a senior full-stack web engineer, product designer, accessibility advocate, and pragmatic test engineer. Build, run, and verify **PrivateBrief — Offline Chat Catch-up Assistant**, a complete browser-based product for turning exported group conversations into a concise, personal catch-up brief.

Do not stop after a plan, mockup, or static screen. Inspect the existing workspace first, then implement the product in its existing structure. Preserve useful existing code and user data; make precise changes, avoid unrelated rewrites, and do not overwrite a more complete working implementation with a scaffold.

## Product promise

Help a user catch up on a conversation quickly by surfacing:

- Messages that directly mention them or appear to request an action.
- Decisions, changes, and possible contradictions.
- Dates, deadlines, overdue items, and upcoming work.
- Who participated, repeated keywords, and messages that may be safe to skim past.

**Privacy and honesty are core product requirements.** The default experience must parse and analyze chats locally in the browser, without accounts, a backend, analytics, or external AI services. Clearly label deterministic analysis as rule-based; do not claim human-level or AI semantic understanding.

## Implementation constraints

- Use HTML5, CSS, and vanilla JavaScript. Do not add a framework, build step, dependency, or package unless the existing project already uses one or there is a compelling, explained need.
- Persist saved conversations and their task states in IndexedDB. If browser storage is unavailable, keep the app usable with an in-memory fallback and communicate that data will not survive closing the tab.
- Keep the app functional when served locally and offline after its app shell has been cached.
- Use a service worker and web app manifest for installable offline support where supported. Explain that service workers require `localhost` or HTTPS.
- Do not load third-party scripts, remote fonts, trackers, analytics, or remote images.
- Never send imported chat content to a remote server. Any optional language-model integration must be disabled by default, explicitly opt-in, target a user-controlled local endpoint only, disclose that choice before sending content, and fail clearly without breaking built-in analysis.
- Keep code modular, readable, and appropriately commented. Prefer small, meaningful files over one giant script, but do not split files without benefit.
- Do not expose secrets, log raw chat contents to the console, or insert imported text as executable HTML.

## Product experience and visual design

Create a polished, premium, privacy-focused dashboard:

- Dark Aurora styling by default, using deep purple with cyan, violet, and pink accents; restrained gradients, translucent/glass panels, clear borders, and subtle motion.
- Three selectable themes: **Aurora**, **Midnight**, and **Daylight**. Persist the preference locally.
- Responsive conversation navigation and content layout for mobile, tablet, and desktop. On narrow screens, navigation must remain usable without horizontal page overflow.
- Readable typography, consistent spacing, clear hierarchy, accessible contrast, visible keyboard focus, semantic controls, and descriptive accessible names.
- Use inline SVG or CSS icons; do not depend on external assets.
- Respect `prefers-reduced-motion`; do not make essential meaning depend on animation or color alone.
- Include clear loading, validation-error, empty, success, and failure feedback. Every visible control must work and provide a useful outcome.

## Main shell and navigation

Provide:

- Product identity and a short explanation of what stays local.
- A clear **Import a chat** action available from the empty state and normal dashboard.
- A list of saved conversations, with the selected conversation clearly indicated and useful status information.
- A truthful connection/offline indicator and a count of application-controlled network requests.
- Privacy information and controls for evidence display and optional sensitive-text blur.
- A helpful empty state with a way to try representative sample chats.

For a selected conversation, provide these distinct data-backed views:

1. **Brief** — the catch-up summary, priority counts, first actions, notable changes, and upcoming deadlines.
2. **Attention** — messages likely to require the user's response or action.
3. **Decisions** — detected decisions, changed plans, and possible conflicts, with links to both source messages.
4. **Deadlines** — overdue, due soon, later, and completed deadline-related items.
5. **Conversation** — searchable original messages with safe highlighting of mentions, dates, and detected items.
6. **People & Topics** — participant activity and repeated keywords, labelled as simple counts/keywords rather than semantic conclusions.
7. A task-management view may be included in addition to those six views.

Navigation must preserve relevant filters when practical. Clicking a dashboard count must open the matching view and filter, not merely switch to a generic page.

## Import and parsing

Support:

- WhatsApp exported `.txt` conversations, including common timestamp/sender formats.
- Telegram JSON exports with a `messages` array.
- Slack JSON exports containing message arrays.
- Plain text with sender-prefixed lines such as `Name: message`.
- Pasted text, file selection, and drag-and-drop where supported.

The import dialog must include:

- Conversation name.
- User's display name as it appears in the chat; explain that it is used for direct-mention matching.
- File selection/drop area and paste field.
- An explicit **Save on this device** choice.
- Analyze and Cancel controls, plus actionable validation feedback.

Parse a conversation into safe structured message records with a stable per-conversation identity, sender (or `Unknown`), original timestamp/date/time where available, content, and message order. Support multiline messages and common exported JSON rich-text shapes. Missing or malformed dates, unknown senders, system messages, and malformed records must not crash import.

Validate file type/size and input before analysis. Handle large files gracefully; if a practical size limit is used, disclose it and explain how to proceed. Reject empty or unreadable input with a clear error. Avoid duplicate imports by identifying matching source content; on duplicate, inform the user and do not create a second conversation. Never silently replace an existing conversation.

## Rule-based analysis

Implement local, deterministic analysis with understandable, inspectable rules. At minimum detect:

- Direct mentions based on the user's configured name and common `@name` forms.
- Questions directed to the user and messages that appear to await a reply.
- Requests, assignments, group-wide asks, and urgent wording.
- Deadline wording, including explicit dates/times, today, tomorrow, weekdays, and common phrases such as `by`, `before`, `until`, `due`, and end-of-day.
- Decisions, agreements, changes/rescheduling/cancellations, and possible conflicts between related messages.
- Participant message counts, repeated keyword topics, total message count, mention counts, and likely low-priority small talk.

Use an explicit, documented scoring scheme to assign **High**, **Medium**, or **Low** priority. Show the exact reason(s) and, when evidence mode is enabled, message number and score. Distinguish:

- **Observed evidence**: quoted source message, sender, and timestamp if present.
- **Rule-based interpretation**: inferred request, urgency, deadline, decision, or conflict.

Never state an inferred deadline, reply obligation, decision, or conflict as certain fact. Unknown or ambiguous dates must be marked as unresolved instead of silently guessed. Use the device clock by default; if another reference date is supported, label it clearly.

## Task controls and search

For extracted items, implement:

- Mark **Done** and **Not important**; reopen either status.
- Filter by status and priority; search sender and message text.
- A suggested reply for suitable items, displayed as an editable or clearly copyable draft. Never send a reply automatically.
- Copy-to-clipboard with success/error feedback.
- Jump to and highlight the original conversation message.
- Evidence text that explains each classification.

Persist status per conversation in local storage/IndexedDB. Updating a status must immediately update all views, summary counts, progress indicators, and saved state. A refresh must preserve it. Do not let one conversation's item state affect another conversation.

## Privacy, storage, and offline behavior

Implement:

- IndexedDB for explicitly saved chats, settings where appropriate, and status changes; use a useful in-memory fallback if unavailable.
- JSON backup export and validated restore, including conversations and their local statuses. Restore must validate the application/version/schema, protect against malformed data, and avoid duplicating conversations.
- Per-conversation delete and a delete-all-data flow with a clear confirmation step. Deletion must remove the relevant persisted and in-memory records.
- A privacy dialog that explains where data is stored, what leaves the device, and what an optional local model would access.
- Online/offline indication driven by browser connectivity events, without implying that online means chat data is being transmitted.
- A request counter and an offline self-test. The self-test should exercise parsing/analysis locally and report actual results; it must not send a probe to a real external site.
- Optional network lockdown only if its behavior can be implemented reliably and explained. Do not describe an app-level counter as a browser- or operating-system-wide network monitor.
- A sensitive-content blur control that can be toggled with keyboard and pointer and does not prevent use of the app.
- A service worker that caches only app assets for offline startup. Do not cache conversation content in Cache Storage or send it through the service worker.

Avoid false privacy guarantees. Browser-local storage is not encryption or protection from other people who can use the same device/profile. Explain that users should export a backup before clearing browser data if they want to retain conversations.

## Security, reliability, and accessibility

- Render imported sender names, messages, file names, and backup values as text or safely escaped content; test malicious HTML/script strings.
- Validate restored data and defensively handle missing/unexpected fields.
- Do not swallow important failures: present storage, parsing, export, restore, clipboard, and service-worker errors to the user in context.
- Avoid unsafe dynamic selectors, inline event handlers, and untrusted values in HTML attributes.
- Use semantic landmarks, headings, labels, buttons, dialogs, tab semantics where appropriate, keyboard-operable navigation, focus visibility, and live announcements for status/toasts.
- Do not use color alone to communicate priority or task state.
- Keep controls usable at mobile widths, zoom, and with reduced motion.

## Suggested project layout

Adapt to the existing workspace; include only files actually used. One suitable layout is:

```text
privatebrief/
├── index.html
├── css/
│   └── style.css
├── js/
│   ├── app.js
│   ├── parser.js
│   ├── analyzer.js
│   ├── storage.js
│   └── ui.js
├── manifest.json
├── service-worker.js
├── icon.svg
└── README.md
```

Do not add empty modules just to match this diagram. A self-contained page is acceptable if it is already maintainable and functional.

## Required implementation workflow

1. Inspect the workspace, existing app, project structure, and available test/build commands before making changes.
2. Identify what is already implemented and preserve useful work. Do not replace a working product with a prototype.
3. Implement the interface and connect every view/control to real conversation data.
4. Implement import formats, normalization, analysis, task status, local persistence, and validated backup/restore.
5. Add privacy controls and service-worker/offline support without introducing external dependencies or chat uploads.
6. Add or update directly relevant documentation with accurate run instructions and current limitations.
7. Run the smallest useful validation first, then test relevant workflows in a browser or existing test runner.
8. Fix issues found and repeat the affected tests. Do not claim tests passed unless they were actually run.

## Acceptance criteria

The product is complete only when all applicable checks pass:

1. A first-time user sees an accurate empty state and can import either pasted text or a supported local file.
2. Representative WhatsApp, Telegram, Slack, and plain-text inputs parse into ordered messages with correct senders/content; multiline content remains intact.
3. Empty/malformed input produces a clear error; malformed timestamps and unknown senders do not crash the page.
4. A sample message mentioning the configured user, asking a question, and including a deadline appears in Attention/Deadlines with a visible rule-based reason and navigates to the source message.
5. Decision-change and possible-conflict examples link to the relevant earlier and later messages and are described as interpretations.
6. Marking an item Done or Not important updates counts and remains after refresh; reopening restores it to pending. State in another conversation stays unchanged.
7. Search and priority/status filters return matching source items; a no-results state is understandable.
8. Export then restore preserves chat content and statuses, rejects an invalid backup clearly, and does not duplicate matching chats.
9. Imported markup such as `<img src=x onerror=alert(1)>` is displayed only as text and never executes.
10. Theme selection persists; blur, evidence mode, navigation, dialogs, and all visible controls work with mouse and keyboard.
11. No imported chat is sent over the network. Request reporting is accurate about application-controlled requests and does not claim to monitor the whole device.
12. With the app shell cached, offline reload opens the app and built-in parse/analysis remains usable. Offline behavior is tested on `localhost` or HTTPS, not inferred from a static mock.
13. The layout is usable on a narrow/mobile viewport, keyboard focus is visible, and reduced-motion preferences are respected.
14. No hardcoded sample statistics remain after a real conversation is analyzed.

## Final handoff

After implementation, report:

- What was built and the important files changed.
- How to open the project in VS Code and run it locally.
- How to try the sample chats and import each supported format.
- Which validation commands and manual/browser tests were actually run, with outcomes.
- Which features are complete and any known limitations, especially around rule-based interpretation, date ambiguity, browser storage, and service-worker requirements.

Deliver working source code in the existing workspace. Do not respond with only a plan, generated mockup, code excerpts, or unsupported claims.
