# Baseline de gobernanza de GitHub Actions

Este documento traduce los controles actuales de repositorios DCP Flow a una baseline organizacional mínima para GitHub Actions.

La fuente normativa sigue siendo DCP Flow, especialmente `standards/github-repository-controls.es.md` y los Quality Gates aplicables.

**Idiomas:** [English](github-actions-governance.md) · **Español** · [Català](github-actions-governance.ca.md)

## Principios

Los workflows de GitHub Actions deben ser:

- de mínimo privilegio por defecto;
- deterministas cuando sea práctico;
- explícitos respecto a secretos y trust boundaries;
- suficientemente pequeños para ser revisables;
- conscientes del stack en lugar de universales;
- reutilizables solo cuando el comportamiento compartido sea realmente común.

## Permisos

Configura permisos de workflow/job al mínimo requerido.

Prefiere una baseline explícita como:

```yaml
permissions:
  contents: read
```

Añade scopes de escritura solo al job que realmente los necesite. No dependas de permisos implícitos amplios cuando pueda definirse un contrato más estrecho.

## Actions externas y supply chain

DCP Flow exige actualmente que las GitHub Actions externas se referencien mediante un commit SHA completo e inmutable.

```yaml
# Evitar referencias mutables
uses: actions/checkout@v4

# Usar un SHA inmutable aprobado
uses: actions/checkout@<full-commit-sha> # comentario de versión legible para humanos
```

Actualizar una Action sigue siendo un cambio de dependencia: revisa el nuevo SHA/source y ejecuta los Quality Gates aplicables.

Las Actions locales del mismo repositorio pueden referenciarse mediante ruta relativa.

## Código no confiable y secretos

Nunca ejecutes código no confiable de un pull request con write tokens, secrets de repositorio/environment o credenciales cloud privilegiadas.

Boundaries importantes:

- el código de PRs desde forks no es confiable;
- el código de workflows modificado en un PR no es confiable hasta ser revisado;
- build scripts, package hooks y test fixtures pueden ejecutar código arbitrario;
- artifacts de jobs no confiables no deben convertirse automáticamente en inputs privilegiados.

Separa validación no confiable de jobs privilegiados de deployment/publishing.

## `pull_request_target`

Trata `pull_request_target` como de alto riesgo porque ejecuta en el contexto del repo base y puede acceder a permisos/secrets no disponibles para PRs ordinarios desde forks.

No combines `pull_request_target` con checkout/ejecución de código no confiable del PR salvo que el modelo de seguridad haya sido diseñado y revisado explícitamente.

Usa `pull_request` ordinario para validación normal siempre que sea posible.

## Secrets

- Nunca imprimas secretos intencionalmente en logs.
- Nunca pongas secretos en texto plano dentro de workflow YAML, archivos del repo o artifacts generados.
- Usa protección a nivel environment para operaciones privilegiadas de producción cuando aplique.
- No pases conjuntos amplios de secrets a reusable workflows salvo necesidad entendida y justificada.
- Prefiere credenciales de corta duración/federadas frente a llaves cloud de larga duración cuando el proveedor lo soporte y la arquitectura lo justifique.

## Artifacts y retención

Los artifacts deben existir con un propósito: evidencia de tests, builds, diagnósticos o handoff de release.

Usa retención proporcional a sensibilidad y necesidad operativa. Evita retención indefinida de artifacts ruidosos o sensibles. Nunca subas secrets, `.env` privados, datos de clientes, credential stores o dumps de producción sin restricciones como artifacts de CI.

## Reusable workflows

Un reusable workflow es apropiado cuando el comportamiento es:

- común a varios repositorios;
- determinista;
- suficientemente neutral respecto a proveedor/stack;
- suficientemente estable para justificar mantenimiento centralizado;
- pequeño y auditable.

Buenos candidatos pueden incluir checks estrechos de gobernanza, como validar referencias externas de Actions o metadata de repositorios una vez que esos contratos estén probados.

Malos candidatos incluyen un workflow universal de build/test/deploy que mezcle Laravel, Astro, Python, documentación y supuestos específicos de clientes.

Este repositorio no introduce un mega-workflow baseline.

## Workflow templates

Los workflow templates organizacionales pueden añadirse bajo `workflow-templates/` cuando un patrón repetido haya demostrado utilidad. Son **ayudas de adopción**, no enforcement heredado: el repo destino sigue siendo dueño del workflow generado y de sus Quality Gates específicos.

## Auto-merge

No existe una baseline global de auto-merge aquí.

En particular:

- upgrades major no deben auto-mergearse por defecto;
- cambios sensibles de seguridad, migraciones, infraestructura, release y alto riesgo requieren los human/review gates aplicables;
- cualquier política local de auto-merge debe ser explícita, limitada y compatible con DCP Flow.

## Checklist de review

Para un workflow nuevo o modificado, verifica según aplique:

```text
[ ] permisos mínimos
[ ] Actions externas fijadas a SHA completo aprobado
[ ] código no confiable sin acceso a secrets/tokens privilegiados
[ ] pull_request_target ausente o explícitamente justificado/revisado
[ ] artifacts sin datos sensibles y con retención razonable
[ ] jobs privilegiados de deploy/publish con trust boundary explícita
[ ] workflow apropiado al stack/proyecto
[ ] ninguna afirmación falsa de herencia organizacional
```
