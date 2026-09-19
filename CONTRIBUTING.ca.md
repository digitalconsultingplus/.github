# Contribuir als repositoris de Digital Consulting Plus

Aquesta és la baseline organitzativa de contribució per als repositoris que no defineixen un `CONTRIBUTING.md` més específic.

Les regles normatives d'enginyeria viuen a [DCP Flow](https://github.com/digitalconsultingplus/dcp-flow). Les instruccions específiques de cada repositori prevalen quan són més estrictes o precises.

**Idiomes:** [English](CONTRIBUTING.md) · [Español](CONTRIBUTING.es.md) · **Català**

## Flux de contribució per defecte

```text
issue / outcome
→ working branch
→ implementació
→ evidència
→ pull request
→ CI / Quality Gate
→ review
→ merge
```

### 1. Partir d'un outcome

Abans d'implementar, identifica el problema, outcome sol·licitat, issue, sprint o unitat de treball acotada. Evita canvis no relacionats a la mateixa branca.

### 2. Utilitzar una working branch

Per als repositoris que adopten el model actual de branques de DCP Flow:

- `dev` és la branca d'integració/default;
- `main` és la baseline estable/release;
- les working branches temporals neixen des de `dev`;
- no es desenvolupa directament sobre `dev` ni `main`.

Prefixos recomanats: `feature/`, `fix/`, `hotfix/`, `refactor/`, `docs/`, `chore/` i `experiment/`.

Els repositoris legacy poden tenir una ruta de transició documentada explícitament. No assumeixis que existeix una excepció.

### 3. Implementar el canvi coherent més petit

Prefereix canvis revisables, reversibles i alineats amb l'arquitectura del repositori, el Blueprint de DCP Flow seleccionat i el Project Truth local.

No introdueixis nous frameworks, proveïdors, dependències o complexitat operativa sense una necessitat concreta.

### 4. Produir evidència

Executa els Quality Gates aplicables al risc i stack reals. L'evidència pot incloure tests, lint/static checks, builds, revisions de dependències/seguretat, revisió de migracions, smoke checks de runtime, screenshots o validació manual quan l'automatització no pugui demostrar el requisit.

Utilitza estats honestos: `PASS`, `FAIL`, `NOT RUN`, `NOT APPLICABLE` o `BLOCKED`. No declaris mai un gate com aprovat si no ha estat executat o verificat.

### 5. Obrir un Pull Request

Descriu:

- outcome/problema resolt;
- scope i exclusions rellevants;
- evidència i Quality Gates executats;
- impacte de seguretat;
- impacte de dades/migracions;
- impacte de documentació;
- rollback/recovery quan el risc ho requereixi;
- qualsevol Human Gate encara pendent.

### 6. Revisar abans del merge

Crear un PR no autoritza el merge. Els PRs de working branches requereixen l'auditoria/review exigida per DCP Flow i els controls propis del repositori. La promoció/release material continua subjecta a acceptació humana.

## Seguretat

No facis mai commit de credencials, tokens, claus privades, dades de clients o altres secrets. Les vulnerabilitats sensibles han de seguir `SECURITY.md` i no s'han de divulgar en issues públics.

## Regles específiques del repositori

Un repositori pot definir regles més estrictes per a:

- topologia de branques;
- nombre de reviews;
- checks requerits;
- CODEOWNERS;
- security gates;
- migracions/gestió de dades;
- deployment/release;
- llicenciament/provenance;
- confidencialitat de clients.

Aquestes regles locals prevalen per a aquell repositori sempre que no debilitin requisits obligatoris de DCP Flow sense una excepció aprovada.
