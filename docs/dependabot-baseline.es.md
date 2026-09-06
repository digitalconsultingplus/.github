# Baseline de Dependabot

La configuración de Dependabot es **específica de cada repositorio**. Un `.github/dependabot.yml` ubicado en el repositorio organizacional `.github` no configura automáticamente los demás repositorios.

Este documento define la baseline organizacional que debe aplicarse localmente cuando Dependabot sea apropiado.

**Idiomas:** [English](dependabot-baseline.md) · **Español** · [Català](dependabot-baseline.ca.md)

## Separar ecosistemas de dependencias

Configura únicamente los ecosistemas realmente presentes en el repositorio.

Categorías típicas:

1. **Dependencias de aplicación** — Composer, npm/pnpm/yarn, pip/Poetry, etc.
2. **GitHub Actions** — referencias de Actions en workflows.
3. **Docker** — imágenes base cuando el repositorio construye u opera contenedores.

No añadas ecosistemas inexistentes solo para satisfacer un template.

## Estrategia de actualización

Baseline recomendada:

- ejecutar checks de dependencias con una cadencia predecible apropiada al repositorio;
- agrupar updates patch/minor de bajo riesgo solo cuando mejore la revisabilidad;
- mantener upgrades major separados salvo que exista un plan explícito de migración;
- nunca auto-mergear globalmente upgrades major por defecto;
- revisar cambios de lockfiles e impacto transitivo;
- ejecutar los Quality Gates aplicables antes del merge;
- tratar Actions de CI e imágenes de contenedor como dependencias de supply chain, no solo librerías de aplicación.

## Actualizaciones de seguridad

Las actualizaciones de seguridad deben priorizarse por exploitability, exposición, runtime afectado e impacto de negocio, no únicamente por el score CVE.

Una alerta de seguridad no justifica saltarse tests, revisión de migraciones o aceptación humana cuando la actualización es material. Las excepciones de emergencia deben seguir el proceso break-glass del repositorio cuando corresponda.

## GitHub Actions

DCP Flow exige que las Actions externas estén fijadas a commit SHAs completos e inmutables. Dependabot puede proponer actualizaciones de Actions, pero el workflow resultante debe seguir cumpliendo esa política y revisarse como un cambio de dependencia de CI.

## Docker

Usa monitoreo de updates Docker cuando el repositorio sea dueño de Dockerfiles o referencias de imágenes que afecten materialmente el build/runtime.

Revisar:

- provenance de la imagen base;
- semántica digest/tag;
- compatibilidad de runtime;
- cambios de sistema operativo/paquetes;
- impacto de tamaño/seguridad;
- evidencia de build y smoke cuando aplique.

## Ejemplo de forma

El siguiente ejemplo es solo ilustrativo. Cópialo y adáptalo dentro del repositorio destino; no asumas que estos ecosistemas o directorios existen en todos los proyectos.

```yaml
version: 2
updates:
  - package-ecosystem: github-actions
    directory: /
    schedule:
      interval: weekly

  # Añadir solo cuando el repositorio use realmente este ecosistema.
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

Los maintainers deben añadir ownership, labels, grouping, target branch y cadence solo cuando esos valores sean conocidos y estén respaldados por Repository Truth.

## Por qué no existe un `dependabot.yml` organizacional aquí

Añadirlo configuraría este repositorio `.github`, no el resto de la organización. Presentarlo como governance heredada sería técnicamente falso. El modelo correcto es una baseline documentada más configuración local por repo o, en el futuro, automatización explícita que cree/mantenga esos archivos con evidencia.
