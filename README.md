# FX-OS — Forex Learning Operating System

One file. No build step. Invite-only trading academy with lessons, replay practice, coach feedback, dungeon challenges, journal, community channels, leaderboard and admin console.

## Deploy to GitHub Pages (2 minutes)

1. Create a repo (e.g. `fx-os`), upload `index.html`.
2. Repo → **Settings → Pages** → Source: `main` branch, `/ (root)` → Save.
3. Your site is live at `https://<username>.github.io/fx-os/`.

Default admin login: **pavan / admin@2026** — change it immediately in Admin → Settings.

## Modes

| | Local mode (default) | Global mode (Supabase) |
|---|---|---|
| Lessons, XP, streaks, dungeon, journal, coach | ✅ per device | ✅ synced |
| Admin-created logins work on students' devices | ❌ | ✅ |
| Community chat between users | ❌ (per device) | ✅ |
| Global leaderboard | ❌ | ✅ |

## Enable global mode (free Supabase tier)

1. Create a project at supabase.com.
2. SQL Editor → run:
   ```sql
   create table kv (key text primary key, value jsonb, updated timestamptz default now());
   alter table kv enable row level security;
   create policy "open" on kv for all using (true) with check (true);
   ```
3. In `index.html`, fill in near the top of the script:
   ```js
   const SUPABASE = { url: "https://xxxx.supabase.co", key: "your-anon-key" };
   ```
4. Commit. Done — logins, chat, support inbox and leaderboard now sync for everyone.

## Security honesty

This is a static site: credentials live in a shared table readable by anyone technical, and the open RLS policy means data isn't private. Perfect for a mentorship study group; **not** for anything involving real money or sensitive data. For a real SaaS you'd add Supabase Auth + proper RLS per user — happy path to upgrade later since the storage layer is already one small adapter.

## Extending the curriculum

All content lives in the `PHASES` array — each lesson is:
```js
{ t:"Title", deep:{ w:"what", h:"how to use", p:["pros"], c:["cons"], u:"when", n:"when NOT", tip:"pro tip" },
  quiz:[{ q:"…", o:["a","b","c","d"], a:1, e:"explanation" }] }
```
Add lessons/phases by copying the shape. Quiz questions automatically join the Daily Challenge pool.
