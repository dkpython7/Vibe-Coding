# Vibe Coding Notes

### The 6 Documents You Need Before Writing Any Code

---

## 📖 Introduction

AI coding tools like Cursor, GitHub Copilot, and Claude Code can build apps very fast.

**But AI tools are not mind readers.**

If you give poor instructions, the AI will:

- ❌ Generate wrong code
- ❌ Use random tech stacks
- ❌ Create broken navigation
- ❌ Build inconsistent UI
- ❌ Add security issues

That is why professional developers create documents **before** coding.

These 6 documents help AI understand:

- What to build
- How to build it
- How users will use it
- How the backend should work
- What design style to follow

---

## 🗺️ Full Workflow Diagram

```
IDEA
  ↓
PRD (Product Requirements)
  ↓
TRD (Technical Requirements)
  ↓
APP FLOW (User Navigation)
  ↓
UI/UX DESIGN (Visual Style)
  ↓
BACKEND SCHEMA (Database + Auth)
  ↓
IMPLEMENTATION PLAN (Build Sequence)
  ↓
AI CODING AGENT (Builds Final Product)
```

---

## 📋 Why These Documents Matter

| Document | Purpose | Why Important |
|---|---|---|
| PRD | Defines the product idea | Prevents confusion |
| TRD | Defines technologies | Keeps stack consistent |
| App Flow | Defines navigation | Connects all screens |
| UI/UX Brief | Defines visual design | Makes UI consistent |
| Backend Schema | Defines database | Prevents data issues |
| Implementation Plan | Defines build order | Prevents broken workflow |

---

## 1️⃣ PRD — Product Requirements Document

### What is PRD?

PRD explains:

- What the app does
- Who will use it
- Why it exists
- What problems it solves

This is the **most important document**. Without a PRD, the AI guesses everything.

### PRD Structure

- App Name
- Tagline
- Problem Statement
- Target Users
- Core Features
- Nice-to-Have Features
- User Stories
- Success Metrics

### Example

| Section | Example |
|---|---|
| App Name | TaskFlow |
| Tagline | Smart AI task manager |
| Problem | People forget tasks |
| Users | Students and developers |
| Features | Tasks, reminders, dashboard |
| Success Metric | 1000 users |

### 🛠️ Good Tools for Writing PRD

