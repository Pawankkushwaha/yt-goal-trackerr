# YouTube Goal Tracker

A web app that turns a YouTube subscriber goal and a deadline into a live pace tracker, so you always know whether you're actually on track, without doing the math yourself.

## Try it
https://yt-goal-tracker.lovable.app

## The problem

Setting a subscriber goal is easy. Knowing whether you're actually on pace to hit it, this week, this month, by the deadline, means either doing the math by hand in YouTube Studio or just guessing. 

## What it does

Set a subscriber target and a deadline once. The app pulls your channel's real current subscriber and view counts, stores a daily snapshot, and turns that history into a live answer: how many subscribers you need per week, your actual measured pace over the last 7 and 30 days, whether you're ahead, on, or behind, and a straight-line projection of where you'll land and when you'd actually cross your target if your current pace holds. It's built to be public and multi-user: anyone can sign up and track their own channel, not just mine.

## Features already built

**Tech stack behind these:** React + [TanStack Start](https://tanstack.com/start) (file-based routing and server functions), Tailwind CSS, [Bun](https://bun.sh) as runtime and package manager, [Supabase](https://supabase.com) for Postgres, auth, and row-level security, and the YouTube Data API v3.

- **Email magic-link sign-in, no password** — Supabase Auth (`signInWithOtp`)
- **One-time setup screen** — channel handle, subscriber target, target date, planned videos per week, all editable any time, stored in Postgres
- **Live dashboard** pulling current subscribers and total views directly from the YouTube Data API, via a server-side function so the API key never reaches the browser
- **Daily snapshot history stored per user** (Postgres table), so pace is measured from real data, never estimated
- **Weekly and monthly pace vs. required pace**, with an on-pace / ahead / behind status, computed client-side from the stored snapshots (pure TypeScript, unit-testable)
- **A straight-line projection**: at your current measured pace, roughly what you'll have by your deadline, and the date you'd actually cross your target if that pace holds
- **A publishing-cadence tracker** ("+1 published" logs a video, and compares actual output against your own stated plan; it deliberately doesn't try to predict subscribers per video, that would be guesswork dressed up as data)
- **Per-user data isolation** — Postgres row-level security (`auth.uid() = user_id`) on every table, not just an app-level check
- **Dark / light mode**

## How it helps creators

- Removes the manual math of "am I actually on pace", the number is always live
- Turns a vague goal ("get to 100K") into an accountable weekly number
- The publishing-cadence card is an honesty check: it shows your actual output against the plan you set for yourself, not a vanity metric

## Features planned next (not built yet)

- **YouTube OAuth + the Analytics API**, replacing the current public-API-only data pull, so a connected account gets real daily history from day one instead of waiting a week for snapshots to build up, plus a real content-type split (long-form vs. Shorts vs. live), which have very different economics
- **Views as a first-class tracked goal** alongside subscribers, with its own weekly/monthly targets per content type
- **Public deployment** (Netlify, off the existing GitHub-synced repo), so this stops being something that only runs on my own machine

## What building this involved

- **Chose portable infrastructure up front.** Connected my own external database project instead of the builder's managed backend, specifically so the data would never be locked into a proprietary format, a call that mattered once the project needed to move past the initial no-code build stage.
- **Scoped it as public and multi-user from day one, not a personal script.** That meant real per-user authentication and database-level access control, not just a page for myself.
- **Made a deliberate product call to keep the "action plan" card honest.** It mirrors my own stated publishing cadence against what I've actually shipped, rather than predicting subscribers per video, since that estimate would be guesswork dressed up as data.
- **Hit a real data limitation mid-build and rearchitected around it.** The public YouTube API only returns current totals, no history, no format breakdown. Rather than ship around that gap, I'm rebuilding the data layer on YouTube's OAuth-based Analytics API instead, a real pivot based on what the data actually supports, not the original plan.

This loop, scoping a real goal, building against real API constraints, and changing direction when the data proved the first approach too limited, is the same kind of ownership behind my Popular Recently Chrome extension: the skill set I look for chances to practice for an Associate Product Manager, Product Manager, or an early co-founder / founder's-office seat at a startup.

## Built with

Built with [Lovable](https://lovable.dev/invite/LFAT07R), which auto-provisioned the app's backend. Stack: React + TanStack Start, Tailwind CSS, a hosted Postgres database with row-level security, and the YouTube Data API.

## About this repo

This repository is a project showcase, not the source code. It exists to document what the app does without publishing the codebase itself.
