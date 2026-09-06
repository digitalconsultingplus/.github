# Contributing to Digital Consulting Plus repositories

This is the organization-level contribution baseline for repositories that do not define a more specific `CONTRIBUTING.md`.

Normative engineering rules live in [DCP Flow](https://github.com/digitalconsultingplus/dcp-flow). Repository-specific instructions override this baseline when they are stricter or more precise.

## Default contribution flow

```text
issue / outcome
→ working branch
→ implementation
→ evidence
→ pull request
→ CI / Quality Gate
→ review
→ merge
```

### 1. Start from an outcome

Before implementation, identify the problem, requested outcome, issue, sprint or bounded unit of work. Avoid unrelated changes in the same branch.

### 2. Use a working branch

For repositories adopting the current DCP Flow branch model:

- `dev` is the integration/default branch;
- `main` is the stable baseline/release branch;
- create temporary working branches from `dev`;
- do not develop directly on `dev` or `main`.

Recommended prefixes include `feature/`, `fix/`, `hotfix/`, `refactor/`, `docs/`, `chore/` and `experiment/`.

Legacy repositories may have an explicitly documented transition path. Do not assume an exception exists.

### 3. Implement the smallest coherent change

Prefer changes that are reviewable, reversible and aligned with the repository's architecture, selected DCP Flow Blueprint and local project truth.

Do not introduce new frameworks, providers, dependencies or operational complexity without a concrete need.

### 4. Produce evidence

Run the Quality Gates applicable to the actual risk and stack. Evidence may include tests, lint/static checks, builds, dependency/security checks, migration review, runtime smoke checks, screenshots or manual validation where automation cannot prove the requirement.

Use honest states: `PASS`, `FAIL`, `NOT RUN`, `NOT APPLICABLE` or `BLOCKED`. Never report a gate as passed when it was not executed or verified.

### 5. Open a pull request

Describe:

- the outcome/problem solved;
- scope and notable exclusions;
- evidence and Quality Gates executed;
- security impact;
- data/migration impact;
- documentation impact;
- rollback/recovery when risk requires it;
- any Human Gate still required.

### 6. Review before merge

A created PR is not merge authorization. Working-branch PRs require the audit/review required by DCP Flow and the repository's own controls. Material promotion/release remains subject to human acceptance.

## Security

Never commit credentials, tokens, private keys, customer data or other secrets. Sensitive vulnerabilities must follow `SECURITY.md` and must not be disclosed in public issues.

## Repository-specific rules

A repository may define stricter rules for:

- branch topology;
- review count;
- required checks;
- CODEOWNERS;
- security gates;
- migrations/data handling;
- deployment/release;
- licensing/provenance;
- client confidentiality.

Those local rules take precedence for that repository, provided they do not weaken mandatory DCP Flow requirements without an approved exception.
