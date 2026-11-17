# Agentic SDLC in VS Code: Participant Workshop

Build **DevFlow**, a GitHub Copilot workflow that takes a feature through **plan, implement, test, review, and ship preparation**, with your approval at every stage.

This is the participant guide for **GitHub Copilot Business in VS Code**. You do not need Copilot CLI, the GitHub Copilot app, a coding agent, or a GitHub Enterprise license.

## Start Here

1. **Before the session:** complete the [pre-work](labs/agentic-sdlc-vscode-prework.md), about 20-30 minutes.
2. **During the session:** follow the [hands-on lab](labs/agentic-sdlc-vscode.md) in order.
3. **Need to catch up?** Use the lab's [solution files](labs/agentic-sdlc-vscode.md#solution-files) and [troubleshooting](labs/agentic-sdlc-vscode.md#troubleshooting).
4. **Not sure it worked?** Compare your run with the [What to Look For](what-to-look-for/README.md) screenshots. They show the steps between your prompt and the answer: which instructions loaded, which skills and subagents ran, and which MCP tools were called.

### Which Repository Do I Work In?

**This repository contains the workshop instructions, not the application.** Read these pages on GitHub; you do not need to clone this guide.

Create your own copy of the public [dotnet-react-starter-demo template](https://github.com/copilot-academy/dotnet-react-starter-demo) using **Use this template > Create a new repository**, then clone **your copy** and open its root folder in VS Code. Keep the name `dotnet-react-starter-demo` so the lab's commands match.

Run the exercises and create all instructions, agents, skills, tests, and feature code **inside your copy of that starter app**, not in this participant guide. Part 11 explicitly creates a separate local `plugin-marketplace` folder.

## Prerequisites

| Requirement | Notes |
|---|---|
| GitHub account with Copilot Business | Signed in to Copilot in the latest stable VS Code |
| Git | Needed to clone the starter and create your local feature branch |
| .NET 8 SDK | Backend and generated tests |
| Node 24 | Frontend, skill scripts, and Playwright MCP |
| Chrome, or Edge on Windows | Browser checks in Part 6 |
| Organization MCP policy | Ask your admin to enable **MCP servers in Copilot** for Part 6 |
| GitHub CLI 2.90.0+ (optional) | Community skill installation; manual clone alternatives are included |

The [pre-work](labs/agentic-sdlc-vscode-prework.md) includes Windows PowerShell and macOS setup, app startup, and a readiness checklist. The API runs at `http://localhost:5000` and the UI at `http://localhost:5173`.

Use **Manual permissions** and the **Copilot** session target by default. Switch to **Local** only where the lab says to, such as for `/pr-description`. Model availability depends on your organization's policies; the lab includes fallbacks.

## What You Will Build

- Lean repository-wide and path-specific custom instructions.
- A community documentation skill and custom task-breakdown and naming-checker skills.
- Planner, Tester, and Code Reviewer agents, coordinated by DevFlow.
- Playwright MCP browser checks, with tools available to DevFlow for delegation to the Tester.
- A local feature branch, reviewed commits, and a reusable `/pr-description` prompt.
- Optional governance hooks, a model-effort comparison, and a shareable plugin.

**Code Reviewer keeps `read`, `search`, and `execute` for Git inspection.** Its instructions prohibit changes, but `execute` is not a read-only sandbox: review command approvals.

## Workshop Route

| Parts | Focus |
|---|---|
| 0-1 | Setup check and customization concepts |
| 2-5 | Instructions, skills, and Code Reviewer |
| 6-8 | Playwright MCP, DevFlow, and local ship preparation |
| 9-11 | Optional/reference: hooks, model strategy, and sharing |

The scheduled session is 2:00pm-5:00pm, including sharing, demos, and a break. Follow the session's pacing; Parts 9-11 are optional/reference rather than guaranteed live exercises. Working through every lab part takes about **2 h 25 min**, excluding pre-work; allow **20-30 minutes** for the DevFlow pipeline.

The sample is .NET 8 + React, but the workflow lessons are host-agnostic. Opening a GitHub pull request or GitLab merge request is an optional concept, not required to finish the hands-on work.

## Included Materials

| Guide | Contents |
|---|---|
| [Pre-work](labs/agentic-sdlc-vscode-prework.md) | Accounts, tools, starter setup, and readiness checks |
| [Hands-on lab](labs/agentic-sdlc-vscode.md) | Parts 0-11, troubleshooting, and copyable solution files |
| [What to Look For](what-to-look-for/README.md) | Annotated screenshots from a dry run, showing the progress steps to check in each part |

All required examples and diagrams are inline in the lab. This edition intentionally excludes presenter decks and notes, internal handoffs, and unrelated demo repositories. No access to the source workshop repository or a private issue tracker is required.

## Credits and Licensing

Adapted from the [Copilot Academy customization lab](https://github.com/copilot-academy/copilot-academy.github.io/blob/main/labs/customization-in-90-minutes.md).

This participant edition does not introduce a new license grant. The starter app and linked community skills retain their own licenses and attribution; check those terms before redistributing their content.
