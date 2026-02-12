PROJECT SPEC — Daily Focus Planner (Personal Use)

1.  Project Goal

Build a minimal daily planning web app to help a single user:

-   Dump all tasks in the morning
-   Select Top 3 priority tasks
-   Enter 30-minute focus mode
-   Log completed sessions
-   Set tomorrow’s first task

This is NOT a SaaS product. This is a personal productivity tool.

2.  Technical Stack

-   Next.js 14 (App Router)
-   TypeScript
-   Tailwind CSS
-   Client-side only
-   Use localStorage for persistence
-   No backend
-   No authentication
-   No database
-   No API routes

The entire app must run statically.

3.  Scope Constraints (Very Important)

DO NOT implement:

-   Authentication
-   Multi-user support
-   Cloud database
-   Server actions
-   API routes
-   Analytics
-   Complex UI library
-   Dark/light toggle
-   Notifications
-   Drag-and-drop
-   AI auto-prioritization

Keep everything minimal and deterministic.

4.  Data Model

type Task = { id: string text: string completed: boolean }

type FocusSession = { taskId: string startedAt: string completed:
boolean }

type DayPlan = { date: string tasks: Task[] topThreeIds: string[]
focusSessions: FocusSession[] tomorrowFirstTask?: string }

Store in localStorage under key: daily-focus-planner

5.  Core Features

5.1 Brain Dump Section

-   Large textarea
-   User enters tasks (one per line)
-   On submit:
    -   Convert each line into Task object
    -   Append to today’s tasks list
    -   Clear textarea

5.2 Task List

Display all tasks:

-   Checkbox to mark completed
-   Button to select as Top 3
-   Limit Top 3 selection to exactly 3 tasks

If 3 already selected, disable further selection.

5.3 Top 3 Section

Display selected tasks clearly separated.

Rules: - Must have exactly 3 selected before enabling Focus Mode - If
less than 3 → show warning message

5.4 Focus Mode

When user clicks “Start Focus”:

-   User must choose 1 of the Top 3 tasks
-   Hide all other UI
-   Show:
    -   Task name
    -   30-minute countdown timer
    -   Cancel button
    -   Mark as Done button

Timer behavior: - Countdown from 30:00 - If reaches 0 → mark session
completed - Log FocusSession - Return to main screen

No background notifications required.

5.5 End of Day Section

At bottom of page:

Input field: - “Tomorrow 9:00 AM first task”

Save to DayPlan.tomorrowFirstTask

6.  Daily Reset Logic

On app load:

-   Check stored date
-   If stored date ≠ today:
    -   Archive previous DayPlan inside localStorage array
    -   Create new empty DayPlan for today

No cloud sync required.

7.  UI Rules

-   Single page only
-   Clean layout
-   Use Tailwind only
-   No external component libraries
-   Desktop-first design
-   Functional > pretty

8.  Folder Structure

app/ page.tsx

lib/ storage.ts date.ts

components/ BrainDump.tsx TaskList.tsx TopThree.tsx FocusMode.tsx
EndDay.tsx

9.  Non-Functional Requirements

-   Must work fully offline
-   Must not crash on page reload
-   Must persist state correctly
-   No console errors
-   No unnecessary dependencies

10. Definition of Done

App is considered complete when:

-   User can dump tasks
-   User can select exactly 3 top tasks
-   User can run 30-minute focus session
-   Focus sessions are logged
-   Tomorrow’s first task is saved
-   Data persists after refresh
-   Day resets correctly

Nothing more.
