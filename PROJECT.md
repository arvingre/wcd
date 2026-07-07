# WCD Project Charter

## Project Name

WCD

## Project Type

AI workflow orchestration and engineering collaboration platform.

## Immediate Objective

Establish a practical collaboration system where:

- GitHub is the source of truth.
- Local Claude Code handles coding-heavy implementation.
- ChatGPT assists with repository reading, planning, architecture review, issue breakdown, PR review, and documentation.
- Every capability is verified before becoming part of the workflow.

## Why This Exists

The project started from the need to move beyond one-question-one-answer AI interaction and build a system where AI agents can participate in long-running engineering work.

The current priority is not large-scale product design. The current priority is to prove that the basic collaboration infrastructure works.

## Phase 0 Scope

Phase 0 is focused on project foundation and capability validation.

Included:

- GitHub repository access validation
- File create/update workflow validation
- Issue workflow validation
- PR review workflow validation
- Claude Code collaboration workflow design
- Minimal project operating documents

Excluded for now:

- Full product implementation
- Multi-agent runtime implementation
- Production deployment
- Complex automation before repository workflow is stable

## Operating Rule

No assumed capability. Only verified capability.

## Initial Work Items

- WCD-0001: Assistant Capability Validation
- WCD-0002: GitHub Operating Workflow
- WCD-0003: Claude Code Local Coding Workflow
- WCD-0004: OpenClaw Integration Research
- WCD-0005: DevOps Organizational Learning MVP
