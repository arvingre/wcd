# WCD-0003: OpenClaw Collaboration Workflow

## Purpose

Define how ChatGPT, OpenClaw, local Claude Code, and GitHub collaborate in WCD.

This document turns the discussion into an executable working model.

## Current Reality

ChatGPT has verified GitHub repository access for `arvingre/wcd`:

- Read repository metadata: verified
- Create files: verified
- Commit files: verified
- Create issues: verified
- Create branch: currently blocked by runtime safety checks

ChatGPT has not yet directly connected to the user's local OpenClaw workspace. Therefore, OpenClaw collaboration must initially happen through GitHub and explicit handoff files.

## Collaboration Roles

### Human Owner

Responsibilities:

- Final decision maker
- Local environment owner
- Approves risky actions
- Runs or authorizes local OpenClaw and Claude Code
- Controls merge, release, and production access

### ChatGPT

Responsibilities:

- Read GitHub repository state
- Create and update planning documents
- Create and refine GitHub issues
- Produce architecture reviews
- Produce prompts and task packs for OpenClaw / Claude Code
- Review PRs and diffs when available
- Maintain operating standards and decision records

Current verified GitHub capabilities:

- Repository read
- File creation
- Commit creation through contents API
- Issue creation

Current unverified or blocked capabilities:

- Branch creation
- PR creation from generated branch
- Direct OpenClaw workspace read/write

### OpenClaw

Expected responsibilities:

- Act as the local AI workflow gateway
- Run role-based agents
- Read project task packs from the repository
- Execute controlled workflows locally
- Coordinate with local tools and environment
- Produce outputs back into GitHub as files, branches, PRs, or issue comments

### Claude Code

Responsibilities:

- Local coding-heavy implementation
- Refactoring
- Running tests
- Producing patches
- Creating branches and PRs from local machine when connector branch creation is blocked

## Source of Truth

GitHub is the source of truth.

OpenClaw and Claude Code should not keep important project decisions only in local memory. Any meaningful result must be pushed back to GitHub as:

- Issue update
- Markdown document
- ADR
- RFC
- PR
- Review comment
- Test report

## Basic Workflow

```text
GitHub Issue
  -> ChatGPT task clarification
  -> Task pack document
  -> Human runs OpenClaw / Claude Code locally
  -> Local agent creates code or analysis
  -> Output returns to GitHub
  -> ChatGPT reviews result
  -> Human approves merge or next step
```

## Task Pack Format

Every OpenClaw task should be written as a task pack in `tasks/`.

Recommended path:

```text
tasks/WCD-xxxx-task-name.md
```

Each task pack should contain:

```markdown
# Task

## Objective

## Repository Context

## Files To Read

## Work To Do

## Constraints

## Expected Output

## Test Command

## Review Checklist
```

## OpenClaw Execution Model

Initial execution should be manual and controlled:

1. ChatGPT creates a task pack in GitHub.
2. Human pulls the latest repository locally.
3. Human runs OpenClaw with the task pack as context.
4. OpenClaw delegates coding tasks to Claude Code or local tools.
5. Claude Code modifies code locally.
6. Human runs tests.
7. Human pushes branch or PR.
8. ChatGPT reviews the PR and updates issues/docs.

## Why Not Fully Automatic Yet

Full automation is not the first step because these capabilities are not fully verified:

- Direct OpenClaw workspace access from ChatGPT
- Branch creation through this connector
- PR creation from generated branch
- Safe execution boundary for local commands

Until these are verified, WCD uses a human-approved workflow.

## Minimal OpenClaw Integration v0

The first practical integration is not an API integration. It is a repository-based workflow:

- ChatGPT writes task packs.
- OpenClaw reads task packs locally.
- Claude Code executes implementation.
- GitHub stores all outputs.
- ChatGPT reviews outputs.

## Next Verification Steps

- Verify ChatGPT can update existing files.
- Verify issue reading and commenting.
- Create first OpenClaw task pack.
- Run one task locally with OpenClaw / Claude Code.
- Push the resulting branch or PR.
- Verify ChatGPT PR review workflow.
