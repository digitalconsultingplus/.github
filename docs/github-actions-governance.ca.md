# Baseline de governança de GitHub Actions

Aquest document tradueix els controls actuals de repositoris DCP Flow a una baseline organitzativa mínima per a GitHub Actions.

La font normativa continua sent DCP Flow, especialment `standards/github-repository-controls.es.md` i els Quality Gates aplicables.

**Idiomes:** [English](github-actions-governance.md) · [Español](github-actions-governance.es.md) · **Català**

## Principis

Els workflows de GitHub Actions han de ser:

- de mínim privilegi per defecte;
- deterministes quan sigui pràctic;
- explícits respecte de secrets i trust boundaries;
- prou petits per ser revisables;
- conscients de l'stack en lloc d'universals;
- reutilitzables només quan el comportament compartit sigui realment comú.

## Permisos

Configura els permisos de workflow/job al mínim requerit.

Prefereix una baseline explícita com:

```yaml
permissions:
  contents: read
```

Afegeix scopes d'escriptura només al job que realment els necessiti. No depenguis de permisos implícits amplis quan es pugui definir un contracte més estret.

## Actions externes i supply chain

DCP Flow exigeix actualment que les GitHub Actions externes es referenciïn mitjançant un commit SHA complet i immutable.

```yaml
# Evitar referències mutables
uses: actions/checkout@v4

# Utilitzar un SHA immutable aprovat
uses: actions/checkout@<full-commit-sha> # comentari de versió llegible per humans
```

Actualitzar una Action continua sent un canvi de dependència: revisa el nou SHA/source i executa els Quality Gates aplicables.

Les Actions locals del mateix repositori es poden referenciar mitjançant ruta relativa.

## Codi no fiable i secrets

No executis mai codi no fiable d'un pull request amb write tokens, secrets de repositori/environment o credencials cloud privilegiades.

Boundaries importants:

- el codi de PRs des de forks no és fiable;
- el codi de workflows modificat en un PR no és fiable fins que sigui revisat;
- build scripts, package hooks i test fixtures poden executar codi arbitrari;
- artifacts de jobs no fiables no s'han de convertir automàticament en inputs privilegiats.

Separa validació no fiable de jobs privilegiats de deployment/publishing.

## `pull_request_target`

Tracta `pull_request_target` com d'alt risc perquè s'executa en el context del repo base i pot accedir a permisos/secrets no disponibles per a PRs ordinaris des de forks.

No combinis `pull_request_target` amb checkout/execució de codi no fiable del PR tret que el model de seguretat hagi estat dissenyat i revisat explícitament.

Utilitza `pull_request` ordinari per a validació normal sempre que sigui possible.

## Secrets

- No imprimeixis mai secrets intencionadament als logs.
- No posis mai secrets en text pla dins de workflow YAML, fitxers del repo o artifacts generats.
- Utilitza protecció a nivell environment per a operacions privilegiades de producció quan sigui aplicable.
- No passis conjunts amplis de secrets a reusable workflows tret que la necessitat sigui entesa i justificada.
- Prefereix credencials de curta durada/federades davant de claus cloud de llarga durada quan el proveïdor ho suporti i l'arquitectura ho justifiqui.

## Artifacts i retenció

Els artifacts han d'existir amb un propòsit: evidència de tests, builds, diagnòstics o handoff de release.

Utilitza retenció proporcional a sensibilitat i necessitat operativa. Evita retenció indefinida d'artifacts sorollosos o sensibles. No pugis mai secrets, `.env` privats, dades de clients, credential stores o dumps de producció sense restriccions com a artifacts de CI.

## Reusable workflows

Un reusable workflow és apropiat quan el comportament és:

- comú a diversos repositoris;
- determinista;
- prou neutral respecte a proveïdor/stack;
- prou estable per justificar manteniment centralitzat;
- petit i auditable.

Bons candidats poden incloure checks estrets de governança, com validar referències externes d'Actions o metadata de repositoris un cop aquests contractes estiguin provats.

Mals candidats inclouen un workflow universal de build/test/deploy que barregi Laravel, Astro, Python, documentació i supòsits específics de clients.

Aquest repositori no introdueix una mega-workflow baseline.

## Workflow templates

Els workflow templates organitzatius es poden afegir sota `workflow-templates/` quan un patró repetit hagi demostrat utilitat. Són **ajudes d'adopció**, no enforcement heretat: el repo destí continua sent propietari del workflow generat i dels seus Quality Gates específics.

## Auto-merge

No hi ha una baseline global d'auto-merge aquí.

En particular:

- upgrades major no s'han d'auto-mergear per defecte;
- canvis sensibles de seguretat, migracions, infraestructura, release i alt risc requereixen els human/review gates aplicables;
- qualsevol política local d'auto-merge ha de ser explícita, limitada i compatible amb DCP Flow.

## Checklist de review

Per a un workflow nou o modificat, verifica segons sigui aplicable:

```text
[ ] permisos mínims
[ ] Actions externes fixades a SHA complet aprovat
[ ] codi no fiable sense accés a secrets/tokens privilegiats
[ ] pull_request_target absent o explícitament justificat/revisat
[ ] artifacts sense dades sensibles i amb retenció raonable
[ ] jobs privilegiats de deploy/publish amb trust boundary explícita
[ ] workflow apropiat a l'stack/projecte
[ ] cap afirmació falsa d'herència organitzativa
```
