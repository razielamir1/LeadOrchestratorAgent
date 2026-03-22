# Lead Orchestrator Agent — Confluence Guide

> Step-by-step usage guide for the multi-agent development system in Claude Code.

---

## Table of Contents

1. [What Is This?](#1-what-is-this)
2. [Prerequisites](#2-prerequisites)
3. [Installation](#3-installation)
4. [Your First Task](#4-your-first-task)
5. [Working With Agents — Step by Step](#5-working-with-agents--step-by-step)
6. [Advanced Workflows](#6-advanced-workflows)
7. [Agent Reference Card](#7-agent-reference-card)
8. [Memory System](#8-memory-system)
9. [Customizing for Your Project](#9-customizing-for-your-project)
10. [Troubleshooting](#10-troubleshooting)

---

## 1. What Is This?

A system of **11 specialized AI agents** that work together as a complete development team inside Claude Code. Instead of one AI doing everything, tasks are routed to the right specialist — just like a real team.

**Key benefits:**
- Each agent has deep expertise in its domain
- Read-only agents (QA, security, code review, architect) can never accidentally break your code
- Agents maintain persistent memory — they learn your project over time
- Heavy analysis happens in isolated agent contexts, keeping your main conversation clean

---

## 2. Prerequisites

| Requirement | Details |
|---|---|
| Claude Code | Installed and authenticated |
| VSCode | With Claude Code extension |
| Git | For version control and subtree sync |
| Node.js | If your project uses Node.js (for agents to run tests, builds, etc.) |

---

## 3. Installation

### Step 3.1 — New Project

1. Go to the GitHub repository
2. Click **"Use this template"** → **"Create a new repository"**
3. Clone your new repository locally
4. Open it in VSCode
5. Edit `CLAUDE.md` — update the **Tech Stack** section to match your project
6. Done! Start chatting with Claude

### Step 3.2 — Existing Project (Recommended)

This method supports pulling future agent updates.

**Step 3.2.1** — Open a terminal in your project directory

**Step 3.2.2** — Add the agents repository as a remote:
```bash
git remote add agents https://github.com/razielamir1/LeadOrchestratorAgent.git
git fetch agents
```

**Step 3.2.3** — Pull the `.claude` directory into your project:
```bash
git subtree add --prefix=.claude agents main --squash
```

**Step 3.2.4** — Copy and customize the orchestrator config:
```bash
cp .claude/../CLAUDE.md ./CLAUDE.md
```

**Step 3.2.5** — Edit `CLAUDE.md` to match your project's tech stack.

**Step 3.2.6** — Commit the changes:
```bash
git add -A && git commit -m "Add Lead Orchestrator Agent system"
```

### Step 3.3 — Updating Agents Later

When new agents are added or existing ones are improved:
```bash
git subtree pull --prefix=.claude agents main --squash
```

---

## 4. Your First Task

Let's walk through a real example to see the system in action.

### Step 4.1 — Open the project in VSCode

Open your project folder. Claude will automatically read `CLAUDE.md` and know about all 11 agents.

### Step 4.2 — Give a simple task

Type in the Claude chat:

```
Build me a health check endpoint that returns the server status and database connection status.
```

### Step 4.3 — Watch the delegation

The Lead Orchestrator will:
1. Recognize this is a backend task
2. Delegate to `backend-developer`
3. The backend-developer will create the endpoint
4. Optionally route to `qa-expert` to verify

### Step 4.4 — Review the output

Each agent returns a summary of what it did. Review the changes, and if you're happy, commit them.

---

## 5. Working With Agents — Step by Step

### 5.1 — Automatic Delegation (Easiest)

**What:** Describe your task in natural language. The orchestrator picks the right agent(s).

**How:**
```
I need a user registration page with email validation and a PostgreSQL table to store users.
```

**What happens:**
1. `database-expert` creates the `users` table and migration
2. `backend-developer` creates `POST /api/users/register` endpoint
3. `frontend-developer` builds the registration form logic
4. `ui-designer` styles the form with TailwindCSS

**When to use:** Most of the time. This is the default way to work.

---

### 5.2 — Direct Agent Invocation (@-mention)

**What:** Target a specific agent when you know exactly who should handle the task.

**How:**
```
@security-analyst scan all files in src/api/ for SQL injection vulnerabilities
```

**More examples:**
```
@architect design the data flow for a real-time chat feature
@tech-writer write JSDoc comments for all exported functions in src/utils/
@performance-optimizer analyze why the /api/products endpoint is slow
@devops-engineer create a Dockerfile for this project
```

**When to use:** When you want a specific expert's opinion, or when the orchestrator delegates to the wrong agent.

---

### 5.3 — Parallel Execution

**What:** Run multiple independent agents at the same time.

**How:**
```
Run these in parallel:
- @code-reviewer review src/api/
- @security-analyst audit src/api/
- @qa-expert check test coverage for src/api/
```

**What happens:** All three agents run simultaneously. Each returns an independent report. No agent blocks another.

**When to use:** For audits, reviews, and any tasks that don't depend on each other.

---

### 5.4 — Sequential Chaining

**What:** Run agents one after another, where each builds on the previous agent's work.

**How:**
```
Build a complete user profile feature:
1. @architect — design the architecture
2. @database-expert — create the schema
3. @backend-developer — build the API
4. @frontend-developer — build the page logic
5. @ui-designer — style the page
6. @qa-expert — test everything
```

**When to use:** For full-stack features where order matters.

---

### 5.5 — Background Execution

**What:** Run an agent in the background while you continue working.

**How:**
```
Run @qa-expert in the background to scan the entire project for bugs
```

**Alternative:** Press **Ctrl+B** to move a currently running task to the background.

**What happens:** The agent works silently. When it finishes, you get a notification with the results.

**When to use:** For long-running tasks like full project scans, security audits, or test suites.

---

### 5.6 — Viewing Available Agents

**What:** See all agents and their descriptions.

**How:** Type `/agents` in the chat.

**What you see:** An interactive menu listing all 11 agents with their descriptions. You can select one to invoke it.

---

## 6. Advanced Workflows

### 6.1 — Full Feature Development

**Scenario:** You need to build a complete "Shopping Cart" feature.

```
I need a shopping cart feature:
- Users can add/remove products
- Cart persists across sessions
- Checkout calculates total with tax
- Responsive cart sidebar UI
```

**Agent flow:**
```
architect          → Designs the overall approach
  ↓
database-expert    → Creates cart_items table, indexes
  ↓
backend-developer  → Builds CRUD endpoints for cart
  ↓
frontend-developer → Builds cart state management, API hooks
  ↓
ui-designer        → Creates cart sidebar, animations
  ↓
qa-expert          → Tests all flows, edge cases
```

---

### 6.2 — Bug Investigation & Fix

**Scenario:** Users report that checkout sometimes fails.

```
Users are reporting intermittent checkout failures. Investigate and fix.
```

**Agent flow:**
```
qa-expert              → Reproduces the bug, identifies the failing path
  ↓
security-analyst       → Checks if it's a security issue (parallel with above)
  ↓
backend-developer      → Fixes the root cause
  ↓
qa-expert              → Verifies the fix
```

---

### 6.3 — Pre-Release Audit

**Scenario:** You're about to release v2.0 and want a full quality check.

```
Run a full pre-release audit on the entire codebase.
```

**Agent flow (all in parallel):**
```
code-reviewer          → Code quality report
security-analyst       → Security vulnerability report
qa-expert              → Test coverage report
performance-optimizer  → Performance bottleneck report
```

All four reports come back independently. Review and address findings before release.

---

### 6.4 — Onboarding Documentation

**Scenario:** A new developer is joining the team and needs documentation.

```
@tech-writer create comprehensive onboarding documentation for this project,
including: architecture overview, setup guide, API reference, and coding conventions.
```

---

## 7. Agent Reference Card

### Agents That WRITE Code

| Agent | Specialization | Best For |
|---|---|---|
| `frontend-developer` | React, TypeScript, hooks, state | Page logic, data fetching, forms |
| `ui-designer` | TailwindCSS, components, layouts | Visual design, styling, responsiveness |
| `backend-developer` | Express, middleware, APIs | Endpoints, auth, server logic |
| `database-expert` | PostgreSQL, SQL, migrations | Schema design, queries, indexes |
| `devops-engineer` | Docker, CI/CD, deployment | Infrastructure, pipelines, configs |
| `performance-optimizer` | Profiling, caching, optimization | Speed improvements, bundle size |
| `tech-writer` | Documentation, JSDoc | API docs, README, guides |

### Agents That ADVISE Only (Read-Only)

| Agent | Specialization | Output |
|---|---|---|
| `architect` | System design, scalability | Design documents, recommendations |
| `code-reviewer` | Best practices, code quality | Review reports with severity levels |
| `security-analyst` | OWASP, vulnerability scanning | Security audit reports |
| `qa-expert` | Testing, bug hunting | Bug reports, coverage analysis |

---

## 8. Memory System

### How It Works

Each agent has a memory file at `.claude/agent-memory/<agent-name>/MEMORY.md`.

| Event | What Happens |
|---|---|
| Agent starts a task | Reads its memory file for past context |
| Agent finishes a task | Updates memory with new insights |
| New project | Memory starts empty, builds over time |

### What Gets Remembered

| Agent | Remembers |
|---|---|
| `architect` | Architecture decisions, trade-offs, patterns |
| `frontend-developer` | State patterns, API conventions, component structure |
| `ui-designer` | Design decisions, color schemes, component library |
| `backend-developer` | API patterns, middleware conventions, auth flows |
| `database-expert` | Schema history, index strategies, migration decisions |
| `devops-engineer` | Infrastructure configs, deployment steps, env vars |
| `code-reviewer` | Coding standards, recurring issues |
| `security-analyst` | Known vulnerabilities, security patterns |
| `performance-optimizer` | Benchmarks, optimization history, bottlenecks |
| `qa-expert` | Bug patterns, fragile areas, test strategies |
| `tech-writer` | Documentation style, terminology, gaps |

### Manually Triggering Memory Save

If an agent didn't automatically save its findings:
```
@qa-expert save what you learned from this session to your memory
```

---

## 9. Customizing for Your Project

### Step 9.1 — Update Tech Stack

Edit `CLAUDE.md` and change the Tech Stack section:

```markdown
## Tech Stack
- Frontend: Vue 3, TypeScript, Vuetify
- Backend: Python, FastAPI
- Database: MongoDB
```

### Step 9.2 — Update Agent Prompts (Optional)

If your stack is very different (e.g., Python instead of Node.js), update the agent prompts in `.claude/agents/` to match. For example, change `backend-developer.md` to reference FastAPI instead of Express.

### Step 9.3 — Add a New Agent

Create a file in `.claude/agents/` with this structure:

```markdown
---
name: my-new-agent
description: When to use this agent (the orchestrator reads this to decide delegation).
model: sonnet
tools: Read, Write, Edit, Bash, Glob, Grep
---
You are a senior [role]. Your expertise is [domain].

# Persistent Memory
Before starting any task, read `.claude/agent-memory/my-new-agent/MEMORY.md`.
When done, update it with new insights.

# Execution Flow
1. **Load Memory**
2. **Explore existing code**
3. **Execute task**
4. **Save Memory**

# Guidelines
- Specific rules for this agent
```

Then create the memory directory:
```bash
mkdir -p .claude/agent-memory/my-new-agent
```

And create an empty `MEMORY.md` in that directory.

### Step 9.4 — Remove an Agent

Delete its file from `.claude/agents/` and optionally remove its memory directory.

---

## 10. Troubleshooting

| Problem | Solution |
|---|---|
| Agent not found | Check that the `.md` file exists in `.claude/agents/` and has correct YAML frontmatter |
| Wrong agent picks up the task | Use `@agent-name` to explicitly target the right agent |
| Agent modifies code it shouldn't | Verify the agent's `tools` field doesn't include `Write` or `Edit` |
| Agent doesn't remember past context | Ask the agent to "read your memory file first", or check that the memory file path is correct in the agent prompt |
| Agent response is too slow | Change `model: opus` to `model: sonnet` for faster responses (at the cost of depth) |
| Subtree pull conflicts | Resolve merge conflicts in `.claude/` directory, keep your memory files |
| Agent generates wrong tech stack code | Update the agent's `.md` file and `CLAUDE.md` to match your actual stack |

---

*Last updated: March 2026*
