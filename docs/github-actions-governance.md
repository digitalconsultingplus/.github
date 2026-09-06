# GitHub Actions governance baseline

This document translates current DCP Flow repository controls into a minimal organization baseline for GitHub Actions.

The normative source remains DCP Flow, especially `standards/github-repository-controls.es.md` and the applicable Quality Gates.

## Principles

GitHub Actions workflows should be:

- least-privilege by default;
- deterministic where practical;
- explicit about secrets and trust boundaries;
- small enough to review;
- stack-aware rather than universal;
- reusable only when the shared behavior is genuinely common.

## Permissions

Set workflow/job permissions to the minimum required.

Prefer an explicit baseline such as:

```yaml
permissions:
  contents: read
```

Add write scopes only to the job that actually needs them. Do not rely on broad implicit permissions when a narrower contract is possible.

## External Actions and supply chain

Current DCP Flow requires external GitHub Actions to be referenced by a full immutable commit SHA.

```yaml
# Avoid mutable references
uses: actions/checkout@v4

# Use an approved immutable SHA
uses: actions/checkout@<full-commit-sha> # human-readable version comment
```

A dependency update to an Action is still a dependency change: review the new SHA/source and execute applicable Quality Gates.

Local Actions in the same repository may be referenced by relative path.

## Untrusted code and secrets

Never execute untrusted pull-request code with write tokens, repository/environment secrets or privileged cloud credentials.

Important boundaries:

- fork PR code is untrusted;
- changed workflow code in a PR is untrusted until reviewed;
- build scripts, package hooks and test fixtures can execute arbitrary code;
- artifacts from untrusted jobs should not automatically become privileged inputs.

Separate untrusted validation from privileged deployment/publishing jobs.

## `pull_request_target`

Treat `pull_request_target` as high-risk because it executes in the context of the base repository and can access permissions/secrets unavailable to ordinary fork PRs.

Do not combine `pull_request_target` with checkout/execution of untrusted PR code unless the security model has been explicitly designed and reviewed.

Use ordinary `pull_request` for normal validation whenever possible.

## Secrets

- Never print secrets to logs intentionally.
- Never place plaintext secrets in workflow YAML, repository files or generated artifacts.
- Use environment-level protection for privileged production operations when appropriate.
- Do not pass broad secret sets to reusable workflows unless required and understood.
- Prefer short-lived/federated credentials over long-lived cloud keys when the provider supports it and the repository's architecture justifies the setup.

## Artifacts and retention

Artifacts should exist for a purpose: test evidence, build outputs, diagnostics or release handoff.

Use retention appropriate to sensitivity and operational need. Avoid indefinite retention of noisy or sensitive artifacts. Never upload secrets, private `.env` files, customer data, credential stores or unrestricted production dumps as CI artifacts.

## Reusable workflows

A reusable workflow is appropriate when the behavior is:

- common to several repositories;
- deterministic;
- provider/stack-neutral enough to share safely;
- stable enough to justify central maintenance;
- small and auditable.

Good candidates may include narrow governance checks such as validating external Action references or checking repository metadata once those contracts are proven.

Bad candidates include a universal build/test/deploy workflow spanning Laravel, Astro, Python, documentation and client-specific runtime assumptions.

This repository does not introduce a mega-workflow baseline.

## Workflow templates

Organization workflow templates may be added under `workflow-templates/` when a repeated pattern has proven useful. Templates are **adoption aids**, not inherited enforcement: the destination repository still owns the generated workflow and its stack-specific Quality Gates.

## Auto-merge

No global auto-merge baseline is defined here.

In particular:

- major dependency upgrades must not auto-merge by default;
- security-sensitive, migration, infrastructure, release and high-risk changes require the applicable human/review gates;
- any repo-specific auto-merge policy must be explicit, narrow and compatible with DCP Flow.

## Review checklist

For a new or changed workflow, verify as applicable:

```text
[ ] permissions are minimal
[ ] external actions are pinned to approved full SHAs
[ ] untrusted code cannot access privileged secrets/tokens
[ ] pull_request_target is absent or explicitly justified/reviewed
[ ] artifacts contain no sensitive data and retention is reasonable
[ ] privileged deployment/publish jobs have an explicit trust boundary
[ ] workflow is stack/project appropriate
[ ] no claim of organization-wide inheritance is made
```
