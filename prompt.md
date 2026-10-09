# Prompt to Create the PrivateBrief Website in VS Code

Act as a senior full-stack web developer and UI/UX designer. Build a complete, functional, responsive website called **PrivateBrief — Offline Chat Catch-up Assistant** using the requirements below.

## 1. Project Objective

Create a privacy-first web application that allows users to import chat conversations and automatically summarize important information so they can catch up quickly without reading every message.

The application should work offline, process conversations locally in the browser, and avoid uploading private chat messages to any external server.

## 2. Technology Stack

Use the following technologies:

- **HTML5** for structure.
- **CSS3** for responsive styling, animations, glassmorphism, and themes.
- **Vanilla JavaScript** for all application logic.
- **IndexedDB** for saving conversations locally.
- **Web Workers**, if useful, for background processing of large conversations.
- No backend or paid API is required for the core features.

Keep the code modular, readable, and easy to maintain. Use separate files such as `index.html`, `style.css`, `script.js`, and additional JavaScript modules when appropriate.

## 3. Website Design

Create a premium, modern dashboard with a futuristic privacy-focused appearance.

Design requirements:

- Dark purple background with cyan, violet, and pink gradients.
- Glassmorphism cards with subtle borders and translucent backgrounds.
- Smooth hover effects, animated backgrounds, and polished transitions.
- Responsive sidebar and main dashboard layout.
- Clean typography, readable spacing, and accessible contrast.
- Three selectable themes: Aurora, Midnight, and Daylight.
- Mobile-friendly navigation and layouts.
- Use inline SVG icons or CSS icons; do not depend on external assets.
The application should look like a professional SaaS dashboard while remaining lightweight and functional.

## 4. Main Dashboard

Create a dashboard containing:

- A welcome section explaining the application's purpose.
- An **Import a Chat** button.
- A list of previously imported conversations.
- Summary statistics for pending tasks, completed items, urgent messages, and missed deadlines.
- Important items that require the user's attention.
- Recent decisions and upcoming deadlines.
- A privacy status indicator.
- A clear empty state when no conversations have been imported.
Clicking a statistic should navigate to the corresponding filtered section.

## 5. Chat Import System

Allow users to import conversations using either file selection or copy-and-paste.

Supported input formats:

- WhatsApp exported `.txt` files.
- Telegram `.json` exports.
- Slack `.json` exports.
- Plain-text conversations with sender names and messages.
The import dialog should include:

- Conversation name.
- The user's name, to identify mentions and personal tasks.
- A file picker.
- A large text area for pasted messages.
- An option to save the conversation on the device.
- An Analyze Chat button.
- Clear validation errors for invalid or empty input.
Parse messages into structured records containing the sender, timestamp, date, and message content. Handle multiline messages and malformed input gracefully.

## 6. Chat Analysis Engine

Implement a working rule-based analysis engine using JavaScript.

Detect and categorize:

**Important messages**

- Direct mentions of the current user.
- Questions that need a reply.
- Requests and assigned tasks.
- Urgent messages.
- Group-wide responsibilities.
**Deadlines**

- Dates and times mentioned in messages.
- Today, tomorrow, weekdays, and common deadline phrases.
- Overdue tasks and deadlines approaching within 24 hours.
**Decisions**

- Confirmed plans and agreements.
- Updated or changed decisions.
- Rescheduled meetings and changed deadlines.
- Potential conflicts between earlier and later messages.
**Conversation insights**

- Most active participants.
- Frequently discussed topics.
- Total message count.
- Unanswered mentions.
- Tasks assigned to the user.
- Messages that can probably be skipped.
Use transparent scoring rules to assign high, medium, and low priorities. Display the reason why each message was flagged, and clearly distinguish detected facts from uncertain interpretations.

Do not pretend to have AI-powered semantic understanding when only rule-based analysis is being used.

## 7. Dashboard Navigation

Create interactive tabs or navigation sections for:

