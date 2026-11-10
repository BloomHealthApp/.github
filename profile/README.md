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

---

<p align="center">
  <sub>Bloom Health is a personal habit tracker, not medical advice. · Made with 💜 by <a href="https://github.com/amankumar-2001">Aman Kumar</a></sub>
</p>
