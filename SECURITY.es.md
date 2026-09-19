# Política de Seguridad

Digital Consulting Plus trata los reportes de seguridad como información sensible hasta que puedan evaluarse y remediarse responsablemente.

Esta política organizacional es el default para repositorios que no definen un `SECURITY.md` específico.

**Idiomas:** [English](SECURITY.md) · **Español** · [Català](SECURITY.ca.md)

## Reportar una vulnerabilidad

**No reportes vulnerabilidades sensibles en issues, discussions, pull requests u otros canales públicos de GitHub.**

Ruta preferida de reporte:

1. utiliza el flujo de private vulnerability reporting / GitHub Security Advisory del repositorio cuando esté habilitado; o
2. contacta a Digital Consulting Plus en **info@digitalconsultingplus.com** e incluye información suficiente para reproducir y evaluar el problema sin enviar secretos ni datos personales/de clientes innecesarios.

Si un repositorio define un canal privado más específico, utiliza ese canal.

## Alcance

Los reportes pueden cubrir, cuando aplique:

- repositorios públicos de Digital Consulting Plus;
- aplicaciones y servicios de propiedad de la organización representados por esos repositorios;
- fallos de autenticación/autorización;
- inyección, exposición de datos o escalamiento de privilegios;
- riesgos de dependencias y software supply chain;
- comportamiento inseguro de CI/CD;
- exposición de secretos;
- debilidades de infraestructura o deployment demostradas directamente por el comportamiento del repositorio.

Los sistemas de clientes, servicios de terceros y repositorios que no controla Digital Consulting Plus pueden requerir una ruta de disclosure distinta. No pruebes sistemas sin autorización.

## Secretos y credenciales

Nunca hagas commit ni publiques:

- API keys o access tokens;
- contraseñas;
- llaves privadas o certificados con material privado;
- credenciales cloud;
- valores `.env` de producción;
- datos de clientes o información confidencial.

Si un secreto queda expuesto, trátalo como comprometido: revócalo/rótalo, evalúa el blast radius y retíralo de uso activo. Eliminarlo solo del último commit no constituye una remediación suficiente.

## Dependencias y supply chain

La revisión de seguridad debe considerar dependencias directas y transitivas, GitHub Actions, contenedores y otros inputs de build/runtime proporcionalmente al riesgo.

Las GitHub Actions externas usadas en repositorios DCP deben seguir los controles vigentes de DCP Flow, incluido el pinning a commit SHA inmutable cuando el estándar lo exija.

Los upgrades major no deben auto-mergearse globalmente por defecto. Los cambios materiales de dependencias requieren Quality Gates aplicables y review.

## Disclosure responsable

Incluye:

- repositorio/componente afectado;
- impacto claro;
- pasos de reproducción o prueba de concepto cuando sea seguro;
- versiones/commits relevantes;
- mitigación sugerida si se conoce.

No incluyas payloads de explotación, secretos ni datos de clientes más allá de lo estrictamente necesario para demostrar el problema.

Digital Consulting Plus evaluará el reporte, determinará ownership y ruta de remediación, y coordinará disclosure cuando corresponda. Un reporte no autoriza pruebas destructivas, persistencia, movimiento lateral ni acceso a datos no relacionados.

## Repositorios públicos vs privados

Un repositorio público puede exponer código fuente, pero no debe exponer secretos operativos ni información confidencial de clientes. Los repositorios privados requieren la misma disciplina; la visibilidad no sustituye control de acceso, secret management ni prácticas de desarrollo seguro.

## DCP Flow

Las decisiones de seguridad y Quality Gates se gobiernan por los estándares actuales de [DCP Flow](https://github.com/digitalconsultingplus/dcp-flow). Este archivo define la baseline organizacional de reporte y manejo, no una metodología de seguridad duplicada.