1. **Brief** — A summary of the conversation and the most important items.
2. **Attention** — Messages, requests, and tasks requiring action.
3. **Decisions** — Confirmed decisions, changed plans, and conflicting messages.
4. **Deadlines** — Upcoming, overdue, and completed deadlines.
5. **Conversation** — The original messages with highlighted mentions and dates.
6. **People & Topics** — Participant activity and frequently discussed subjects.
Each tab must display real data from the imported conversation rather than static placeholder content.

## 8. Task Management

Allow users to manage extracted items with working controls:

- Mark an item as Done.
- Mark an item as Not Important.
- Reopen completed items.
- Filter by priority and status.
- Search messages by sender or keyword.
- Draft suggested replies for messages requiring a response.
- Copy suggested replies to the clipboard.
- Navigate from an extracted item to its original message.
- Display evidence explaining why a message was selected.
Store status changes locally and preserve them when the page is refreshed.

## 9. Privacy and Offline Functionality

Privacy must be a core feature, not just a marketing claim.

Implement:

- Local-only parsing and analysis.
- IndexedDB storage for saved chats.
- An in-memory fallback if IndexedDB is unavailable.
- Export and restore functionality for conversation backups.
- A delete-all-data option with confirmation.
- A privacy information dialog.
- An offline status indicator based on actual browser connectivity.
- An offline self-test that checks whether the core features continue working without internet access.
- A request counter for application-controlled network requests.
- A text-blurring option for sensitive content.
- An evidence mode showing message numbers, priority scores, and reasons for classification.
Do not include analytics, tracking scripts, remote fonts, or third-party libraries loaded from a CDN. Do not send chat contents to external servers. Make privacy indicators reflect actual application behavior.

## 10. Additional Features

Implement these optional enhancements if feasible:

- Sample conversations so users can test the app immediately.
- Animated success feedback when tasks are completed.
- Search and filtering across imported conversations.
- A conversation backup in JSON format.
- Persistent theme preferences.
- Keyboard accessibility and visible focus indicators.
- Graceful handling of large files.
- Empty states, loading states, error messages, and success notifications.
- Reduced-motion support for accessibility.

## 11. Functional Requirements

Every visible button, tab, dialog, form, and toggle must work.

Requirements:

- Do not leave placeholder buttons or dead links.
- Do not use hardcoded dashboard statistics once a conversation has been analyzed.
- Prevent duplicate or corrupted imports.
- Safely render imported messages to avoid HTML or script injection.
- Keep data isolated between conversations.
- Update the dashboard immediately when task statuses change.
- Handle missing timestamps and unknown senders.
- Clearly explain any analysis limitations.
- Keep the application usable without a network connection after its required files are available locally.

## 12. Suggested Project Structure

Create a clean structure similar to:

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
└── README.md
```

Use only files that are actually needed. Implement a service worker and web app manifest if adding installable offline support.

## 13. Development Instructions

Work in the following order:

1. Set up the project and create all necessary files.
2. Build the complete responsive interface.
3. Implement the chat import and parsing functionality.
4. Implement the analysis engine and deadline detection.
5. Connect the dashboard to actual analysis results.
6. Implement tabs, filtering, task management, and search.
7. Add IndexedDB persistence and backup restoration.
8. Add privacy controls, themes, and offline support.
9. Test all features using sample WhatsApp, Telegram, and Slack conversations.
10. Fix runtime errors, parsing bugs, layout issues, and broken interactions.
If you have access to the project files in VS Code, create and edit the files directly. Otherwise, provide the complete contents of every required file with its filename clearly identified.

Do not provide only a demonstration or a static UI. Build a working application.

## 14. Final Deliverables

At the end, provide:

- The complete project source code.
- A clear explanation of the project structure.
- Instructions for opening the folder in VS Code.
- Instructions for running the website locally.
- A list of implemented features.
- Test cases for importing and analyzing conversations.
- Any known limitations or features that still require implementation.

**Final goal:** Deliver a polished, fully functional PrivateBrief website that helps users catch up on group conversations in seconds while keeping their private messages on their own device.
