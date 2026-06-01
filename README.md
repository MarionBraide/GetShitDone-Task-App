# GetShitDone

## Overview

GetShitDone is a simple, no-nonsense task management web application built to help users stay focused and get things done. Stripped of all distractions and built around a clean, minimal interface, GetShitDone makes it easy to capture tasks, track what's in progress, and mark things off as complete.

Users can create tasks, view their active task list, mark tasks as done, and watch their completed tasks accumulate — all with zero friction.

---

## Problem Statement

Most productivity tools are bloated. They come with tags, priorities, subtasks, integrations, and endless settings — and people spend more time managing the app than actually doing the work.

GetShitDone solves this by doing less, on purpose.

### Key Problems Solved

- Eliminates setup friction — tasks are one input away
- Keeps the interface clean so there's nothing to distract from the work
- Gives users a clear visual separation between what's pending and what's done
- Reinforces completion habits through a simple, satisfying task-done interaction
- No accounts, no backend, no syncing — just tasks, locally stored and instantly available

---

## Features

### Create a Task
Users can quickly add a task through a simple input — just type what needs to be done and submit.

### Active Task List
All pending tasks are displayed in a clean list. Each task is clearly presented and ready to be acted on.

### Mark as Complete
A single interaction moves a task from active to completed, giving users that satisfying sense of progress.

### Completed Tasks
Completed tasks are tracked separately so users can see what they've accomplished.

### Persistent Storage
All task data is saved in browser localStorage, meaning tasks survive page refreshes without requiring any backend setup.

---

## Framework Choice: Why Vue?

Vue was the natural choice for GetShitDone given the app's simplicity and the need to ship fast without sacrificing structure.

### Component-Based Architecture
Despite being a simple app, GetShitDone is built from clean, reusable components:

- Task input form
- Task list
- Individual task items
- Completed task section

Vue's single-file components (`.vue`) keep each piece of UI self-contained with its template, logic, and styles in one place.

### Reactivity Without the Overhead
The core interactions — adding a task, completing a task, clearing tasks — all require immediate UI updates. Vue's built-in reactivity system handles this naturally without boilerplate. The UI stays in sync with the data automatically.

### Why Vue for GetShitDone Specifically?
GetShitDone is a focused, single-purpose app. Vue's gentle learning curve, minimal configuration, and clean template syntax made it the right tool for building something useful quickly — without the overhead of a heavier framework. The result is a lightweight app that matches its own philosophy: simple, fast, and effective.

---

## Tech Stack

- **Vue 3**
- **Vite**
- **CSS**
- **JavaScript**
- **Local Storage API**

---

## How to Run GetShitDone Locally

### Prerequisites

- Node.js (v16 or higher)
- npm (included with Node.js)

Download Node.js from: https://nodejs.org

### Installation

Clone the repository:

```bash
git clone https://github.com/your-username/get-shit-done.git
```

Navigate into the project folder:

```bash
cd get-shit-done
```

Install dependencies:

```bash
npm install
```

### Start the Development Server

Run:

```bash
npm run dev
```

Vite will start a local development server, usually at:

```
http://localhost:5173
```

If port 5173 is already in use, Vite will automatically assign another available port.

### Open the Application

Open the URL displayed in your terminal, typically:

```
http://localhost:5173
```

---

## What to Expect

### Task Input
- Clean single-field task entry
- Submit with Enter or the add button

### Active Tasks
- All pending tasks displayed in order
- One-click completion for each task

### Completed Tasks
- Completed tasks move to a separate section
- A running record of everything you've shipped

### Data Persistence
GetShitDone uses the browser's localStorage API. This means:

- Tasks remain available after page refreshes
- No backend setup required
- All data is stored locally on the device

---

## Troubleshooting

### Port Already in Use
If port 5173 is occupied, Vite will automatically use another available port. Check the terminal output for the correct URL.

### Dependency Errors
If packages fail to install or load correctly, run:

```bash
npm install
```

---

## Deployment

The app is deployed at:

[https://get-shit-done-task-app.vercel.app](https://get-shit-done-task-app.vercel.app)
