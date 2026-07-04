# ⏱️ FocusFlow

FocusFlow is a single-page Pomodoro-style productivity app for tracking study/focus sessions. It combines a focus timer, a session log, and an analytics dashboard — all running entirely in the browser, with session data persisted to `localStorage` (no backend required).

## Features

- **Pomodoro Timer** — Animated circular countdown timer with **Focus** (25 min), **short break** (5 min), and **long break** (15 min) modes. Start/pause/reset controls, a completion sound (Web Audio API beep), and smooth mode-switch transitions.
- **Session Complete modal** — When a focus session finishes, you're prompted to log what you studied and rate your productivity (1–5 stars), with a little confetti celebration on save.
- **Dashboard** — At-a-glance stats (total study hours, completed pomodoros, average productivity) plus a rotating motivational quote and a quick "Start Focus Session" shortcut.
- **Study Log** — Chronological, filterable list (all / focus / break) of every logged session, grouped by date ("Today", "Yesterday", or full date), with the ability to delete entries.
- **Analytics** — Charts (via Recharts) covering the last 7 days of study hours and productivity, top studied topics, and summary stats including total sessions, total hours, average productivity, and longest daily streak.
- **Polished UI** — Dark, glassmorphic design with gradient accents and animated transitions (via `motion`/Framer Motion), plus a sidebar navigation between Dashboard, Focus, Study Log, and Analytics.

## Tech Stack

- **React 18** + **Vite 6** (with the SWC-based React plugin)
- **Tailwind CSS** (utility classes used throughout components)
- **motion** (Framer Motion) for animations and page transitions
- **Recharts** for the Analytics charts (bar / line / pie)
- **lucide-react** for icons
- A large set of **Radix UI primitives** (accordion, dialog, dropdown-menu, select, tabs, tooltip, etc.) and supporting libraries (`class-variance-authority`, `clsx`, `tailwind-merge`, `cmdk`, `sonner`, `vaul`, `react-hook-form`, `react-day-picker`, `embla-carousel-react`, `next-themes`) available as a general-purpose UI toolkit, though the current pages primarily use the custom components below
- **No backend** — all data is stored in the browser's `localStorage` under the `studySessions` key

## Project Structure

```
FocusFlow Productivity App/
├── index.html
├── vite.config.ts
├── package.json
└── src/
    ├── main.jsx                  # App entry point
    ├── App.jsx                   # Page state & routing (dashboard/timer/log/analytics), page transitions
    ├── index.css / styles/globals.css
    └── components/
        ├── Navigation.jsx        # Sidebar nav between the 4 pages
        ├── Dashboard.jsx         # Summary stats + motivational quote + CTA
        ├── StatCard.jsx          # Reusable stat display card used on the Dashboard
        ├── PomodoroTimer.jsx     # Core focus/break timer with animated progress ring
        ├── SessionModal.jsx      # Post-session logging modal (topic, productivity rating, confetti)
        ├── StudyLog.jsx          # Filterable, deletable history of logged sessions
        └── Analytics.jsx         # Recharts-based weekly/topic analytics and stats
```

## Getting Started

### Prerequisites

- Node.js (v18+) and npm

### Install & run

```bash
cd "FocusFlow Productivity App"
npm install
npm run dev
```

Vite will start a local dev server (default `http://localhost:5173`) — open it in your browser.

### Build for production

```bash
npm run build
```

This outputs a static production build (via Vite) that can be deployed to any static host.

## How Data Works

FocusFlow doesn't use a database or API — every completed session is saved as a JSON object to the browser's `localStorage` under the key `studySessions`:

```json
{
  "id": 1719999999999,
  "topic": "React Hooks",
  "duration": 25,
  "productivity": 4,
  "date": "2026-07-04T12:00:00.000Z",
  "type": "pomodoro"
}
```

The Dashboard, Study Log, and Analytics pages all read from this same key, so data persists across reloads but is local to a single browser (clearing site data/localStorage will erase your history, and it won't sync across devices).

## Notes

- The completion sound is generated on the fly with the Web Audio API (no audio files bundled).
- The included `.git` history suggests this was initialized as its own repository; if you're extending it, `npm run build` + your preferred static host (Vercel, Netlify, GitHub Pages, etc.) is enough to ship it since there's no server component.
