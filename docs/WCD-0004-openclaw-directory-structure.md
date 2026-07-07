# WCD-0004: OpenClaw Directory Structure

## Purpose

Define the local OpenClaw workspace structure for WCD so that ChatGPT, OpenClaw, Claude Code, and the human owner can collaborate through a stable filesystem contract.

This structure is designed for the current verified reality:

- GitHub is the source of truth.
- ChatGPT can create and update GitHub files.
- OpenClaw runs locally on the user's machine.
- Claude Code performs local coding work.
- Important outputs must return to GitHub.

## Recommended Local Workspace

Recommended path:

```text
~/openclaw-workspaces/wcd/
```

Inside it:

```text
~/openclaw-workspaces/wcd/
├── README.md
├── workspace.md
├── repo/
├── inbox/
├── outbox/
├── agents/
├── memory/
├── tasks/
├── decisions/
├── runbooks/
├── rca/
├── prompts/
├── workflows/
├── tools/
├── logs/
└── tmp/
```

## Directory Responsibilities

### `repo/`

Local clone of the GitHub repository.

```text
repo/
└── wcd/
```

This is the actual working copy used by Claude Code and local agents.

Recommended setup:

```bash
mkdir -p ~/openclaw-workspaces/wcd/repo
cd ~/openclaw-workspaces/wcd/repo
git clone https://github.com/arvingre/wcd.git
```

### `inbox/`

Tasks received from GitHub / ChatGPT.

OpenClaw should read task packs from here.

```text
inbox/
├── WCD-0001.md
├── WCD-0002.md
└── WCD-0003.md
```

A task pack is copied from GitHub `tasks/` into local `inbox/` before execution.

### `outbox/`

OpenClaw and Claude Code write completed outputs here before pushing to GitHub.

```text
outbox/
├── reports/
├── patches/
├── pr-descriptions/
├── test-results/
└── review-notes/
```

Nothing important should stay only in `outbox/`. It must be moved back into the GitHub repo after review.

### `agents/`

Local OpenClaw agent role definitions.

```text
agents/
├── orchestrator/
│   └── ROLE.md
├── architect/
│   └── ROLE.md
├── devops-engineer/
│   └── ROLE.md
├── knowledge-engineer/
│   └── ROLE.md
├── reviewer/
│   └── ROLE.md
└── coder/
    └── ROLE.md
```

Recommended first agents:

| Agent | Responsibility |
|---|---|
| orchestrator | Reads task pack, assigns work, writes execution summary |
| architect | Reviews system design and constraints |
| devops-engineer | Handles DevOps/Kubernetes/monitoring tasks |
| knowledge-engineer | Converts work into memory, ADR, RCA, runbook |
| reviewer | Checks output before GitHub update |
| coder | Delegates or coordinates with Claude Code |

### `memory/`

Local operational memory used by OpenClaw.

```text
memory/
├── company-context.md
├── project-context.md
├── verified-capabilities.md
├── open-questions.md
├── glossary.md
└── index.md
```

Important rule:

Local memory is not the source of truth. Stable memory must be promoted into GitHub documents.

### `tasks/`

Local active task queue.

```text
tasks/
├── active/
├── blocked/
├── done/
└── archive/
```

A task moves through:

```text
inbox -> tasks/active -> tasks/done -> GitHub update
```

### `decisions/`

Local ADR drafts before committing to GitHub.

```text
decisions/
├── draft/
└── approved/
```

Approved decisions should be copied into GitHub:

```text
docs/adr/
```

### `runbooks/`

Local runbook drafts produced by OpenClaw.

```text
runbooks/
├── kubernetes/
├── github/
├── openclaw/
└── incident-response/
```

Stable runbooks should be promoted to GitHub:

```text
docs/runbooks/
```

### `rca/`

Incident investigation and RCA drafts.

```text
rca/
├── draft/
├── reviewed/
└── archive/
```

### `prompts/`

Reusable prompts for local agents and Claude Code.

```text
prompts/
├── claude-code-implementation.md
├── openclaw-task-runner.md
├── pr-review.md
├── rca-generation.md
└── runbook-generation.md
```

### `workflows/`

Executable or semi-executable workflow definitions.

```text
workflows/
├── github-task-loop.md
├── claude-code-loop.md
├── incident-rca-loop.md
├── docs-promotion-loop.md
└── pr-review-loop.md
```

### `tools/`

Local scripts and helper commands.

```text
tools/
├── sync-from-github.sh
├── sync-to-github.sh
├── run-task.sh
├── collect-context.sh
└── validate-output.sh
```

### `logs/`

Local execution logs.

```text
logs/
├── openclaw/
├── claude-code/
├── task-runs/
└── errors/
```

Logs can be summarized into GitHub, but raw logs should not be committed unless needed and scrubbed.

### `tmp/`

Temporary files. Safe to delete.

```text
tmp/
```

## GitHub Repository Structure

The GitHub repo should mirror only stable artifacts:

```text
wcd/
├── README.md
├── PROJECT.md
├── docs/
│   ├── WCD-0001-assistant-capability-validation.md
│   ├── WCD-0002-github-operating-workflow.md
│   ├── WCD-0003-openclaw-collaboration-workflow.md
│   ├── WCD-0004-openclaw-directory-structure.md
│   ├── adr/
│   ├── runbooks/
│   └── rca/
├── tasks/
├── prompts/
└── workflows/
```

## Local vs GitHub Boundary

| Local OpenClaw Workspace | GitHub Repository |
|---|---|
| Execution state | Source of truth |
| Temporary memory | Approved memory |
| Agent runtime logs | Stable reports |
| Draft RCA | Reviewed RCA |
| Draft ADR | Approved ADR |
| Claude Code working files | Committed code / PR |

## First Setup Command

```bash
mkdir -p ~/openclaw-workspaces/wcd/{repo,inbox,outbox,agents,memory,tasks,decisions,runbooks,rca,prompts,workflows,tools,logs,tmp}
mkdir -p ~/openclaw-workspaces/wcd/tasks/{active,blocked,done,archive}
mkdir -p ~/openclaw-workspaces/wcd/outbox/{reports,patches,pr-descriptions,test-results,review-notes}
mkdir -p ~/openclaw-workspaces/wcd/agents/{orchestrator,architect,devops-engineer,knowledge-engineer,reviewer,coder}
```

Then clone repository:

```bash
cd ~/openclaw-workspaces/wcd/repo
git clone https://github.com/arvingre/wcd.git
```

## First Execution Loop

```text
1. ChatGPT creates GitHub issue and task pack.
2. Human pulls latest repo locally.
3. Copy task pack into OpenClaw inbox.
4. OpenClaw orchestrator reads task pack.
5. Claude Code performs coding or document update.
6. Output goes to outbox.
7. Human reviews locally.
8. Push branch or direct docs update.
9. ChatGPT reviews GitHub result.
```

## Rule

OpenClaw is the local execution brain. GitHub is the organizational memory. ChatGPT is the remote reviewer and planner. Claude Code is the local coding worker.
