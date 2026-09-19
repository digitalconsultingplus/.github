# Modelo de gobernanza de repositorios

Este documento define las convenciones organizacionales ligeras implementadas por `digitalconsultingplus/.github`.

No es el estándar normativo de DCP Flow. Si una regla aquí entra en conflicto con DCP Flow, prevalece DCP Flow.

**Idiomas:** [English](repository-governance.md) · **Español** · [Català](repository-governance.ca.md)

## Estados de lifecycle del repositorio

Estos estados describen la postura operativa de un repositorio. Son intencionalmente independientes del versionado o releases de software.

| Estado | Significado | Postura esperada |
|---|---|---|
| `ACTIVE` | Desarrollo u operación activos | Ownership actual, Project Truth y Quality Gates proporcionales al riesgo. |
| `MAINTENANCE` | Estable; recibe fixes, upgrades o cambios limitados | Mantener seguridad/dependencias y ownership; evitar feature churn salvo aprobación. |
| `PAUSED` | Trabajo suspendido intencionalmente con expectativa de retomarse | Registrar razón/contexto; evitar supuestos operativos silenciosos; mantener entendido cualquier riesgo crítico. |
| `PLANNING` | Repo aprobado o candidato antes de implementación normal | Arquitectura/scope pueden evolucionar; no presentar capacidades planificadas como implementadas. |
| `EXPERIMENTAL` | Lab, PoC o evaluación acotada | Puede aplicar menor ceremonia, pero siguen vigentes reglas de secretos, provenance y seguridad. No implica readiness de producción. |
| `ARCHIVED` | No se espera desarrollo activo | Debe quedar read-only/archivado cuando sea práctico; documentar sucesor o razón cuando sea útil. |

Los cambios de lifecycle son cambios de gobernanza y deben basarse en evidencia, no inferirse solo por frecuencia de commits.

## Clasificación de repositorios

La organización utiliza los siguientes tipos prácticos para catálogo y discovery de automatización:

| Tipo | Uso previsto |
|---|---|
| `product` | Producto, SaaS o capacidad de aplicación propiedad de DCP. |
| `website` | Sitio marketing, corporativo, documentation front-end o static-first. |
| `internal-tool` | Herramienta interna operativa, de ingeniería o negocio. |
| `client-project` | Repo ligado principalmente a un engagement o outcome de cliente. |
| `methodology` | Métodos, estándares, blueprints u operating system como DCP Flow. |
| `documentation` | Repo documentation-first sin producto runtime primario. |
| `library` | Paquete, módulo, SDK o asset de código reusable. |
| `foundation` | Foundation de organización/plataforma, templates o infraestructura de governance. |
| `lab` | Experimentos, evaluaciones y PoCs. |
| `profile` | Contenido de perfil/meta presentación organizacional. |

La clasificación es descriptiva, no un nivel automático de seguridad. El riesgo, los datos y el contexto de deployment siguen determinando los gates.

## Contrato ligero de metadata del repositorio

Un catálogo machine-readable será útil para Kai y para el inventario futuro de repositorios, pero este repo no impone tooling nuevo antes de validar el contrato.

Hasta que DCP Flow defina o seleccione un schema canónico machine-readable para catálogo, se recomiendan los siguientes **campos lógicos**:

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

### Reglas del contrato

- Son campos de catálogo, no sustituyen Project Truth, ADRs ni configuración de DCP Flow.
- No crear un owner ficticio para satisfacer el schema.
- No almacenar credenciales, secretos de clientes ni detalles sensibles de deployment.
- `default_branch` y `release_branch` deben reflejar el estado **observado** del repo, no solo la política deseada.
- `data_classification` describe la postura de manejo del sistema/repositorio y no concede acceso.
- Un schema canónico futuro de DCP Flow debe sustituir este candidato sin mantener dos contratos paralelos.

## Guía de criticality

Una escala simple de cuatro niveles es suficiente para routing de governance:

- `low` — blast radius limitado, recuperación simple, no sensible;
- `medium` — impacto material de negocio/repositorio pero recuperación acotada;
- `high` — impacto importante en producción, cliente, seguridad o datos;
- `critical` — infraestructura privilegiada, datos irreversibles/de alto valor, identidad, dinero o blast radius amplio.

La criticality informa reviews/gates, pero no sustituye la clasificación de riesgo de Quality Gates para un cambio concreto.

## Guía de data classification

- `public` — publicable intencionalmente;
- `internal` — información no pública de la organización con sensibilidad limitada;
- `confidential` — información de negocio/cliente que requiere acceso controlado;
- `restricted` — secretos, datos regulados/de alto impacto o material que requiere controles máximos.

Los secretos reales no deben comitearse incluso si el repo está clasificado como `restricted`.

## Baseline de CODEOWNERS

`CODEOWNERS` es específico de cada repo y **no se hereda** desde este `.github` organizacional.

Crea un `CODEOWNERS` local solo cuando pueda expresarse ownership real. Patrones típicos pueden mapear:

```text
*                         <real-default-owner>
/.github/                 <real-governance-owner>
/infrastructure/          <real-platform-owner>
/security-sensitive-path <real-security-owner>
```

No copies estos placeholders a un archivo real. Los owners deben ser usuarios o equipos GitHub reales con acceso adecuado al repositorio.

Usa CODEOWNERS proporcionalmente al riesgo y estructura del equipo. Un lab pequeño puede no necesitar ownership por paths; un repo de producción/cliente/alta criticality normalmente sí.

## Modelo de herencia de governance

### Defaults que GitHub puede descubrir desde el `.github` público de la organización

Cuando el repo destino no tiene equivalente local, los community health defaults soportados pueden incluir:

- `CONTRIBUTING.md`;
- `SECURITY.md`;
- pull request templates;
- issue templates y chooser config.

El perfil organizacional se proporciona mediante `profile/README.md`.

### Controles que deben ser locales o configurarse explícitamente en otra capa

No se heredan automáticamente solo por existir aquí:

- CODEOWNERS;
- configuración de Dependabot;
- workflows del repositorio;
- invocación de reusable workflows;
- branch protection / repository rulesets;
- secrets/variables;
- environments;
- deployment settings;
- licenses;
- tests y Quality Gates específicos del proyecto.

Los workflow templates pueden ofrecerse desde `workflow-templates/`, pero su adopción crea/copia configuración en el repo destino; no representan enforcement global silencioso.

## Gobernanza progresiva

Aplica controles según el riesgo y contexto operativo reales. El objetivo es consistencia sin burocracia:

```text
minimum coherent defaults
+ repository truth
+ risk-proportionate controls
+ evidence
+ human authority
```

No añadas automatización solo porque pueda centralizarse. Añádela cuando sea común, determinista, segura y reduzca drift de forma medible.
