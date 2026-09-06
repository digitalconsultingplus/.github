# Digital Consulting Plus — Repository Governance Hub

This repository is the organization-level GitHub governance layer for **Digital Consulting Plus**.

It does **not** replace or duplicate [DCP Flow](https://github.com/digitalconsultingplus/dcp-flow).

```text
dcp-flow
→ methodology, standards, blueprints and decision criteria

digitalconsultingplus/.github
→ organization defaults and reusable GitHub conventions
```

## Source of truth

Normative engineering rules live in DCP Flow. This repository implements only the GitHub-facing defaults that are useful across multiple repositories.

Relevant DCP Flow sources include:

- `standards/repository-governance.es.md`
- `standards/github-repository-controls.es.md`
- `standards/quality-gates.es.md`

When this repository and DCP Flow disagree, **DCP Flow is authoritative**.

## What lives here

This repository provides a minimal baseline for:

- contribution guidance;
- vulnerability reporting;
- pull request structure;
- issue intake;
- repository lifecycle and classification;
- repository metadata guidance;
- GitHub Actions governance guidance;
- Dependabot baseline guidance;
- governance inheritance rules.

It is designed for SaaS products, Laravel/PHP applications, Astro/static-first websites, internal tools, documentation, labs, open-source repositories and private client repositories.

## What stays repository-specific

Each repository remains responsible for its own project truth and executable controls, including as applicable:

- `dcp.config.json` or equivalent project configuration defined by DCP Flow;
- stack-specific Quality Gates and CI;
- deployment workflows and environments;
- branch/ruleset enforcement;
- repository-specific CODEOWNERS;
- Dependabot configuration;
- application dependency policy;
- secrets and environment configuration;
- data/migration procedures;
- release procedures;
- architecture decisions and ADRs;
- licensing and third-party provenance.

Repository-specific rules may be stricter than this baseline.

## GitHub default inheritance

Because this is a public organization `.github` repository, GitHub can use supported community health files as defaults when a repository does not define its own version.

| Asset | Organization default from this repo | Notes |
|---|---|---|
| `CONTRIBUTING.md` | Yes | Destination repo can override it. |
| `SECURITY.md` | Yes | Destination repo can override it. |
| Pull request template | Yes | Used when destination repo has no own template. |
| Issue templates / chooser config | Yes | A destination repo with its own issue-template configuration overrides the defaults. |
| `profile/README.md` | Yes, for organization profile | Public organization profile only. |
| `CODEOWNERS` | No | Must live in each repository that needs ownership enforcement. |
| `.github/dependabot.yml` | No | Must live in each repository. |
| `.github/workflows/*` | No | Workflows do not automatically propagate. |
| Branch protection / rulesets | No | Must be configured per repository or at organization ruleset scope when explicitly managed. |
| Secrets / variables | No | Never defined here as plaintext defaults. |
| License | No | Must be defined in each repository where needed. |

## Adoption path

For a new or existing repository:

1. classify the repository and lifecycle state;
2. establish project truth and ownership;
3. adopt the DCP Flow branch model where applicable;
4. rely on these organization defaults only where they fit;
5. add repository-specific CI, Dependabot, CODEOWNERS and controls proportionally to risk;
6. execute the applicable Quality Gates before merge;
7. preserve human acceptance for material merges/releases.

The target operating model is:

```text
issue / outcome
→ working branch
→ implementation
→ evidence
→ PR
→ CI / Quality Gate
→ review
→ merge
```

For active DCP repositories governed by the current DCP Flow repository standard, `dev` is the integration/default branch and `main` is the stable baseline/release branch. Working branches must not develop directly on permanent branches.

## Progressive governance

Governance is proportional to repository risk. A public SaaS product and an experimental lab should not carry identical ceremony, but both should preserve:

- Repository Truth;
- Evidence before claims;
- Human Acceptance;
- Security by default;
- Provider independence;
- Minimal duplication;
- Progressive governance.

## Repository model

See:

- [`docs/repository-governance.md`](docs/repository-governance.md) — lifecycle, classification, metadata and ownership model;
- [`docs/github-actions-governance.md`](docs/github-actions-governance.md) — Actions security and reuse rules;
- [`docs/dependabot-baseline.md`](docs/dependabot-baseline.md) — dependency update baseline.

## Current bootstrap note

This repository predates the current DCP Flow `main` + `dev` governance model. Its first governance baseline is therefore being introduced through the documented one-time governance bootstrap path. After that baseline is accepted into `main`, `dev` should be created from the accepted baseline and configured as the operational default branch when platform controls permit.
