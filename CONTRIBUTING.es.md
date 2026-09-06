# Contribuir a los repositorios de Digital Consulting Plus

Esta es la baseline organizacional de contribución para repositorios que no definen un `CONTRIBUTING.md` más específico.

Las reglas normativas de ingeniería viven en [DCP Flow](https://github.com/digitalconsultingplus/dcp-flow). Las instrucciones específicas de cada repositorio prevalecen cuando son más estrictas o precisas.

**Idiomas:** [English](CONTRIBUTING.md) · **Español** · [Català](CONTRIBUTING.ca.md)

## Flujo de contribución por defecto

```text
issue / outcome
→ working branch
→ implementación
→ evidencia
→ pull request
→ CI / Quality Gate
→ review
→ merge
```

### 1. Partir de un outcome

Antes de implementar, identifica el problema, outcome solicitado, issue, sprint o unidad de trabajo acotada. Evita cambios no relacionados en la misma rama.

### 2. Usar una working branch

Para repositorios que adoptan el modelo actual de ramas de DCP Flow:

- `dev` es la rama de integración/default;
- `main` es la baseline estable/release;
- las working branches temporales nacen desde `dev`;
- no se desarrolla directamente sobre `dev` ni `main`.

Prefijos recomendados: `feature/`, `fix/`, `hotfix/`, `refactor/`, `docs/`, `chore/` y `experiment/`.

Los repositorios legacy pueden tener una ruta de transición explícitamente documentada. No asumas que existe una excepción.

### 3. Implementar el cambio coherente más pequeño

Prefiere cambios revisables, reversibles y alineados con la arquitectura del repositorio, el Blueprint de DCP Flow seleccionado y el Project Truth local.

No introduzcas nuevos frameworks, proveedores, dependencias o complejidad operativa sin una necesidad concreta.

### 4. Producir evidencia

Ejecuta los Quality Gates aplicables al riesgo y stack reales. La evidencia puede incluir tests, lint/static checks, builds, revisiones de dependencias/seguridad, revisión de migraciones, smoke checks de runtime, screenshots o validación manual cuando la automatización no pueda demostrar el requisito.

Usa estados honestos: `PASS`, `FAIL`, `NOT RUN`, `NOT APPLICABLE` o `BLOCKED`. Nunca declares un gate como aprobado si no fue ejecutado o verificado.

### 5. Abrir un Pull Request

Describe:

- outcome/problema resuelto;
- scope y exclusiones relevantes;
- evidencia y Quality Gates ejecutados;
- impacto de seguridad;
- impacto de datos/migraciones;
- impacto de documentación;
- rollback/recovery cuando el riesgo lo requiera;
- cualquier Human Gate todavía pendiente.

### 6. Revisar antes de merge

Crear un PR no autoriza el merge. Los PRs de working branches requieren la auditoría/review exigida por DCP Flow y los controles propios del repositorio. La promoción/release material sigue sujeta a aceptación humana.

## Seguridad

Nunca hagas commit de credenciales, tokens, llaves privadas, datos de clientes u otros secretos. Las vulnerabilidades sensibles deben seguir `SECURITY.md` y no deben divulgarse en issues públicos.

## Reglas específicas del repositorio

Un repositorio puede definir reglas más estrictas para:

- topología de ramas;
- número de reviews;
- checks requeridos;
- CODEOWNERS;
- security gates;
- migraciones/manejo de datos;
- deployment/release;
- licenciamiento/provenance;
- confidencialidad de clientes.

Estas reglas locales prevalecen para ese repositorio siempre que no debiliten requisitos obligatorios de DCP Flow sin una excepción aprobada.
