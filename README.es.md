# Digital Consulting Plus — Hub de Gobernanza de Repositorios

Este repositorio es la capa organizacional de gobernanza GitHub de **Digital Consulting Plus**.

No reemplaza ni duplica [DCP Flow](https://github.com/digitalconsultingplus/dcp-flow).

**Idiomas:** [English](README.md) · **Español** · [Català](README.ca.md)

```text
dcp-flow
→ metodología, estándares, blueprints y criterios de decisión

digitalconsultingplus/.github
→ defaults organizacionales y convenciones reutilizables de GitHub
```

## Fuente de verdad

Las reglas normativas de ingeniería viven en DCP Flow. Este repositorio implementa únicamente defaults orientados a GitHub que son útiles de forma transversal entre repositorios.

Fuentes relevantes de DCP Flow:

- `standards/repository-governance.es.md`
- `standards/github-repository-controls.es.md`
- `standards/quality-gates.es.md`

Cuando este repositorio y DCP Flow entren en conflicto, **DCP Flow es la autoridad**.

## Qué vive aquí

Este repositorio proporciona una baseline mínima para:

- guía de contribución;
- reporte de vulnerabilidades;
- estructura de Pull Requests;
- intake de issues;
- lifecycle y clasificación de repositorios;
- guía de metadata de repositorios;
- gobernanza de GitHub Actions;
- baseline de Dependabot;
- reglas de herencia de gobernanza.

Está diseñado para productos SaaS, aplicaciones Laravel/PHP, sitios Astro/static-first, herramientas internas, documentación, labs, repositorios open source y repositorios privados de clientes.

## Qué sigue siendo específico de cada repositorio

Cada repositorio conserva responsabilidad sobre su Project Truth y controles ejecutables, incluyendo cuando aplique:

- `dcp.config.json` o configuración equivalente definida por DCP Flow;
- Quality Gates y CI específicos del stack;
- workflows y environments de despliegue;
- enforcement de branches/rulesets;
- CODEOWNERS específicos;
- configuración de Dependabot;
- políticas de dependencias de aplicación;
- secretos y configuración de entornos;
- procedimientos de datos/migraciones;
- procedimientos de release;
- decisiones de arquitectura y ADRs;
- licenciamiento y provenance de terceros.

Las reglas locales pueden ser más estrictas que esta baseline.

## Herencia de defaults de GitHub

Al ser este un repositorio público `.github` de organización, GitHub puede utilizar determinados community health files como defaults cuando el repositorio destino no define su propia versión.

| Asset | Default organizacional desde este repo | Notas |
|---|---|---|
| `CONTRIBUTING.md` | Sí | El repo destino puede sobrescribirlo. |
| `SECURITY.md` | Sí | El repo destino puede sobrescribirlo. |
| Pull request template | Sí | Se usa cuando el repo destino no tiene uno propio. |
| Issue templates / chooser config | Sí | La configuración local del repo destino prevalece. |
| `profile/README.md` | Sí, para el perfil organizacional | Solo perfil público de la organización. |
| `CODEOWNERS` | No | Debe vivir en cada repositorio que necesite enforcement de ownership. |
| `.github/dependabot.yml` | No | Debe vivir en cada repositorio. |
| `.github/workflows/*` | No | Los workflows no se propagan automáticamente. |
| Branch protection / rulesets | No | Deben configurarse por repo o a nivel organizacional explícito. |
| Secrets / variables | No | Nunca se definen aquí como defaults en texto plano. |
| License | No | Debe definirse por repositorio cuando aplique. |

## Ruta de adopción

Para un repositorio nuevo o existente:

1. clasificar el repositorio y su estado de lifecycle;
2. establecer Project Truth y ownership;
3. adoptar el modelo de ramas de DCP Flow cuando aplique;
4. usar estos defaults organizacionales solo donde encajen;
5. añadir CI, Dependabot, CODEOWNERS y controles específicos proporcionalmente al riesgo;
6. ejecutar los Quality Gates aplicables antes del merge;
7. preservar aceptación humana para merges/releases materiales.

Modelo operativo objetivo:

```text
issue / outcome
→ working branch
→ implementación
→ evidencia
→ PR
→ CI / Quality Gate
→ review
→ merge
```

Para repositorios DCP activos gobernados por el estándar actual, `dev` es la rama de integración/default y `main` la baseline estable/release. No se desarrolla directamente sobre ramas permanentes.

## Gobernanza progresiva

La gobernanza es proporcional al riesgo. Un SaaS público y un lab experimental no deberían tener la misma ceremonia, pero ambos deben preservar:

- Repository Truth;
- Evidence before claims;
- Human Acceptance;
- Security by default;
- Provider independence;
- Minimal duplication;
- Progressive governance.

## Modelo de repositorio

Consulta:

- [`docs/repository-governance.es.md`](docs/repository-governance.es.md) — lifecycle, clasificación, metadata y ownership;
- [`docs/github-actions-governance.es.md`](docs/github-actions-governance.es.md) — seguridad y reutilización de Actions;
- [`docs/dependabot-baseline.es.md`](docs/dependabot-baseline.es.md) — baseline de actualización de dependencias.

## Política de idiomas

La documentación de gobernanza se mantiene en **English, Español y Català**. Los archivos canónicos que GitHub descubre automáticamente permanecen en inglés para mantener una única superficie operativa; sus traducciones se publican como variantes `.es.md` y `.ca.md`. Los templates operativos de PR/issues no se duplican por idioma para evitar multiplicar opciones en la interfaz de GitHub.
