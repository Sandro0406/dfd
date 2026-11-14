
## 7.3. Continuous Deployment

En KairoLabs, el despliegue continuo (*Continuous Deployment*) extiende la entrega continua descrita en la sección 7.2: todo cambio que se integra en la rama de producción de un repositorio, y que ya superó las validaciones previas (pruebas unitarias, de integración, escenarios BDD y análisis estático), se publica automáticamente en el entorno de producción, sin pasos manuales de compilación, copia o subida de archivos.

Cada producto del ecosistema —**Landing Page**, **Web Application** y **RESTful API**— tiene su propio pipeline y se despliega de forma independiente. Así, una mejora visual en la Landing Page no obliga a redesplegar la API, y una corrección en la API no interrumpe la Web Application. Este enfoque es especialmente importante para el dominio del producto: los almacenes farmacéuticos, farmacias y operadores logísticos que usan KairoLabs para vigilar la cadena de frío de medicamentos necesitan que las correcciones lleguen a producción en minutos y que, ante un error, sea posible volver de inmediato a la versión anterior.

### 7.3.1. Tools and Practices.

Para llevar a cabo el proceso de gestión de versiones en Git, el equipo utiliza la terminal de comandos (Git CLI) y el modelo de ramificación **GitFlow**: las funcionalidades se desarrollan en ramas `feature/*`, se integran en `develop` y llegan a la rama de producción —`main` en la Landing Page y en la RESTful API, `master` en la Web Application— a través de ramas `release/*` o `hotfix/*`. Los mensajes de commit siguen **Conventional Commits** y las versiones se nombran con **Semantic Versioning**, lo que permite vincular cada despliegue con el cambio exacto que lo originó. Como la rama de producción es el único punto que dispara un despliegue, GitFlow funciona como la compuerta de calidad del pipeline: solo llega a `main`/`master` código que ya fue revisado e integrado en `develop`.

Para automatizar el despliegue, el equipo no administra servidores propios: utiliza las integraciones nativas con GitHub de **Vercel** (Landing Page y Web Application) y **Render** (RESTful API), que detectan cada nuevo commit en la rama de producción y ejecutan automáticamente la construcción y la publicación del producto correspondiente. La Tabla 7.3-1 resume las herramientas que intervienen en este proceso.

**Tabla 7.3-1**

*Herramientas utilizadas en el proceso de despliegue continuo de KairoLabs*

| Herramienta | Categoría | Uso en KairoLabs |
| :--- | :--- | :--- |
| **Git (CLI) + GitFlow** | Control de versiones | Gestión local de ramas y commits; flujo `feature/*` → `develop` → `release/*` o `hotfix/*` → `main`/`master`. |
| **GitHub** | Repositorio remoto (SCM) | Organización `1ASI0732-2620-9082-Aether-System` con los repositorios KairoLabs-Landing-Page, KairoLabs-Frontend, KairoLabs-Mobile, KairoLabs-Platform y KairoLabs-Project-Report. Origen de los eventos que disparan los despliegues. |
| **Vercel** | Plataforma de despliegue (frontend) | Despliegue automático de la Landing Page y la Web Application al integrar cambios en la rama de producción; publicación en su Edge Network (CDN) con HTTPS; *Preview Deployments* para ramas que no son de producción. |
| **Vite + npm** | Build del frontend | `vite build` genera el bundle optimizado (`dist/`) de la SPA en Vue 3; npm resuelve las dependencias a partir de `package-lock.json`. |
| **Docker** | Empaquetado del backend | Dockerfile multi-stage: imagen `sdk:10.0` para restaurar, compilar y publicar; imagen `aspnet:10.0` para ejecutar la API. |
| **Render** | Plataforma de despliegue (backend) | Web Service con runtime Docker y *Auto-Deploy* ante cada commit en la rama conectada; ejecuta la API ASP.NET Core (Kestrel) en el puerto 8080. |
| **Filess.io** | Base de datos gestionada | Instancia MySQL de producción; la cadena de conexión se inyecta como variable de entorno en Render. |
| **Swagger UI (OpenAPI 3.0)** | Verificación post-despliegue | `/swagger/index.html` permite comprobar que los endpoints de la API responden después de cada despliegue. |
| **UptimeRobot y Vercel Analytics** | Monitoreo | Detectan caídas o degradación de la versión recién desplegada (detalle en la sección de Continuous Monitoring). |

*Nota.* Elaboración propia con base en los repositorios de la organización Aether-System y la configuración de despliegue del Capítulo V.

Las prácticas de despliegue continuo adoptadas por el equipo son las siguientes:

