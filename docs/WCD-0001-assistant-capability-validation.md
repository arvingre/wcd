# WCD-0001: Assistant Capability Validation

## Status

In progress

## Purpose

Validate the real collaboration capabilities available to ChatGPT, GitHub Connector, local Claude Code, and the human owner before relying on them in the WCD engineering workflow.

The main problem this task solves is capability uncertainty. WCD should not assume that an assistant can read, write, review, or modify project assets unless those actions are directly verified.

## Repository Under Test

- Repository: `arvingre/wcd`
- Default branch: `main`
- Visibility: public

## Verified Capabilities

| Capability | Status | Evidence |
|---|---|---|
| Read repository metadata | Verified | `get_repo` succeeded for `arvingre/wcd` |
| Detect repository permissions | Verified | Connector returned admin/maintain/push/pull/triage as true |
| Create repository file | Verified | Created `README.md` through GitHub contents API |
| Commit through GitHub Connector | Verified | Commit SHA: `c0cc24e319fff4ee98c5bb0a818a65651fca2810` |
| Create project documentation file | Verified | This file was created through GitHub Connector |

## Partially Verified / Pending Capabilities

| Capability | Status | Next Verification |
|---|---|---|
| Create branch | Blocked | `create_branch` calls were blocked by safety checks in this session |
| Open pull request | Pending | Requires a branch or an alternative workflow |
| Update file | Pending | Fetch file SHA, then update the file |
| Delete file | Pending | Only test on a temporary file if needed |
| Read source files | Pending | Requires non-empty source tree |
| Search repository files | Pending | Requires files to exist |
| Read issues | Pending | Requires existing issues or create a test issue |
| Create issue | Pending | Can be tested next with WCD task issues |
| PR review comments | Pending | Requires a PR |

## Current Findings

The GitHub Connector is usable for direct repository write operations on `arvingre/wcd`. It can create files on the default branch. Branch creation is currently blocked by the runtime safety layer, so the first collaboration workflow should avoid depending on branch creation until that capability is separately verified.

## Working Model for Now

Until branch and pull request workflows are verified, WCD should use a direct-main documentation/bootstrap workflow for low-risk project foundation files.

For code changes, the preferred workflow remains:

1. Human or local Claude Code creates a branch locally.
2. Claude Code performs coding and tests locally.
3. Changes are pushed to GitHub.
4. ChatGPT reads the PR, reviews diffs, writes review comments, and helps produce fixes.

## Next Actions

1. Create the WCD project operating structure.
2. Create initial issues for capability validation and project foundation.
3. Verify update-file workflow by editing a controlled document.
4. Verify issue creation and issue reading.
5. Use local Claude Code for coding-heavy work once repository workflow is stable.
