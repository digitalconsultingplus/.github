# Política de Seguretat

Digital Consulting Plus tracta els reports de seguretat com a informació sensible fins que es puguin avaluar i remediar de manera responsable.

Aquesta política organitzativa és el default per als repositoris que no defineixen un `SECURITY.md` específic.

**Idiomes:** [English](SECURITY.md) · [Español](SECURITY.es.md) · **Català**

## Reportar una vulnerabilitat

**No reportis vulnerabilitats sensibles en issues, discussions, pull requests o altres canals públics de GitHub.**

Ruta preferida de report:

1. utilitza el flux de private vulnerability reporting / GitHub Security Advisory del repositori quan estigui habilitat; o
2. contacta amb Digital Consulting Plus a **info@digitalconsultingplus.com** i inclou informació suficient per reproduir i avaluar el problema sense enviar secrets ni dades personals/de clients innecessàries.

Si un repositori defineix un canal privat més específic, utilitza aquell canal.

## Abast

Els reports poden cobrir, quan sigui aplicable:

- repositoris públics de Digital Consulting Plus;
- aplicacions i serveis propietat de l'organització representats per aquests repositoris;
- errors d'autenticació/autorització;
- injecció, exposició de dades o escalada de privilegis;
- riscos de dependències i software supply chain;
- comportament insegur de CI/CD;
- exposició de secrets;
- debilitats d'infraestructura o deployment demostrades directament pel comportament del repositori.

Els sistemes de clients, serveis de tercers i repositoris que no controla Digital Consulting Plus poden requerir una ruta de disclosure diferent. No provis sistemes sense autorització.

## Secrets i credencials

No facis mai commit ni publiquis:

- API keys o access tokens;
- contrasenyes;
- claus privades o certificats amb material privat;
- credencials cloud;
- valors `.env` de producció;
- dades de clients o informació confidencial.

Si un secret queda exposat, tracta'l com a compromès: revoca'l/rota'l, avalua el blast radius i retira'l de l'ús actiu. Eliminar-lo només de l'últim commit no és una remediació suficient.

## Dependències i supply chain

La revisió de seguretat ha de considerar dependències directes i transitives, GitHub Actions, contenidors i altres inputs de build/runtime proporcionalment al risc.

Les GitHub Actions externes utilitzades en repositoris DCP han de seguir els controls vigents de DCP Flow, inclòs el pinning a commit SHA immutable quan l'estàndard ho exigeixi.

Els upgrades major no s'han d'auto-mergear globalment per defecte. Els canvis materials de dependències requereixen Quality Gates aplicables i review.

## Disclosure responsable

Inclou:

- repositori/component afectat;
- impacte clar;
- passos de reproducció o prova de concepte quan sigui segur;
- versions/commits rellevants;
- mitigació suggerida si es coneix.

No incloguis payloads d'explotació, secrets ni dades de clients més enllà del que sigui estrictament necessari per demostrar el problema.

Digital Consulting Plus avaluarà el report, determinarà ownership i ruta de remediació, i coordinarà disclosure quan correspongui. Un report no autoritza proves destructives, persistència, moviment lateral ni accés a dades no relacionades.

## Repositoris públics vs privats

Un repositori públic pot exposar codi font, però no ha d'exposar secrets operatius ni informació confidencial de clients. Els repositoris privats requereixen la mateixa disciplina; la visibilitat no substitueix control d'accés, secret management ni pràctiques de desenvolupament segur.

## DCP Flow

Les decisions de seguretat i Quality Gates es governen pels estàndards actuals de [DCP Flow](https://github.com/digitalconsultingplus/dcp-flow). Aquest fitxer defineix la baseline organitzativa de report i gestió, no una metodologia de seguretat duplicada.
