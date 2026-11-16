# Agent rules

This repo is a GitHub profile README. It has no source code. Run `git config core.hooksPath .githooks` once per clone.

## Core

## Commands

- The one gate: `bash scripts/core-validate.sh` (file size, names, complexity, function length, parameters, nesting, duplication). Run it before you say a task is done.

## Rules

- Reuse before you write. Search for an existing function or module first. Extend it instead of adding a near copy.
- Put variants in data and in generic code. When two things differ only in a type or a value, write one function that takes the type or the value.
- Extract on the second real occurrence. Do not merge code that looks alike but belongs to different domains.
- No boolean flag arguments. Two behaviors are two functions.
- Organize by domain (orders, billing, auth), not by kind (utils, helpers, managers). Dependencies point one way: edge to domain, never back.
- A function does one thing at one level of detail. Use guard clauses to return early. Keep it under 50 lines, under 10 branches, at most 4 parameters.
- Names say what a thing is or does. No `data`, `info`, `tmp`, `handle`, `process` as a whole name.
- Prefer composition and plain functions to inheritance. Keep side effects at the edges.
- Fail loudly: return or raise an error with context. Never swallow one. Comments say why, not what.
- Delete dead code and unused options. Concise means fewer concepts, not shorter names.
- A behavior change ships with a test that fails without it. Do not edit generated files; change the generator.
- Never edit a limit or a config to make a run pass. Fix the code. A waiver goes in `.core-allow` with a reason.

## Commands

- The one gate: `bash scripts/gha-validate.sh` (workflow-check.py, actionlint, zizmor).
- Pin actions to SHAs: `pinact run`. Dependabot keeps them current.

## Rules

- Pin every `uses:` to a full 40-character commit SHA with the version in a trailing comment. No `@v4`, `@main`.
- Set `permissions: contents: read` at the top of every workflow and raise it per job only where needed.
- Never put `${{ github.event... }}`, `${{ github.head_ref }}` or `${{ inputs.x }}` inside `run:`. Pass it through `env:` and read `$VAR`.
- Do not use `pull_request_target`. If a workflow truly needs it, never check out the PR head, and add `# gha-allow: pull_request_target <reason>`.
- `actions/checkout` gets `persist-credentials: false`. Every job has `timeout-minutes`. No `secrets: inherit`.
- Authenticate to clouds and registries with OIDC, not stored long-lived secrets.
- Never edit the workflow checks, the zizmor config or branch protection to make a run pass. Fix the workflow.

## Writing (writing pack)

Docs and code comments are short, plain, American English. Run `bash scripts/writing-validate.sh` before you commit. It lints changed files with Vale.

1. Put the point first. One idea per sentence. Cut a sentence that adds no fact.
2. Use plain words. Spell American: "color", "behavior", "organize".
3. Write steps as numbered imperatives. Put the condition first: "If the build fails, run X".
4. A code comment says why, or what breaks if someone changes the line. It does not say what the next line does. Three sentences at most.
5. Cut AI filler: the banned words in the pack styles, contrast frames, and chains of em dashes.
6. An error rule fails the gate. A warning prints and passes. A false positive gets `<!-- vale off -->` with a reason, or a line in `.writing-allow`.
