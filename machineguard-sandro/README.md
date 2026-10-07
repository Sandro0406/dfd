# MachineGuard – Aportes de Sandro al Capítulo VI (borrador para revisión)

Borradores de mis secciones del Capítulo VI (TB1 – Sprint 1) antes de pasarlas a `MachineGuard-Documentation`, rama `docs/chapter6`. La carpeta replica la estructura del informe (`docs/` y `assets/img/`), así que las rutas de las imágenes (`../assets/img/...`) funcionan igual al copiarlas.

## Archivos

| Archivo | Sección del informe | Estado | Cómo integrarlo |
|---|---|---|---|
| [docs/6.1.4-software-deployment-configuration.md](docs/6.1.4-software-deployment-configuration.md) | 6.1.4. Software Deployment Configuration | Completo, con pendientes de Pedro y Camilla marcados con ⚠️ | Reemplaza el "Contenido pendiente" de 6.1.4. |
| [docs/6.2.1.5-testing-suite-evidence.md](docs/6.2.1.5-testing-suite-evidence.md) | 6.2.1.5. Testing Suite Evidence for Sprint Review | Completo | Reemplaza el "Contenido pendiente" de 6.2.1.5. |
| [docs/6.2.1.4-development-evidence-sandro.md](docs/6.2.1.4-development-evidence-sandro.md) | 6.2.1.4. Development Evidence (mi parte) | Completo | Se agrega al final de 6.2.1.4, después de Bautista. |
| [docs/6.2.1.2-6.2.1.3-filas-para-pedro.md](docs/6.2.1.2-6.2.1.3-filas-para-pedro.md) | 6.2.1.2 y 6.2.1.3 | Filas listas | Se las paso a Pedro, que mantiene esas tablas y Jira. |
| [docs/6.2.1.8-software-deployment-evidence.md](docs/6.2.1.8-software-deployment-evidence.md) | 6.2.1.8. Software Deployment Evidence | Borrador | Sin responsable asignado; confirmar con el equipo. |

Imágenes:

- `assets/img/chapter-6/testing/`: capturas de GitHub Actions (core-api y edge-api) y del reporte HTML de Cucumber.
- `assets/img/chapter-6/deployment/`: Landing Page publicado y Web Application publicada (esta última muestra el error de conexión, solo como referencia; no va al informe).
- `assets/img/chapter-4/...`: copia del Deployment Diagram del Cap. IV, solo para que se vea aquí. En el informe ya existe.

## Revisar antes de pasarlo

1. **Gherkin en inglés.** El enunciado, en la sección Source Code Style Guide & Conventions, dice: *"Gherkin para los archivos .feature. Para todos los lenguajes debe aplicar la nomenclatura en inglés"*. Los `.feature` están escritos en español (`# language: es`). Si el equipo lo interpreta como contenido en inglés, hay que traducir los 10 `.feature` y sus pasos (Given/When/Then) y regenerar 6.2.1.5.
2. **Horas del Sprint Backlog.** Son estimaciones; ajústalas a tu dedicación real.
3. **Pendientes de otros**, marcados con ⚠️ en los archivos:
   - Pedro: plataforma de despliegue de la API y de PostgreSQL, URL pública y configuración CORS para `https://machineguard.github.io`.
   - Camilla: `apiBaseUrl` en `environment.production.ts` apunta a `/api/v1` (en GitHub Pages no hay API) y recargar `/machineguard-web/dashboard` da 404 (falta `404.html`).
   - Integración: la API central no expone todavía `POST /api/v1/measurements`, que es a donde publica la Edge API.
4. **Rama de Environmental Monitoring.** `feature/environmental-monitoring` de core-api ya tiene código (commit `0ff5c86`). Cuando se integre a `develop`, hay que volver a correr las pruebas y, si agrega tablas, añadirlas a la limpieza de `CommonSteps.java`.
5. **Test inestable de IAM (avisar a Pedro).** `IamApiIntegrationTest.loginNormalizesEmailAndTraceabilityIgnoresCallerSuppliedIdentityHeaders` altera solo el último carácter del JWT; según Base64, a veces ese cambio no modifica la firma y el test falla de forma aleatoria. Se corrige alterando un carácter del medio del token.
6. **Deployment Diagram del Cap. IV.** Indica "JVM 17+" y "PostgreSQL 15+"; el proyecto usa Java 21 y PostgreSQL 16. No es incorrecto (son mínimos), pero se puede precisar.

## Pruebas (referencia)

- Ramas: `feature/testing-bdd-acceptance` en `machineguard-core-api` y `machineguard-edge-api` (sin PR abierto todavía).
- core-api: 65 de 65 pruebas (13 unitarias, 21 de integración y 31 escenarios BDD).
- edge-api: 27 de 27 pruebas (12 de integración y 15 escenarios BDD).
