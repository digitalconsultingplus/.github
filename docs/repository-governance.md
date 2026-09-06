# Repository governance model

This document defines the lightweight organization conventions implemented by `digitalconsultingplus/.github`.

It is **not** the normative DCP Flow standard. When a rule here conflicts with DCP Flow, DCP Flow wins.

## Repository lifecycle states

These states describe the operating posture of a repository. They are intentionally independent of software release/versioning.

| Status | Meaning | Expected posture |
|---|---|---|
| `ACTIVE` | Actively developed or operated | Current ownership, project truth and proportionate Quality Gates expected. |
| `MAINTENANCE` | Stable; receives fixes, upgrades or limited changes | Keep security/dependency maintenance and ownership current; avoid feature churn unless approved. |
| `PAUSED` | Work intentionally suspended but expected to resume | Record reason/context; avoid silent operational assumptions; keep critical security exposure understood. |
| `PLANNING` | Approved or candidate repository before normal implementation | Architecture/scope may evolve; do not represent planned capability as implemented. |
| `EXPERIMENTAL` | Lab, PoC or bounded evaluation | Lower ceremony may be appropriate, but secrets, provenance and safety rules still apply. No implied production readiness. |
| `ARCHIVED` | No active development expected | Repository should be read-only/archived when practical; document successor or reason when useful. |

Lifecycle changes are governance changes and should be evidence-based, not inferred from commit frequency alone.

## Repository classification

The organization uses the following practical repository types for cataloging and automation discovery:

| Type | Intended use |
|---|---|
| `product` | DCP-owned product or SaaS/application capability. |
| `website` | Marketing, corporate, documentation-front-end or static-first website. |
| `internal-tool` | Internal operational/engineering/business tool. |
| `client-project` | Repository primarily tied to a client engagement or client-owned outcome. |
| `methodology` | Methods, standards, blueprints or operating system such as DCP Flow. |
| `documentation` | Documentation-first repository without a primary runtime product. |
| `library` | Reusable package, module, SDK or shared code asset. |
| `foundation` | Organization/platform foundation, templates or governance infrastructure. |
| `lab` | Experiments, evaluations and PoCs. |
| `profile` | Organization/profile/meta presentation content. |

Classification is descriptive, not an automatic security level. Risk, data and deployment context still determine gates.

## Lightweight repository metadata contract

A machine-readable catalog will be useful for Kai and future repository inventory, but this repository does not impose new tooling before the contract is proven.

Until DCP Flow defines or selects a canonical machine-readable repository catalog schema, the recommended **logical fields** are:

```yaml
name: <repository-name>
type: product | website | internal-tool | client-project | methodology | documentation | library | foundation | lab | profile
status: ACTIVE | MAINTENANCE | PAUSED | PLANNING | EXPERIMENTAL | ARCHIVED
criticality: low | medium | high | critical
owner: <real accountable person/team/role reference>
methodology: dcp-flow | <approved alternative/exception>
default_branch: dev
release_branch: main
stack:
  - <technology>
deployment: <none | provider/environment reference without secrets>
data_classification: public | internal | confidential | restricted
```

### Contract rules

- These are catalog fields, not a replacement for project truth, ADRs or DCP Flow configuration.
- Do not create a fake owner to satisfy the schema.
- Do not store credentials, customer secrets or sensitive deployment details.
- `default_branch` and `release_branch` must reflect the repository's **observed** state, not only desired policy.
- `data_classification` describes the repository/system handling posture and does not grant access.
- A future DCP Flow canonical schema should supersede this candidate without maintaining two parallel contracts.

## Criticality guidance

A simple four-level scale is enough for governance routing:

- `low` — limited blast radius, easy recovery, non-sensitive;
- `medium` — material business/repository impact but bounded recovery;
- `high` — important production/client/security/data impact;
- `critical` — privileged infrastructure, irreversible/high-value data, identity, money or wide blast radius.

Criticality informs review/gates but does not replace the DCP Flow Quality Gates risk classification for a specific change.

## Data classification guidance

- `public` — intentionally publishable;
- `internal` — non-public organization information with limited sensitivity;
- `confidential` — business/client information requiring controlled access;
- `restricted` — secrets, regulated/high-impact data or material requiring strongest controls.

Actual secrets must not be committed even when a repository is classified `restricted`.

## CODEOWNERS baseline

`CODEOWNERS` is repository-specific and is **not inherited** from this organization `.github` repository.

Create a repository `CODEOWNERS` only when real ownership can be expressed. Typical patterns may map:

```text
*                         <real-default-owner>
/.github/                 <real-governance-owner>
/infrastructure/          <real-platform-owner>
/security-sensitive-path <real-security-owner>
```

Do not copy these placeholders into a real file. Owners must be actual GitHub users or teams with appropriate repository access.

Use CODEOWNERS proportionally to risk and team structure. A tiny lab repository may not need path-level ownership; a production/client/high-criticality repository often does.

## Governance inheritance model

### Defaults GitHub can discover from the public organization `.github` repository

When a destination repository has no local equivalent, supported community-health defaults can include:

- `CONTRIBUTING.md`;
- `SECURITY.md`;
- pull request templates;
- issue templates and chooser configuration.

The organization profile is provided by `profile/README.md`.

### Controls that must be local or explicitly configured elsewhere

The following are **not automatically inherited** merely because they exist in this repository:

- CODEOWNERS;
- Dependabot configuration;
- repository workflows;
- reusable-workflow invocation;
- branch protection / repository rulesets;
- repository secrets/variables;
- environments;
- deployment settings;
- licenses;
- project-specific tests and Quality Gates.

Workflow templates can be offered from `workflow-templates/`, but adoption creates/copies workflow configuration into the target repository; it is not silent global enforcement.

## Progressive governance

Apply controls according to actual risk and operating context. The baseline goal is consistency without bureaucracy:

```text
minimum coherent defaults
+ repository truth
+ risk-proportionate controls
+ evidence
+ human authority
```

Do not add automation just because it can be centralized. Add it when it is common, deterministic, secure and measurably reduces drift.