* **Despliegue disparado por Git:** el push o merge en `main`/`master` es el único evento que publica en producción. No existen despliegues manuales por FTP ni compilaciones hechas en máquinas locales.
* **Despliegues independientes por producto:** la Landing Page, la Web Application y la RESTful API tienen pipelines separados, de modo que un fallo en uno no bloquea a los demás.
* **Builds reproducibles y artefactos inmutables:** cada despliegue en Vercel produce una versión inmutable con URL propia; en Render, cada despliegue construye una nueva imagen Docker a partir del commit. Las versiones de las dependencias están fijadas en `package-lock.json` y en el archivo `.csproj`.
* **Configuración externalizada por entorno:** las URLs y credenciales no se escriben en el código. La Web Application usa `.env.development` para el entorno local y `.env.production` para producción (`VITE_API_BASE_URL` apunta a la API en Render); la API recibe `ASPNETCORE_ENVIRONMENT` y `ConnectionStrings__DefaultConnection` desde las variables de entorno de Render.
* **Previews antes de producción:** Vercel genera *Preview Deployments* para las ramas que no son de producción, lo que permite revisar cambios visuales con una URL temporal antes del merge.
* **Verificación post-despliegue (smoke test):** tras cada despliegue se comprueba la carga de la Landing Page, el inicio de sesión en la Web Application y la respuesta de Swagger UI; el monitoreo continuo confirma luego la disponibilidad.
* **Rollback inmediato:** si una versión falla, se recupera la anterior con *Instant Rollback* en Vercel o redesplegando el deploy previo en Render. La corrección definitiva se integra mediante una rama `hotfix/*` o un `git revert`, sin reescribir el historial.
* **Trazabilidad:** cada despliegue queda asociado a su commit (SHA y mensaje Conventional Commit), y Vercel reporta el estado del despliegue en el propio commit de GitHub.

### 7.3.2. Production Deployment Pipeline Components.

El pipeline de despliegue a producción de KairoLabs integra los componentes que se describen a continuación. La Figura 7.3-1 muestra cómo se articulan desde el commit del desarrollador hasta la verificación en producción.

**Figura 7.3-1**

*Pipeline de despliegue continuo a producción de KairoLabs*

<p align="center">
  <img src="../assets/chapter-7/figures/CD-PRODUCTION-PIPELINE.png" alt="Pipeline de despliegue continuo a producción de KairoLabs" width="1000">
</p>

*Nota.* Cada producto se despliega de forma independiente cuando se integra un cambio en su rama de producción. El carril superior corresponde a Vercel y el inferior a Render. Elaboración propia.

Los componentes del pipeline son los siguientes:

* **Source Control Management:** Git y GitHub, bajo el modelo GitFlow. Las ramas de producción son `main` (Landing Page y RESTful API) y `master` (Web Application).
* **Build and compilation:** Vite (`npm run build` → `vite build`) para la Web Application; .NET SDK 10 (`dotnet restore`, `dotnet build` y `dotnet publish -c Release`) dentro del Dockerfile para la RESTful API. La Landing Page es un sitio estático y no requiere compilación.
* **Artifact repository:** registro de npm (dependencias del frontend), NuGet (paquetes .NET), Microsoft Container Registry (imágenes base `sdk:10.0` y `aspnet:10.0`), almacenamiento inmutable de deployments de Vercel y registro interno de imágenes de Render.
* **Deployment automation:** integración Git de Vercel y *Auto-Deploy* de Render, activados por cada commit en la rama de producción.
* **Production environment:** Vercel Edge Network (Landing Page y Web Application), Render Web Service con runtime Docker (RESTful API) y Filess.io (base de datos MySQL).
* **Configuration management:** variables de entorno de Vercel y Render, archivos `.env.*` y `vercel.json` (sitio estático en la Landing Page; regla de reescritura SPA en la Web Application).
* **Verification and monitoring:** smoke test manual, Swagger UI, UptimeRobot y Vercel Analytics.
* **Rollback and recovery:** *Instant Rollback* de Vercel, redeploy de la versión anterior en Render y `git revert` o `hotfix/*` en el repositorio.

**Tabla 7.3-2**

*Configuración del pipeline de producción por producto*

