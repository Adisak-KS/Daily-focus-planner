# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This repo contains the **Daily Focus Planner** — a minimal, single-user daily planning web app. It is a personal productivity tool (not SaaS). Two directories exist:

- `daily-focus-planner/` — the main app (active development)
- `daily-focus-planner-scaffold/` — a clean Next.js scaffold (reference only)

The project spec lives at `Daily_Focus_Planner_Spec.txt`. A detailed feature spec with acceptance scenarios is at `daily-focus-planner/specs/001-daily-focus-planner/spec.md`. The project constitution (architectural rules) is at `daily-focus-planner/.specify/memory/constitution.md`.

## Commands

All commands run from `daily-focus-planner/`:

```bash
cd daily-focus-planner
npm run dev      # Start dev server
npm run build    # Production build
npm run lint     # ESLint
npm start        # Serve production build
```

No test framework is configured yet.

## Architecture

**Stack**: Next.js 14 (App Router), TypeScript (strict), React 18, Tailwind CSS. No backend, no API routes, no server actions, no external component libraries, no state management libraries.

**Client-only**: The entire app runs in the browser. All state is persisted via `localStorage` under the key `daily-focus-planner`. The app must work fully offline after initial load.

**Single page**: `app/page.tsx` is the sole route. It is a `'use client'` component that orchestrates all state.

### Data Flow

1. On mount, `page.tsx` calls `initializeDayPlan()` from `lib/storage.ts`, which handles daily reset (archiving yesterday's plan if the date changed)
2. All state lives in a single `StorageData` object (`{ currentPlan: DayPlan, archive: DayPlan[] }`) held in React state
3. Every mutation function in `lib/storage.ts` returns a new `StorageData` and also writes to `localStorage` as a side effect
4. Components receive data as props and fire callbacks up to `page.tsx`

### Key Files

- `lib/types.ts` — `Task`, `FocusSession`, `DayPlan`, `StorageData` type definitions
- `lib/storage.ts` — All persistence logic: load, save, initialize, add tasks, toggle completion, toggle top-3, log focus sessions, set tomorrow task
- `lib/date.ts` — Date helpers (`getToday`, `isSameDay`)
- `components/BrainDump.tsx` — Textarea for entering tasks (one per line)
- `components/TaskList.tsx` — Task display with completion checkboxes and top-3 selection
- `components/TopThree.tsx` — Displays selected priorities, gates focus mode (requires exactly 3)
- `components/FocusMode.tsx` — 30-minute countdown timer, replaces all other UI when active
- `components/EndDay.tsx` — Input for tomorrow's first task

### Constraints (from project constitution)

- No backend, database, cloud sync, API routes, or server actions
- No auth, multi-user, analytics, notifications, drag-and-drop, or AI features
- No external UI libraries or date libraries
- Tailwind CSS only for styling, desktop-first
- `@/*` path alias maps to the project root (configured in `tsconfig.json`)
- Focus timer state is ephemeral — does not survive page refresh (by design)

## SpecKit Workflow

The `daily-focus-planner/.claude/commands/` directory contains SpecKit slash commands (`speckit.analyze`, `speckit.specify`, `speckit.plan`, `speckit.implement`, `speckit.tasks`, `speckit.checklist`, `speckit.clarify`, `speckit.constitution`, `speckit.taskstoissues`). These follow a spec-driven development workflow. Templates live in `.specify/templates/`.
