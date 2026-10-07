# MachineGuard – Aportes de Sandro al Capítulo VI (borrador para revisión)

Borradores de **mi parte** del Capítulo VI (TB1 – Sprint 1), para revisarlos antes de agregarlos a `MachineGuard-Documentation`, rama `docs/chapter6`. Cada archivo es un bloque que **se agrega** a la sección correspondiente, después de lo que ya escribió el equipo, sin modificar su contenido. Al inicio de cada archivo hay una nota "Cómo integrarlo" que no se copia al informe.

La carpeta replica la estructura del informe (`docs/` y `assets/img/`), así que las rutas de las imágenes (`../assets/img/...`) funcionan igual al copiarlas.

## Archivos

| Archivo | Sección | Dónde va |
|---|---|---|
| [docs/6.1.4-software-deployment-configuration.md](docs/6.1.4-software-deployment-configuration.md) | 6.1.4. Software Deployment Configuration | Después del bloque **Landing Page** (de Jhoan). Cubre la Web Application, la RESTful API y la Edge API, el CI y el Deployment Diagram, que la nota del equipo marca como pendientes. |
| [docs/6.2.1.4-development-evidence-sandro.md](docs/6.2.1.4-development-evidence-sandro.md) | 6.2.1.4. Development Evidence | Al final, después de "Jhoan Janampa — Landing Page". |
| [docs/6.2.1.5-testing-suite-evidence.md](docs/6.2.1.5-testing-suite-evidence.md) | 6.2.1.5. Testing Suite Evidence | Al final, después de "Jhoan Janampa — Landing Page". |
| [docs/6.2.1.8-software-deployment-evidence.md](docs/6.2.1.8-software-deployment-evidence.md) | 6.2.1.8. Software Deployment Evidence | Al final, después de "Deployment del Landing Page". |
| [docs/6.2.1.2-6.2.1.3-filas-para-pedro.md](docs/6.2.1.2-6.2.1.3-filas-para-pedro.md) | 6.2.1.2 y 6.2.1.3 | Filas para pasarle a Pedro, que mantiene esas tablas y Jira. |

Imágenes:

- `assets/img/chapter-6/testing/`: capturas de GitHub Actions (core-api y edge-api) y del reporte HTML de Cucumber.
- `assets/img/chapter-4/...`: copia del Deployment Diagram del Cap. IV, solo para que se vea aquí; en el informe ya existe con la misma ruta.

## Revisar antes de pasarlo

1. **Gherkin en inglés.** El enunciado, en la sección Source Code Style Guide & Conventions, dice: *"Gherkin para los archivos .feature. Para todos los lenguajes debe aplicar la nomenclatura en inglés"*. Los `.feature` están en español (`# language: es`). Si el equipo lo interpreta como contenido en inglés, hay que traducir los 10 `.feature` y sus pasos y regenerar 6.2.1.5.
2. **Horas del Sprint Backlog.** Son estimaciones; ajústalas a tu dedicación real.
3. **Pruebas no documentadas por nadie en 6.2.1.5.** La sección tiene las pruebas de Environmental Monitoring (Camilla) y del Landing (Jhoan), pero no las de IAM (Pedro: `IamApiIntegrationTest`, 8 pruebas), Traceability (Diego S: `DomainModelTest` y `TraceabilityApiIntegrationTest`, 26 pruebas) ni Edge Processing (Diego S: `test_edge_processing.py`, 12 pruebas). Avisar para que cada uno agregue su parte.

## Pendientes de otros (avisar al grupo)

- **Pedro:** plataforma de despliegue y URL pública de la API; configuración CORS para `https://machineguard.github.io` (hoy core-api no tiene CORS).
- **Camilla:** `apiBaseUrl` de producción apunta a `/api/v1`, y recargar `/machineguard-web/dashboard` en GitHub Pages da 404 (falta `404.html`, como sí tiene el Landing).
- **Pedro (test inestable):** `IamApiIntegrationTest.loginNormalizesEmailAndTraceabilityIgnoresCallerSuppliedIdentityHeaders` altera solo el último carácter del JWT; por cómo funciona Base64, a veces ese cambio no modifica la firma y el test falla al azar. Se corrige alterando un carácter del medio del token.
- **Integración:** cuando `feature/environmental-monitoring` (core-api) se integre a `develop`, hay que volver a correr las pruebas BDD y, si agrega tablas, añadirlas a la limpieza de `CommonSteps.java`.

## Pruebas (referencia)

- Ramas: `feature/testing-bdd-acceptance` en `machineguard-core-api` y `machineguard-edge-api` (sin PR abierto todavía).
- core-api: 65 de 65 pruebas en esa rama (34 existentes y 31 escenarios BDD).
- edge-api: 27 de 27 pruebas (12 existentes y 15 escenarios BDD).
