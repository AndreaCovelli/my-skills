---
name: github-actions
description: >
  Create, edit, review, harden, and optimize GitHub Actions workflow files in
  `.github/workflows`, plus related reusable workflows and composite-action
  decisions. Use when the task requires changing repository automation or
  reviewing workflow YAML for correctness, security, maintainability, or CI/CD
  design. Do not use for docs-grounded GitHub Actions Q&A when no workflow
  change or repository-specific review is needed.
---

# GitHub Actions

Use this skill for repository-specific GitHub Actions implementation and review.
Apply it when changing workflow files, designing automation for a repo, or
reviewing workflow changes for correctness and security.

Do not use this skill for general GitHub Actions documentation questions that do
not require editing or reviewing repository automation. Use the separately
installed docs skill for those.

## Required Inputs

- The repository workflow files under `.github/workflows/`, if any.
- The user request: new workflow, workflow fix, review, migration, hardening, or optimization.
- Relevant project context such as language stack, test commands, deployment targets, branching model, and release process.

## Workflow

1. Inspect existing workflows before writing a new one.
   Check triggers, job boundaries, runner choices, permissions, caching, and whether a reusable workflow already exists.
2. Choose the simplest workflow shape that satisfies the task.
   Prefer a small number of clearly named jobs with explicit dependencies over large monolithic jobs.
   If the task is only asking what GitHub Actions documentation says, stop and use the docs skill instead of this one.
3. Scope triggers carefully.
   Use the minimum events, branch filters, tag filters, and path filters needed for the intended behavior.
4. Minimize privilege.
   Set explicit `permissions`, prefer `GITHUB_TOKEN` over long-lived credentials when possible, and use OIDC for cloud or package publishing when supported.
5. Treat workflow inputs and contexts as potentially untrusted.
   Avoid unsafe interpolation into shell scripts or commands.
6. Reuse instead of duplicating when the pattern is stable.
   Prefer reusable workflows for multi-job reusable pipelines and composite actions for repeated step sequences.
7. Optimize only after correctness is clear.
   Add caching, matrices, concurrency control, and artifact flow only where they improve the workflow materially.
8. Validate behavior against the repo's real use case.
   Check event coverage, branch behavior, secret requirements, job ordering, and cancellation semantics.

## Final Checks

- The workflow trigger matches the intended repository events and does not fire too broadly.
- Permissions and secrets exposure are minimized for the jobs that need them.
- Third-party actions and reusable workflows are pinned appropriately for the repo's security bar.
- Concurrency, caching, and matrix strategy improve the workflow instead of making it harder to reason about.
- If the workflow depends on assumptions about branches, environments, or release flow, state them clearly.
- If the task turned out to be a docs lookup rather than a repository change, hand it off to the docs skill instead of answering from this playbook.

## Reference Guide

Read [references/workflow-standards.md](references/workflow-standards.md) when you need detailed guidance for:

- trigger and job design
- matrices, caching, and artifacts
- permissions, secrets, and OIDC
- reusable workflows and composite actions
- concurrency, cancellation, and deployment patterns

Read [references/sources.md](references/sources.md) when you need the upstream documentation links for those conventions.