**1. [Notion](https://www.notion.so/)**
- Best for documentation
- Team collaboration
- Templates available
- 💡 Prompt: `"Create a PRD template for a [your app type] app, including problem statement, target users, core features, and success metrics."`

**2. [Obsidian](https://obsidian.md/)**
- Markdown-based notes
- Offline-first
- 💡 Prompt: `"Write a structured PRD note in markdown for an app called [App Name] that solves [problem]."`

**3. [Google Docs](https://docs.google.com/)**
- Easy sharing
- Simple formatting
- 💡 Prompt: `"Draft a one-page PRD for [App Name], a tool that helps [target users] do [main task]."`

### 🔗 Useful Resources

**Repositories**
- [Awesome PRD Templates](https://github.com/search?q=awesome+prd+templates&type=repositories)
- [Startup PRD Examples](https://github.com/search?q=startup+prd+examples&type=repositories)

**Videos**
- [How to Write a PRD (YouTube Search)](https://www.youtube.com/results?search_query=how+to+write+a+prd)

---

## 2️⃣ TRD — Technical Requirements Document

### What is TRD?

TRD defines:

- Frontend framework
- Backend framework
- Database
- APIs
- Hosting
- Authentication

### Architecture Diagram

```
Frontend (Next.js)
       ↓
Backend API (Node.js / FastAPI)
       ↓
Database (PostgreSQL)
       ↓
Deployment (Vercel / Railway)
```

### Example TRD

| Technology | Example |
|---|---|
| Frontend | Next.js |
| Backend | Node.js |
| Database | PostgreSQL |
| Hosting | Vercel |
| Auth | Clerk |
| AI API | OpenAI |

### 🛠️ Best AI Coding Tools

**1. [Cursor](https://www.cursor.com/)**
- AI code generation
- Smart autocomplete
- Codebase understanding
- Repo: [Cursor Community Resources](https://github.com/search?q=cursor+ai+community&type=repositories)
- Video: [Cursor AI Tutorial](https://www.youtube.com/results?search_query=cursor+ai+tutorial)
- 💡 Prompt: `"Using my PRD and TRD, scaffold the initial project structure for [App Name] with [Next.js/Node.js/etc]."`

**2. [Windsurf](https://windsurf.com/)**
- AI-native IDE
- Agent workflow
- Multi-file editing
- Docs: [Windsurf Docs](https://docs.windsurf.com/)
- Video: [Windsurf AI IDE Tutorial](https://www.youtube.com/results?search_query=windsurf+ai+ide+tutorial)
- 💡 Prompt: `"Using the attached TRD, set up the backend API routes and database connection for [App Name]."`

**3. [GitHub Copilot](https://github.com/features/copilot)**
- AI pair programming
- Autocomplete
- Chat assistant
- Docs: [Copilot Docs](https://docs.github.com/en/copilot)
- Video: [GitHub Copilot Crash Course](https://www.youtube.com/results?search_query=github+copilot+crash+course)
- 💡 Prompt: `"Explain and refactor this function to match the architecture described in my TRD."`

**4. [Claude Code](https://claude.com/claude-code)**
- Large context coding
- Code explanation
- Architecture planning
- Docs: [Anthropic Docs](https://docs.claude.com/)
- Video: [Claude Code Guide](https://www.youtube.com/results?search_query=claude+code+guide)
- 💡 Prompt: `"Read my PRD, TRD, App Flow, UI/UX Brief, Backend Schema, and Implementation Plan files in this repo, then build the project step-by-step following the Implementation Plan."`

**5. [Replit AI](https://replit.com/ai)**
- Cloud coding
- AI generation
- One-click deployment
- Repo: [Replit GitHub](https://github.com/replit)
- Video: [Replit AI Tutorial](https://www.youtube.com/results?search_query=replit+ai+tutorial)
- 💡 Prompt: `"Build a working prototype of [App Name] in Replit using the tech stack defined in my TRD."`

---

## 3️⃣ App Flow — User Navigation

### What is App Flow?

App Flow explains:

- Which pages exist
- How users move between screens
- Login flow
- Dashboard flow
- Error handling

### App Flow Diagram

```
Landing Page
     ↓
Login / Signup
     ↓
Onboarding
     ↓
Dashboard
     ↓
Projects
     ↓
Settings
```

### Important Screens

| Screen | Purpose |
|---|---|
| Landing Page | Introduce product |
| Login | User authentication |
| Dashboard | Main control center |
| Settings | User preferences |

💡 Prompt: `"Create a detailed app flow document for [App Name], listing every screen, the navigation between them, and what happens on errors (e.g., failed login, empty states)."`

---

## 4️⃣ UI/UX Design Brief

### What is UI/UX Brief?

This document defines:

- Colors
- Fonts
- Spacing
- Dark mode
- Button styles
- Overall design vibe

### Design Example

| Design Element | Example |
|---|---|
| Style | Minimal |
| Theme | Dark mode |
| Font | Inter |
| Radius | Rounded corners |
| Inspiration | Linear, Notion |

💡 Prompt: `"Write a UI/UX design brief for [App Name] in the style of Linear and Notion — minimal, dark mode, rounded corners, using the Inter font."`

### 🎨 UI Inspiration Tools

**1. [Dribbble](https://dribbble.com/)**
- UI inspiration platform

**2. [Behance](https://www.behance.net/)**
- Professional design showcase

**3. [Mobbin](https://mobbin.com/)**
- Real app UI references

---

## 5️⃣ Backend Schema

### What is Backend Schema?

Defines:

- Database tables
- Relationships
- Authentication
- User roles
- Security rules

### Backend Diagram

```
Users Table
    ↓
Projects Table
    ↓
Tasks Table
    ↓
Comments Table
```

### Example Database

| Table | Purpose |
|---|---|
| users | Stores users |
| projects | Stores projects |
| tasks | Stores tasks |
| comments | Stores feedback |

💡 Prompt: `"Design a backend database schema for [App Name] with tables for users, projects, tasks, and comments, including relationships and authentication rules."`

### 🛠️ Backend Tools

**1. [Supabase](https://supabase.com/)**
- PostgreSQL database
- Authentication
- Storage
- APIs
- Repo: [Supabase GitHub](https://github.com/supabase/supabase)
- Video: [Supabase Full Course](https://www.youtube.com/results?search_query=supabase+full+course)
- 💡 Prompt: `"Set up a Supabase project with tables matching my backend schema, including row-level security for user data."`

**2. [Firebase](https://firebase.google.com/)**
- Authentication
- Realtime database
- Hosting
- Repo: [Firebase GitHub](https://github.com/firebase)
- Video: [Firebase Tutorial](https://www.youtube.com/results?search_query=firebase+tutorial)
- 💡 Prompt: `"Configure Firebase Authentication and Firestore collections based on my backend schema document."`

---

## 6️⃣ Implementation Plan

### What is Implementation Plan?

Defines:

- Build order
- Project phases
- Milestones
- Testing workflow

### Build Sequence Diagram

```
Setup
  ↓
Database
  ↓
Authentication
  ↓
Core Features
  ↓
UI Polish
  ↓
Testing
  ↓
Deployment
```

💡 Prompt: `"Create a step-by-step implementation plan for building [App Name], broken into phases: setup, database, authentication, core features, UI polish, testing, and deployment."`

### 🚀 Best Deployment Tools

**1. [Vercel](https://vercel.com/)**
- Next.js deployment
- Fast hosting
- Free tier
- Repo: [Vercel GitHub](https://github.com/vercel/vercel)
- 💡 Prompt: `"Deploy this Next.js project to Vercel and set up environment variables for production."`

**2. [Railway](https://railway.app/)**
- Backend hosting
- Database hosting
- Easy deployment
- Docs: [Railway Docs](https://docs.railway.app/)
- 💡 Prompt: `"Deploy my backend API and PostgreSQL database to Railway, following the deployment phase in my Implementation Plan."`

---

## 🔁 Recommended AI Workflow

1. Write PRD
2. Write TRD
3. Create App Flow
4. Design UI/UX Brief
5. Create Backend Schema
6. Write Implementation Plan
7. Paste all docs into AI chat
8. Start generating code

💡 Master Prompt (use after all 6 docs are ready):
```
I am attaching 6 planning documents: PRD, TRD, App Flow, UI/UX Brief,
Backend Schema, and Implementation Plan for an app called [App Name].

Read all of them carefully, then follow the Implementation Plan step-by-step
to build the project. Confirm the tech stack from the TRD before writing
any code, and ask me clarifying questions if anything is ambiguous.
```

---

## 💡 Final Advice

> Good AI coding is not about writing better prompts.
> It is about:
> - Better planning
> - Better documentation
> - Better architecture
> - Better workflows

**The more context you give AI, the better the output becomes.**
Professional developers do not start with coding. They start with documents.

---

### 📌 Quick Reference: All Tools & Links

| Category | Tool | Link |
|---|---|---|
| Docs | Notion | https://www.notion.so/ |
| Docs | Obsidian | https://obsidian.md/ |
| Docs | Google Docs | https://docs.google.com/ |
| AI Coding | Cursor | https://www.cursor.com/ |
| AI Coding | Windsurf | https://windsurf.com/ |
| AI Coding | GitHub Copilot | https://github.com/features/copilot |
| AI Coding | Claude Code | https://claude.com/claude-code |
| AI Coding | Replit AI | https://replit.com/ai |
| Design | Dribbble | https://dribbble.com/ |
| Design | Behance | https://www.behance.net/ |
| Design | Mobbin | https://mobbin.com/ |
| Backend | Supabase | https://supabase.com/ |
| Backend | Firebase | https://firebase.google.com/ |
| Deployment | Vercel | https://vercel.com/ |
| Deployment | Railway | https://railway.app/ |

---

*Made for developers who vibe code smarter, not harder.* ✨