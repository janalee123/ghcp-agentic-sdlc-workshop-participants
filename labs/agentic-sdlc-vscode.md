# Lab: Agentic SDLC in VS Code

> **Duration:** ~2 h 25 min for all parts, including a 7-minute buffer | **Level:** Intermediate | **Format:** Guided, hands-on | **Prerequisites:** the [pre-work](./agentic-sdlc-vscode-prework.md), [GitHub Copilot Business](https://github.com/features/copilot/plans) on github.com, VS Code with GitHub Copilot, .NET 8, Node 24, and a copy of the [dotnet-react-starter-demo repo](https://github.com/copilot-academy/dotnet-react-starter-demo)

> **Workspace:** follow these instructions in your own copy of the starter app, not in this participant-guide repository. See [Start Here](../README.md#start-here).

<a id="contents"></a>

## Contents

**How to use this lab**

* Finish the [pre-work](./agentic-sdlc-vscode-prework.md) before the session.
* Work through the parts in order. Each part builds on the previous one.
* **Legend:** 🛠️ Hands-on = do it in VS Code. 📖 Read = the facilitator presents it; skim and come back later. 💡 Concept = read only, no steps.
* At the end of each part, use the **Next** link. Fell behind? Each hands-on part ends with a **Catch up** tip that links to the [Solution files](#solution-files).
* Stuck? See [Troubleshooting](#troubleshooting). Prefer a sidebar? Select the **Outline** button (☰) at the top of the file on GitHub.

| Section | Type |
|---------|------|
| [Objective](#objective), [Agenda](#facilitator-agenda), [What You'll Build](#what-youll-build) | 📖 Read |
| [Part 0: Prerequisites](#part-0-prerequisites) | ✅ Setup check |
| [Part 1: Why Customize?](#part-1-why-customize-10-min) | 📖 Read (facilitator-led) |
| [Part 2: Custom Instructions: The Foundation](#part-2-custom-instructions-the-foundation-15-min-all-stages) | 🛠️ Hands-on |
| [Part 3: Discover & Reuse Community Skills](#part-3-discover--reuse-community-skills-7-min) | 🛠️ Hands-on |
| [Part 4: Building Skills with the Skill Creator](#part-4-building-skills-with-the-skill-creator-15-min) | 🛠️ Hands-on |
| [Part 5: Your First Agent: Code Reviewer](#part-5-your-first-agent-code-reviewer-10-min-review-stage) | 🛠️ Hands-on |
| [Part 6: MCP Server: A Real Browser for the Test Stage](#part-6-mcp-server-a-real-browser-for-the-test-stage-10-min-test) | 🛠️ Hands-on |
| [Part 7: The DevFlow Orchestrator](#part-7-the-devflow-orchestrator-17-min-plan--implement--test--review) | 🛠️ Hands-on |
| [Part 8: Ship: Branch, Commit, MR/PR Description](#part-8-ship-branch-commit-mrpr-description-12-min-ship) | 🛠️ Hands-on + 💡 concept |
| [Part 9 (Optional): Governance with Agent Hooks](#part-9-optional-governance-with-agent-hooks-5-min) | 🛠️ Hands-on (optional) |
| [Part 10: Model Strategy with Copilot Fusion](#part-10-model-strategy-with-copilot-fusion-15-min) | 📖 Read + 🛠️ Hands-on |
| [Part 11: Package & Share](#part-11-package--share-8-min) | 🛠️ Hands-on + 💡 concept |
| [Troubleshooting](#troubleshooting) | Reference |
| [Agentic SDLC Recap](#agentic-sdlc-recap), [Next Steps](#next-steps), [Additional Resources](#additional-resources) | 📖 Read |
| [Appendix: Solution Files](#solution-files) | Reference (catch up) |

<a id="objective"></a>

## Objective

In this lab you will build **DevFlow**, an **agentic SDLC pipeline** that runs entirely inside **GitHub Copilot in VS Code**. DevFlow moves a feature through five stages: **plan → implement → test → review → ship**. Specialized agents do the work. You approve every stage gate.

```
  ┌────────┐   ✋   ┌───────────┐   ✋   ┌────────┐   ✋   ┌──────────┐   ✋   ┌────────┐
  │  PLAN  │ ─────► │ IMPLEMENT │ ─────► │  TEST  │ ─────► │  REVIEW  │ ─────► │  SHIP  │
  │Planner │        │  DevFlow  │        │ Tester │        │ Reviewer │        │  You + │
  │+skill  │        │  +skills  │        │+MCP    │        │          │        │ prompt │
  └────────┘        └───────────┘        └────────┘        └──────────┘        └────────┘
      ✋ = human-in-the-loop checkpoint: the agent stops and waits for your approval
  ═══════════════ custom instructions: the foundation under every stage ═══════════════
```

Along the way you will:

* Map every way to customize Copilot in VS Code by **scope** (personal, workspace, team).
* Lay the foundation with **custom instructions**: a lean repo-wide file plus path-specific rules for backend and frontend.
* Discover and install community skills.
* Use the **skill-creator** meta-skill to build your own skills.
* Build custom agents and an orchestrator.
* Give the Test stage a real browser with the **Playwright MCP server**.
* Pick the right model for each stage.
* Ship the change with a branch, a commit, and a reusable **prompt file** that drafts your merge request or pull request description.
* Optionally, add a governance hook.
* Learn where your tokens go.
* Package everything as a plugin your team can install.

> [!NOTE]
> **Tooling scope for this lab**
>
> Everything in this lab runs in **VS Code**:
>
> * The **Chat view** and the **Agent Customizations editor**.
> * The **Source Control** view.
> * The **integrated terminal**, which runs ordinary developer tools such as `git`, `gh`, `dotnet`, and `npm`.
>
> You do **not** need Copilot CLI, the GitHub Copilot app, or a GitHub Enterprise license. Your Copilot license still comes from your GitHub account, so you sign in to Copilot in VS Code with GitHub.

> [!NOTE]
> **Works with GitLab or GitHub**
>
> The lab's sample repo lives on GitHub, but the SDLC lessons are **host-agnostic**. Your day-to-day code might live on **GitLab**. Every hands-on step uses local Git and VS Code. Host-specific steps, like opening a **merge request** (GitLab) or **pull request** (GitHub), connecting an issue tracker over MCP, or publishing a shared plugin repo, are covered as **concepts**.

> [!NOTE]
> **Copilot in VS Code is evolving fast**
>
> VS Code ships monthly, and Copilot features and UI labels change often. If something looks different, check the [VS Code agent customization docs](https://code.visualstudio.com/docs/agent-customization/overview) and the [GitHub Copilot docs](https://docs.github.com/en/copilot).

<a id="facilitator-agenda"></a>

### Agenda

| Part | Topic | SDLC stage | Time |
|------|-------|------------|------|
| 0 | Prerequisites: [pre-work page](./agentic-sdlc-vscode-prework.md) before the session, then a quick check and the session-target box | — | pre-work + 2 min check (inside Part 1's time) |
| 1 | Why customize, ways to customize Copilot in VS Code, and the agentic SDLC | — | 10 min |
| 2 | Custom instructions: the foundation | All stages | 15 min |
| 3 | Discover & reuse community skills | Plan / Ship (docs) | 7 min |
| 4 | Build skills with the skill-creator | Plan / Implement | 15 min |
| 5 | Your first agent: Code Reviewer | Review | 10 min |
| 6 | MCP server: Playwright for browser testing | Test | 10 min |
| 7 | DevFlow orchestrator: Planner, Tester, stage gates | Plan → Implement → Test → Review | 25 min (20–30) |
| 8 | Ship: branch, commit, `/pr-description` prompt file (opening the MR/PR is a concept) | Ship | 12 min |
| 9 | *(Optional)* Governance with agent hooks | All stages | 5 min |
| 10 | Model strategy with Copilot Fusion (context check, effort levels per stage, hands-on comparison) | Implement / Review | 15 min |
| 11 | Package & share as a plugin (11.2 APM is optional) | — | 8 min |
| — | **Buffer / catch-up.** Part 7 timing varies with the model and agent speed, so spend this where the group needs it | — | 7 min |
| — | Agentic SDLC recap & wrap-up | — | 4 min |
| | **Total** | | **~2 h 25 min** (about 140 min without Part 9) |

> [!TIP]
> **Pacing and catch-up**
>
> Follow the session's pacing. Parts 9–11 are optional/reference if time is short. Part 10.3 also depends on the models your organization allows; use its fallback if Fusion High is unavailable.
>
> Behind? Use the **Catch up** tip at the end of Parts 2, 4, 5, 6, and 7. Each links to the [Solution files](#solution-files) appendix. The estimates above cover the full lab, not the session's separate sharing, demo, and break time.

<a id="what-youll-build"></a>

## What You'll Build

```
                         ┌───────────────────────┐
                         │       DevFlow         │
                         │  Orchestrator agent   │
                         │  (stage gates ✋)     │
                         └───────────┬───────────┘
                                     │  delegates via subagents
         ┌──────────────────────┬────┴──────────────┬──────────────────────┐
         │                      │                   │                      │
  ┌──────▼───────┐     ┌────────▼──────┐     ┌──────▼───────┐       ┌──────▼───────┐
  │ Planner      │     │ IMPLEMENT     │     │ Tester       │       │ Code Reviewer│
  │ (PLAN)       │     │ (DevFlow +    │     │ (TEST)       │       │ (REVIEW)     │
  │ skill:       │     │  you)         │     │ MCP:         │       │ review-only  │
  │ task-        │     │ skill:        │     │ playwright/* │       │              │
  │ breakdown    │     │ naming-checker│     │              │       │              │
  └──────────────┘     └───────────────┘     └──────────────┘       └──────────────┘
                                     │
                              ┌──────▼───────┐
                              │ SHIP (you)   │  branch → commit → /pr-description
                              │ Source       │  → open MR/PR on your Git host
                              │ Control      │    (GitLab or GitHub, concept)
                              └──────────────┘

  Foundation under every stage: .github/copilot-instructions.md
                                + .github/instructions/*.instructions.md (applyTo)
  Community skill (installed via gh skill): documentation-writer
```

By the end of the lab, you will have:

- ✅ **Lean custom instructions**: one repo-wide `copilot-instructions.md` plus two path-specific `.instructions.md` files (backend C# and frontend React)
- ✅ **A community skill**: `documentation-writer`, discovered, previewed, and installed with `gh skill`
- ✅ **2 custom skills**: `task-breakdown` and `naming-checker`, built with the skill-creator
- ✅ **3 custom agents**: `Planner`, `Tester`, and `Code Reviewer`
- ✅ **An MCP server**: Playwright in `.vscode/mcp.json`, available to DevFlow for delegation to the `Tester` agent
- ✅ **1 orchestrator**: a `DevFlow` agent that runs the SDLC with human-in-the-loop stage gates
- ✅ **A prompt file**: `/pr-description`, which drafts a merge request or pull request description from your branch
- ✅ **A committed feature branch**: ready to push to GitLab or GitHub and open as an MR/PR
- ✅ **Per-agent model selection**: a right-sized model for each stage
- ✅ *(Optional)* **A governance hook**: blocks risky commands before an agent runs them
- ✅ **A Copilot Fusion effort-level comparison**: compare Fusion Standard with Fusion High or Max, see which execution pattern (Single, Cascade, or Critique) it picks for a review-heavy change, and what it costs
- ✅ **A shareable plugin**: `plugin.json` packaging that your team can install in VS Code

**Next:** [Part 0: Prerequisites →](#part-0-prerequisites) · [Back to contents](#contents)

<a id="part-0-prerequisites"></a>

## Part 0: Prerequisites

<a id="accounts-and-vs-code"></a>

### Accounts and VS Code

* A github.com account with **Copilot Business**. On Copilot Business, your organization admin controls which models and features you can use. If you can't see something in this lab, ask your admin.
* The latest stable [VS Code](https://code.visualstudio.com/), signed in to GitHub, with GitHub Copilot enabled.
* *(Optional)* [GitHub CLI](https://cli.github.com/) **2.90.0 or later**, signed in with `gh auth login`, for `gh skill` in Part 3. This is the general-purpose GitHub CLI, not Copilot CLI. It only *reads* public skill repos. If you skip `gh`, Part 3 includes a manual `git clone` alternative.
* Node 24, installed in the pre-work. The Playwright MCP server in Part 6 needs Node 18 or later to start with `npx`.
* **Google Chrome** (or, on Windows, Microsoft Edge). Playwright MCP opens a real browser in Part 6. See [Playwright Can't Open a Browser](#troubleshooting-playwright-browser) if neither is installed.
* **Copilot Business only:** The **MCP servers in Copilot** policy is **disabled by default** for organizations. Ask your org admin to enable it before the session, or Part 6 won't work. See [Extending Copilot Chat with MCP](https://docs.github.com/en/copilot/how-tos/provide-context/use-mcp-in-your-ide/extend-copilot-chat-with-mcp).

<a id="get-the-sample-app"></a>

### Get the Sample App and Tools

Complete the **[Agentic SDLC in VS Code: Pre-work](./agentic-sdlc-vscode-prework.md)** before the session. It takes about 20–30 minutes and has macOS and Windows steps for:

* Copying the [dotnet-react-starter-demo repo](https://github.com/copilot-academy/dotnet-react-starter-demo) to your own GitHub account with **Use this template**, cloning it, and opening it in VS Code.
* Installing .NET 8 and Node 24.
* Installing and signing in to the GitHub CLI (optional, for Part 3).
* Running the backend and frontend.

> [!TIP]
> **Use the VS Code integrated terminal**
>
> Every command in this lab runs in the **VS Code integrated terminal**. Open it with **Terminal → New Terminal**, or press `` Ctrl+` ``. Use the **+** button to open more terminals, for example one for the backend and one for the frontend.

<a id="prework-check"></a>

### Pre-work Check

At the start of the session, confirm each item. If one fails, go back to the [pre-work](./agentic-sdlc-vscode-prework.md) or see [Troubleshooting](#troubleshooting).

- [ ] `dotnet --version` prints `8.x`.
- [ ] `node --version` prints `v24.x`.
- [ ] `gh auth status` shows you're signed in, **or** you'll use the manual `git clone` path in Part 3.
- [ ] Your copy of the sample repo is open in VS Code as the workspace folder.
- [ ] The backend (http://localhost:5000) and frontend (http://localhost:5173) are running.
- [ ] The Chat view opens and you're signed in to Copilot.
- [ ] In the chat **model picker**, **Copilot Fusion** is selected with the **Fusion Standard** effort level. This is the default model for the whole lab. Steps that use a different model say so.
- [ ] **Copilot Business only:** your admin has enabled the **MCP servers in Copilot** policy.

<a id="session-target"></a>

### Copilot vs Local: Which Session Target?

The **Session Target** picker in the Chat view chooses which *agent harness* runs your chat session. You only need two of them in this lab:

| Session Target | Use it for | In this lab |
|----------------|-----------|-------------|
| **Copilot** | Everything by default: instructions, skills, custom agents, subagents, MCP servers, and hooks | **Default.** Use it unless a step says otherwise |
| **Local** (VS Code's built-in harness) | Features that only the Local harness supports: prompt files, the `/init` command, **Configure Tools** in the chat input, and the `argument-hint` and `handoffs` agent fields | Only when a step tells you to, for example running `/pr-description` in Part 8 |

> [!TIP]
> **If something seems to be missing, check the Session Target first**
>
> A slash command that isn't listed, a tool picker that looks different, or a hook that doesn't fire is often just the wrong harness. Switch back to **Copilot** after any step that uses **Local**.

---

**Next:** [Part 1: Why Customize? →](#part-1-why-customize-10-min) · [Back to contents](#contents)

<a id="part-1-why-customize-10-min"></a>

## Part 1: Why Customize? (~10 min)

> [!NOTE]
> **Read or follow along.** Skim Part 1 now and come back to its tables and diagrams as a reference. No slide deck is needed. The hands-on work starts in [Part 2](#part-2-custom-instructions-the-foundation-15-min-all-stages).

Before you build anything, look at *why* teams invest in customization. These are the challenges teams most often report:

| Challenge | Symptom | Customization Solution |
|-----------|---------|----------------------|
| **Generic responses** | Copilot ignores your frameworks and coding standards | Custom Instructions |
| **Repetitive prompting** | Developers retype the same context every session | Prompt Files |
| **Inconsistent quality** | Different team members get different results | Skills with standardized workflows |
| **Context window waste** | Large instruction files load on every request | Skills (on-demand) vs Instructions (always-on) |
| **No institutional knowledge** | Copilot doesn't know your team conventions | Custom Agents encoding expertise |
| **Tool sprawl** | Developers switch between Copilot and external tools | MCP Servers bringing tools into Copilot |
| **No guardrails for agents** | Agents might run risky commands unattended | Hooks (deterministic policy) + human checkpoints |
| **Can't share customizations** | Each developer reinvents the wheel | Plugins or APM for packaging and distribution |
| **Cost/performance concerns** | Higher-cost models used for simple tasks | Per-agent model selection + Copilot Fusion |

<a id="11-the-agentic-sdlc-in-vs-code"></a>

### 1.1 The Agentic SDLC in VS Code

An *agentic* SDLC doesn't take you out of the loop. Agents do the repetitive work at each stage, and **you approve the transitions**. In VS Code, each stage maps to a customization primitive:

| SDLC stage | What the agent does | Human-in-the-loop checkpoint | VS Code primitive |
|------------|--------------------|------------------------------|-------------------|
| **Foundation** (all stages) | Follows project and path-specific conventions automatically | You review and trim the instructions | `.github/copilot-instructions.md` + `.github/instructions/*.instructions.md` |
| **Plan** | Decomposes the feature into sized tasks | You approve the plan | `Planner` custom agent + `task-breakdown` skill (an issue-tracker MCP server is a concept) |
| **Implement** | Edits code, follows conventions | You review diffs (Keep/Undo) and approve moving to test | `DevFlow` agent + `naming-checker` skill + path-specific instructions |
| **Test** | Writes and runs tests, then checks the UI in a real browser | You check results | `Tester` custom agent (subagent) + Playwright MCP server |
| **Review** | Finds bugs, security, and performance issues | You decide which findings to fix | `Code Reviewer` custom agent (subagent, review-only with Git inspection) |
| **Ship** | Prepares docs, a commit message, and an MR/PR description | **You** create the branch, commit, and open the MR/PR on your Git host | `documentation-writer` skill + `/pr-description` prompt file + Source Control view |
| **Guardrails** | Blocks risky tool calls deterministically | Policy, not prompts | Agent hooks in `.github/hooks/` (Preview) |

<a id="12-ways-to-customize-copilot-in-vs-code"></a>

### 1.2 Ways to Customize Copilot in VS Code

Every customization lives at a **scope**. The scope decides who gets it and where it applies. Choose the narrowest scope that fits:

* **Personal (user):** your preferences, across all your workspaces.
* **Workspace (repo):** committed with the project and shared with everyone who clones it.
* **Team / org:** packaged and distributed across many repos.

```
┌──────────────────────────────────────────────────────────────────────────┐
│ TEAM / ORG   Agent plugins (marketplace = any Git repo) · APM (apm.yml)  │
│              GitHub org custom instructions (Copilot Business orgs)       │
├──────────────────────────────────────────────────────────────────────────┤
│ WORKSPACE    .github/copilot-instructions.md · AGENTS.md                  │
│ (this repo)  .github/instructions/*.instructions.md (applyTo)             │
│              .github/prompts/*.prompt.md · .github/skills/*/SKILL.md      │
│              .github/agents/*.agent.md · .vscode/mcp.json                 │
│              .github/hooks/*.json                                         │
├──────────────────────────────────────────────────────────────────────────┤
│ PERSONAL     ~/.copilot/copilot-instructions.md · ~/.copilot/instructions/│
│ (just you)   ~/.copilot/skills/ · ~/.copilot/agents/                      │
│              ~/.copilot/mcp-config.json · VS Code user settings           │
└──────────────────────────────────────────────────────────────────────────┘
```

| Customization | Scope → file location | When to use it | Covered in |
|---------------|-----------------------|----------------|-----------|
| **Repo-wide instructions** | Workspace: `.github/copilot-instructions.md` | Short, always-true project facts: stack, build/test commands, key conventions | Part 2 |
| **`AGENTS.md`** | Workspace: `AGENTS.md` at the repo root (and in subfolders) | The same role as above, in a format that many AI coding tools read. Useful when your team mixes tools | Part 2 |
| **Path-specific instructions** | Workspace: `.github/instructions/*.instructions.md` with `applyTo` glob | Rules that only matter for some files, such as C# backend vs React frontend | Part 2 |
| **Personal instructions** | Personal: `~/.copilot/copilot-instructions.md` and `~/.copilot/instructions/` | Your own preferences, such as answer style or tone, that shouldn't be imposed on the team | Part 2 |
| **Organization instructions** | Team: configured by an org admin on GitHub; VS Code reads them when `github.copilot.chat.organizationInstructions.enabled` is on | Company-wide rules for every repo | Concept |
| **Prompt files** | Workspace: `.github/prompts/*.prompt.md`; Personal: your VS Code profile | A saved task you trigger yourself with `/name`. **Local harness only** | Part 8 |
| **Skills** | Workspace: `.github/skills/<name>/SKILL.md`; Personal: `~/.copilot/skills/` | A repeatable multi-step workflow, optionally with scripts and templates, loaded on demand | Parts 3–4 |
| **Custom agents** | Workspace: `.github/agents/*.agent.md`; Personal: `~/.copilot/agents/` | A role with its own instructions, tools, and model, usable as a subagent | Parts 5, 7 |
| **MCP servers** | Workspace: `.vscode/mcp.json`; Personal: `~/.copilot/mcp-config.json` (via **MCP: Add Server**) | Give the agent tools for an external system: browser, database, issue tracker | Part 6 |
| **Hooks** (Preview) | Workspace: `.github/hooks/*.json` | A rule that must run at a lifecycle event, regardless of what the model decides | Part 9 |
| **Agent plugins** (Preview) | Team: a plugin marketplace Git repo, or a local folder via `chat.pluginLocations` | Install a bundle of agents, skills, hooks, and MCP servers as one unit | Part 11 |
| **APM** | Team: `apm.yml` + `apm.lock.yaml` in each consuming repo | Reproducible, version-locked agent setup across many repos, from any Git host | Part 11.2 |

**VS Code settings and controls that shape every session:**

| Control | Where | What it does |
|---------|-------|--------------|
| **Session Target** (harness) | Chat view | **Copilot** (recommended default) or **Local** (VS Code's built-in harness). Some customizations, such as prompt files, only run in **Local**. See [Copilot vs Local](#session-target) |
| **Agent picker** | Chat input | Switch between built-in agents (**Agent**, **Ask**, **Plan**) and your custom agents. In the Copilot harness, you can also start Plan by typing `/plan` |
| **Permissions picker** and **Autopilot** | Chat input | **Manual permissions** (default) asks before actions that aren't auto-approved. **Assisted permissions** uses a model to judge each tool call. **Allow all** skips confirmations. **Autopilot** (a mode in the Copilot harness) works on its own until done and answers its own questions. Use **Manual** in this lab. See [Manage approvals and permissions](https://code.visualstudio.com/docs/agents/run/approvals) |
| **Model picker** | Chat input | Choose **Copilot Fusion** (with an effort level) or a specific model allowed by your plan or org |
| **Tools** | **Agent Customizations editor → Tools** (Copilot harness) or **Configure Tools** in the chat input (Local harness) | Turn built-in, extension, and MCP tools on or off. Fewer tools means fewer tokens. User-defined *tool sets* are deprecated and not supported in the Copilot harness, so use custom agents with a `tools:` list instead |
| **Approvals** | Settings: `chat.permissions.default`, `chat.tools.eligibleForAutoApproval`, `chat.tools.terminal.autoApprove`, `chat.tools.urls.autoApprove` | Decide which actions run automatically and which wait for your confirmation. These are your human-in-the-loop dial. See [Manage approvals and permissions](https://code.visualstudio.com/docs/agents/run/approvals) |
| **Generated text instructions** | Settings: `github.copilot.chat.commitMessageGeneration.instructions`, `github.copilot.chat.pullRequestDescriptionGeneration.instructions` | Extra guidance for **Generate Commit Message** and PR description generation |

> [!TIP]
> **See everything that's loaded**
>
> In the Chat view, select **Open Customizations** in the title bar, or run **Chat: Open Customizations** from the Command Palette. The **Agent Customizations editor** lists your agents, skills, instructions, prompts, hooks, MCP servers, and plugins in one place. To see what actually loaded for a session, run **Developer: Open Agent Debug Logs** and select the session.

> [!NOTE]
> **Harness compatibility**
>
> Not every customization type works in every harness or scope. For example, prompt files and tool sets are **Local**-harness features, and skills are the cross-harness alternative for reusable workflows. See [Customize agent behavior in VS Code](https://code.visualstudio.com/docs/agent-customization/overview) and [Understand agent customization](https://code.visualstudio.com/docs/agents/concepts/customization).

<a id="when-to-use-what"></a>

#### When to Use What

Use this decision matrix whenever you're not sure which customization type fits. Part 10 maps model choices onto the same SDLC stages.

| I need to... | Best Choice | Why Not the Others? |
|-------------|------------|-------------------|
| Set coding standards for the whole project | **Instructions** | Skills load and unload; agents are overkill for global rules; keep it lean by pointing to docs in the repo |
| Define how to handle a specific task | **Skill** | Instructions would waste tokens on every request |
| Create a specialized AI persona for an SDLC stage | **Agent** | Skills don't change who Copilot "is"; instructions are too broad |
| Connect to Jira, Slack, or a database | **MCP Server** | Other options can't reach external systems, but consider a skill + API/CLI |
| Run a saved, human-triggered task (like an MR/PR description) | **Prompt file** | You decide when it runs; use a skill instead if the agent should decide or you need it outside the Local harness |
| Enforce a rule no matter what the model decides | **Hook** | Other options rely on the LLM following instructions |
| Use a specific model per stage | **Agent `model` field** | Instructions and skills inherit the session model |

<a id="13-token-optimization-mental-model"></a>

### 1.3 Token Optimization Mental Model

Not everything belongs in a custom instruction. To customize efficiently, you need to know *where* tokens get consumed:

```
Token Impact by Customization Type
──────────────────────────────────────────────────────────────

  Hooks            │████                     Zero LLM tokens
                   │                         (scripts only)
                   │
  Skills           │████████                 On-demand: loaded
                   │                         only when relevant
                   │
  Custom Agents    │████████████             Separate context window
                   │                         (subagent isolation)
                   │
  MCP Servers      │████████████████         Tool definitions + results
                   │                         injected into context
                   │
  Instructions     │████████████████████     ALWAYS loaded, on every
                   │                         single request
──────────────────────────────────────────────────────────────
                   Low ◄──────────► High  (context window cost)
```

<a id="what-a-system-prompt-actually-looks-like"></a>

#### What a System Prompt Actually Looks Like

Copilot builds a fresh system prompt from several layers on every request. The diagram below shows example token costs for each layer:

* **Fixed costs:** the base system prompt and the tool definitions.
* **Variable costs:** instructions, agent definitions, and skills, depending on your customizations.
* **Growing costs:** conversation history and editor context, which grow as you work.

```
System Prompt Anatomy (assembled fresh on every request)
═══════════════════════════════════════════════════════════════════════

  ┌──────────────────────────────────────────────────────────────┐
  │ 1. BASE SYSTEM PROMPT                          [always]      │
  │    Copilot's built-in behavior, safety rules,                │
  │    tool-use instructions                      ~500 tokens    │
  ├──────────────────────────────────────────────────────────────┤
  │ 2. TOOL DEFINITIONS                            [always]      │
  │    Built-in tools (read, search, edit…)                      │
  │    + MCP server tools if configured                          │
  │    → 10 tools  ≈  500–1,000 tokens            variable       │
  │    → 100+ tools ≈ 5,000–10,000+ tokens ⚠️                    │
  ├──────────────────────────────────────────────────────────────┤
  │ 3. INSTRUCTIONS FILE                           [always]      │
  │    .github/copilot-instructions.md                           │
  │    Loaded on every single request             ~300–2,000     │
  ├──────────────────────────────────────────────────────────────┤
  │ 4. AGENT DEFINITION                    [if agent is active]  │
  │    .github/agents/my-agent.agent.md                          │
  │    Loaded when agent is selected or invoked   ~200–500       │
  ├──────────────────────────────────────────────────────────────┤
  │ 5. SKILL FRONTMATTER              [always: enables matching] │
  │    name + description for every skill in scope               │
  │    Copilot reads these to decide what to trigger  ~100/skill │
  │                                                              │
  │    SKILL BODY                          [only when triggered] │
  │    Full SKILL.md instructions + bundled scripts              │
  │    Loaded only when description matches the prompt ~400–600  │
  ├──────────────────────────────────────────────────────────────┤
  │ 6. CONVERSATION HISTORY                        [always]      │
  │    Prior messages in this session                            │
  │    Grows until compaction                     variable       │
  ├──────────────────────────────────────────────────────────────┤
  │ 7. EDITOR CONTEXT                              [always]      │
  │    Current file, selection, attached context  ~200–2,000     │
  ├──────────────────────────────────────────────────────────────┤
  │ 8. YOUR MESSAGE                                [always]      │
  │    The actual prompt you typed                variable       │
  └──────────────────────────────────────────────────────────────┘

  Layers 1–3 are the fixed tax on every request.
  Layers 4–5 are opt-in and only appear when relevant.
  Layers 6–7 grow as you work and are the main source of bloat.
```

Because layers 1–3 load on every request, a bloated instructions file costs you every time. Skills (layer 5) are surgical: they cost very little until they trigger.

> [!TIP]
> **The Rule of Thumb**
>
> | If it applies to... | Use this | Why |
> |---------------------|----------|-----|
> | **Every task** | Instruction | Always-on context (keep it lean) |
> | **A type of task** | Skill | Loaded on demand, so it costs little when unused |
> | **Who Copilot "is"** | Agent | Can get its own context window as a subagent (isolation) |
> | **External data/systems** | MCP Server | Brings APIs and databases into Copilot, but results consume tokens |
> | **A rule that must always hold** | Hook | Deterministic, runs outside the model |

<a id="14-choosing-the-right-model"></a>
<a id="14-choosing-the-right-model-pro-and-business"></a>

### 1.4 Choosing the Right Model

Copilot usage is measured in **GitHub AI Credits**. Every interaction consumes **input**, **output**, and **cached** tokens. Each model has its own per-token rates, and GitHub converts the resulting cost into AI credits. A long agent session therefore costs more than a short chat, even on the same model. Code completions and next edit suggestions aren't billed in AI credits and stay unlimited on paid plans.

On **Copilot Business**, your organization manages included usage, budgets, and which models you can use. See [Billing for organizations](https://docs.github.com/en/copilot/concepts/billing-and-usage/organizations-and-enterprises/billing) and ask your org admin if a model is missing from your picker.

| Task Complexity | Model class (pick from *your* model picker) | Cost pattern | Example SDLC stages |
|----------------|-------------------|--------------------------|---------------|
| **Simple** | Small/fast models (for example, Haiku-class or "mini" models) | Lower token rates | Test generation, scaffolding, formatting |
| **Balanced** | General-purpose models (for example, Sonnet-class) | Balanced capability and cost | Code review, refactoring, documentation |
| **Complex** | Frontier reasoning models (for example, Opus-class) | Higher token rates, and complex sessions use more tokens | Architecture, hard debugging |
| **Unpredictable** | **Copilot Fusion** | Copilot routes each request to the right model(s). Billed per model used | Planning, orchestration, everyday work |

Check [Models and pricing for GitHub Copilot](https://docs.github.com/en/copilot/reference/copilot-billing/models-and-pricing) for current rates. Don't rely on fixed multipliers. Model availability changes, and on Business it also depends on your org's policies.

<a id="14-copilot-fusion"></a>
<a id="14-hydrafusion"></a>

#### Copilot Fusion: let Copilot route and orchestrate models

You have two ways to choose a model: **pin a specific model**, or select **Copilot Fusion** in the Chat **model picker**. Copilot Fusion isn't a model. It's a runtime router and orchestrator that, for each request, picks the models to use and an *execution pattern*, then returns a single response. In VS Code it replaces the older **Auto** option, and it brings together Auto's model routing and the multi-model orchestration from the HydraFusion research preview ([Copilot Fusion docs](https://docs.github.com/en/copilot/concepts/models/copilot-fusion)).

After you select **Copilot Fusion**, choose an **effort level**:

| Effort level | Priority | Typical use |
|--------------|----------|-------------|
| **Fusion Standard** | Balances cost, quality, and latency. Picks one model per task | Everyday work |
| **Fusion High** | Balanced orchestration with moderate token savings | Substantial, well-scoped tasks, such as a complex bug or a change across several files |
| **Fusion Max** | Frontier capability, still token-aware | Tasks that need the deepest reasoning |

Depending on the effort level and the task, Copilot Fusion uses one of three execution patterns, and it can choose a different one for each prompt in the same session:

| Execution pattern | What happens | Rough SDLC analogy |
|-------------------|--------------|--------------------|
| **Single** | One model completes the task. | A normal request |
| **Cascade** | A solver model completes the task, a judge model reviews the result, and the solver fixes it only if the judge finds problems. | A built-in review pass with low overhead |
| **Critique** | One model drafts, other models critique it in parallel, and the drafting model revises. | A built-in review loop at IMPLEMENT |

**Billing:** you're billed for each model Copilot Fusion uses, at that model's standard rate. There's no separate charge. Fusion High and Fusion Max can use more than one model per task, so a task can use more AI credits than with Fusion Standard. Through November 30, 2026, paid plans get a 30% discount on model costs with Copilot Fusion ([changelog](https://github.blog/changelog/2026-10-28-route-and-orchestrate-models-with-github-copilot-fusion/)).

> [!CAUTION]
> **Know the limits**
>
> * **Copilot Business:** Copilot Fusion only uses models your admin's model policies allow. If Fusion Max is missing, a model it needs is probably disabled. If none of its models are allowed, Copilot Fusion doesn't appear in the picker.
> * Copilot Fusion **doesn't start subagents**. It works separately from subagent delegation.
> * If Copilot Fusion discards a draft, file edits that draft already made **aren't undone** automatically. Review changes before you commit.
>
> You'll compare effort levels hands-on in [Part 10.3](#103-hands-on-compare-copilot-fusion-effort-levels).

> [!NOTE]
> **About model names in this lab**
>
> **Default for this lab: Copilot Fusion, effort level Fusion Standard.** Keep it selected in the model picker unless a step tells you otherwise. The agent files in this lab have **no `model` line**, so they use the picker's model too. The exceptions are:
>
> * **Part 5.3 and Part 7:** you *optionally* pin a model in an agent file, choosing a different model **size** or **family** for a stage.
> * **Part 10.3:** you compare Fusion effort levels.
>
> Pinning a model only works with models in *your* picker, so it stays portable across orgs.
>
> **Subagents with Copilot Fusion:** in the Copilot harness, DevFlow's subagents start normally when **Copilot Fusion** is selected in the picker. Agents with no `model` line run on the model Fusion picked for the main session (the subagent entry shows which). If subagents don't start in your environment, select a specific model in the picker for Part 7.

---

**Next:** [Part 2: Custom Instructions: The Foundation →](#part-2-custom-instructions-the-foundation-15-min-all-stages) · [Back to contents](#contents)

<a id="part-2-custom-instructions-the-foundation-15-min-all-stages"></a>

## Part 2: Custom Instructions: The Foundation (~15 min, ALL STAGES)

Every SDLC stage, whether a human or an agent runs it, depends on the agent knowing your project's conventions. **Custom instructions** are that foundation. They're the always-on layer that DevFlow, the Planner, the Tester, and the Code Reviewer all inherit. Get this layer right, and keep it *lean*, before you build anything on top of it.

| Type | File | When it's applied |
|------|------|-------------------|
| Repo-wide | `.github/copilot-instructions.md` | Automatically, on every chat request in this workspace |
| Repo-wide (multi-tool) | `AGENTS.md` | Automatically. Many AI coding tools read this format |
| Path-specific | `.github/instructions/*.instructions.md` | When the files the agent works on match the `applyTo` glob, or when the task matches the `description` |
| Personal | `~/.copilot/copilot-instructions.md`, `~/.copilot/instructions/` | Your own preferences, in every workspace |

> [!NOTE]
> **Instructions don't affect inline suggestions**
>
> Custom instructions apply to chat and agent sessions. They aren't used for inline code completions as you type.

<a id="21-inspect-whats-already-there"></a>

### 2.1 Inspect What's Already There

The sample repo already ships a `.github/copilot-instructions.md`. Open it.

It's about **16 KB**, a full "Organization Development Standards" document. Because repo-wide instructions load on **every request**, that's roughly **4,000 tokens** spent even when you ask Copilot to fix a typo. The content is good. It's just in the wrong place.

<a id="22-move-the-standards-doc-out-of-the-always-on-path"></a>

### 2.2 Move the Standards Doc Out of the Always-On Path

Keep the full standards as documentation that agents can *read when they need it*, by moving it out of `.github/`:

```bash
git mv .github/copilot-instructions.md docs/development-standards.md
```

`git mv` works the same in the macOS and Windows integrated terminals. `docs/` already exists in the sample repo.

<a id="23-generate-a-lean-repo-wide-instructions-file"></a>

### 2.3 Generate a Lean Repo-Wide Instructions File

Let Copilot analyze the repo and write a fresh, short instructions file.

1. In the Chat view, start a **new session** (**+**), keep the Session Target on **Copilot**, and select the built-in **Agent**.
2. Enter:

```
Analyze this repository and create .github/copilot-instructions.md.
Keep it under 40 lines. Include: the tech stack (backend, frontend, tests),
the exact build/test/lint commands, the folder layout, and the 5-8 most
important conventions. For everything else, link to
docs/development-standards.md instead of repeating it.
```

> [!TIP]
> **Other ways to generate instructions**
>
> * The **Agent Customizations editor** (**Chat: Open Customizations**) can start a chat that analyzes your repository and creates instructions for the selected harness.
> * In the **Local** harness, the `/init` slash command generates workspace instructions. See [Copilot vs Local](#session-target).
>
> Whichever you use, **always review the result**. Generated instructions are a draft, not the final file.

3. **Trim it.** Review the generated file and delete anything that's obvious from the code, duplicated in the standards doc, or only relevant to one part of the repo. That last group goes in path-specific files next. Aim for something like:

```markdown
# Copilot instructions

## Stack
- Backend: ASP.NET Core (.NET 8) Web API in `backend/src/Api`, xUnit tests in `backend/tests/Api.Tests`
- Frontend: React 18 + TypeScript + Vite + Tailwind in `frontend/`
- `legacy/` is reference-only. Don't modify it.

## Commands
- Backend: `dotnet build backend/Api.sln`, `dotnet test backend/Api.sln`
- Frontend (from `frontend/`): `npm run lint`, `npm run build` (there is no frontend test script)

## Conventions
- Commits use Conventional Commits with scopes `backend`, `frontend`, `tests`, `config`.
- Never commit directly to `main`. Use a feature branch.
- Full standards: [docs/development-standards.md](../docs/development-standards.md). Read the relevant section before larger changes.
```

That's roughly 200 tokens instead of 4,000, on **every** request.

<a id="24-add-path-specific-instructions"></a>

### 2.4 Add Path-Specific Instructions

Backend and frontend rules don't need to load together. Put each set in its own file, scoped with `applyTo`.

Create `.github/instructions/backend-csharp.instructions.md`:

```markdown
---
name: "Backend C# conventions"
description: "Conventions for the ASP.NET Core API and its xUnit tests. Use when writing or reviewing C# in backend/."
applyTo: "backend/**/*.cs"
---

# Backend (ASP.NET Core, .NET 8)

## API design
- Routes: `[Route("v1/[controller]")]`, no `/api/` prefix, plural lowercase resources, no trailing slash.
- Wrap responses in `ItemResponseDto<T>` / `CollectionResponseDto<T>` (item/items + metadata + links).
- Paginate with `offset`/`limit` (default 20, max 100).
- Error codes follow `ORG-<AREA>-<NNN>`, for example `ORG-VAL-001`.
- Annotate actions with `[ProducesResponseType]`.

## Naming
- Booleans end in `Indicator` (`activeIndicator`), never `is`/`has` prefixes.
- Dates end in `Date`, identifiers end in `Id`.
- Async methods end in `Async` and take `CancellationToken` as the last parameter.

## Structure
- DTOs live in `DTOs/` with `[JsonPropertyName("camelCase")]` and XML doc comments.
- Data access goes through interfaces in `Repositories/`, business logic in `Services/`.
- Use `AsNoTracking()` for reads. No stored procedures.
- Register services with `AddScoped` in `Extensions/ServiceCollectionExtensions.cs`.

## Tests
- Name tests `{Method}_{Scenario}_{Expected}`, with `// Arrange`, `// Act`, `// Assert` comments.
- Use the EF Core InMemory provider (never SQLite) and the existing test data builders.
```

Create `.github/instructions/frontend-react.instructions.md`:

```markdown
---
name: "Frontend React conventions"
description: "Conventions for the React + TypeScript + Tailwind frontend. Use when writing or reviewing code in frontend/."
applyTo: "frontend/**/*.{ts,tsx}"
---

# Frontend (React 18, TypeScript, Vite, Tailwind)

- One component per folder: `src/components/Name/Name.tsx` plus an `index.ts` re-export.
- TypeScript strict mode. Use `interface` for object shapes, `type` for unions. Shared types go in `src/lib/types.ts`.
- Call the API only through `fetchApi` in `src/lib/api.ts`. The base URL comes from `VITE_API_BASE_URL`.
- Style with Tailwind utility classes only. Use brand colors through the `--color-brand-*` CSS variables. Never hardcode hex values.
- Don't hardcode user-facing strings inline in components.
- Before finishing, run `npm run lint` and `npm run build` from `frontend/`.
```

> [!TIP]
> **Writing good instructions**
>
> * **Be specific and checkable.** "Booleans end in `Indicator`" beats "use good names".
> * **Keep each file short.** Link to docs for the long version.
> * **Write a strong `description`.** It lets the agent pull the file in for a matching task, even when no file matches `applyTo` yet.
> * You can also create these files from **Agent Customizations editor → Instructions → New Instruction**, or with **Chat: New Instructions File** from the Command Palette.

<a id="25-agentsmd-and-personal-instructions"></a>

### 2.5 `AGENTS.md` and Personal Instructions

* **`AGENTS.md`** is an open format that many AI coding tools read. VS Code treats an `AGENTS.md` at the repo root as always-on instructions, just like `copilot-instructions.md`, and combines them with no precedence order. In the Copilot harness, `AGENTS.md` files in the working directory and intermediate folders are also discovered. In the **Local** harness, root `AGENTS.md` is controlled by the `chat.useAgentsMdFile` setting (on by default), and nested ones by `chat.useNestedAgentsMdFiles` (off by default). **Pick one repo-wide file** so you don't load the same rules twice. If your team also uses other AI tools, `AGENTS.md` is the portable choice.
* **Personal instructions** apply only to you, in every workspace. Examples: "Explain changes as a short bulleted list" or "I'm new to C#, so explain idioms briefly". In the Copilot harness, put them in `~/.copilot/copilot-instructions.md` or `~/.copilot/instructions/`. Don't put team rules there. Teammates won't get them.
* **Organization instructions** are configured by a GitHub org admin. VS Code applies them when `github.copilot.chat.organizationInstructions.enabled` is on. Treat this as a concept for this lab.

<a id="26-test-that-the-right-instructions-load"></a>

### 2.6 Test That the Right Instructions Load

1. Start a **new chat session** so the old instructions aren't cached in the conversation.
2. Open `backend/src/Api/Controllers/HealthController.cs` and ask the **Agent**:

```
Add a GET endpoint to this controller that reports whether the database is reachable. There's no DbContext yet, so add a minimal one using the EF Core InMemory provider already referenced in Api.csproj, and check it with Database.CanConnectAsync.
```

> [!NOTE]
> The starter app has no database yet. Naming the InMemory provider in the prompt stops the agent from asking which database to set up, and it keeps the test free of new packages or a database server.

3. Check which instructions loaded. In the **Copilot** harness the response may not show a **References** list, so use either of these:
   * The response's progress steps: look for **Read backend-csharp.instructions.md**. `copilot-instructions.md` is always in context, so it isn't listed as a read.
   * **Agent Debug Logs** (Command Palette: **Developer: Open Agent Debug Logs**). Select the session, then **View Logs**. The **Custom Instructions** row lists the always-on files and the on-demand instructions available to the agent.

   Check that the generated code follows the rules, for example a `databaseReachableIndicator` property rather than `isDatabaseReachable`. Select **Undo** to discard the change. If **Undo** doesn't respond, discard the files in the integrated terminal instead: `git restore backend` to revert edited files, then delete the new files the agent created (`git status` lists them).
4. Start another new session, open `frontend/src/components/LoadingSpinner/LoadingSpinner.tsx`, and ask:

```
Create a StatusBadge component next to this one that shows "Healthy" or "Unhealthy".
```

5. This time, the agent should read `frontend-react.instructions.md` and **not** the backend file. Check the progress steps or Agent Debug Logs as in step 3. Check that the agent created `StatusBadge/StatusBadge.tsx` plus `index.ts`. Select **Undo**.

> [!TIP]
> **Debug which instructions loaded**
>
> If a file doesn't show up, run **Developer: Open Agent Debug Logs** and check the **Instructions Discovery** row. It lists every instruction file found and the folders searched. Also check that the file appears under **Instructions** in **Chat: Open Customizations**, and that the `applyTo` glob is relative to the workspace root.

Leave these changes uncommitted for now. In Part 8 you'll commit them to a feature branch, together with the rest of DevFlow's work. The repo's own conventions say never to commit directly to `main`.

> [!TIP]
> **Catch up**
>
> Behind? Run the `git mv` command and copy the three instructions files from [Solution files: Part 2](#solution-part-2). Then go on to Part 3.

---

**Next:** [Part 3: Discover & Reuse Community Skills →](#part-3-discover--reuse-community-skills-7-min) · [Back to contents](#contents)

<a id="part-3-discover--reuse-community-skills-7-min"></a>

## Part 3: Discover & Reuse Community Skills (~7 min)

Before you build a skill from scratch, **check what already exists**. The GitHub CLI includes `gh skill` commands for discovering and installing skills from across the community.

> [!IMPORTANT]
> Run these commands in the **VS Code integrated terminal**. `gh skill` is part of the general-purpose GitHub CLI (`gh`), not Copilot CLI. It was announced in [this changelog post](https://github.blog/changelog/2026-04-16-manage-agent-skills-with-github-cli/). Update `gh` to version 2.90.0 or later.

<a id="31-search-for-existing-skills"></a>

### 3.1 Search for Existing Skills

```bash
# Search for skills related to your use case - don't install yet!
gh skill search "task breakdown"

# Try other topics
gh skill search "code review"
gh skill search "naming conventions"
```

The search covers published skills in GitHub's community repositories and in any repo with the `agent-skills` topic.

<a id="32-preview-before-installing"></a>

### 3.2 Preview Before Installing

This command renders the skill's SKILL.md in your terminal without installing anything:

```bash
gh skill preview github/awesome-copilot documentation-writer
```

To browse interactively, leave off the skill name: `gh skill preview github/awesome-copilot`.

<a id="33-install-a-community-skill"></a>

### 3.3 Install a Community Skill

Install the `documentation-writer` skill into your project. DevFlow uses it at the **Ship** stage.

```bash
gh skill install github/awesome-copilot documentation-writer --dir .github/skills
```

`--dir .github/skills` installs the skill into the project's `.github/skills/` folder, so it's committed and shared with your team, and Part 11 can package it. Without `--dir`, current `gh` versions install project-scope skills into `.agents/skills/` instead. VS Code reads both folders; if yours landed in `.agents/skills/`, note it, because you copy from it in [11.1](#111-agent-plugins).

> [!TIP]
> **No `gh`, or can't sign in? Install the skill manually**
>
> `gh skill` needs the GitHub CLI signed in with `gh auth login`, but it only *reads* public repos. If that's a burden, a skill is just a folder, so copy it yourself:
>
> <details><summary>macOS</summary>
>
>
> ```bash
> git clone --depth 1 https://github.com/github/awesome-copilot.git /tmp/awesome-copilot
> mkdir -p .github/skills
> cp -r /tmp/awesome-copilot/skills/documentation-writer .github/skills/
> ```
>
>
> </details>
> <details><summary>Windows (PowerShell)</summary>
>
>
> ```powershell
> git clone --depth 1 https://github.com/github/awesome-copilot.git $env:TEMP\awesome-copilot
> New-Item -ItemType Directory -Force -Path .github\skills | Out-Null
> Copy-Item -Recurse $env:TEMP\awesome-copilot\skills\documentation-writer .github\skills\
> ```
>
>
> </details>
>
> You lose `gh skill update`, so you'll need to recopy the folder to get updates. APM (Part 11.2) can manage that for you from any Git host.

<a id="34-verify-in-vs-code"></a>

### 3.4 Verify in VS Code

1. In the Chat view, select **Open Customizations** in the title bar.
2. Select **Skills**. `documentation-writer` should appear under **Workspace**.

Alternatively, type `/` in the chat input. Skills appear in the list of slash commands.

> [!TIP]
> **VS Code-native alternatives to `gh skill`**
>
> * **Agent Customizations editor → Skills → New Skill** creates a skill from a template.
> * **Agent plugins** (Part 11) can bundle skills. Browse them in **Chat: Open Customizations → Plugins** (the Extensions view's `@agentPlugins` search now points there).
> * The [awesome-copilot](https://github.com/github/awesome-copilot) repository and its [skills gallery](https://awesome-copilot.github.com/skills/) are good places to browse. Using or adapting an existing skill is usually faster than writing one from scratch.

<a id="35-manage-installed-skills"></a>

### 3.5 Manage Installed Skills

```bash
# Update all installed skills to latest versions
gh skill update --all

# Update a specific skill
gh skill update github/awesome-copilot documentation-writer

# Publish your own skill to your repo (validates against the Agent Skills spec)
gh skill publish --dry-run
```

---

**Next:** [Part 4: Building Skills with the Skill Creator →](#part-4-building-skills-with-the-skill-creator-15-min) · [Back to contents](#contents)

<a id="part-4-building-skills-with-the-skill-creator-15-min"></a>

## Part 4: Building Skills with the Skill Creator (~15 min)

When no existing skill fits, you don't have to write one line by line. Anthropic publishes **skill-creator**, a meta-skill that teaches your AI agent how to build other skills, with best practices built in.

<a id="41-install-the-skill-creator"></a>

### 4.1 Install the Skill Creator

`skill-creator` is a **dev-time tool**: you use it to build skills, but your project doesn't need it at run time. Install it at **user scope** so it stays out of the repo, out of your commits, and out of the Code Reviewer's view in Part 5.

Option A (recommended) works the same on every platform. Run it in the integrated terminal:

```bash
gh skill install anthropics/skills skill-creator --agent github-copilot --scope user
```

Option B is a manual copy into your personal `~/.copilot/skills/` folder, which differs by shell:

<details><summary>macOS</summary>

```bash
git clone https://github.com/anthropics/skills.git /tmp/anthropic-skills
mkdir -p ~/.copilot/skills
cp -r /tmp/anthropic-skills/skills/skill-creator ~/.copilot/skills/skill-creator
```

</details>
<details><summary>Windows (PowerShell)</summary>

```powershell
git clone https://github.com/anthropics/skills.git $env:TEMP\anthropic-skills
New-Item -ItemType Directory -Force -Path "$HOME\.copilot\skills" | Out-Null
Copy-Item -Recurse -Force `
  "$env:TEMP\anthropic-skills\skills\skill-creator" `
  "$HOME\.copilot\skills\skill-creator"
```

</details>

Confirm it loaded. Open **Open Customizations → Skills** and check for `skill-creator` under **User**. Or ask in the Chat view:

```
What skills do you have?
```

<a id="42-create-your-first-skill-task-breakdown-plan-stage"></a>

### 4.2 Create Your First Skill: Task Breakdown (PLAN stage)

Open the **Chat view**, choose the built-in **Agent** in the agent picker, and enter:

```
Create a new skill called "task-breakdown" that breaks down feature requests and user stories into implementable sub-tasks with T-shirt size estimates.

It should trigger when I ask to "break down", "decompose", or "plan implementation" for a feature.

The output should be a structured table with: task title, component, T-shirt size (XS through XL), dependencies, and acceptance criteria. Include a risk assessment step for tasks touching shared code paths or requiring database migrations.

Save it under .github/skills/task-breakdown/.
```

The skill-creator might ask you a few questions. Usually it plans and then creates the skill: the SKILL.md frontmatter and body, plus any scripts and tests. If it offers to run the tests, accept. If it doesn't, move on.

> [!NOTE]
> **Human-in-the-loop in VS Code**
>
> Copilot asks before it runs terminal commands and shows its file edits as a diff. Use **Keep** or **Undo** on each change. This is your first human checkpoint: review what the agent wrote before you accept it.

When the skill-creator finishes, open `.github/skills/task-breakdown/SKILL.md` and review it. (VS Code also discovers skills in `.agents/skills/`. If the skill-creator put it there, that works too.)

| Part | Purpose | What to Look For |
|------|---------|-----------------|
| **YAML frontmatter** | Metadata: `name`, `description` | The `description` controls auto-activation. It's the trigger. |
| **Markdown body** | Instructions Copilot follows when the skill activates | Step-by-step process, output format template |

> [!IMPORTANT]
> **The skill `name` must match its folder**
>
> VS Code expects the `name` in the frontmatter to match the skill's directory name (`task-breakdown`). If they don't match, the skill might not load.

The skill-creator's key principle is that the **description is the trigger**. All "when to use" information belongs in the `description` frontmatter, not the body:

* The frontmatter loads in the system prompt, and Copilot uses it to decide when to activate the skill.
* The body loads only *after* the skill triggers.

That's how skills stay lightweight and load only when relevant.

<a id="43-test-the-task-breakdown-skill"></a>

### 4.3 Test the Task Breakdown Skill

```
Break down this feature: "Add user authentication with JWT tokens, including login, logout, and token refresh endpoints"
```

Copilot should activate your `task-breakdown` skill automatically and follow its template. To invoke it explicitly, type `/task-breakdown` in the chat input. In a real pipeline, the input would come from GitHub Issues, Jira, Azure Boards, or another work tracker instead of typed text.

> [!NOTE]
> **Token Optimization in Action**
>
> A skill has two parts:
>
> * The **frontmatter** (`name` + `description`, ~100 tokens) loads in the system prompt on *every* request, so Copilot knows the skill exists.
> * The **body** (~400 tokens) loads *only* when the description matches your prompt.
>
> Compare that to `copilot-instructions.md`, where all the content loads on every request. That's the core of token optimization: **put detailed guidance in skills, not instructions**. This pattern is called *progressive disclosure*.

<a id="44-create-a-skill-with-scripts-naming-checker-implement-stage"></a>

### 4.4 Create a Skill with Scripts: Naming Checker (IMPLEMENT stage)

Skills aren't just instructions. They can also include **executable scripts** that Copilot runs, which makes parts of the skill deterministic. Use the skill-creator again:

```
Create a new skill called "naming-checker" under .github/skills/ that validates file and variable naming conventions against project standards. It should:

- Include a Node.js script (so it runs on macOS and Windows) that scans for camelCase files that should be kebab-case (excluding .agent.md and .test. files)
- Check naming conventions for files, variables, constants, and types
- Output findings as a table with severity and suggested fixes
```

The skill-creator generates:
1. `SKILL.md`, with instructions to run the bundled script
2. `scripts/check-names.js` (or similar), with the checking logic

If it offers to run the tests, accept.

<a id="45-review-skills-as-capabilities"></a>

### 4.5 Review: Skills as Capabilities

Expand `.github/skills/naming-checker` in the Explorer, or run:

<details><summary>macOS</summary>

```bash
find .github/skills/naming-checker -type f
```

</details>
<details><summary>Windows (PowerShell)</summary>

```powershell
Get-ChildItem -Recurse -File .github\skills\naming-checker | Select-Object FullName
```

</details>

You should see a structure like:

```
.github/skills/naming-checker/
├── SKILL.md              # Instructions that reference the script
└── scripts/
    └── check-names.js    # Executable validation script
```

> [!NOTE]
> **Terminal approvals in VS Code**
>
> When a skill runs a script, VS Code asks you to approve the terminal command. You can allow a command for the session or always. Manage auto-approval rules in **Settings** (search for `chat.tools.terminal.autoApprove`). Keep risky commands on manual approval. Each approval is a human checkpoint.

<a id="46-test-the-naming-checker"></a>

### 4.6 Test the Naming Checker

```
Check naming conventions in the frontend/src directory
```

Review the results. Don't make changes yet.

> [!TIP]
> **Skill Creator Best Practices**
>
> The skill-creator follows these principles:
> - **Concise is key:** Ask of every piece, "Does this justify its token cost?"
> - **The description is the trigger:** All "when to use" information goes in the frontmatter description, not the body.
> - **Test scripts by running them:** Never ship untested code in a skill.
> - **Delete what you don't need:** Extra template files add clutter and waste tokens.
> - **Progressive disclosure:** Metadata (~100 tokens) is always in context, the SKILL.md body loads on trigger, and bundled resources load as needed.

> [!TIP]
> **Catch up**
>
> Behind? Copy the `task-breakdown` and `naming-checker` skills from [Solution files: Parts 3–4](#solution-part-4). For `documentation-writer`, use the manual `git clone` steps in Part 3.3. Then go on to Part 5.

---

**Next:** [Part 5: Your First Agent: Code Reviewer →](#part-5-your-first-agent-code-reviewer-10-min-review-stage) · [Back to contents](#contents)

<a id="part-5-your-first-agent-code-reviewer-10-min-review-stage"></a>

## Part 5: Your First Agent: Code Reviewer (~10 min, REVIEW stage)

Custom agents are **named personas** with their own tool access and model selection. When one runs as a **subagent**, it also gets its own **context window**: Copilot starts it with a fresh context, so your earlier conversation doesn't clutter it. That saves a lot of tokens. It can also be a drawback when the agent needs the full context of the main session.

<a id="51-create-the-agent"></a>

### 5.1 Create the Agent

You can create the agent file through the UI or by hand.

**Option A: VS Code UI (recommended)**
1. In the Chat view, select **Open Customizations** in the title bar, then **Agents → New Agent** next to **Workspace**. Or run **Chat: New Custom Agent** from the Command Palette.
2. Choose **Workspace** (`.github/agents`) and name it `reviewer`. Type only `reviewer`: VS Code adds the `.agent.md` extension itself, so typing `reviewer.agent` creates `reviewer.agent.agent.md`.

**Option B: by hand.** Create the folder in the integrated terminal:

<details><summary>macOS</summary>

```bash
mkdir -p .github/agents
```

</details>
<details><summary>Windows (PowerShell)</summary>

```powershell
New-Item -ItemType Directory -Force -Path .github\agents | Out-Null
```

</details>

Either way, replace the contents of `.github/agents/reviewer.agent.md` with:

```markdown
---
name: "Code Reviewer"
description: "Review code changes for bugs, security issues, and standards compliance. Use when asked to review code, check a PR, or audit changes."
tools: ['read', 'search', 'execute']
---

You are a senior code reviewer. Your job is to review code changes 
thoroughly but efficiently, focusing on what matters most.

## Tool Boundaries
- Use `execute` only for non-mutating inspection commands such as
  `git status --short`, `git diff`, `git diff --cached`, `git log`, and `git show`.
- Inspect both staged and unstaged changes; read untracked files directly.
- Do not modify files, run fixes or formatters, install dependencies, or
  stage, commit, push, or change branches. Report proposed fixes instead.
- Respect command approvals. Access to `execute` is not a read-only sandbox.

## Review Priority (in order)

1. **🔴 Bugs** — Logic errors, null pointer risks, race conditions,
   off-by-one errors
2. **🟠 Security** — SQL injection, XSS, credential exposure,
   insecure deserialization
3. **🟡 Performance** — N+1 queries, unnecessary re-renders,
   missing indexes, memory leaks
4. **🔵 Maintainability** — Code duplication, unclear naming,
   missing error handling

## What You Do NOT Review
- Formatting and style (leave that to linters)
- Import ordering
- Comment style preferences

## Review Process
1. Run Git inspection commands, then read the changed files to understand the diff
2. Search for related code to understand impact
3. Check for the priority items above
4. Present findings in a structured table

## Output Format

### Review Summary
| Category | Findings | Severity |
|----------|----------|----------|
| Bugs | [count] | 🔴 |
| Security | [count] | 🟠 |
| Performance | [count] | 🟡 |
| Maintainability | [count] | 🔵 |

### Detailed Findings
For each finding:
- **File**: path/to/file.ts:line
- **Severity**: 🔴/🟠/🟡/🔵
- **Issue**: What's wrong
- **Fix**: Recommended change

You never edit files. The human decides which findings to fix.
```

<a id="52-understand-the-agent-anatomy"></a>

### 5.2 Understand the Agent Anatomy

| Frontmatter Field | Purpose | Your Choice |
|-------------------|---------|-------------|
| `name` | Name shown in the agent picker; also the name other agents use to call it | `"Code Reviewer"` |
| `description` | Tells Copilot when to use the agent (and shows as placeholder text) | Review-related triggers |
| `tools` | Tools the agent may use. Tool sets like `read`, `search`, `edit`, `execute`, `agent`, `todo`, and `web` keep it short | `read`, `search`, `execute`: inspect files and run Git commands; no `edit` tool. Review-only behavior is an instruction, not a sandbox |
| `model` | Which model this agent uses (a name or a prioritized list). Leave it out to use the picker's model | Not set for now (see 5.3) |
| `agents` | Which custom agents this agent can call as subagents | None. The reviewer doesn't delegate. |

See the [custom agent file reference](https://code.visualstudio.com/docs/agents/reference/custom-agent-file-reference) for every field, including `argument-hint`, `user-invocable`, and `disable-model-invocation`.

<a id="53-per-agent-model-selection"></a>

### 5.3 Per-Agent Model Selection

Code review is a **balanced task**. It needs to understand code context and spot patterns, but it doesn't need the deep multi-step reasoning of architecture work. A general-purpose model usually gives you the best quality for the cost.

```
                          ┌────────────────────────────────────────┐
  Task Complexity         │            Model Selection             │
  ────────────────        │                                        │
                          │  ┌──────────┐                          │
  Simple tasks ──────────►│  │  Small   │ Lower token rates        │
  (tests, scaffolding)    │  └──────────┘                          │
                          │  ┌──────────┐                          │
  Balanced tasks ────────►│  │ General  │ Balanced cost    ◄── YOU │
  (review, refactoring)   │  └──────────┘                     ARE  │
                          │  ┌──────────┐                     HERE │
  Complex tasks ─────────►│  │ Frontier │ Higher token rates       │
  (architecture)          │  └──────────┘                          │
                          │  ┌──────────┐                          │
  Unknown complexity ────►│  │  Fusion  │ Routed, billed per model │
                          │  └──────────┘                          │
                          └────────────────────────────────────────┘
```

**Exercise: pin a model for the reviewer**
1. In the Chat view, open the **model picker** and note the exact name of a balanced, general-purpose model that your plan offers.
2. In `reviewer.agent.md`, add a `model:` line to the frontmatter (after `tools:`) with that name followed by ` (copilot)`, for example `model: 'Claude Sonnet 5.5 (copilot)'`. You can also give a fallback list: `model: ['<preferred model> (copilot)', '<fallback model> (copilot)']`.
3. If the model isn't available (for example, if your Business admin hasn't enabled it), VS Code falls back. Leaving out the `model` line is always a safe choice: the agent then uses the model selected in your picker.

> [!CAUTION]
> **Model not in your picker?**
>
> On Copilot Business, your admin's model policies decide which models appear in your model picker. Model names in this lab, such as `Claude Sonnet 5.5` or Haiku-class models, are only examples. If a model you pin isn't in **your** picker, remove the `model` line. Without a `model` field, the agent uses the model that's selected in the picker.

<a id="choose-by-size-and-by-family"></a>

#### Choose by Size and by Family

When you pin a model for an agent, make two separate choices:

| Choice | What it means | How to apply it |
|--------|---------------|-----------------|
| **Size** (capability) | Small/fast, general-purpose, or frontier reasoning models. Bigger models usually cost more per token and respond more slowly | Match it to the task: small for focused, repetitive work (tests, docs), general for review, frontier for hard design or debugging |
| **Family** (provider) | Models from different providers, for example Anthropic Claude, OpenAI GPT, or Google Gemini, are trained differently and have different blind spots | Use a **different family to check work than the one that produced it**. For example, if your main session usually writes code with a GPT-family model, pin the Code Reviewer to a Claude-family model, or the other way round |

A reviewer from another family acts like a second opinion from a colleague with a different background. It doesn't guarantee correct findings, so you still decide which findings to fix.

> [!NOTE]
> **How a custom agent's model applies**
>
> * **Selected from the agent picker:** the agent's `model` line overrides the model picker for that session. With no `model` line, it uses the picker's model (Copilot Fusion in this lab).
> * **Run as a subagent** (Part 7): the agent's `model` line decides which model the subagent uses. With no `model` line, the subagent uses the main session's model. The model each subagent used appears on its entry in the chat.
> * **Cost limit:** in the Local harness, a subagent can't use a model that costs more than the main session's model. If it would, the subagent doesn't run and lists the models you can use. In the Copilot harness with Copilot Fusion selected, subagents with no `model` line inherit the model Fusion picked for the main session. Behavior for a pinned model that costs more than the main session's model wasn't tested.

> [!NOTE]
> **Copilot Fusion vs pinned models**
>
> **Copilot Fusion** in the picker routes each request to the right model(s) for you ([1.4](#14-copilot-fusion)). **Pinning** a model in an agent file gives you *predictable* behavior and cost for that stage. Per-agent model selection is about choosing, stage by stage, which of the two you want.

<a id="54-subagent-context-isolation"></a>

### 5.4 Subagent Context Isolation

When another agent delegates to the Code Reviewer, it runs as a **subagent** with its own context window:

```
┌─────────────────────────────────────────────┐
│  Your Main Session                          │
│                                             │
│  "I've been working on the auth module      │
│   for the past 30 minutes..."               │
│                                             │
│  Context: 60% full with conversation        │
│                                             │
│  ┌───────────────────────────────────┐      │
│  │  Code Reviewer (Subagent)         │      │
│  │                                   │      │
│  │  Fresh context window!            │      │
│  │  Only review-relevant info        │      │
│  │  Not polluted by earlier work     │      │
│  │                                   │      │
│  │  Context: 5% used                 │      │
│  └───────────────────────────────────┘      │
└─────────────────────────────────────────────┘
```

**Subagents don't inherit your conversation history.** Each one starts with a clean slate focused on its task, which saves a lot of tokens. In the Chat view, a subagent run shows as a collapsible tool call. Expand it to see the subagent's prompt, tool calls, and result.

<a id="55-test-the-agent"></a>

### 5.5 Test the Agent

1. Open the **agent picker** at the bottom of the Chat view and select **Code Reviewer**. If it isn't listed, run **Developer: Reload Window**, or run **Developer: Open Agent Debug Logs** and check the **Agent Discovery** row for load errors.
2. Enter:

```
Review my uncommitted changes in this repository (run git status and git diff to find them)
```

Your uncommitted Part 2 changes (the instructions files and the moved standards doc) give it something real to review. The reviewer has `read`, `search`, and `execute`, so it can run `git status --short`, `git diff`, and `git diff --cached` itself, then read untracked files directly. Approve only the non-mutating inspection commands. It should return findings without changing files. In a real workflow, you'd point the reviewer at your feature branch or an MR/PR.

> **Permission boundary:** Omitting `edit` does not prevent terminal commands from changing files. The review-only instructions constrain intended behavior; keep command approvals enabled rather than treating `execute` as read-only enforcement.

> [!TIP]
> **Catch up**
>
> Behind? Copy `.github/agents/reviewer.agent.md` from [Solution files: Part 5](#solution-part-5), then run **Developer: Reload Window** if **Code Reviewer** isn't in the agent picker.

---

**Next:** [Part 6: MCP Server: A Real Browser for the Test Stage →](#part-6-mcp-server-a-real-browser-for-the-test-stage-10-min-test) · [Back to contents](#contents)

<a id="part-6-mcp-server-a-real-browser-for-the-test-stage-10-min-test"></a>

## Part 6: MCP Server: A Real Browser for the Test Stage (~10 min, TEST)

Your agents can read, edit, and run terminal commands. But the sample frontend has **no test script**, so nothing checks the UI. The **Model Context Protocol (MCP)** lets you plug external tools into Copilot. In this part you'll add the **Playwright MCP server**, so the Tester can open the app in a real browser and check the feature you build in Part 7.

Playwright MCP runs locally with `npx` and doesn't depend on where your code is hosted.

> [!WARNING]
> **Copilot Business: check the MCP policy first**
>
> For organizations, the **MCP servers in Copilot** policy is **disabled by default**. If you can't start the server, ask your org admin to enable it.

<a id="61-add-the-playwright-mcp-server"></a>

### 6.1 Add the Playwright MCP Server

Create `.vscode/mcp.json` in the sample repo:

```json
{
  "servers": {
    "playwright": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@playwright/mcp@0.0", "--isolated"]
    }
  }
}
```

* `@playwright/mcp@0.0` installs the newest stable `0.0.x` release. Some corporate npm mirrors point `@latest` at an old 2025 alpha whose tools have no input parameters, so `browser_navigate` can't take a URL. The `0.0` range avoids that.
* `--isolated` gives each chat session its own in-memory browser profile. VS Code starts a separate Playwright server for each chat session, so without it a browser left open by one session (for example, 6.4) blocks the next one (Part 7) with a "browser profile is already in use" error.

Playwright MCP needs **Node 18 or later**, which you installed in Part 0. The sample repo's `.gitignore` excludes `.vscode/`, so to share the server with your team you must commit it with `git add -f .vscode/mcp.json` (see [8.2](#82-review-and-commit)).

> [!NOTE]
> **Other ways to add a server**
>
> * Run **MCP: Add Server** from the Command Palette, choose **Command (stdio)**, enter `npx -y @playwright/mcp@0.0 --isolated`, and give it the ID `playwright`. The guided flow offers destinations such as your personal `~/.copilot/mcp-config.json`. Newer VS Code versions may label `.vscode/mcp.json` as a legacy format and offer a portable `.mcp.json` at the workspace root (experimental, uses an `mcpServers` key). Both work. This lab uses `.vscode/mcp.json` because VS Code forwards its servers to Copilot harness sessions.
> * Browse the MCP server gallery in the **Extensions** view by searching for `@mcp`, then select **Install** (personal) or **Install in Workspace**.

<a id="62-start-and-trust-the-server"></a>

### 6.2 Start and Trust the Server

1. With `.vscode/mcp.json` open, select **Start** above the `playwright` entry. You can also run **MCP: List Servers**, select `playwright`, and then **Start Server**.
2. VS Code may ask you to **trust** the server before it runs. MCP servers run code on your machine, so only trust servers whose source you know. Confirm the prompt if it appears.
3. Check the server's status: the CodeLens above the entry changes to **✓ Running | Stop | More...**. If it fails to start, select **More... → Show Output** to read the log. The first start can take up to a minute while `npx` downloads the package.

<a id="63-see-and-limit-the-tools"></a>

### 6.3 See and Limit the Tools

Open **Chat: Open Customizations** and select **Tools**. In the **Local** harness ([Copilot vs Local](#session-target)), use **Configure Tools** in the chat input instead. You'll see the Playwright tools grouped under the server, such as navigate, click, type, and snapshot.

Every enabled tool's name, description, and schema is sent to the model on **every request**, so turn off tools you don't need. You'll measure this in Part 10.1.

<a id="64-try-it"></a>

### 6.4 Try It

Make sure the backend and frontend from Part 0 are still running. In a new chat session with the built-in **Agent**, enter:

```
Use the Playwright tools to open http://localhost:5173, take an accessibility
snapshot, and list the main navigation items and any errors you see.
```

Approve the tool calls when prompted. The approval card shows the tool input; for **Navigate to a URL** it should read `{ "url": "http://localhost:5173" }`. If the input is empty, you're on an old package version (see [6.1](#61-add-the-playwright-mcp-server)). Expand a tool call in the chat to see the exact input and output. A browser window opens and the agent reports what it found: **Home** and **System Health** navigation items, and no page errors. If the tool call fails with a "browser not found" or "chrome" error, see [Playwright Can't Open a Browser](#troubleshooting-playwright-browser).

Playwright MCP saves snapshots and console logs in a `.playwright-mcp/` folder at the repo root. Add it to `.gitignore` now so it doesn't show up when you commit in Part 8:

<details><summary>macOS</summary>

```bash
echo ".playwright-mcp/" >> .gitignore
```

</details>
<details><summary>Windows (PowerShell)</summary>

```powershell
Add-Content -Path .gitignore -Value ".playwright-mcp/"
```

</details>

<a id="65-restrict-mcp-tools-per-agent"></a>

### 6.5 Restrict MCP Tools Per Agent

Least privilege applies to MCP too. You don't want every agent to have browser control. In Part 7 you'll assign browser work to the **Tester**, by adding the server's tools to its `tools` list with the `<server>/*` pattern. DevFlow also needs those tools to delegate them, as explained below:

```yaml
tools: ['read', 'search', 'edit', 'execute', 'playwright/*']
```

The Planner and Code Reviewer won't list `playwright/*`, so they never see those tools. That's safer, and it keeps their context smaller.

> [!IMPORTANT]
> **Subagents can only use tools their parent also has**
>
> In the Copilot harness, a subagent's tools are limited to the tools of the agent that delegates to it. In Part 7, the Tester runs as a subagent of DevFlow, so DevFlow's `tools` list must **also** include `playwright/*`. If it doesn't, the Tester reports "Playwright tools aren't available" even though it works when you select it directly.

> [!TIP]
> **Built-in alternative**
>
> VS Code also has built-in browser tools that let the agent open and inspect pages without an MCP server. Playwright MCP is used here because it's a portable, standard MCP example that works the same way in other MCP hosts.

<a id="66-concept-an-issue-tracker-mcp-server-for-the-plan-stage"></a>

### 6.6 Concept: An Issue-Tracker MCP Server for the PLAN Stage

In a real pipeline, the Planner should read the actual work item instead of typed text. You can connect your tracker the same way:

| Your tracker | MCP option | Notes |
|--------------|-----------|-------|
| **GitLab** issues / merge requests | [GitLab MCP server](https://docs.gitlab.com/user/model_context_protocol/mcp_server/) | Official, hosted by your GitLab instance (GitLab.com, Self-Managed, Dedicated). OAuth sign-in. Check your GitLab version and admin settings |
| **GitHub** issues / pull requests | [GitHub MCP server](https://github.com/github/github-mcp-server) | Remote server at `https://api.githubcopilot.com/mcp/` with OAuth sign-in in VS Code |
| **Jira**, **Azure Boards**, others | Your tracker's MCP server | Check your vendor's docs and your org's MCP policy |

Then scope it to the Planner only, for example `tools: ['read', 'search', 'todo', 'gitlab/*']`. Remote servers that access company data usually need admin approval, so treat this as a follow-up for your team, not part of today's hands-on.

> [!TIP]
> **Catch up**
>
> Behind? Copy `.vscode/mcp.json` from [Solution files: Part 6](#solution-part-6), then start and trust the server as in [6.2](#62-start-and-trust-the-server). If MCP is blocked by policy, skip it. The Tester still runs backend tests and the frontend lint and build without it.

---

**Next:** [Part 7: The DevFlow Orchestrator →](#part-7-the-devflow-orchestrator-17-min-plan--implement--test--review) · [Back to contents](#contents)

<a id="part-7-the-devflow-orchestrator-17-min-plan--implement--test--review"></a>

## Part 7: The DevFlow Orchestrator (~25 min, PLAN → IMPLEMENT → TEST → REVIEW)

Now you'll connect the agents with an **orchestrator**: a main agent that delegates to specialized subagents at each SDLC stage and **stops at every stage gate for your approval**.

<a id="71-create-the-planner-agent-plan"></a>

### 7.1 Create the Planner Agent (PLAN)

Create `.github/agents/planner.agent.md`:

```markdown
---
name: "Planner"
description: "Break down features and issues into implementation plans. Use when asked to plan, decompose, or create tasks for a feature."
tools: ['read', 'search', 'todo']
---

You are a technical planning specialist. You create clear, actionable 
implementation plans from feature requests and issues.

## Your Process
1. Analyze the request to understand scope and requirements
2. Search the codebase to understand current architecture
3. Use the task-breakdown skill for structured decomposition
4. Create a prioritized plan with dependencies

## Constraints
- Each task should be completable in under 1 day
- Always identify risks and unknowns
- Include testing tasks alongside implementation tasks
- Flag tasks that need external input or decisions
- Never edit code. Planning only.
```

The Planner has no `model` line because planning complexity varies a lot. It uses the model selected in your picker, so **Copilot Fusion** can match each request to the right model(s).

> [!TIP]
> **Peek at the built-in Plan agent**
>
> VS Code includes a built-in **Plan** agent. In the Copilot harness, type `/plan` followed by a task (or select **Plan** in the agent picker). It asks clarifying questions and then shows a **Review Plan** card with **Implement Plan**, **Approve Plan Only**, **Implement with Autopilot**, and **Reject**. Your Planner is different on purpose: it returns a plan to DevFlow as a subagent, using your team's `task-breakdown` format. Use `/plan` for ad hoc work and your Planner inside the pipeline.

<a id="72-create-the-tester-agent-test"></a>

### 7.2 Create the Tester Agent (TEST)

Create `.github/agents/tester.agent.md`:

```markdown
---
name: "Tester"
description: "Generate and improve tests. Use when asked to write tests, improve coverage, or validate test quality."
tools: ['read', 'search', 'edit', 'execute', 'playwright/*']
---

You are a testing specialist focused on writing effective, 
maintainable tests.

## Approach
1. Read the source code to understand what needs testing
2. Identify untested paths, edge cases, and error conditions
3. Write tests following the project's existing test patterns
4. Run backend tests with `dotnet test backend/Api.sln`
5. For frontend changes, run `npm run lint` and `npm run build` in
   `frontend/`, then use the Playwright tools to open
   http://localhost:5173, exercise the new UI, and report what you saw

## Constraints
- Match the project's existing test framework and patterns
- Write descriptive test names that explain the scenario
- Include both happy path and error case tests
- Never modify production code unless specifically asked
```

The `playwright/*` entry gives the Tester the Playwright MCP tools from Part 6. DevFlow also lists these tools so it can delegate them to the Tester; the Planner and Code Reviewer do not.

Test generation is a focused, well-defined task. **Optional exercise:** as you did in 5.3, pin a small or fast model from your model picker. Smaller models generally have lower per-token rates. Total AI credit usage still depends on the number of tokens used. If the model isn't in your picker, remove the `model` line ([5.3](#53-per-agent-model-selection)).

<a id="73-create-the-devflow-orchestrator"></a>

### 7.3 Create the DevFlow Orchestrator

This is the centerpiece. Create `.github/agents/dev-flow.agent.md`:

```markdown
---
name: "DevFlow"
description: "Agentic SDLC orchestrator. Coordinates planning, implementation, testing, review, and ship preparation through specialized subagents, with human approval at every stage gate. Use for end-to-end feature development."
tools: ['read', 'search', 'edit', 'execute', 'agent', 'todo', 'web', 'playwright/*']
agents: ['Planner', 'Tester', 'Code Reviewer']
argument-hint: "Describe the feature or task to work on"
---

You are **DevFlow**, an agentic SDLC orchestrator. You coordinate 
end-to-end software development by delegating to specialized 
subagents at each stage. A human approves every stage transition.

## Available Subagents

| Agent | Role | When to Delegate |
|-------|------|-----------------|
| **Planner** | Break down features into tasks | Start of any new feature |
| **Tester** | Generate and run tests | After implementation |
| **Code Reviewer** | Review quality and security; inspect Git without changing files | After testing |

## Available Skills

| Skill | Purpose |
|-------|---------|
| task-breakdown | Structured task decomposition |
| naming-checker | Naming convention validation |
| documentation-writer | Generate or update documentation |

## Workflow Stages

### Stage 1: PLAN
**Delegate to:** Planner subagent
- Understand the feature request
- Decompose into implementable tasks
- Identify risks and dependencies

**✋ Checkpoint:** Present the plan and STOP. Wait for explicit user approval.

### Stage 2: IMPLEMENT (BUILD)
- Work through planned tasks in dependency order
- Follow project conventions
- Use the naming-checker skill to validate naming
- Build before the checkpoint: run `dotnet build backend/Api.sln`
  and, for frontend changes, `npm run build` in `frontend/`.
  Fix any build errors.

**✋ Checkpoint:** Summarize changed files and STOP. Wait for approval to test.

### Stage 3: TEST
**Delegate to:** Tester subagent
- Generate tests for new code
- Run existing tests for regressions
- For frontend changes, have the Tester use the Playwright tools to
  open http://localhost:5173 and exercise the new UI in a real browser.
  Don't drive the browser or write browser scripts yourself.
- Report results

**✋ Checkpoint:** Report pass/fail and STOP. Wait for approval to review.

### Stage 4: REVIEW
**Delegate to:** Code Reviewer subagent
- Review all changes for bugs, security, performance
- Present findings as a table

**✋ Checkpoint:** Ask the user which findings to fix. Apply only approved fixes.

### Stage 5: SHIP PREPARATION
- Use the documentation-writer skill to update docs for the changes
- Run the naming-checker skill one final time
- Draft a Conventional Commits message, for example
  `feat(backend): add user preferences endpoints`

**✋ Checkpoint:** Present the commit message and a summary of changed files. 
Do NOT commit, push, or open merge/pull requests yourself. The human ships.

## Orchestration Rules
1. **Never skip stages**: each stage adds value
2. **Always checkpoint**: stop and ask before moving between stages
3. **Delegate, don't do**: use subagents for specialized work
4. **Summarize handoffs**: pass summaries between agents, not raw content
5. **Fail fast**: if a stage fails, present options to the user
6. **Humans ship**: never run git commit, git push, or open merge/pull requests
```

<a id="74-understand-the-key-patterns"></a>

### 7.4 Understand the Key Patterns

**The `agents:` array** lists the custom agents DevFlow can call as subagents. Names are case-sensitive and must match each agent's `name`. The `agent` tool must be in `tools` for delegation to work. To block all subagents, use `agents: []`.

**Stage gates** are the `✋` checkpoints. They keep you in control, because the orchestrator won't move from planning to coding, or from review to shipping, without your confirmation. This is the core of a *human-in-the-loop* agentic SDLC.

**Least-privilege tools** give each agent only what it needs. The Code Reviewer has `read`, `search`, and `execute` for Git inspection, but no `edit` tool; its instructions prohibit changes. The Planner can't run commands. DevFlow and the Tester also have `execute`. Both list `playwright/*`: the Tester does the browser work, and DevFlow needs it because a subagent can only use tools its parent also has ([6.5](#65-restrict-mcp-tools-per-agent)). The Planner and Code Reviewer never see the browser tools. Because terminal commands can write files, keep command approvals enabled for the Reviewer.

**Agent files are read when a session starts.** If you edit an agent file, for example to add a tool, start a new chat session (**+**) before you try again. An existing session keeps the old definition.

**Model strategy across agents:**

| Agent | Model | Why |
|-------|-------|-----|
| `DevFlow` | Not set (picker's model) | Orchestration requests vary; select a model or **Copilot Fusion** in the picker |
| `Planner` | Not set (picker's model) | Planning complexity varies widely |
| `Code Reviewer` | balanced model you pinned in 5.3, ideally from a **different family** than your main model (or not set) | Consistent review quality, plus a second opinion from a model with different blind spots |
| `Tester` | **smaller**, faster model (optional) or not set | Focused task; lower token rates |

This mixes **sizes** (a smaller model for the Tester) and **families** (a different provider for the Code Reviewer) inside one pipeline, while the orchestrator keeps **Copilot Fusion** from the picker. After the run in 7.5, check the model shown on each subagent's entry in the chat to confirm which model did the work ([5.3: Choose by size and by family](#choose-by-size-and-by-family)).

Any pinned model must be in your model picker. If it isn't, for example because of a Business model policy, remove the `model` line ([5.3](#53-per-agent-model-selection)).

> [!NOTE]
> **Which agent harness?**
>
> In VS Code, the **Session Target** picker in the Chat view selects the agent harness:
>
> * **Copilot** is the recommended default.
> * **Local** is VS Code's built-in harness.
>
> Custom agents, `tools`, `agents`, and `model` work in both, but some fields are harness-specific. For example, `argument-hint` and `handoffs` apply to **Local**. For a plain-language summary, see [Copilot vs Local](#session-target). If a field doesn't seem to take effect, see the [custom agent file reference](https://code.visualstudio.com/docs/agents/reference/custom-agent-file-reference) and [agent harnesses](https://code.visualstudio.com/docs/agents/run/agent-harnesses).

<a id="75-run-the-pipeline"></a>

### 7.5 Run the Pipeline

> [!NOTE]
> **What to expect**
>
> * **Time:** a full run usually takes **20–30 minutes**, depending on the model and how quickly you approve. Stay with it: DevFlow stops at every ✋ gate and waits for you.
> * **Approvals:** expect many prompts to approve terminal commands (`dotnet build`, `dotnet test`, `npm run lint`, `npm run build`, sometimes longer PowerShell scripts that call the local API) and Playwright tool calls. Read each one before you approve it. When several are queued, the prompt shows **1/N** and an **Allow All** button; approve them one at a time.
> * **Done looks like:** new backend endpoint(s) and tests for preferences, a theme and notifications UI in the frontend, a review findings table, and a drafted Conventional Commits message.
> * **If it stalls or goes off track:** tell DevFlow what to do next in plain words, for example `Continue with Stage 3: TEST`. If the pipeline doesn't finish, use the [Catch up](#solution-part-7) tip at the end of this part.

1. Start a **new session** (**+**). Check the chat input: Session Target **Copilot**, permissions **Manual permissions**, and *not* **Autopilot**. Autopilot answers its own questions, so it would skip DevFlow's ✋ gates.
2. In the agent picker, select **DevFlow**, then enter:

```
Build a user preferences feature that lets users save their display theme (light/dark) and notification settings.
Store preferences on the backend in an in-memory repository for a single demo user (there is no database or authentication in this lab).
One notifications on/off toggle is enough. Defaults: light theme, notifications off.
```

> [!NOTE]
> **Why the storage hint?**
>
> The sample app has no authentication and no database connection. Without the hint, the Planner stops to ask whether to store preferences in the browser or on a server, and what the notification settings and defaults should be. If you choose a SQL-backed design, the preferences endpoint returns **503** at the browser check because no SQL Server is running.

3. **PLAN:** DevFlow delegates to the Planner, which shows up in chat as a subagent tool call. Expand it to see the Planner's work. Review the plan, then approve it, for example: `Approved, proceed with implementation`. If the Planner still asks a question, answer it in the same message.
4. **IMPLEMENT (BUILD):** Watch the edits stream in. Review the diffs with **Keep**/**Undo**, and approve terminal commands as they come up. Notice the naming-checker skill activate. DevFlow builds the backend and frontend and fixes build errors before it stops at the gate. If DevFlow asks to run `git switch -c` or `git checkout -b`, select **Skip**: you create the branch yourself in Part 8.

   > [!TIP]
   > **Watch your Part 2 foundation at work**
   >
   > Look for **Read backend-csharp.instructions.md** and **Read frontend-react.instructions.md** in DevFlow's progress steps, or check **Developer: Open Agent Debug Logs**. Then check the code: `...Indicator` booleans, `v1/` routes, the response envelope, and components in their own folder with an `index.ts`. You didn't repeat any of these rules in the prompt or in DevFlow's agent file.

5. **TEST:** Approve the move to testing. The Tester subagent writes and runs backend tests in the terminal, lints and builds the frontend, and then uses **Playwright MCP** to open the preferences page and try it. **Before you approve the move to testing, restart the backend** so it serves the new endpoint (see [Restart the backend](#75-restart-backend) below). The frontend dev server reloads on its own. If the Tester says the browser check was skipped or the Playwright tools aren't available, check that DevFlow's `tools` list includes `playwright/*`, start a new session, and ask: `Continue from Stage 3: TEST and have the Tester do the Playwright browser check`. Expand the Tester's subagent entry and confirm it called `playwright` tools; if DevFlow writes its own browser script instead, tell it to delegate the browser check to the Tester.

   > [!TIP]
   > **"File is locked" during build or test**
   >
   > While the backend is running, it locks its own build output, so `dotnet build` or `dotnet test` from an agent can fail with a file-lock error. Agents usually work around this, for example by building to a different output folder. If they get stuck, stop the backend (`Ctrl+C`), let the agent run the command, then start the backend again.
6. **REVIEW:** Approve the move to review. The Code Reviewer subagent returns a findings table. Tell DevFlow which findings to fix.
7. **SHIP PREP:** DevFlow updates docs and drafts a commit message. **Copy the commit message.** You'll use it in Part 8, along with a prompt file that writes the MR/PR description.

> [!TIP]
> **Watching subagents in VS Code**
>
> Subagent runs appear in the Chat view as collapsible tool calls that show the subagent's name, prompt, and result. Hover over a subagent run to see its model and tool calls. VS Code can run several subagents in parallel when the work is independent.

<a id="75-restart-backend"></a>

#### Restart the backend

The running backend doesn't pick up new C# code on its own. In the integrated terminal where the backend is running (the one you started from `backend/` in the pre-work):

1. Press `Ctrl+C` to stop it.
2. Run it again:

<details><summary>macOS</summary>

```bash
# You should still be in the backend/ folder
ASPNETCORE_ENVIRONMENT=Development dotnet run --project src/Api
```

</details>
<details><summary>Windows (PowerShell)</summary>

```powershell
# You should still be in the backend\ folder
$env:ASPNETCORE_ENVIRONMENT = "Development"
dotnet run --project src/Api
```

</details>

If you closed that terminal, open a new one, run `cd backend`, then run the command above.

When the pipeline is done, open http://localhost:5173. Use the new preferences page or settings control that DevFlow added (its location depends on the plan you approved; DevFlow's summary names the page or component) and update your theme and notification settings.

> [!TIP]
> **The orchestrator → subagent → skill pattern**
>
> DevFlow pairs one orchestrator with specialized subagents and on-demand skills. The pattern works for any project and any tech stack.

> [!TIP]
> **Catch up**
>
> Behind? Copy the Planner, Tester, and DevFlow agents from [Solution files: Part 7](#solution-part-7). If the pipeline didn't finish, you can still do Part 8 with whatever changes are in your working tree. Shipping a partial feature to a branch is fine for this lab.

---

**Next:** [Part 8: Ship: Branch, Commit, MR/PR Description →](#part-8-ship-branch-commit-mrpr-description-12-min-ship) · [Back to contents](#contents)

<a id="part-8-ship-branch-commit-mrpr-description-12-min-ship"></a>

## Part 8: Ship: Branch, Commit, MR/PR Description (~12 min, SHIP)

In an agentic SDLC, **shipping is a human decision**. DevFlow prepared the code, tests, docs, and a commit message. Now you create the branch and the commit yourself in VS Code. Then you use a reusable **prompt file** to draft the merge request (MR) or pull request (PR) description. All of this is local Git, so it's identical whether your team hosts code on **GitLab** or **GitHub**.

<a id="81-create-a-branch"></a>

### 8.1 Create a Branch

1. Select the branch name in the **Status Bar** (bottom left), or run **Git: Create Branch...** from the Command Palette.
2. Name it `feature/user-preferences`.

Or, in the integrated terminal:

```bash
git checkout -b feature/user-preferences
```

Your uncommitted changes from Parts 2–7 come with you to the new branch.

<a id="82-review-and-commit"></a>

### 8.2 Review and Commit

1. Open the **Source Control** view (`Ctrl+Shift+G`, or `Cmd+Shift+G` on macOS).
2. Review each changed file in the diff editor. This is your final human review.
3. Stage the changes you want to ship. That includes the feature code and tests, plus the customizations you created: `.github/copilot-instructions.md`, `.github/instructions/`, `.github/skills/` (and `.agents/skills/` if `gh skill` installed `documentation-writer` there), `.github/agents/`, and `docs/development-standards.md`. The sample repo's `.gitignore` excludes `.vscode/`, so `.vscode/mcp.json` doesn't appear in Source Control. Add it from the integrated terminal with `git add -f .vscode/mcp.json`. You can make separate commits if you prefer, for example one `chore(config)` commit for the customizations and one `feat` commit for the feature.
4. Paste DevFlow's commit message into the message box, or select the **✨ Generate Commit Message** button (sparkle icon) to have Copilot draft one. Your `copilot-instructions.md` already says to use Conventional Commits, and you can add more guidance with the `github.copilot.chat.commitMessageGeneration.instructions` setting.
5. Select **Commit**. Don't push yet.

<a id="83-create-a-prompt-file-pr-description"></a>

### 8.3 Create a Prompt File: `/pr-description`

Writing an MR/PR description is a task you do the same way every time, on demand. That makes it a good fit for a **prompt file**: a saved prompt you run as a slash command.

Create `.github/prompts/pr-description.prompt.md`:

```markdown
---
name: pr-description
description: "Draft a merge request / pull request description from the commits on the current branch"
argument-hint: "Optional: issue or ticket ID, for example PROJ-123"
agent: agent
tools: ['read', 'search', 'execute']
---

Draft a merge request / pull request description for the current branch.

1. Run `git log --oneline main..HEAD` and `git diff --stat main...HEAD`
   in the terminal to see what changed. Read changed files as needed.
2. If the user supplied an issue or ticket ID, reference it in the title
   and in a "Closes" line.
3. Output Markdown only, using exactly this structure:

## Summary
One or two sentences on what this change does and why.

## Changes
- Bulleted list grouped by area (backend, frontend, tests, config)

## How to test
- Numbered steps a reviewer can follow locally

## Test evidence
- Tests added/updated and their result, plus any browser (Playwright) checks

## Risks and follow-ups
- Anything a reviewer should look at closely

Keep it platform-neutral so it can be pasted into a GitLab merge request
or a GitHub pull request. Do not push, and do not create the MR/PR.
```

Then run it:

1. **Stop the backend first** (`Ctrl+C` in its terminal). While it runs, it locks the Debug build of `Api.exe`, so the agent can't rebuild and may run tests against stale binaries. That reports the wrong test count in **Test evidence**. Restart the backend afterwards if you need it.
2. In the Chat view, switch the **Session Target** to **Local**. Prompt files run in the Local harness ([Copilot vs Local](#session-target)). Switch back to **Copilot** when you're done.
3. Type `/pr-description` in the chat input, optionally followed by a ticket ID, and press Enter. You can also run **Chat: Run Prompt** from the Command Palette, or select the play button in the editor title bar while the prompt file is open.
4. Approve the `git` and test commands. Review the draft and copy it.
5. Commit the prompt file so your team gets `/pr-description` too. In **Source Control**, stage `.github/prompts/pr-description.prompt.md` and commit it with a message such as `chore(config): add pr-description prompt file`.

> [!NOTE]
> **Prompt file vs skill vs custom agent**
>
> | | **Prompt file** | **Skill** | **Custom agent** |
> |---|---|---|---|
> | **What it is** | A saved prompt (`.prompt.md`) | A workflow folder with `SKILL.md`, plus optional scripts and templates | A role with instructions, tools, and a model (`.agent.md`) |
> | **Who triggers it** | **You**, with `/name` | The agent, when the task matches the description (or you, with `/name`) | You pick it in the agent picker, or another agent delegates to it |
> | **Harness** | Local only | Copilot and Local (and other skills-compatible tools) | Copilot and Local |
> | **Best for** | Quick, repeatable, human-initiated tasks | Multi-step workflows reused across agents and tools | Stage roles with least-privilege tools |
> | **In DevFlow** | `/pr-description` | `task-breakdown`, `naming-checker`, `documentation-writer` | Planner, Tester, Code Reviewer, DevFlow |
>
> If you need the same workflow in the Copilot harness or in other AI tools, convert the prompt file into a skill.

<a id="84-concept-open-the-merge-request-or-pull-request-on-your-git-host"></a>

### 8.4 Concept: Open the Merge Request or Pull Request on Your Git Host

Pushing the branch and opening the review request happens on your Git host. It's the same human-in-the-loop step everywhere, with different names:

| | **GitLab** (your day-to-day) | **GitHub** (the lab repo) |
|---|---|---|
| Review request | **Merge request (MR)** | **Pull request (PR)** |
| Push the branch | Source Control → **Publish Branch** (or `git push -u origin feature/user-preferences`) | Same |
| Open it | The GitLab web UI, or the [GitLab for VS Code](https://marketplace.visualstudio.com/items?itemName=GitLab.gitlab-workflow) extension | The GitHub web UI, or the [GitHub Pull Requests](https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-pull-request-github) extension |
| Description | Paste the `/pr-description` output | Same |
| Automated review | Your MR pipeline and approval rules | Your PR checks and branch protection, or [Copilot code review](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/use-code-review) where your plan and org allow it |

Before you push, you can also ask your **Code Reviewer** agent to review `git diff main...HEAD` in VS Code. That works the same on any host.

> [!NOTE]
> **Optional for this lab**
>
> Your copy of the sample repo is on GitHub, so you *may* publish the branch and open a PR there to see the full loop. It isn't required. The lesson is host-agnostic: in your real GitLab projects, the same branch, commit, and `/pr-description` flow ends in a merge request.

---

**Next:** [Part 9 (Optional): Governance with Agent Hooks →](#part-9-optional-governance-with-agent-hooks-5-min) · [Back to contents](#contents)

<a id="part-9-optional-governance-with-agent-hooks-5-min"></a>

## Part 9 *(Optional)*: Governance with Agent Hooks (~5 min)

Prompts are guidance; **hooks are policy**. A hook is a script that VS Code runs at agent lifecycle events, such as before a tool runs. It costs **zero LLM tokens** and can deterministically **deny** a tool call. Use one to make sure agents never push to `main` or force-push. That rule backs up the "humans ship" principle with something stronger than a prompt.

> [!WARNING]
> **Preview feature**
>
> Agent hooks are in **Preview** in VS Code and are supported in the **Copilot** agent harness, so keep the Session Target on **Copilot** ([Copilot vs Local](#session-target)). Behavior and payload fields might change. Check the [VS Code hooks docs](https://code.visualstudio.com/docs/agent-customization/hooks) and the [GitHub hooks reference](https://docs.github.com/en/copilot/reference/hooks-reference). Your Business admin might also manage hook policies.

Create `.github/hooks/devflow-guardrails.json`:

```json
{
  "version": 1,
  "hooks": {
    "preToolUse": [
      {
        "type": "command",
        "command": "node .github/hooks/devflow-guardrails.cjs",
        "timeoutSec": 10
      }
    ]
  }
}
```

Create `.github/hooks/devflow-guardrails.cjs`:

```js
// Deny risky shell commands before any agent runs them.
let input = '';
process.stdin.on('data', (chunk) => (input += chunk));
process.stdin.on('end', () => {
  let event = {};
  try { event = JSON.parse(input || '{}'); } catch { /* ignore */ }

  const args = typeof event.toolArgs === 'string'
    ? event.toolArgs
    : JSON.stringify(event.toolArgs ?? {});

  const blocked = [
    /git\s+push\b.*(--force|-f\b)/i,              // force push
    /git\s+push\b.*\b(main|master)\b/i,           // push to default branch
    /rm\s+-rf\s+[\/~]/i,                          // destructive delete
    /Remove-Item\b.*-Recurse\b.*-Force\b.*[A-Z]:\\/i,
  ];

  if (blocked.some((rule) => rule.test(args))) {
    process.stdout.write(JSON.stringify({
      permissionDecision: 'deny',
      permissionDecisionReason: 'Blocked by DevFlow guardrails: shipping and destructive commands require a human.',
    }));
  } else {
    process.stdout.write('{}'); // no decision: normal approval flow continues
  }
});
```

**Test it:**
1. Open **Open Customizations → Hooks** and confirm `devflow-guardrails` is listed.
2. In the Chat view with the built-in **Agent**, enter: `Run git push origin main in the terminal`.
3. The tool call should be denied with no approval prompt. The chat may show the denied call as a failed step (for example **"Execution subagent" failed**), followed by the agent explaining that the DevFlow guardrails blocked the push.

> [!CAUTION]
> **If VS Code asks you to approve the `git push` command, the hook didn't fire. Select Skip (don't approve it)**, or the push really runs against your GitHub copy of the repo. Then see [Hook Doesn't Fire](#troubleshooting-hook).

---

**Next:** [Part 10: Model Strategy with Copilot Fusion →](#part-10-model-strategy-with-copilot-fusion-15-min) · [Back to contents](#contents)

<a id="part-10-model-strategy-with-copilot-fusion-15-min"></a>
<a id="part-10-token-optimization--model-strategy-7-min"></a>

## Part 10: Model Strategy with Copilot Fusion (~15 min)

You've built a pipeline that runs different agents at different stages. This part is about choosing *which models* do the work, and letting **Copilot Fusion** make that choice for you when the task warrants it. First, a quick look at where your tokens go.

<a id="101-check-your-context-usage"></a>

### 10.1 Check Your Context Usage (~2 min)

In the Chat view, select the **context window control** next to the chat input. It shows how full the current session's context window is and what's using the space.

* **Try it:** in a chat where the Playwright server is enabled, check the control, then turn the server's tools off in **Agent Customizations editor → Tools** and compare. Every enabled tool definition is sent with each request, even if you only asked about a typo.
* When the context window fills up, VS Code automatically **compacts** the conversation by summarizing older history. You can also select the context window control, then **Compact Conversation**. See [Manage sessions: compact conversation context](https://code.visualstudio.com/docs/agents/run/sessions/manage-sessions).

> [!TIP]
> **Start fresh between SDLC stages**
>
> A new chat session (**+** in the Chat view) for each feature or stage keeps the context lean. Subagents give you that isolation automatically. For which customization types cost tokens on every request, see the matrix in [1.2](#when-to-use-what) and the mental model in [1.3](#13-token-optimization-mental-model).

<a id="102-copilot-fusion-for-each-sdlc-stage"></a>
<a id="105-model-strategy-recap"></a>

### 10.2 Copilot Fusion for Each SDLC Stage (~3 min)

[1.4](#14-copilot-fusion) introduced Copilot Fusion's effort levels and execution patterns. Here's how they map to the pipeline you built:

| SDLC stage | Suggested choice | Why |
|------------|------------------|-----|
| **Everyday edits, questions, docs** | **Fusion Standard** | Picks one model per task, balancing cost, quality, and latency |
| **PLAN** (Planner, DevFlow) | **Fusion Standard**, or **Fusion High** for large features | Planning complexity varies; no `model` line in the agent file means it uses the picker |
| **IMPLEMENT**: a complex bug or a change across several files | **Fusion High** | Fusion High can add a **Cascade** or **Critique** pass, a built-in review loop before you see the result |
| **IMPLEMENT**: hard architecture or debugging | **Fusion Max** | Deepest reasoning; can use the most AI credits |
| **TEST** (Tester) | A pinned small/fast model, or **Fusion Standard** | Focused, well-defined task |
| **REVIEW** (Code Reviewer) | A **pinned** balanced model, ideally from a **different family** than the model that wrote the code ([5.3](#53-per-agent-model-selection)) | Consistent, repeatable reviews, plus a second opinion with different blind spots |

**Pinned per-agent models vs Copilot Fusion:**

* **Pinned** models are *predictable*. You know which model runs at each stage and roughly what it costs.
* **Copilot Fusion** is *adaptive*. It decides per request which models to use and whether a second model should critique or take over. It's billed for every model it uses, so cost and latency vary.
* Copilot Fusion **doesn't start subagents**, so it complements DevFlow rather than replacing it. Use it in a regular Agent session, or as the picker's model for agents with no `model` line.
* Everything stays within your org's policies: Copilot Fusion uses only the models your admin allows.

<a id="103-hands-on-compare-copilot-fusion-effort-levels"></a>
<a id="106-use-case-copilot-fusion-for-a-review-heavy-change"></a>
<a id="106-use-case-hydrafusion-for-a-review-heavy-change"></a>

### 10.3 Hands-on: Compare Copilot Fusion Effort Levels (~10 min)

**Goal:** run the same well-scoped IMPLEMENT task with **Fusion Standard** and with **Fusion High** (or **Fusion Max**), and decide when a multi-model Critique or Cascade pass is worth its extra cost and time.

> [!NOTE]
> **Before you start**
>
> * Copilot Fusion must be in your model picker. If it isn't, see [Copilot Fusion Not in Your Picker](#troubleshooting-copilot-fusion).
> * You choose the effort level with the control next to **Copilot Fusion** in the chat input (it shows **Standard**, **High**, or **Max**), not as a separate entry in the model list. If **High** isn't offered, your admin's model policies probably block a model it needs. Run step 1 yourself and follow step 2 on the facilitator's screen.
> * This uses the preferences feature from [Part 7](#75-run-the-pipeline). If your run didn't finish, use any well-scoped change that touches a few files in the sample app.
> * Make sure your work is **committed** first ([Part 8](#82-review-and-commit)), including the Part 9 hook files if you did Part 9. `git clean` in step 1 below deletes any uncommitted new files. Don't use `git stash` here: stashing would also hide the preferences feature that this task builds on. With a clean commit, you can discard each attempt completely and both runs start from the same state.

1. **Baseline with Fusion Standard.** Start a new chat session (**+**), select the built-in **Agent**, not DevFlow, choose **Copilot Fusion** in the model picker, and check that the effort level next to it shows **Standard**. Send:

   ```text
   Add a "Reset to defaults" capability to user preferences: a DELETE
   endpoint on the preferences controller that restores the default theme
   and notification settings, and a Reset button on the preferences page.
   Add backend tests for the reset behavior.
   ```

   > [!NOTE]
   > This task is new work on purpose. DevFlow usually adds input validation to the preferences endpoint in Part 7, so a validation task would have nothing left to do.

   Note how long it takes, what files it changes, and whether the tests pass. Then discard **all** of its changes, including new test files, in the integrated terminal:

   ```bash
   git restore .
   git clean -fd
   ```

   `git restore .` reverts edited files, and `git clean -fd` deletes new untracked files. Both only touch uncommitted work, which is why you committed first. Run `git status` and check that it says the working tree is clean.

2. **Same task with a higher effort level.** Start another new session, select the **Agent**, choose **Copilot Fusion**, and set the effort level next to it to **High** (or **Max**). Send the same prompt.

3. **Observe while it works:** the Chat view shows the progress of each step. Which pattern did it choose: **Single**, **Cascade**, or **Critique**? When it finishes, hover over the effort level name below the response to see which models it used. Do the same for the Fusion Standard run if you still have that session open.

4. **Compare the results:** did the tests pass? Did a Cascade or Critique pass catch something, such as a missing edge case or a weak error message, that the Fusion Standard run missed? Review every change before you keep it. If Copilot Fusion discards a draft, file edits that draft already made aren't undone automatically. When you're done comparing, run `git restore .` and `git clean -fd` again to discard this attempt too, unless you want to keep it.

5. **Compare the cost:** Copilot Fusion is billed for every model it used, so Fusion High and Max can use more AI credits. Check your AI credit usage as described in [Monitoring your GitHub AI Credits usage](https://docs.github.com/en/copilot/how-tos/manage-and-track-spending/monitor-ai-usage). Usage may not update immediately.

**Discuss (2 min):**

* When could a **Critique** pass replace a separate **Code Reviewer** pass, and when do you still want the review-only role from Part 5 with its own checklist, Git inspection tools, and a human gate?
* Was Fusion High's result worth the extra AI credits for this task? Which tasks in your own backlog would you send to Fusion High or Max?
* Which agents would you pin to a specific model, and which would you leave on Copilot Fusion?

> [!NOTE]
> **About the benchmarks**
>
> GitHub's [Copilot Fusion announcement](https://github.blog/changelog/2026-10-28-route-and-orchestrate-models-with-github-copilot-fusion/) reports that on the Artificial Analysis Coding Agent Index, Fusion Max scored within 2 points of Claude Code with Opus 5.5 Max, at about half the cost and tokens and about 40% less time.
>
> These are vendor-reported results under specific configurations. Your results will vary. Measure on your own tasks.

---

**Next:** [Part 11: Package & Share →](#part-11-package--share-8-min) · [Back to contents](#contents)

<a id="part-11-package--share-8-min"></a>

## Part 11: Package & Share (~8 min)

You've built an agentic SDLC framework. Now share it with your team. The simplest option is to **commit `.github/agents/` and `.github/skills/` to the repo**, and everyone who clones it gets them automatically. To reuse the framework *across* repos, package it.

<a id="111-agent-plugins"></a>

### 11.1 Agent Plugins

An agent plugin is a packaged bundle of customizations: skills, custom agents, hooks, MCP servers, and slash commands. In VS Code, you can install plugins from marketplaces (Git repositories) or directly from a source repo.

> [!WARNING]
> **Preview feature**
>
> Agent plugins in VS Code are in **Preview**. Enable them with the `chat.plugins.enabled` setting. Some related settings are marked experimental. See [Agent plugins in VS Code](https://code.visualstudio.com/docs/agent-customization/agent-plugins).

<a id="step-1-create-a-marketplace-repo-and-plugin-manifest-local"></a>

#### Step 1: Create a marketplace repo and plugin manifest (local)

You'll build the plugin in a **local Git repo**. Hosting it comes later, as a concept, so you don't need to create a repo on any Git host today.

* Create a folder named `plugin-marketplace` next to the sample app. Open it in a **new VS Code window**, and run `git init` in the integrated terminal.
* In the integrated terminal, create the directory structure:

<details><summary>macOS</summary>

```bash
mkdir -p .github/plugin
mkdir -p plugins/dev-flow/agents plugins/dev-flow/skills
```

</details>
<details><summary>Windows (PowerShell)</summary>

```powershell
New-Item -ItemType Directory -Force -Path .github\plugin | Out-Null
New-Item -ItemType Directory -Force -Path plugins\dev-flow\agents, plugins\dev-flow\skills | Out-Null
```

</details>

* Copy your customizations from the sample app repo. Run this in the `plugin-marketplace` terminal. It assumes the sample app folder sits next to `plugin-marketplace` and is named `dotnet-react-starter-demo`; change `../dotnet-react-starter-demo` if yours is different. The commands copy only the four DevFlow agents, not the sample repo's other agents. If `gh skill` installed `documentation-writer` into `.agents/skills/` ([3.3](#33-install-a-community-skill)), copy it from there instead.

<details><summary>macOS</summary>

```bash
SRC=../dotnet-react-starter-demo/.github
cp "$SRC"/agents/{dev-flow,planner,reviewer,tester}.agent.md plugins/dev-flow/agents/
cp -r "$SRC"/skills/naming-checker "$SRC"/skills/task-breakdown plugins/dev-flow/skills/
DOCS=$( [ -d "$SRC"/skills/documentation-writer ] && echo "$SRC"/skills/documentation-writer || echo ../dotnet-react-starter-demo/.agents/skills/documentation-writer )
cp -r "$DOCS" plugins/dev-flow/skills/
find plugins -type f
```

</details>
<details><summary>Windows (PowerShell)</summary>

```powershell
$src = "..\dotnet-react-starter-demo\.github"
"dev-flow", "planner", "reviewer", "tester" | ForEach-Object { Copy-Item "$src\agents\$_.agent.md" plugins\dev-flow\agents\ }
Copy-Item -Recurse -Force "$src\skills\naming-checker", "$src\skills\task-breakdown" plugins\dev-flow\skills\
$docs = if (Test-Path "$src\skills\documentation-writer") { "$src\skills\documentation-writer" } else { "..\dotnet-react-starter-demo\.agents\skills\documentation-writer" }
Copy-Item -Recurse -Force $docs plugins\dev-flow\skills\
Get-ChildItem -Recurse -File plugins | Select-Object FullName
```

</details>

You should end up with:

```bash
plugins/dev-flow/agents/dev-flow.agent.md
plugins/dev-flow/agents/planner.agent.md
plugins/dev-flow/agents/reviewer.agent.md
plugins/dev-flow/agents/tester.agent.md
plugins/dev-flow/skills/documentation-writer/
plugins/dev-flow/skills/naming-checker/
plugins/dev-flow/skills/task-breakdown/
```

> [!NOTE]
> **Public content and dev dependencies**
>
> `documentation-writer` and `skill-creator` are existing public skills. This lab doesn't package `skill-creator`, because it's a dev-time tool. `documentation-writer` is packaged so your team controls its version. Check each skill's license before you redistribute it.

* Create `plugins/dev-flow/README.md` to describe the plugin, what's in it, and how to use it. A good README makes the plugin easier to discover and use. Use Copilot to draft it from the agent and skill files.

* Create the plugin manifest, `plugins/dev-flow/plugin.json`. The `repository` value is just metadata. Point it at wherever your team will host the repo, on GitLab or GitHub:

```json
{
  "name": "dev-flow",
  "description": "Agentic SDLC framework: planning, implementation, testing, review, and ship preparation with specialized agents, skills, and human-in-the-loop stage gates",
  "version": "1.0.0",
  "author": {
    "name": "Your Name"
  },
  "repository": "https://gitlab.com/your-group/plugin-marketplace",
  "keywords": ["sdlc", "agentic", "planning", "testing", "code-review"],
  "agents": [
    "./agents"
  ],
  "skills": [
    "./skills/documentation-writer",
    "./skills/naming-checker",
    "./skills/task-breakdown"
  ]
}
```

Only add a `license` field if it accurately describes the rights you can grant for the packaged content; an example manifest does not relicense third-party skills.

* Create the marketplace metadata file, `.github/plugin/marketplace.json`:

```json
{
  "name": "plugin-marketplace",
  "metadata": {
    "description": "My team's collection of GitHub Copilot plugins, agents, and skills",
    "version": "1.0.0",
    "pluginRoot": "./plugins"
  },
  "owner": {
    "name": "My Name",
    "email": "my-email@example.com"
  },
  "plugins": [
    {
      "name": "dev-flow",
      "source": "dev-flow",
      "description": "Agentic SDLC orchestration for GitHub Copilot: plan, implement, test, review, and ship with human-in-the-loop stage gates.",
      "version": "1.0.0",
      "author": {
        "name": "Your Name"
      }
    }
  ]
}
```

* Commit locally from the **Source Control** view, or in the integrated terminal:

```bash
git add .
git commit -m "Add DevFlow plugin with agents and skills"
```

<a id="step-2-install-and-verify-the-plugin-locally-in-vs-code"></a>

#### Step 2: Install and Verify the Plugin Locally in VS Code

Open a **different** folder in VS Code (any project without the DevFlow files, or an empty folder), then:

1. Open **Settings**, search for `chat.plugins.enabled`, and turn it on.
2. Register the plugin folder with the `chat.pluginLocations` setting (experimental). This setting is edited as JSON: run **Preferences: Open User Settings (JSON)** from the Command Palette, and add the entry inside the outer `{ }`. Use the absolute path to `plugins/dev-flow` on your machine (in the `plugin-marketplace` terminal, run `pwd` to get the folder path; on Windows, double each backslash in the JSON, as in the example):

<details><summary>macOS</summary>

```json
// settings.json
"chat.pluginLocations": {
  "/Users/you/src/plugin-marketplace/plugins/dev-flow": true
}
```

</details>
<details><summary>Windows</summary>

```json
// settings.json
"chat.pluginLocations": {
  "C:\\Users\\you\\src\\plugin-marketplace\\plugins\\dev-flow": true
}
```

</details>

3. Verify:
   - **Chat: Open Customizations → Plugins** lists **dev-flow**. (The Extensions view's `@agentPlugins` search now just links to Customizations.)
   - **Open Customizations → Agents** lists DevFlow, Planner, Tester, and Code Reviewer.
   - **Open Customizations → Skills** lists the three skills.
   - The agent picker in the Chat view shows **DevFlow**.

Set the value to `false` to keep the plugin registered but disabled.

<a id="step-3-concept-publish-the-marketplace-on-your-git-host"></a>

#### Step 3: Concept: Publish the Marketplace on Your Git Host

To share with your team, push `plugin-marketplace` to your Git host, for example a GitLab project. Teammates then add it to the `chat.plugins.marketplaces` setting and install **dev-flow** from **Chat: Open Customizations → Plugins → Browse**.

VS Code accepts marketplace references in these forms: `owner/repo` shorthand (public **GitHub** repos only), an **HTTPS git remote ending in `.git`**, an **SCP-style SSH remote**, or a `file:///` path to a local clone. Private repositories are supported. For a GitLab-hosted marketplace, use the full git URL:

```json
// settings.json
"chat.plugins.marketplaces": [
  "https://gitlab.com/your-group/plugin-marketplace.git"
]
```

> [!CAUTION]
> **Verify GitLab hosting in your environment**
>
> The VS Code docs list generic HTTPS and SSH git remotes as supported marketplace sources, but they only show GitHub URLs in their examples. Test a GitLab-hosted marketplace, including authentication to a private GitLab instance, before you roll it out to your team.

> [!TIP]
> **Recommend the plugin to your whole team**
>
> To have VS Code prompt teammates to install the plugin when they open a project, add workspace plugin recommendations (`extraKnownMarketplaces` and `enabledPlugins`) to `.github/copilot/settings.json`. See [Agent plugins: workspace recommendations](https://code.visualstudio.com/docs/agent-customization/agent-plugins).

<a id="distribution-options"></a>

#### Distribution Options

| Method | Scope | How (VS Code) |
|--------|-------|-----|
| **Git repository** | Anyone who clones | Commit `.github/agents/`, `.github/skills/`, `.github/instructions/`, and `.vscode/mcp.json` to the repo, on any Git host |
| **Local plugin** | Single developer | Register a local folder in the `chat.pluginLocations` setting |
| **Plugin marketplace** | Team or community | A marketplace Git repo in `chat.plugins.marketplaces` + **Open Customizations → Plugins** |
| **APM** | Team, across many repos | `apm install <git URL>` in the integrated terminal. Works with GitLab, GitHub, and other Git hosts |
| **GitHub CLI** | Public community skills | `gh skill install owner/repo` in the integrated terminal |

<a id="step-4-customization-levels"></a>

#### Concept: Customization Levels

```
┌───────────────────────────────────────────────────┐
│  Personal    ~/.copilot/agents/                   │
│              ~/.copilot/skills/                   │
│              Your customizations, your machine    │
├───────────────────────────────────────────────────┤
│  Project     .github/agents/                      │
│              .github/skills/                      │
│              .github/hooks/                       │
│              Shared via git: everyone on the      │
│              project gets these automatically     │
├───────────────────────────────────────────────────┤
│  Team        Shared plugin marketplace repo       │
│              Installed via VS Code agent plugins  │
│              (or APM, below)                      │
└───────────────────────────────────────────────────┘
```

**Best practice:** Keep shared agents and skills in a "customization" repository that teams can contribute to and install from. You can organize it with directories for different teams, projects, or types of customization.

> [!NOTE]
> **Organization-wide "push" distribution**
>
> Some GitHub plans can push agents and policies to every member of an organization or enterprise, without each person installing a plugin. This lab sticks to features available on **Copilot Business**, and to sharing that works from any Git host. So it uses the *pull* model: repos, plugins, and APM.

<a id="112-optional-agent-package-manager-apm"></a>

### 11.2 *(Optional)* Agent Package Manager (APM)

APM is an open-source dependency manager for AI agent configuration. Think `package.json`, but for agents.

* **Plugins** bundle customizations and install them on demand.
* **APM** adds **reproducibility**. You declare agent dependencies once in `apm.yml` and commit it. Every developer who clones the repo gets the same agent setup, with version locking, transitive dependency resolution, and built-in security scanning.

| | Plugins | APM |
|---|---|---|
| **Primary use** | Bundle & distribute customizations | Declarative dependency management for teams |
| **Configuration** | `plugin.json` + `marketplace.json` | `apm.yml` + `apm.lock.yaml` |
| **Install in VS Code** | **Open Customizations → Plugins** or the `chat.pluginLocations` setting | `apm install` in the integrated terminal |
| **Lockfile** | No consumer-side lockfile | `apm.lock.yaml` pins exact resolved commits |
| **Security scanning** | None | Built-in hidden Unicode / tampering scan |

<a id="install-apm-integrated-terminal"></a>

#### Install APM (integrated terminal)

<details><summary>macOS</summary>

```bash
curl -sSL https://aka.ms/apm-unix | sh
apm --version
```

</details>
<details><summary>Windows (PowerShell)</summary>

```powershell
irm https://aka.ms/apm-windows | iex
apm --version
```

</details>

<a id="convert-the-marketplace-repo-to-an-apm-package"></a>

#### Convert the Marketplace Repo to an APM Package

APM uses a `.apm/` directory as the **source of truth** for your own primitives. `apm install` deploys them to `.github/agents/`, `.github/skills/`, and so on. In the `plugin-marketplace` window, run:

<details><summary>macOS</summary>

```bash
apm init --yes
mkdir -p .apm/agents .apm/skills
cp plugins/dev-flow/agents/*.agent.md .apm/agents/
cp -r plugins/dev-flow/skills/* .apm/skills/
echo "apm_modules/" >> .gitignore
git add .apm/ apm.yml .gitignore
git commit -m "Add APM manifest and .apm source content"
```

</details>
<details><summary>Windows (PowerShell)</summary>

```powershell
apm init --yes
New-Item -ItemType Directory -Force -Path .apm\agents, .apm\skills | Out-Null
Copy-Item plugins\dev-flow\agents\*.agent.md .apm\agents\
Copy-Item -Recurse -Force plugins\dev-flow\skills\* .apm\skills\
Add-Content -Path .gitignore -Value "apm_modules/"
git add .apm/ apm.yml .gitignore
git commit -m "Add APM manifest and .apm source content"
```

</details>

To try it hands-on without hosting anything, install from the local folder in any other project. APM supports local path dependencies:

```bash
apm install ../plugin-marketplace
```

**Concept:** Once the repo is pushed to your Git host, teammates install it by URL. APM supports GitLab, GitHub, Bitbucket, Azure DevOps, and other Git hosts:

```bash
apm install https://gitlab.com/your-group/plugin-marketplace.git
```

APM deploys the agents into `.github/agents/` and the skills into `.github/skills/`. VS Code picks them up right away; reload the window if needed. Commit `apm.yml`, `apm.lock.yaml`, and the deployed `.github/` files. Add `apm_modules/` to `.gitignore`.

| Command | Purpose |
|---------|---------|
| `apm init` | Scaffold `apm.yml` in the current directory |
| `apm install <pkg>` | Add a package, download, and deploy |
| `apm install` | Install all packages from `apm.yml` |
| `apm deps update` | Pull latest versions of all dependencies |
| `apm audit` | Scan packages for hidden Unicode and tampering |
| `apm pack --format plugin` | Export as a standalone plugin bundle |

> [!NOTE]
> **Security by Default**
>
> `apm install` scans every package for **hidden Unicode characters**. Attackers can hide these invisible characters in prompt files to hijack agent behavior. The scan runs automatically on every install. To run it on demand or in CI, use `apm audit`.

---

**Next:** [Agentic SDLC Recap →](#agentic-sdlc-recap) · [Back to contents](#contents)

<a id="troubleshooting"></a>

## Troubleshooting

<a id="troubleshooting-mcp"></a>

### MCP Server Won't Start

* **Copilot Business:** the **MCP servers in Copilot** policy is disabled by default. Ask your org admin to enable it. Until then, skip Part 6. The rest of the lab still works.
* Run **MCP: List Servers**, select `playwright`, and select **Show Output** to read the error.
* Check that `node --version` prints 18 or later in the integrated terminal. A corporate proxy can also block `npx` from downloading `@playwright/mcp`.
* **Corporate npm mirror:** run `npm view @playwright/mcp dist-tags`. If `latest` shows an old `1.52.0-alpha` build, your mirror's tag is stale. Use `@playwright/mcp@0.0` as in [6.1](#61-add-the-playwright-mcp-server), not `@latest`.
* If you dismissed the trust prompt, start the server again and confirm it.

<a id="troubleshooting-playwright-profile"></a>

### Playwright: "Browser Profile Is Already in Use"

VS Code starts a separate Playwright MCP server for each chat session. Without `--isolated`, they share one browser profile, and a browser left open by an earlier session blocks the next one. Add `"--isolated"` to the `args` in `.vscode/mcp.json` ([6.1](#61-add-the-playwright-mcp-server)), close any leftover Playwright Chrome window, restart the server (**MCP: List Servers → playwright → Restart Server**), and start a new chat session.

<a id="troubleshooting-playwright-subagent"></a>

### Tester Says Playwright Tools Aren't Available

If the Tester can use Playwright when you select it directly but not when DevFlow delegates to it, DevFlow's `tools` list is missing `playwright/*`. A subagent can only use tools its parent also has. Add `playwright/*` to `dev-flow.agent.md` and start a **new** chat session, because existing sessions keep the old agent definition.

<a id="troubleshooting-playwright-browser"></a>

### Playwright Can't Open a Browser

By default, Playwright MCP drives the **Google Chrome** installed on your machine. If the tool call fails with an error about Chrome or a missing browser:

* **Install Chrome**, or let Playwright install it: run `npx playwright install chrome` in the integrated terminal. This can need admin rights.
* **Windows alternative:** use Microsoft Edge, which is already installed. In `.vscode/mcp.json`, add `"--browser", "msedge"` to the end of `args`, then restart the server (**MCP: List Servers → playwright → Restart Server**).
* A corporate proxy can block the browser download. Ask your IT team, or use the Edge option.

<a id="troubleshooting-model"></a>

### Model Not in Your Picker

On Copilot Business, admin policies control which models you can use. Choose **Copilot Fusion** or another listed model in the picker. In agent files, remove the `model` line, and the agent uses the picker's model.

<a id="troubleshooting-copilot-fusion"></a>
<a id="troubleshooting-hydrafusion"></a>

### Copilot Fusion Not in Your Picker

* Update VS Code (**Help → Check for Updates**), then reload the window. In VS Code, **Copilot Fusion** replaces the older **Auto** option.
* **Copilot Business:** Copilot Fusion only uses models your admin's model policies allow. If **Fusion Max** is missing, a model it needs is probably disabled. If Copilot Fusion is missing entirely, none of its models are allowed. Ask your admin.
* If it's still missing, select a specific model and skip step 2 of section 10.3.

<a id="troubleshooting-gh-skill"></a>

### `gh skill` Fails

* Run `gh --version` and check for **2.90.0 or later**.
* Run `gh auth status`. If you aren't signed in, run `gh auth login`.
* If you can't install or sign in to `gh`, use the manual `git clone` steps in [3.3](#33-install-a-community-skill).

<a id="troubleshooting-agent-skill"></a>

### Custom Agent or Skill Doesn't Appear

* **Location:** agents go in `.github/agents/<name>.agent.md`. Skills go in `.github/skills/<name>/SKILL.md`, and the skill's `name` must match its folder name.
* **Frontmatter:** it must start and end with `---` lines and be valid YAML. Quote values that contain `:` or `#`.
* **Workspace:** the sample repo itself must be the folder you opened in VS Code, not a parent folder.
* Run **Developer: Reload Window**. Then check **Chat: Open Customizations**, or run **Developer: Open Agent Debug Logs** and check the discovery rows for load errors.

<a id="troubleshooting-instructions"></a>

### Instructions Don't Load

* `applyTo` globs are relative to the workspace root, for example `backend/**/*.cs`. Quote the glob in the frontmatter.
* If the brace glob `frontend/**/*.{ts,tsx}` doesn't match in your VS Code version, use `frontend/**` instead.
* Run **Developer: Open Agent Debug Logs**: the **Instructions Discovery** row shows which files were found, and the **Custom Instructions** rows show what each request included. In the Copilot harness, a **Read *.instructions.md** progress step shows an on-demand file being loaded.
* Path-specific files are attached when the agent works on matching files, so ask about a specific file in that folder.

<a id="troubleshooting-prompt-file"></a>

### Prompt File Not Found

* Prompt files run in the **Local** harness. Switch the Session Target ([Copilot vs Local](#session-target)).
* The file must be `.github/prompts/<name>.prompt.md`. You can also run **Chat: Run Prompt** from the Command Palette.

<a id="troubleshooting-hook"></a>

### Hook Doesn't Fire

* Hooks are a **Preview** feature and run in the **Copilot** harness. Check the Session Target.
* Confirm that the hook is listed under **Open Customizations → Hooks**, and check the **Hook Discovery** row in **Developer: Open Agent Debug Logs** for errors.
* Make sure the JSON is valid and `node` is on your `PATH`. Run `node --version` in the integrated terminal.
* On Copilot Business, your admin might manage hook policies.
* **Never approve** a `git push` while you test the hook. Select **Skip** until the hook blocks it.

<a id="troubleshooting-ports"></a>

### App Ports Are Busy

The backend uses port **5000**. The frontend uses port **5173**, and Vite is configured with `strictPort`, so it stops with an error instead of switching ports. Find and stop whatever is using the port:

<details><summary>macOS</summary>

```bash
lsof -i :5000      # or :5173
kill <PID>
```

On macOS, the **AirPlay Receiver** often uses port 5000. Turn it off in **System Settings → General → AirDrop & Handoff**.

</details>
<details><summary>Windows (PowerShell)</summary>

```powershell
Get-NetTCPConnection -LocalPort 5000 | Select-Object OwningProcess   # or 5173
Stop-Process -Id <PID>
```

</details>

If you change a port instead, update the Vite proxy target and the URLs in your prompts and in the Playwright check.

<a id="agentic-sdlc-recap"></a>

## Agentic SDLC Recap

Each SDLC stage maps to the customization primitive that powers it:

| SDLC stage | Agent / actor | Customization primitive(s) | Human-in-the-loop checkpoint | File(s) |
|------------|---------------|----------------------------|------------------------------|---------|
| **Foundation** | Every agent | Custom instructions: repo-wide + path-specific (`applyTo`) | You keep them lean and reviewed | `.github/copilot-instructions.md`, `.github/instructions/*.instructions.md` |
| **Plan** | Planner (subagent) | Custom agent + skill (`task-breakdown`); issue-tracker MCP *(concept)* | Approve the plan | `.github/agents/planner.agent.md`, `.github/skills/task-breakdown/` |
| **Implement** | DevFlow | Orchestrator agent (`agents:`) + skill with script (`naming-checker`), following the instructions files | Keep/Undo diffs, approve terminal commands | `.github/agents/dev-flow.agent.md`, `.github/skills/naming-checker/` |
| **Test** | Tester (subagent) | Custom agent with `execute` + Playwright MCP server (`playwright/*`) | Approve test results | `.github/agents/tester.agent.md`, `.vscode/mcp.json` |
| **Review** | Code Reviewer (subagent) | Review-only role + Git inspection via `execute` + per-agent `model` | Choose which findings to fix | `.github/agents/reviewer.agent.md` |
| **Ship** | **You** (+ DevFlow drafts) | Community skill (`documentation-writer`) + prompt file (`/pr-description`) + Source Control; opening the MR/PR on your Git host *(concept)* | You branch, commit, and open the MR/PR | `.github/skills/documentation-writer/`, `.github/prompts/pr-description.prompt.md` |
| **Guardrails** *(optional)* | Hook script | Agent hooks (Preview) | Policy enforced deterministically | `.github/hooks/devflow-guardrails.json` |
| **Share** | Your team | Agent plugin / APM (any Git host) | Team installs and reviews versions | `plugin.json`, `marketplace.json`, `apm.yml` |
| **Model strategy** | Copilot Fusion effort levels | Model picker: Copilot Fusion (Standard/High/Max) or pinned per-agent `model` (Critique ≈ parallel critique, Cascade ≈ solver/judge review pass) | You review every change and compare cost | None (a picker choice, not a file) |

<a id="next-steps"></a>

## Next Steps

* **Add more skills** to complete DevFlow: `code-scaffold`, `test-generator`, and `changelog-entry`.
* **Connect PLAN to real work items:** if your org allows it, add your issue tracker's MCP server (for example the [GitLab MCP server](https://docs.gitlab.com/user/model_context_protocol/mcp_server/)) and give only the Planner its tools.
* **Grow your instructions carefully:** add path-specific `.instructions.md` files as you notice repeated corrections, and keep the repo-wide file short.
* **Go deeper on customization:** explore the [VS Code customization guide](https://code.visualstudio.com/docs/agent-customization/overview).

<a id="additional-resources"></a>

## Additional Resources

- [VS Code: Customization concepts](https://code.visualstudio.com/docs/agents/concepts/customization)
- [VS Code: Custom instructions](https://code.visualstudio.com/docs/agent-customization/custom-instructions)
- [VS Code: Prompt files](https://code.visualstudio.com/docs/agent-customization/prompt-files)
- [VS Code: MCP servers](https://code.visualstudio.com/docs/agent-customization/mcp-servers)
- [VS Code: Approvals](https://code.visualstudio.com/docs/agents/run/approvals)
- [VS Code: Custom agents](https://code.visualstudio.com/docs/agent-customization/custom-agents)
- [VS Code: Custom agent file reference](https://code.visualstudio.com/docs/agents/reference/custom-agent-file-reference)
- [VS Code: Agent Skills](https://code.visualstudio.com/docs/agent-customization/agent-skills)
- [VS Code: Subagents](https://code.visualstudio.com/docs/agents/run/subagents)
- [VS Code: Agent hooks (Preview)](https://code.visualstudio.com/docs/agent-customization/hooks)
- [VS Code: Agent plugins (Preview)](https://code.visualstudio.com/docs/agent-customization/agent-plugins)
- [VS Code: Agent harnesses](https://code.visualstudio.com/docs/agents/run/agent-harnesses)
- [GitHub Docs: Custom agents configuration reference](https://docs.github.com/en/copilot/reference/custom-agents-configuration)
- [GitHub Docs: Usage-based billing for individuals](https://docs.github.com/en/copilot/concepts/billing-and-usage/individuals/billing)
- [GitHub Docs: Models and pricing](https://docs.github.com/en/copilot/reference/copilot-billing/models-and-pricing)
- [GitHub Docs: About Copilot Fusion](https://docs.github.com/en/copilot/concepts/models/copilot-fusion)
- [GitHub Changelog: Route and orchestrate models with GitHub Copilot Fusion](https://github.blog/changelog/2026-10-28-route-and-orchestrate-models-with-github-copilot-fusion/)
- [GitLab for VS Code extension](https://marketplace.visualstudio.com/items?itemName=GitLab.gitlab-workflow)
- [GitLab MCP server](https://docs.gitlab.com/user/model_context_protocol/mcp_server/)
- [Playwright MCP server](https://github.com/microsoft/playwright-mcp)
- [github/awesome-copilot: community-curated agents and skills](https://github.com/github/awesome-copilot)
- [agentskills/agentskills: open standard specification for skills](https://github.com/agentskills/agentskills)
- [Agent Package Manager (APM)](https://microsoft.github.io/apm/) and its [GitHub repo](https://github.com/microsoft/apm)

<a id="solution-files"></a>

## Appendix: Solution Files

Use these reference files to catch up. Create each file at the path shown, relative to the root of your sample repo. Files that Copilot generates for you, such as the instructions and skills, will differ from these. That's expected. These are minimal working versions.

<a id="solution-part-2"></a>

### Part 2 Files: Custom Instructions

First move the long standards doc out of the always-on path:

```bash
git mv .github/copilot-instructions.md docs/development-standards.md
```

<details>
<summary><code>.github/copilot-instructions.md</code></summary>

```markdown
# Copilot instructions

## Stack
- Backend: ASP.NET Core (.NET 8) Web API in `backend/src/Api`, xUnit tests in `backend/tests/Api.Tests`
- Frontend: React 18 + TypeScript + Vite + Tailwind in `frontend/`
- `legacy/` is reference-only. Don't modify it.

## Commands
- Backend: `dotnet build backend/Api.sln`, `dotnet test backend/Api.sln`
- Frontend (from `frontend/`): `npm run lint`, `npm run build` (there is no frontend test script)

## Conventions
- Commits use Conventional Commits with scopes `backend`, `frontend`, `tests`, `config`.
- Never commit directly to `main`. Use a feature branch.
- Full standards: [docs/development-standards.md](../docs/development-standards.md). Read the relevant section before larger changes.
```

</details>

<details>
<summary><code>.github/instructions/backend-csharp.instructions.md</code></summary>

```markdown
---
name: "Backend C# conventions"
description: "Conventions for the ASP.NET Core API and its xUnit tests. Use when writing or reviewing C# in backend/."
applyTo: "backend/**/*.cs"
---

# Backend (ASP.NET Core, .NET 8)

## API design
- Routes: `[Route("v1/[controller]")]`, no `/api/` prefix, plural lowercase resources, no trailing slash.
- Wrap responses in `ItemResponseDto<T>` / `CollectionResponseDto<T>` (item/items + metadata + links).
- Paginate with `offset`/`limit` (default 20, max 100).
- Error codes follow `ORG-<AREA>-<NNN>`, for example `ORG-VAL-001`.
- Annotate actions with `[ProducesResponseType]`.

## Naming
- Booleans end in `Indicator` (`activeIndicator`), never `is`/`has` prefixes.
- Dates end in `Date`, identifiers end in `Id`.
- Async methods end in `Async` and take `CancellationToken` as the last parameter.

## Structure
- DTOs live in `DTOs/` with `[JsonPropertyName("camelCase")]` and XML doc comments.
- Data access goes through interfaces in `Repositories/`, business logic in `Services/`.
- Use `AsNoTracking()` for reads. No stored procedures.
- Register services with `AddScoped` in `Extensions/ServiceCollectionExtensions.cs`.

## Tests
- Name tests `{Method}_{Scenario}_{Expected}`, with `// Arrange`, `// Act`, `// Assert` comments.
- Use the EF Core InMemory provider (never SQLite) and the existing test data builders.
```

</details>

<details>
<summary><code>.github/instructions/frontend-react.instructions.md</code></summary>

```markdown
---
name: "Frontend React conventions"
description: "Conventions for the React + TypeScript + Tailwind frontend. Use when writing or reviewing code in frontend/."
applyTo: "frontend/**/*.{ts,tsx}"
---

# Frontend (React 18, TypeScript, Vite, Tailwind)

- One component per folder: `src/components/Name/Name.tsx` plus an `index.ts` re-export.
- TypeScript strict mode. Use `interface` for object shapes, `type` for unions. Shared types go in `src/lib/types.ts`.
- Call the API only through `fetchApi` in `src/lib/api.ts`. The base URL comes from `VITE_API_BASE_URL`.
- Style with Tailwind utility classes only. Use brand colors through the `--color-brand-*` CSS variables. Never hardcode hex values.
- Don't hardcode user-facing strings inline in components.
- Before finishing, run `npm run lint` and `npm run build` from `frontend/`.
```

</details>

<a id="solution-part-4"></a>

### Parts 3–4 Files: Skills

For the community `documentation-writer` skill, use the manual `git clone` steps in [3.3](#33-install-a-community-skill).

<details>
<summary><code>.github/skills/task-breakdown/SKILL.md</code></summary>

```markdown
---
name: task-breakdown
description: "Break down feature requests and user stories into implementable sub-tasks with T-shirt size estimates and a risk assessment. Use when asked to break down, decompose, or plan implementation for a feature."
---

# Task Breakdown

1. Restate the feature in one sentence. List assumptions and open questions.
2. Identify the affected components: backend API, frontend UI, tests, config, docs.
3. Split the work into sub-tasks sized XS, S, M, L, or XL. Each task must be small enough to review on its own.
4. Risk assessment: flag every task that touches shared code paths or needs a database migration, and say how to reduce the risk.
5. Output the table below, then the risks. Don't write code.

| # | Task | Component | Size | Depends on | Acceptance criteria |
|---|------|-----------|------|------------|---------------------|

## Risks
- <task #>: <risk> → <mitigation>
```

</details>

<details>
<summary><code>.github/skills/naming-checker/SKILL.md</code></summary>

```markdown
---
name: naming-checker
description: "Check file and identifier naming conventions: kebab-case files and folders, PascalCase React components and types, camelCase variables, UPPER_SNAKE_CASE constants. Use when asked to check naming, validate naming conventions, or review new files before committing."
---

# Naming Checker

1. From the repo root, run the bundled script. The default directory is `frontend/src`:
   `node .github/skills/naming-checker/scripts/check-names.js frontend/src`
2. The script flags camelCase file and folder names that should be kebab-case. PascalCase component names are allowed. `.agent.md` and `.test.` files are skipped.
3. Read the changed files and check identifiers: camelCase variables and functions, PascalCase types and components, UPPER_SNAKE_CASE constants. For C#, follow `.github/instructions/backend-csharp.instructions.md`.
4. Report one table: | File | Issue | Severity | Suggested fix |. Don't rename anything unless the user asks.
```

</details>

<details>
<summary><code>.github/skills/naming-checker/scripts/check-names.js</code></summary>

```js
#!/usr/bin/env node
// Usage: node check-names.js [dir]   (default: frontend/src)
// Flags camelCase file and folder names that should be kebab-case.
// PascalCase (React components) is allowed. Skips *.agent.md and *.test.* files.
const fs = require('fs');
const path = require('path');

const root = process.argv[2] || 'frontend/src';
const SKIP_DIRS = new Set(['node_modules', 'dist', 'bin', 'obj', '.git']);
const camelCase = /^[a-z]+[A-Z][A-Za-z0-9]*$/;
const toKebab = (s) => s.replace(/([a-z0-9])([A-Z])/g, '$1-$2').toLowerCase();
const findings = [];

function walk(dir) {
  for (const entry of fs.readdirSync(dir, { withFileTypes: true })) {
    if (SKIP_DIRS.has(entry.name)) continue;
    const full = path.join(dir, entry.name);
    if (entry.name.endsWith('.agent.md') || entry.name.includes('.test.')) continue;
    const base = entry.isDirectory() ? entry.name : entry.name.split('.')[0];
    if (camelCase.test(base)) {
      const fixed = entry.name.replace(base, toKebab(base));
      findings.push({ file: full, severity: 'warning', fix: fixed });
    }
    if (entry.isDirectory()) walk(full);
  }
}

if (!fs.existsSync(root)) {
  console.error(`Directory not found: ${root}`);
  process.exit(2);
}
walk(root);

if (findings.length === 0) {
  console.log(`No naming issues found in ${root}.`);
} else {
  console.log('| File | Severity | Suggested name |');
  console.log('|------|----------|----------------|');
  for (const f of findings) console.log(`| ${f.file} | ${f.severity} | ${f.fix} |`);
}
```

</details>

<a id="solution-part-5"></a>

### Part 5 Files: Code Reviewer Agent

<details>
<summary><code>.github/agents/reviewer.agent.md</code></summary>

```markdown
---
name: "Code Reviewer"
description: "Review code changes for bugs, security issues, and standards compliance. Use when asked to review code, check a PR, or audit changes."
tools: ['read', 'search', 'execute']
---

You are a senior code reviewer. Your job is to review code changes 
thoroughly but efficiently, focusing on what matters most.

## Tool Boundaries
- Use `execute` only for non-mutating inspection commands such as
  `git status --short`, `git diff`, `git diff --cached`, `git log`, and `git show`.
- Inspect both staged and unstaged changes; read untracked files directly.
- Do not modify files, run fixes or formatters, install dependencies, or
  stage, commit, push, or change branches. Report proposed fixes instead.
- Respect command approvals. Access to `execute` is not a read-only sandbox.

## Review Priority (in order)

1. **🔴 Bugs** — Logic errors, null pointer risks, race conditions,
   off-by-one errors
2. **🟠 Security** — SQL injection, XSS, credential exposure,
   insecure deserialization
3. **🟡 Performance** — N+1 queries, unnecessary re-renders,
   missing indexes, memory leaks
4. **🔵 Maintainability** — Code duplication, unclear naming,
   missing error handling

## What You Do NOT Review
- Formatting and style (leave that to linters)
- Import ordering
- Comment style preferences

## Review Process
1. Run Git inspection commands, then read the changed files to understand the diff
2. Search for related code to understand impact
3. Check for the priority items above
4. Present findings in a structured table

## Output Format

### Review Summary
| Category | Findings | Severity |
|----------|----------|----------|
| Bugs | [count] | 🔴 |
| Security | [count] | 🟠 |
| Performance | [count] | 🟡 |
| Maintainability | [count] | 🔵 |

### Detailed Findings
For each finding:
- **File**: path/to/file.ts:line
- **Severity**: 🔴/🟠/🟡/🔵
- **Issue**: What's wrong
- **Fix**: Recommended change

You never edit files. The human decides which findings to fix.
```

</details>

<a id="solution-part-6"></a>

### Part 6 Files: MCP Server

<details>
<summary><code>.vscode/mcp.json</code></summary>

```json
{
  "servers": {
    "playwright": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@playwright/mcp@0.0", "--isolated"]
    }
  }
}
```

</details>

<a id="solution-part-7"></a>

### Part 7 Files: Planner, Tester, and DevFlow Agents

<details>
<summary><code>.github/agents/planner.agent.md</code></summary>

```markdown
---
name: "Planner"
description: "Break down features and issues into implementation plans. Use when asked to plan, decompose, or create tasks for a feature."
tools: ['read', 'search', 'todo']
---

You are a technical planning specialist. You create clear, actionable 
implementation plans from feature requests and issues.

## Your Process
1. Analyze the request to understand scope and requirements
2. Search the codebase to understand current architecture
3. Use the task-breakdown skill for structured decomposition
4. Create a prioritized plan with dependencies

## Constraints
- Each task should be completable in under 1 day
- Always identify risks and unknowns
- Include testing tasks alongside implementation tasks
- Flag tasks that need external input or decisions
- Never edit code. Planning only.
```

</details>

<details>
<summary><code>.github/agents/tester.agent.md</code></summary>

```markdown
---
name: "Tester"
description: "Generate and improve tests. Use when asked to write tests, improve coverage, or validate test quality."
tools: ['read', 'search', 'edit', 'execute', 'playwright/*']
---

You are a testing specialist focused on writing effective, 
maintainable tests.

## Approach
1. Read the source code to understand what needs testing
2. Identify untested paths, edge cases, and error conditions
3. Write tests following the project's existing test patterns
4. Run backend tests with `dotnet test backend/Api.sln`
5. For frontend changes, run `npm run lint` and `npm run build` in
   `frontend/`, then use the Playwright tools to open
   http://localhost:5173, exercise the new UI, and report what you saw

## Constraints
- Match the project's existing test framework and patterns
- Write descriptive test names that explain the scenario
- Include both happy path and error case tests
- Never modify production code unless specifically asked
```

</details>

<details>
<summary><code>.github/agents/dev-flow.agent.md</code></summary>

```markdown
---
name: "DevFlow"
description: "Agentic SDLC orchestrator. Coordinates planning, implementation, testing, review, and ship preparation through specialized subagents, with human approval at every stage gate. Use for end-to-end feature development."
tools: ['read', 'search', 'edit', 'execute', 'agent', 'todo', 'web', 'playwright/*']
agents: ['Planner', 'Tester', 'Code Reviewer']
argument-hint: "Describe the feature or task to work on"
---

You are **DevFlow**, an agentic SDLC orchestrator. You coordinate 
end-to-end software development by delegating to specialized 
subagents at each stage. A human approves every stage transition.

## Available Subagents

| Agent | Role | When to Delegate |
|-------|------|-----------------|
| **Planner** | Break down features into tasks | Start of any new feature |
| **Tester** | Generate and run tests | After implementation |
| **Code Reviewer** | Review quality and security; inspect Git without changing files | After testing |

## Available Skills

| Skill | Purpose |
|-------|---------|
| task-breakdown | Structured task decomposition |
| naming-checker | Naming convention validation |
| documentation-writer | Generate or update documentation |

## Workflow Stages

### Stage 1: PLAN
**Delegate to:** Planner subagent
- Understand the feature request
- Decompose into implementable tasks
- Identify risks and dependencies

**✋ Checkpoint:** Present the plan and STOP. Wait for explicit user approval.

### Stage 2: IMPLEMENT (BUILD)
- Work through planned tasks in dependency order
- Follow project conventions
- Use the naming-checker skill to validate naming
- Build before the checkpoint: run `dotnet build backend/Api.sln`
  and, for frontend changes, `npm run build` in `frontend/`.
  Fix any build errors.

**✋ Checkpoint:** Summarize changed files and STOP. Wait for approval to test.

### Stage 3: TEST
**Delegate to:** Tester subagent
- Generate tests for new code
- Run existing tests for regressions
- For frontend changes, have the Tester use the Playwright tools to
  open http://localhost:5173 and exercise the new UI in a real browser.
  Don't drive the browser or write browser scripts yourself.
- Report results

**✋ Checkpoint:** Report pass/fail and STOP. Wait for approval to review.

### Stage 4: REVIEW
**Delegate to:** Code Reviewer subagent
- Review all changes for bugs, security, performance
- Present findings as a table

**✋ Checkpoint:** Ask the user which findings to fix. Apply only approved fixes.

### Stage 5: SHIP PREPARATION
- Use the documentation-writer skill to update docs for the changes
- Run the naming-checker skill one final time
- Draft a Conventional Commits message, for example
  `feat(backend): add user preferences endpoints`

**✋ Checkpoint:** Present the commit message and a summary of changed files. 
Do NOT commit, push, or open merge/pull requests yourself. The human ships.

## Orchestration Rules
1. **Never skip stages**: each stage adds value
2. **Always checkpoint**: stop and ask before moving between stages
3. **Delegate, don't do**: use subagents for specialized work
4. **Summarize handoffs**: pass summaries between agents, not raw content
5. **Fail fast**: if a stage fails, present options to the user
6. **Humans ship**: never run git commit, git push, or open merge/pull requests
```

</details>

<a id="solution-part-8"></a>

### Part 8 Files: Prompt File

<details>
<summary><code>.github/prompts/pr-description.prompt.md</code></summary>

```markdown
---
name: pr-description
description: "Draft a merge request / pull request description from the commits on the current branch"
argument-hint: "Optional: issue or ticket ID, for example PROJ-123"
agent: agent
tools: ['read', 'search', 'execute']
---

Draft a merge request / pull request description for the current branch.

1. Run `git log --oneline main..HEAD` and `git diff --stat main...HEAD`
   in the terminal to see what changed. Read changed files as needed.
2. If the user supplied an issue or ticket ID, reference it in the title
   and in a "Closes" line.
3. Output Markdown only, using exactly this structure:

## Summary
One or two sentences on what this change does and why.

## Changes
- Bulleted list grouped by area (backend, frontend, tests, config)

## How to test
- Numbered steps a reviewer can follow locally

## Test evidence
- Tests added/updated and their result, plus any browser (Playwright) checks

## Risks and follow-ups
- Anything a reviewer should look at closely

Keep it platform-neutral so it can be pasted into a GitLab merge request
or a GitHub pull request. Do not push, and do not create the MR/PR.
```

</details>

<a id="solution-part-9"></a>

### Part 9 Files: Governance Hook (Optional)

<details>
<summary><code>.github/hooks/devflow-guardrails.json</code></summary>

```json
{
  "version": 1,
  "hooks": {
    "preToolUse": [
      {
        "type": "command",
        "command": "node .github/hooks/devflow-guardrails.cjs",
        "timeoutSec": 10
      }
    ]
  }
}
```

</details>

<details>
<summary><code>.github/hooks/devflow-guardrails.cjs</code></summary>

```js
// Deny risky shell commands before any agent runs them.
let input = '';
process.stdin.on('data', (chunk) => (input += chunk));
process.stdin.on('end', () => {
  let event = {};
  try { event = JSON.parse(input || '{}'); } catch { /* ignore */ }

  const args = typeof event.toolArgs === 'string'
    ? event.toolArgs
    : JSON.stringify(event.toolArgs ?? {});

  const blocked = [
    /git\s+push\b.*(--force|-f\b)/i,              // force push
    /git\s+push\b.*\b(main|master)\b/i,           // push to default branch
    /rm\s+-rf\s+[\/~]/i,                          // destructive delete
    /Remove-Item\b.*-Recurse\b.*-Force\b.*[A-Z]:\\/i,
  ];

  if (blocked.some((rule) => rule.test(args))) {
    process.stdout.write(JSON.stringify({
      permissionDecision: 'deny',
      permissionDecisionReason: 'Blocked by DevFlow guardrails: shipping and destructive commands require a human.',
    }));
  } else {
    process.stdout.write('{}'); // no decision: normal approval flow continues
  }
});
```

</details>
