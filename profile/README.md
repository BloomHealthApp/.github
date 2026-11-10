<p align="center">
  <img src="assets/banner.png" alt="Bloom Health — Small daily habits. Kind streaks." width="720"/>
</p>

<h3 align="center">Small daily habits. Kind streaks.</h3>

<p align="center">
  Bloom Health is a gentle habit and wellness tracker for Android and iOS.<br/>
  Log water, steps, workouts and your daily habits, keep a streak that forgives rest days,<br/>
  and chat with <b>Bloomy</b>, a friendly AI companion — on your own or with someone cheering you on.
</p>

<p align="center">
  <img src="assets/home.png" width="160" alt="Home"/>
  <img src="assets/progress.png" width="160" alt="Progress"/>
  <img src="assets/bloomy.png" width="160" alt="Bloomy AI chat"/>
  <img src="assets/calendar.png" width="160" alt="Calendar"/>
  <img src="assets/profile.png" width="160" alt="Profile"/>
</p>

---

### ✨ What you can do

- **🌱 Daily habits** — start with 8 ready-made habits or create your own, schedule them by weekday, and set a daily goal (80% by default).
- **💧 Water & steps** — one-tap logging with quick-add buttons and progress toward your target.
- **🏋️ Workouts** — a timer that keeps running even if the app is closed, plus manual entries.
- **🔥 Kind streaks** — Sundays and rest days never break your streak. Milestones at 3, 7, 14, 30 and 100 days.
- **📅 Calendar & progress** — a monthly calendar, weekly chart and heatmap to see how far you've come.
- **🌸 Period tracker** — private cycle logging with flow, symptoms and mood, and a gentle estimate of your next cycle. Never shared with viewers.
- **💬 Bloomy** — an AI chat companion for encouragement and habit ideas.
- **👀 Invite a viewer** — let a partner, friend or family member follow your progress, read-only.
- **⏰ Reminders** — as a notification, a tone or a loud alarm.
- **📱 Home-screen widgets** — streak, daily progress, and steps & calories (Android).
- **🎨 Make it yours** — 15 Bloom avatars and five themes, each in light and dark.

### 🔒 Private by design

- Sign in with a one-time code — no passwords to remember.
- Works offline: everything saves on your phone first and syncs when you're back online.
- Viewers can only look, never edit, and never see your period data.

### 🧩 How it's built

| Repository | Role | Stack |
|---|---|---|
| `bloom-health-app` | The mobile app (offline-first, with a local sync queue) | Expo · React Native · TypeScript · SQLite |
| [`bloom-health-service`](https://github.com/BloomHealthApp/bloom-health-service) | REST + sync API, OTP auth, owner/viewer access | Go · Gin · MongoDB Atlas |
| `bloom-health-task-worker` | Background worker for Bloomy chat and scheduled jobs | Go · MongoDB · GCP |

### 🏗️ Architecture

```mermaid
flowchart LR
  subgraph phone["📱 Your phone · bloom-health-app"]
    ui["Expo Router screens"]
    store[("SQLite cache<br/>+ sync queue")]
    device["Steps from HealthKit / Health Connect<br/>Reminders · Alarms · Widgets"]
    ui <--> store
    device --> ui
  end

  subgraph cloud["☁️ Backend"]
    api["bloom-health-service<br/>Go · Gin · Vercel"]
    db[("MongoDB Atlas")]
    worker["bloom-health-task-worker<br/>Go · GCP VM"]
  end

  smtp{{"Email · one-time codes"}}
  google{{"Google Sign-In"}}
  groq{{"Groq · AI model"}}

  store <-->|"HTTPS /v1 · push & pull"| api
  ui -->|"Bloomy chat"| api
  api --> smtp
  api --> google
  api <--> db
  worker <-->|"claim & settle chat turns"| db
  worker -->|"Bloomy replies"| groq
  worker -.->|"daily account clean-up"| api
```

### 🔄 How data flows

**Offline-first sync.** Everything you log is saved to SQLite on your phone first and added to a sync queue. When you're online, the app *pushes* queued changes. Each change has its own id, so a retry never applies it twice. Then it *pulls* everything new since its last cursor. If the same record was changed in two places, a version check settles it the same way every time.

**Sign-in.** Enter your phone or email and you get a one-time code by email, or you can sign in with Google. The service returns a short-lived access token and a refresh session. There are no passwords anywhere.

**Bloomy chat.** The app never calls the AI model itself. The service queues the turn in MongoDB, and the worker picks it up:

```mermaid
sequenceDiagram
  autonumber
  participant A as 📱 App
  participant S as bloom-health-service
  participant M as MongoDB
  participant W as bloom-health-task-worker
  participant G as Groq

  A->>S: Send message
  S->>M: Save your message, queue a pending reply
  S-->>A: Accepted (reply pending)
  W->>M: Claim the pending reply
  W->>G: Ask the model
  G-->>W: Answer
  W->>M: Save the reply (or retry later / mark failed)
  loop until the reply is ready
    A->>S: Check the conversation
    S->>M: Read messages
    S-->>A: Bloomy's reply 🌸
  end
```

Each chat allows one reply in flight at a time. Every user has a daily message cap, and retries back off when the model is busy.

### 🔐 Privacy built in

- **Period data stays with you.** The server filters it out for viewers, both in sync and in stats.
- **No health data goes to the AI.** Bloomy only sees your chat.
- **Chat text is never logged**, by the service or the worker.
- **No secrets on your phone.** Model and database keys live only on the servers.

---

<p align="center">
  <sub>Bloom Health is a personal habit tracker, not medical advice. · Made with 💜 by <a href="https://github.com/amankumar-2001">Aman Kumar</a></sub>
</p>
