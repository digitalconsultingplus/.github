# Model de governança de repositoris

Aquest document defineix les convencions organitzatives lleugeres implementades per `digitalconsultingplus/.github`.

No és l'estàndard normatiu de DCP Flow. Si una regla d'aquí entra en conflicte amb DCP Flow, preval DCP Flow.

**Idiomes:** [English](repository-governance.md) · [Español](repository-governance.es.md) · **Català**

## Estats de lifecycle del repositori

Aquests estats descriuen la postura operativa d'un repositori. Són intencionadament independents del versionat o releases de software.

| Estat | Significat | Postura esperada |
|---|---|---|
| `ACTIVE` | Desenvolupament o operació actius | Ownership actual, Project Truth i Quality Gates proporcionals al risc. |
| `MAINTENANCE` | Estable; rep fixes, upgrades o canvis limitats | Mantenir seguretat/dependències i ownership; evitar feature churn tret que s'aprovi. |
| `PAUSED` | Treball suspès intencionadament amb expectativa de reprendre's | Registrar raó/context; evitar supòsits operatius silenciosos; mantenir entès qualsevol risc crític. |
| `PLANNING` | Repo aprovat o candidat abans de la implementació normal | Arquitectura/scope poden evolucionar; no presentar capacitats planificades com implementades. |
| `EXPERIMENTAL` | Lab, PoC o avaluació acotada | Pot aplicar menys cerimònia, però continuen vigents les regles de secrets, provenance i seguretat. No implica readiness de producció. |
| `ARCHIVED` | No s'espera desenvolupament actiu | Hauria de quedar read-only/arxivat quan sigui pràctic; documentar successor o motiu quan sigui útil. |

Els canvis de lifecycle són canvis de governança i s'han de basar en evidència, no inferir-se només per la freqüència de commits.

## Classificació de repositoris

L'organització utilitza els tipus pràctics següents per a catàleg i discovery d'automatització:

| Tipus | Ús previst |
|---|---|
| `product` | Producte, SaaS o capacitat d'aplicació propietat de DCP. |
| `website` | Lloc de marketing, corporatiu, documentation front-end o static-first. |
| `internal-tool` | Eina interna operativa, d'enginyeria o negoci. |
| `client-project` | Repo lligat principalment a un engagement o outcome de client. |
| `methodology` | Mètodes, estàndards, blueprints o operating system com DCP Flow. |
| `documentation` | Repo documentation-first sense producte runtime primari. |
| `library` | Paquet, mòdul, SDK o asset de codi reutilitzable. |
| `foundation` | Foundation d'organització/plataforma, templates o infraestructura de governance. |
| `lab` | Experiments, avaluacions i PoCs. |
| `profile` | Contingut de perfil/meta presentació organitzativa. |

La classificació és descriptiva, no un nivell automàtic de seguretat. El risc, les dades i el context de deployment continuen determinant els gates.

## Contracte lleuger de metadata del repositori

Un catàleg machine-readable serà útil per a Kai i per a l'inventari futur de repositoris, però aquest repo no imposa tooling nou abans de validar el contracte.

Fins que DCP Flow defineixi o seleccioni un schema canònic machine-readable per a catàleg, es recomanen els **camps lògics** següents:

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

### Regles del contracte

- Són camps de catàleg, no substitueixen Project Truth, ADRs ni configuració de DCP Flow.
- No crear un owner fictici per satisfer l'schema.
- No emmagatzemar credencials, secrets de clients ni detalls sensibles de deployment.
- `default_branch` i `release_branch` han de reflectir l'estat **observat** del repo, no només la política desitjada.
- `data_classification` descriu la postura de gestió del sistema/repositori i no concedeix accés.
- Un schema canònic futur de DCP Flow ha de substituir aquest candidat sense mantenir dos contractes paral·lels.

## Guia de criticality

Una escala simple de quatre nivells és suficient per al routing de governance:

- `low` — blast radius limitat, recuperació simple, no sensible;
- `medium` — impacte material de negoci/repositori però recuperació acotada;
- `high` — impacte important en producció, client, seguretat o dades;
- `critical` — infraestructura privilegiada, dades irreversibles/d'alt valor, identitat, diners o blast radius ampli.

La criticality informa reviews/gates, però no substitueix la classificació de risc de Quality Gates per a un canvi concret.

## Guia de data classification

- `public` — publicable intencionadament;
- `internal` — informació no pública de l'organització amb sensibilitat limitada;
- `confidential` — informació de negoci/client que requereix accés controlat;
- `restricted` — secrets, dades regulades/d'alt impacte o material que requereix controls màxims.

Els secrets reals no s'han de comitejar encara que el repo estigui classificat com `restricted`.

## Baseline de CODEOWNERS

`CODEOWNERS` és específic de cada repo i **no s'hereta** des d'aquest `.github` organitzatiu.

Crea un `CODEOWNERS` local només quan es pugui expressar ownership real. Patrons típics poden mapar:

```text
*                         <real-default-owner>
/.github/                 <real-governance-owner>
/infrastructure/          <real-platform-owner>
/security-sensitive-path <real-security-owner>
```

No copiïs aquests placeholders a un fitxer real. Els owners han de ser usuaris o equips GitHub reals amb accés adequat al repositori.

Utilitza CODEOWNERS proporcionalment al risc i estructura de l'equip. Un lab petit pot no necessitar ownership per paths; un repo de producció/client/alta criticality normalment sí.

## Model d'herència de governance

### Defaults que GitHub pot descobrir des del `.github` públic de l'organització

Quan el repo destí no té equivalent local, els community health defaults suportats poden incloure:

- `CONTRIBUTING.md`;
- `SECURITY.md`;
- pull request templates;
- issue templates i chooser config.

El perfil organitzatiu es proporciona mitjançant `profile/README.md`.

### Controls que han de ser locals o configurar-se explícitament en una altra capa

No s'hereten automàticament només perquè existeixin aquí:

- CODEOWNERS;
- configuració de Dependabot;
- workflows del repositori;
- invocació de reusable workflows;
- branch protection / repository rulesets;
- secrets/variables;
- environments;
- deployment settings;
- licenses;
- tests i Quality Gates específics del projecte.

Els workflow templates es poden oferir des de `workflow-templates/`, però la seva adopció crea/copia configuració al repo destí; no representen enforcement global silenciós.

## Governança progressiva

Aplica controls segons el risc i context operatiu reals. L'objectiu és consistència sense burocràcia:

```text
minimum coherent defaults
+ repository truth
+ risk-proportionate controls
+ evidence
+ human authority
```

No afegeixis automatització només perquè es pugui centralitzar. Afegeix-la quan sigui comuna, determinista, segura i redueixi drift de manera mesurable.
