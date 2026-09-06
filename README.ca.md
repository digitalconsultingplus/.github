# Digital Consulting Plus — Hub de Governança de Repositoris

Aquest repositori és la capa organitzativa de governança GitHub de **Digital Consulting Plus**.

No substitueix ni duplica [DCP Flow](https://github.com/digitalconsultingplus/dcp-flow).

**Idiomes:** [English](README.md) · [Español](README.es.md) · **Català**

```text
dcp-flow
→ metodologia, estàndards, blueprints i criteris de decisió

digitalconsultingplus/.github
→ defaults organitzatius i convencions reutilitzables de GitHub
```

## Font de veritat

Les regles normatives d'enginyeria viuen a DCP Flow. Aquest repositori implementa únicament defaults orientats a GitHub que són útils de manera transversal entre repositoris.

Fonts rellevants de DCP Flow:

- `standards/repository-governance.es.md`
- `standards/github-repository-controls.es.md`
- `standards/quality-gates.es.md`

Quan aquest repositori i DCP Flow entrin en conflicte, **DCP Flow és l'autoritat**.

## Què viu aquí

Aquest repositori proporciona una baseline mínima per a:

- guia de contribució;
- report de vulnerabilitats;
- estructura de Pull Requests;
- intake d'issues;
- lifecycle i classificació de repositoris;
- guia de metadata de repositoris;
- governança de GitHub Actions;
- baseline de Dependabot;
- regles d'herència de governança.

Està dissenyat per a productes SaaS, aplicacions Laravel/PHP, llocs Astro/static-first, eines internes, documentació, labs, repositoris open source i repositoris privats de clients.

## Què continua sent específic de cada repositori

Cada repositori conserva responsabilitat sobre el seu Project Truth i els controls executables, incloent-hi quan sigui aplicable:

- `dcp.config.json` o configuració equivalent definida per DCP Flow;
- Quality Gates i CI específics de l'stack;
- workflows i environments de desplegament;
- enforcement de branches/rulesets;
- CODEOWNERS específics;
- configuració de Dependabot;
- polítiques de dependències d'aplicació;
- secrets i configuració d'entorns;
- procediments de dades/migracions;
- procediments de release;
- decisions d'arquitectura i ADRs;
- llicenciament i provenance de tercers.

Les regles locals poden ser més estrictes que aquesta baseline.

## Herència de defaults de GitHub

Com que aquest és un repositori públic `.github` d'organització, GitHub pot utilitzar determinats community health files com a defaults quan el repositori destí no defineix la seva pròpia versió.

| Asset | Default organitzatiu des d'aquest repo | Notes |
|---|---|---|
| `CONTRIBUTING.md` | Sí | El repo destí el pot sobreescriure. |
| `SECURITY.md` | Sí | El repo destí el pot sobreescriure. |
| Pull request template | Sí | S'utilitza quan el repo destí no en té cap de propi. |
| Issue templates / chooser config | Sí | La configuració local del repo destí preval. |
| `profile/README.md` | Sí, per al perfil organitzatiu | Només perfil públic de l'organització. |
| `CODEOWNERS` | No | Ha de viure a cada repositori que necessiti enforcement d'ownership. |
| `.github/dependabot.yml` | No | Ha de viure a cada repositori. |
| `.github/workflows/*` | No | Els workflows no es propaguen automàticament. |
| Branch protection / rulesets | No | S'han de configurar per repo o explícitament a nivell d'organització. |
| Secrets / variables | No | Mai no es defineixen aquí com defaults en text pla. |
| License | No | S'ha de definir per repositori quan sigui aplicable. |

## Ruta d'adopció

Per a un repositori nou o existent:

1. classificar el repositori i el seu estat de lifecycle;
2. establir Project Truth i ownership;
3. adoptar el model de branques de DCP Flow quan sigui aplicable;
4. utilitzar aquests defaults organitzatius només on encaixin;
5. afegir CI, Dependabot, CODEOWNERS i controls específics proporcionalment al risc;
6. executar els Quality Gates aplicables abans del merge;
7. preservar l'acceptació humana per a merges/releases materials.

Model operatiu objectiu:

```text
issue / outcome
→ working branch
→ implementació
→ evidència
→ PR
→ CI / Quality Gate
→ review
→ merge
```

Per als repositoris DCP actius governats per l'estàndard actual, `dev` és la branca d'integració/default i `main` la baseline estable/release. No es desenvolupa directament sobre branques permanents.

## Governança progressiva

La governança és proporcional al risc. Un SaaS públic i un lab experimental no haurien de tenir la mateixa cerimònia, però tots dos han de preservar:

- Repository Truth;
- Evidence before claims;
- Human Acceptance;
- Security by default;
- Provider independence;
- Minimal duplication;
- Progressive governance.

## Model de repositori

Consulta:

- [`docs/repository-governance.ca.md`](docs/repository-governance.ca.md) — lifecycle, classificació, metadata i ownership;
- [`docs/github-actions-governance.ca.md`](docs/github-actions-governance.ca.md) — seguretat i reutilització d'Actions;
- [`docs/dependabot-baseline.ca.md`](docs/dependabot-baseline.ca.md) — baseline d'actualització de dependències.

## Política d'idiomes

La documentació de governança es manté en **English, Español i Català**. Els fitxers canònics que GitHub descobreix automàticament romanen en anglès per mantenir una única superfície operativa; les seves traduccions es publiquen com a variants `.es.md` i `.ca.md`. Els templates operatius de PR/issues no es dupliquen per idioma per evitar multiplicar opcions a la interfície de GitHub.
