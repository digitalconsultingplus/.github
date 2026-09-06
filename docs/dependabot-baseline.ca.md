# Baseline de Dependabot

La configuració de Dependabot és **específica de cada repositori**. Un `.github/dependabot.yml` ubicat al repositori organitzatiu `.github` no configura automàticament els altres repositoris.

Aquest document defineix la baseline organitzativa que s'ha d'aplicar localment quan Dependabot sigui apropiat.

**Idiomes:** [English](dependabot-baseline.md) · [Español](dependabot-baseline.es.md) · **Català**

## Separar ecosistemes de dependències

Configura únicament els ecosistemes realment presents al repositori.

Categories típiques:

1. **Dependències d'aplicació** — Composer, npm/pnpm/yarn, pip/Poetry, etc.
2. **GitHub Actions** — referències d'Actions als workflows.
3. **Docker** — imatges base quan el repositori construeix o opera contenidors.

No afegeixis ecosistemes inexistents només per satisfer un template.

## Estratègia d'actualització

Baseline recomanada:

- executar checks de dependències amb una cadència previsible apropiada al repositori;
- agrupar updates patch/minor de baix risc només quan millori la revisabilitat;
- mantenir upgrades major separats tret que existeixi un pla explícit de migració;
- no auto-mergear mai globalment upgrades major per defecte;
- revisar canvis de lockfiles i impacte transitiu;
- executar els Quality Gates aplicables abans del merge;
- tractar Actions de CI i imatges de contenidor com dependències de supply chain, no només llibreries d'aplicació.

## Actualitzacions de seguretat

Les actualitzacions de seguretat s'han de prioritzar per exploitability, exposició, runtime afectat i impacte de negoci, no només pel score CVE.

Una alerta de seguretat no justifica saltar-se tests, revisió de migracions o acceptació humana quan l'actualització és material. Les excepcions d'emergència han de seguir el procés break-glass del repositori quan correspongui.

## GitHub Actions

DCP Flow exigeix que les Actions externes estiguin fixades a commit SHAs complets i immutables. Dependabot pot proposar actualitzacions d'Actions, però el workflow resultant ha de continuar complint aquesta política i revisar-se com un canvi de dependència de CI.

## Docker

Utilitza monitoratge d'updates Docker quan el repositori sigui propietari de Dockerfiles o referències d'imatges que afectin materialment el build/runtime.

Revisar:

- provenance de la imatge base;
- semàntica digest/tag;
- compatibilitat de runtime;
- canvis de sistema operatiu/paquets;
- impacte de mida/seguretat;
- evidència de build i smoke quan sigui aplicable.

## Exemple de forma

L'exemple següent és només il·lustratiu. Copia'l i adapta'l dins del repositori destí; no assumeixis que aquests ecosistemes o directoris existeixen a tots els projectes.

```yaml
version: 2
updates:
  - package-ecosystem: github-actions
    directory: /
    schedule:
      interval: weekly

  # Afegir només quan el repositori utilitzi realment aquest ecosistema.
  # - package-ecosystem: composer
  #   directory: /
  #   schedule:
  #     interval: weekly

  # - package-ecosystem: npm
  #   directory: /
  #   schedule:
  #     interval: weekly

  # - package-ecosystem: docker
  #   directory: /
  #   schedule:
  #     interval: weekly
```

Els maintainers han d'afegir ownership, labels, grouping, target branch i cadence només quan aquests valors siguin coneguts i estiguin respaldats per Repository Truth.

## Per què no hi ha un `dependabot.yml` organitzatiu aquí

Afegir-lo configuraria aquest repositori `.github`, no la resta de l'organització. Presentar-lo com governance heretada seria tècnicament fals. El model correcte és una baseline documentada més configuració local per repo o, en el futur, automatització explícita que creï/mantingui aquests fitxers amb evidència.