| Producto | Repositorio · rama | Build | Artefacto | Destino |
| :--- | :--- | :--- | :--- | :--- |
| **Landing Page** | KairoLabs-Landing-Page · `main` | Sin compilación; `vercel.json` con `outputDirectory: "."` y `cleanUrls: true` | Archivos estáticos (HTML, CSS, JS, imágenes) | Vercel<br>[landing-page-kairolabs.vercel.app](https://landing-page-kairolabs.vercel.app) |
| **Web Application** | KairoLabs-Frontend · `master` | `npm install` + `npm run build` (`vite build`) | Bundle `dist/` + reescritura SPA hacia `/index.html` | Vercel<br>[kairo-labs-frontend.vercel.app](https://kairo-labs-frontend.vercel.app) |
| **RESTful API** | KairoLabs-Platform · `main` | Docker multi-stage (.NET SDK 10, configuración Release) | Imagen Docker `aspnet:10.0` (puerto 8080) | Render (Docker)<br>[medi-track-sensor-platform.onrender.com](https://medi-track-sensor-platform.onrender.com/swagger/index.html) |
| **Base de datos** | — | — | — | Filess.io (MySQL), conexión mediante variable de entorno |

*Nota.* Elaboración propia con base en los archivos `vercel.json`, `package.json` y `Dockerfile` de cada repositorio.

**Tabla 7.3-3**

*Etapas del pipeline de despliegue a producción*

| Etapa | Descripción | Herramienta | Criterio de salida |
| :--- | :--- | :--- | :--- |
| **1. Integración** | Merge de `release/*` o `hotfix/*` en `main`/`master`. | Git, GitHub | Pruebas de 7.1 y 7.2 aprobadas; commit con mensaje Conventional Commit. |
| **2. Disparo** | GitHub notifica el nuevo commit a la plataforma de despliegue. | GitHub → Vercel / Render | Despliegue encolado con el SHA del commit. |
| **3. Build** | Instalación de dependencias y compilación. | npm + Vite / Docker + .NET SDK 10 | Build sin errores. Si falla, el pipeline se detiene y la versión anterior sigue activa. |
| **4. Empaquetado** | Generación del artefacto inmutable. | Output de Vercel / imagen Docker | Artefacto asociado al commit. |
| **5. Release** | Publicación y asignación del dominio de producción. | Vercel Edge Network / Render | Estado *Ready* (Vercel) o *Live* (Render). |
| **6. Verificación** | Smoke test y monitoreo de la nueva versión. | Navegador, Swagger UI, UptimeRobot | Landing Page, inicio de sesión y endpoints responden correctamente. |
| **7. Recuperación** | Reversión ante un fallo en producción. | Instant Rollback / redeploy / `git revert` | Servicio restablecido en la última versión estable. |

*Nota.* Elaboración propia.

---

## 7.4. Continuous deployment

Vistazo general de los pipelines utilizados en KairoLabs:

El ecosistema opera con dos tipos de pipelines. Los de tipo **Test** validan el código antes de que llegue a la rama de producción; los de tipo **Deploy** publican automáticamente cada producto cuando ese código se integra. La Tabla 7.4-1 resume ambos grupos con su último resultado registrado.

**Tabla 7.4-1**

*Pipelines de tipo Test y Deploy de KairoLabs*

| Pipeline | Tipo | Producto | Disparador / comando | Último resultado |
| :--- | :---: | :--- | :--- | :--- |
| **Análisis estático** | Test | Web Application | `npm run lint` y `npm run quality` (ESLint, jscpd, npm audit) | Ejecutado; 77 advertencias de ESLint en seguimiento (02/11/2026) |
| **Pruebas unitarias** | Test | Web Application | `npm run test:unit` (`node:test`) | 20/20 aprobadas (02/11/2026) |
| **Pruebas de integración** | Test | RESTful API | `npm test` (Node.js + Kestrel + MySQL de pruebas) | 14/14 aprobadas (02/11/2026) |
| **BDD / sistema** | Test | Web App + API + MySQL | `npm run test:bdd` (Cucumber.js + Playwright) | 4/4 escenarios y 22/22 pasos en escritorio y móvil emulado (02/11/2026) |
| **Deploy Landing Page** | Deploy | Landing Page | Push a `main` → Vercel | *Production*: commit `34434e8` (14/09/2026) |
| **Deploy Web Application** | Deploy | Web Application | Push a `master` → Vercel | *Production*: commit `ffadea8` (14/09/2026); estado ✓ en el último commit (Figura 7.4-4) |
| **Deploy RESTful API** | Deploy | RESTful API | Commit en la rama conectada → Render (Docker) | *Live* en medi-track-sensor-platform.onrender.com (Figura 7.4-5) |

*Nota.* Los pipelines de tipo Test se ejecutan mediante scripts npm reproducibles antes de integrar los cambios en la rama de producción; los de tipo Deploy se ejecutan automáticamente en Vercel y Render. Resultados tomados de las evidencias del Capítulo VI y del Capítulo V. Elaboración propia.

**Veamos un pipeline de tipo Test:**

El pipeline de pruebas de la Web Application encadena cuatro etapas que se ejecutan desde la raíz del repositorio KairoLabs-Frontend. Si cualquiera de ellas falla, el cambio no se integra en `master` y, por lo tanto, no se dispara ningún despliegue.

1. **Lint** — `npm run lint`: ESLint con las reglas de Vue y de seguridad (`eslint-plugin-vue`, `eslint-plugin-security`) sobre la carpeta `src/`.
2. **Unit** — `npm run test:unit`: 20 pruebas de las entidades principales del dominio con el runner nativo de Node.js (`node:test`).
3. **BDD / sistema** — `npm run test:bdd`: el runner crea una base MySQL temporal, levanta la API ASP.NET Core y la Web Application en local, ejecuta los escenarios Gherkin de `bdd/core.feature` con Playwright (Chromium) en escritorio y en un Pixel 7 emulado, y elimina los recursos al terminar.
4. **Calidad** — `npm run quality`: métricas de complejidad, duplicación de código (jscpd) y auditoría de dependencias.

**Figura 7.4-1**

*Resultado del pipeline de tipo Test (BDD) en escritorio*

<p align="center">
  <img src="../assets/chapter-6/evidence/bdd/cucumber-desktop.png" alt="Reporte Cucumber.js: 4 de 4 escenarios aprobados" width="800">
</p>

*Nota.* Reporte HTML de Cucumber.js: 4 de 4 escenarios aprobados en 22 segundos. Captura del Capítulo VI, 2 de noviembre de 2026.

**Veamos un pipeline de tipo Deploy:**

El pipeline de despliegue de la Web Application se ejecuta en Vercel cada vez que se integra un commit en `master`. Sus etapas son:

1. **Cloning** — Vercel clona KairoLabs-Frontend en el commit exacto que disparó el despliegue.
2. **Installing** — instala las dependencias con npm a partir de `package-lock.json`.
3. **Building** — ejecuta `npm run build` (`vite build`); Vite carga las variables de `.env.production`, entre ellas `VITE_API_BASE_URL`, que apunta a la API en Render.
4. **Deploying** — publica la carpeta `dist/` en la Edge Network y aplica la regla de `vercel.json` que redirige toda ruta a `/index.html` (enrutamiento SPA).
5. **Ready** — asigna el dominio de producción kairo-labs-frontend.vercel.app y reporta el estado del despliegue en el commit de GitHub.

**Figura 7.4-2**

*Historial de despliegues de producción de la Web Application en Vercel*

> **[Insertar captura]** Pestaña *Deployments* del proyecto kairo-labs-frontend en Vercel, con los despliegues en estado *Ready* y el commit que originó cada uno.

*Nota.* Elaboración propia.

El pipeline de la RESTful API sigue la misma lógica en Render: clona el repositorio, construye la imagen con el Dockerfile multi-stage (restore, build y publish en configuración Release sobre .NET SDK 10), arranca un contenedor `aspnet:10.0` que expone el puerto 8080 y, cuando el servicio queda disponible, lo marca como *Live* y dirige el tráfico hacia la nueva versión.

**Figura 7.4-3**

*Registro de despliegue del Web Service de la RESTful API en Render*

> **[Insertar captura]** Pestaña *Events* o *Logs* del servicio medi-track-sensor-platform en Render, mostrando las etapas de build de Docker y el estado *Live*.

*Nota.* Elaboración propia.

Un caso real muestra el papel del build como compuerta de calidad: el 14/09/2026, un carácter sobrante en `vite.config.js` rompió el build de la Web Application en Vercel. El despliegue fallido no reemplazó la versión en producción, y la corrección (`fix: remove stray character breaking Vite config on Vercel build`) se publicó automáticamente con el siguiente push, como muestra el check de estado de la Figura 7.4-4.

**Figura 7.4-4**

*Commit de corrección con despliegue exitoso reportado en GitHub*

<p align="center">
  <img src="../assets/chapter-5/gh-frontend-repo.png" alt="Repositorio KairoLabs-Frontend con check de despliegue exitoso" width="900">
</p>

*Nota.* Repositorio KairoLabs-Frontend en la rama `master`; el check verde junto al último commit indica que el despliegue se completó. Captura del Capítulo V.

Por último, la verificación post-despliegue de la RESTful API se realiza sobre Swagger UI, servido directamente desde el entorno de producción en Render.

**Figura 7.4-5**

*Verificación post-despliegue de la RESTful API en Render*

<p align="center">
  <img src="../assets/chapter-5/api-swagger-overview.png" alt="Swagger UI de MediTrack Sensor API v1 en Render" width="900">
</p>

*Nota.* Swagger UI de MediTrack Sensor API v1 (OAS 3.0) con el servidor https://medi-track-sensor-platform.onrender.com — Render (producción). Captura del Capítulo V.

Como mejora planificada, el equipo migrará los pipelines de tipo Test a **GitHub Actions** para que se ejecuten automáticamente en cada Pull Request hacia `develop` y `main`/`master`, y condicionará el despliegue a que todas las pruebas resulten aprobadas.
