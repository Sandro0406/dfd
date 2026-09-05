# Capítulo II: Requirements Elicitation & Analysis

El presente capítulo documenta el proceso de elicitación y análisis de requisitos de MachineGuard, la plataforma SaaS + IoT de monitoreo ambiental para PyMEs de manufactura y almacenaje de Lima descrita en el Capítulo I. El trabajo parte de dos frentes complementarios: por un lado, un análisis de la competencia que permite contrastar las suposiciones iniciales del equipo con la oferta realmente disponible en el mercado peruano; por el otro, un proceso de Needfinding sustentado en entrevistas a representantes de los segmentos objetivo, registradas en video.

Los hallazgos de ambos frentes convergen en la construcción de los artefactos de investigación de usuario —User Personas, User Task Matrix, User Journey Maps (As-Is) y Empathy Maps—, en la exploración del dominio de negocio mediante Big Picture EventStorming y, finalmente, en la formalización de un Ubiquitous Language que elimina la ambigüedad terminológica entre los miembros del equipo y los stakeholders. Estos artefactos constituyen la entrada directa para la especificación de requisitos del Capítulo III.

**Segmentos objetivo considerados en todo el capítulo.** De acuerdo con lo establecido en la sección 1.3, y en línea con los dos segmentos que el equipo definió al delimitar el alcance del proyecto precisamente para someterlos a validación en este proceso de Needfinding, MachineGuard se dirige a: **(1) Jefes de Almacén y Gerentes de Operaciones** —categoría que comprende también a los supervisores de almacén— de PyMEs de manufactura y almacenaje, responsables de custodiar insumos y productos sensibles a la humedad y la temperatura, y que necesitan enterarse con rapidez cuando una condición sale de rango; y **(2) Encargados de Control de Calidad** de esas mismas empresas, responsables de evidenciar la trazabilidad de las condiciones ambientales ante auditorías internas y externas y ante procesos de certificación.

---

## 2.1. Competidores

MachineGuard compite en el mercado de plataformas de monitoreo ambiental remoto (temperatura y humedad relativa) basadas en dispositivos IoT conectados a un servicio en la nube. Se identificaron tres competidores directos cuyo modelo de negocio se sustenta en productos digitales equivalentes —hardware sensor más plataforma web y aplicación móvil bajo suscripción— y que resultan accesibles para una PyME peruana, ya sea por venta directa en línea o a través de distribuidores locales.

**UbiBot.** Marca de UbiBot Ltd. especializada en sensores inalámbricos autónomos de temperatura, humedad, luz y vibración (líneas WS1, WS1 Pro y GS1) con conectividad WiFi, 4G, LoRa o Ethernet. Los dispositivos se vinculan a la plataforma en la nube UbiBot IoT Platform, que ofrece dashboard web, aplicación móvil iOS/Android, alertas por correo, app y canales tipo IFTTT, exportación de datos y generación de reportes en PDF. Su modelo comercial es de compra única del hardware con un plan gratuito de plataforma (con cuotas de almacenamiento y tráfico) y planes de pago para mayor volumen, más usuarios y retención extendida del histórico. Es el competidor más cercano a MachineGuard en la variable precio.

**Monnit (iMonnit).** Fabricante estadounidense de la línea ALTA de sensores inalámbricos de largo alcance (banda sub-GHz) que se comunican con un gateway propietario y de allí con la plataforma en la nube iMonnit. Ofrece sensores de temperatura y humedad con certificado de calibración NIST, reglas de alerta configurables, notificaciones por SMS/correo/llamada y una API para integración. Su modelo combina venta de hardware (sensores + gateway) con suscripción a iMonnit por niveles de servicio. Representa la alternativa de gama profesional accesible sin llegar al costo de un SCADA.

**Testo Saveris (Testo AG).** Sistema profesional alemán de monitoreo continuo de temperatura y humedad orientado a industria alimentaria, farmacéutica, laboratorios y cadena de frío. Combina sondas radio/Ethernet, una base de datos central y software de trazabilidad (incluida la variante validable para entornos regulados). Se comercializa en Perú mediante distribuidores autorizados, con servicios de instalación, calibración certificada y mantenimiento. Es el referente de precisión y cumplimiento normativo del sector, y por lo tanto el competidor que define el techo de expectativas de los Encargados de Control de Calidad.

**Competidores indirectos.** Además de los anteriores, el equipo identificó cuatro alternativas que resuelven parcialmente la misma necesidad y que, en la práctica, son las que la PyME evalúa primero:

- **Dataloggers autónomos** (Elitech, Novus, Kusitest y equivalentes comercializados en Lima): registran temperatura y humedad de forma continua a bajo costo, pero exigen descarga manual del archivo, no notifican en tiempo real y no exponen los datos a otros sistemas.
- **Integradores locales de facility management y BMS** (Tgestiona, Integrity Perú, LAIN Holdings y similares): entregan proyectos a medida con levantamiento técnico e integración al BMS del cliente; son solventes técnicamente, pero manejan un ticket elevado y ciclos de venta largos que expulsan a la pequeña empresa.
- **Sistemas SCADA industriales** (Siemens, Schneider EcoStruxure y equivalentes): el sustituto de alta gama que cubre el problema por completo, pero cuya inversión inicial y costo de mantenimiento están fuera del alcance del segmento objetivo, tal como se sustentó en la sección 1.2.1.
- **La "no solución": rondas manuales.** El termohigrómetro portátil combinado con una planilla en papel u hoja de cálculo sigue siendo la alternativa dominante en el segmento. Es gratuita en apariencia, está profundamente arraigada en el procedimiento operativo y, por lo tanto, constituye el competidor real contra el que MachineGuard debe demostrar valor.

---

### 2.1.1. Análisis competitivo

A continuación se presenta el Competitive Analysis Landscape elaborado por el equipo. El ejercicio permitió contrastar la idea inicial que se tenía de la competencia —"no existe una oferta accesible para la PyME"— con la realidad del mercado: sí existen soluciones de bajo costo, pero ninguna resuelve simultáneamente el precio de entrada, la continuidad de la medición ante caídas de conexión y la integración con el ERP que el cliente ya opera.

**¿Por qué llevar a cabo este análisis?** Determinar con evidencia si las soluciones de monitoreo ambiental ya disponibles para una PyME peruana cubren las tres condiciones que MachineGuard considera críticas —costo de entrada por punto de monitoreo, continuidad de la medición ante pérdida de conectividad e integración con el ERP existente del cliente— con el fin de identificar el espacio real de diferenciación y ajustar la propuesta de valor antes de comprometer el desarrollo.

| Categoría | Aspecto | MachineGuard | UbiBot | Monnit (iMonnit) | Testo Saveris |
|---|---|---|---|---|---|
| **Perfil** | Overview | Plataforma SaaS + IoT peruana para monitoreo continuo de temperatura y humedad en almacenes y plantas de PyMEs.<br><br>Nodos ESP32 + DHT22 de bajo costo, un Edge Service que calibra y filtra localmente, una API REST central que evalúa umbrales y una Web App y Mobile App para alertas e historial.<br><br>Expone además una API pública para que el ERP del cliente consuma mediciones y alertas sin reemplazar sus sistemas. | Sensores inalámbricos autónomos (WS1, WS1 Pro, GS1) con WiFi, 4G, LoRa o Ethernet, vinculados a la plataforma en la nube UbiBot.<br><br>Enfoque "plug and play": el usuario configura el dispositivo desde la app y comienza a medir en minutos, sin instalador.<br><br>Cobertura funcional amplia (temperatura, humedad, luz, vibración) con reportes y exportación de datos. | Ecosistema de sensores inalámbricos ALTA de largo alcance que reportan a un gateway propietario y de allí a la plataforma iMonnit.<br><br>Orientado a monitoreo remoto de instalaciones, equipos e infraestructura crítica en el mercado norteamericano.<br><br>Sensores con certificado de calibración NIST y reglas de notificación configurables. | Sistema profesional de monitoreo y documentación de temperatura y humedad para industria alimentaria, farmacéutica y laboratorios.<br><br>Sondas radio/Ethernet, base de datos central y software de trazabilidad con soporte para entornos regulados.<br><br>Se entrega como proyecto llave en mano con instalación, calibración certificada y mantenimiento. |
| **Perfil de Marketing** | Ventaja competitiva: ¿Qué valor ofrece a los clientes? | Monitoreo continuo a un costo de entrada que la PyME puede aprobar sin comité de inversiones (S/ 35–50 por punto más suscripción mensual).<br><br>Continuidad de la medición aunque se caiga el internet, gracias al buffer del Edge Service.<br><br>Integración abierta: el dato llega al ERP que el cliente ya usa, en lugar de obligarlo a mirar un sistema más. | Precio bajo por dispositivo y ausencia de suscripción obligatoria para volúmenes pequeños.<br><br>Autonomía total del usuario: compra en línea, configura y opera sin intervención de terceros. | Confiabilidad y alcance: cobertura de naves e instalaciones amplias sin depender del WiFi del cliente.<br><br>Trazabilidad respaldada por certificados de calibración y un catálogo de sensores muy extenso. | Precisión metrológica certificada y evidencia documental aceptada por auditores y entes reguladores.<br><br>Respaldo de marca, servicio técnico y recalibración periódica. |
| **Perfil de Marketing** | Mercado objetivo | PyMEs de manufactura y almacenaje de Lima y provincias (10–100 trabajadores) sin presupuesto para SCADA.<br><br>Almacenes de insumos, centros de acopio y plantas de producción de pequeña y mediana escala. | Mercado global de consumo y pequeña empresa: hogares, invernaderos, servidores, tiendas, laboratorios pequeños.<br><br>Compra individual o por lotes reducidos, sin segmentación vertical fuerte. | Mediana y gran empresa de Norteamérica y Europa: facilities, retail, agricultura, salud e industria.<br><br>Clientes con equipo de TI o mantenimiento propio capaz de desplegar gateways. | Empresas reguladas de alimentos, farmacia, salud y logística de frío que deben demostrar cumplimiento (HACCP, buenas prácticas de almacenamiento, cadena de frío).<br><br>Predominantemente mediana y gran empresa. |
| **Perfil de Marketing** | Estrategias de marketing | Landing Page con calculadora de pérdidas evitadas y llamados a la acción diferenciados por segmento.<br><br>Marketing de contenidos en LinkedIn y grupos de logística y calidad del Perú.<br><br>Piloto gratuito de 30 días con dos puntos de monitoreo instalados.<br><br>Alianzas con asociaciones de PyMEs, cámaras de comercio y proveedores de ERP locales. | Venta en línea directa y a través de marketplaces internacionales.<br><br>Posicionamiento SEO por palabras clave de producto y comparativas.<br><br>Reseñas de usuarios y demostraciones en video. | Venta consultiva y red de distribuidores e integradores.<br><br>Casos de éxito por industria, webinars y material técnico descargable.<br><br>Presencia en ferias sectoriales. | Venta técnica mediante distribuidores autorizados.<br><br>Contenido de cumplimiento normativo, guías HACCP y capacitaciones.<br><br>Participación en ferias de alimentos, farmacéutica y metrología. |
| **Perfil de Producto** | Productos y servicios | Nodos de monitoreo ESP32 + DHT22 (Embedded App).<br>Edge API que calibra, filtra y almacena localmente las lecturas.<br>RESTful API central de usuarios, plantas, zonas, umbrales, alertas e historial.<br>Web App de dashboard en tiempo real e historial.<br>Mobile App con alertas push.<br>API pública para integración con el ERP del cliente y servicio externo de terceros para notificaciones y contexto climático.<br>Landing Page informativa. | Sensores multivariable con pantalla y batería.<br>Plataforma cloud con dashboard, alertas, exportación y reportes.<br>Apps iOS y Android.<br>API y automatizaciones tipo IFTTT. | Catálogo amplio de sensores inalámbricos ALTA.<br>Gateways ethernet y celulares.<br>Plataforma iMonnit con reglas, notificaciones y reportes.<br>API para integración y servicios de calibración NIST. | Sondas de temperatura y humedad radio y Ethernet.<br>Base de datos y software de análisis y documentación.<br>Servicios de instalación, mapeo térmico, calibración y validación.<br>Alarmas por correo, SMS y relé. |
| **Perfil de Producto** | Precios y costos | S/ 35–50 por punto de monitoreo (nodo ESP32 + DHT22) como costo de hardware.<br><br>Suscripción mensual al servicio SaaS escalonada por número de puntos y retención de histórico.<br><br>Sin costo de gateway propietario: el Edge Service corre sobre un equipo de bajo costo por sede.<br><br>Instalación realizada por el propio personal del cliente con guía asistida. | Costo medio-bajo por dispositivo, con compra única.<br><br>Plan de plataforma gratuito con cuotas de almacenamiento y tráfico; planes de pago para mayor volumen y retención.<br><br>Costos adicionales de importación, flete y garantía internacional para el comprador peruano. | Costo por sensor superior al de la gama de consumo, más la inversión obligatoria en gateway.<br><br>Suscripción a iMonnit por niveles de servicio.<br><br>Sin presencia comercial directa en Perú: importación, aranceles y soporte remoto. | Inversión de sistema significativamente alta: sondas, base, software y servicio de puesta en marcha.<br><br>Costos recurrentes de recalibración y mantenimiento.<br><br>Fuera del alcance presupuestal de la PyME objetivo. |
| **Perfil de Producto** | Canales de distribución (Web y/o Móvil) | Landing Page propia.<br>Web App responsiva (Angular).<br>Mobile App Android/iOS con notificaciones push.<br>API pública consumida desde el ERP del cliente.<br>Venta directa y alianzas locales. | Sitio web propio y tienda en línea.<br>Marketplaces internacionales.<br>Apps iOS y Android. | Sitio web propio.<br>Red de distribuidores e integradores.<br>Portal web iMonnit y app móvil. | Sitio web corporativo y distribuidores autorizados en Perú.<br>Software de escritorio y acceso web.<br>Fuerza de venta técnica presencial. |
| **Análisis FODA** | Fortalezas | Costo de entrada por punto sensiblemente menor al de cualquier alternativa importada.<br>Edge Computing propio: calibra, filtra y conserva las lecturas ante caídas de conexión.<br>API pública pensada desde el inicio para integrarse al ERP del cliente.<br>Equipo local: soporte en español, en horario de Perú y con visita presencial posible.<br>Conocimiento directo del contexto operativo de la PyME limeña. | Precio de hardware muy competitivo.<br>Plataforma madura y probada, con app móvil consolidada.<br>Instalación inmediata sin técnico.<br>Multivariable en un solo dispositivo. | Alcance de radio superior al WiFi convencional.<br>Certificación NIST que respalda la medición.<br>Catálogo de sensores muy amplio.<br>Plataforma escalable con API documentada. | Precisión certificada y prestigio de marca.<br>Evidencia documental aceptada en auditorías reguladas.<br>Servicio integral: instalación, calibración y mantenimiento.<br>Distribución establecida en Perú. |
| **Análisis FODA** | Debilidades | Marca nueva y sin historial: genera desconfianza inicial frente a fabricantes establecidos.<br>Sensores DHT22 con menor exactitud que instrumentos certificados; no aptos por sí solos para entornos regulados exigentes.<br>Sin certificado de calibración trazable en la primera versión.<br>Equipo reducido: capacidad limitada de soporte y de despliegue simultáneo.<br>Dependencia de servicios de terceros para notificaciones. | Sin procesamiento en el borde: si se cae la conexión, el histórico depende del almacenamiento del propio dispositivo.<br>Soporte y garantía desde el exterior; tiempos de reposición largos para el cliente peruano.<br>Integración con ERP no guiada: queda a cargo del cliente.<br>Orientación de consumo, no industrial. | Costo por punto elevado para la PyME peruana.<br>Dependencia de un gateway propietario que encarece el despliegue mínimo.<br>Sin canal ni soporte local en Perú.<br>Interfaz y documentación centradas en el mercado anglosajón. | Precio prohibitivo para el segmento objetivo de MachineGuard.<br>Puesta en marcha dependiente de personal especializado.<br>Ciclo de venta e implementación largo.<br>Rigidez: sobredimensionado para un almacén de insumos pequeño. |
| **Análisis FODA** | Oportunidades | Segmento PyME desatendido y numeroso en Lima y provincias.<br>Presión creciente de clientes y auditores por evidencia de condiciones de almacenamiento.<br>Alianzas con proveedores de ERP locales para ofrecer el monitoreo como módulo complementario.<br>Ampliación natural a otras variables (puerta abierta, energía, CO₂) sobre el mismo nodo.<br>Expansión regional a mercados latinoamericanos con la misma brecha de precio. | Crecimiento del mercado de monitoreo doméstico y de pequeña empresa.<br>Entrada a verticales específicas mediante integradores locales. | Expansión hacia mercados emergentes vía distribuidores.<br>Crecimiento del monitoreo de infraestructura crítica y mantenimiento predictivo. | Endurecimiento de la regulación sanitaria y de cadena de frío.<br>Crecimiento de la agroexportación peruana, que exige trazabilidad certificada. |
| **Análisis FODA** | Amenazas | Un competidor establecido puede lanzar una línea de bajo costo con soporte local.<br>Percepción de "solución hechiza" por usar hardware de bajo costo.<br>Volatilidad del tipo de cambio y de los precios de componentes importados.<br>Resistencia al cambio: el cliente puede seguir prefiriendo la ronda manual por costo cero aparente.<br>Fuga de clientes hacia un fabricante certificado apenas la empresa entre a un mercado regulado. | Presión de fabricantes que ofrecen hardware equivalente aún más barato.<br>Cambios en las condiciones del plan gratuito que erosionen su propuesta. | Competencia de plataformas cloud generalistas con hardware genérico.<br>Barreras arancelarias y logísticas en mercados fuera de su red. | Aparición de alternativas de bajo costo que alcancen precisión suficiente para cumplir la norma.<br>Presión de precio en licitaciones de mediana empresa. |

*Tabla 1. Competitive Analysis Landscape de MachineGuard frente a sus competidores directos.*

**Fuentes consultadas para el análisis:** sitios oficiales de los fabricantes (ubibot.com, monnit.com, testo.com) y relevamiento de proveedores locales de instrumentación y monitoreo ambiental en Lima (Tgestiona, Integrity Perú, LAIN Holdings, Kusitest, Novus Perú), consultados en septiembre de 2026. Los rangos de precio de los competidores se expresan de forma cualitativa porque varían según modelo, cantidad y condiciones de importación; el equipo mantiene el detalle de la cotización en el repositorio del proyecto.

---

### 2.1.2. Estrategias y tácticas frente a competidores

**Delimitación previa del posicionamiento.** Antes de formular las estrategias conviene explicitar una decisión que el equipo adoptó al acotar el alcance del proyecto y que condiciona la lectura de todo el análisis competitivo: MachineGuard se plantea como un servicio SaaS acompañado de hardware por suscripción, comercializable a múltiples clientes, y no como un módulo de gestión interno construido para una sola empresa. De esa decisión se derivan tres límites asumidos de forma deliberada. Primero, no se reemplaza el sistema de gestión del cliente: se le entregan datos y alertas a través de una API pública. Segundo, no se exige hardware industrial de tipo PLC o SCADA, porque eso reintroduciría la barrera de costo que da origen al problema. Y tercero, no se compite por amplitud funcional frente a las plataformas globales consolidadas, sino por accesibilidad económica, facilidad de despliegue e integración con lo que el cliente ya opera.

A partir del FODA elaborado en la sección anterior, el equipo construyó una matriz de estrategias cruzadas que relaciona los factores internos de MachineGuard (fortalezas y debilidades) con los factores del entorno competitivo (oportunidades y amenazas). De esa matriz se desprenden las estrategias y tácticas preliminares que orientan tanto el posicionamiento comercial como las decisiones de alcance del producto.

| | Oportunidades (O) | Amenazas (A) |
|---|---|---|
| **Fortalezas (F)** | **E1 (FO)** — Penetración por costo en el segmento desatendido: usar el precio por punto y el soporte local para capturar PyMEs que hoy ninguna alternativa importada atiende.<br><br>**E2 (FO)** — Alianza de integración con ERP locales: convertir la API pública en un módulo ofrecido junto a proveedores de ERP peruanos. | **E3 (FA)** — Blindaje por continuidad y cercanía: sostener el Edge Computing y el soporte presencial como barrera frente a una eventual línea económica de un fabricante global.<br><br>**E4 (FA)** — Demostración contra la ronda manual: cuantificar en la venta la merma evitada para desplazar al competidor por defecto. |
| **Debilidades (D)** | **E5 (DO)** — Construcción acelerada de confianza: pilotos gratuitos, casos documentados y testimonios que compensen la falta de historial de marca.<br><br>**E6 (DO)** — Hoja de ruta hacia la trazabilidad certificada: incorporar sensores de mayor exactitud y calibración trazable para acompañar al cliente cuando su mercado se regule. | **E7 (DA)** — Contención del riesgo de fuga: retener al cliente con el histórico acumulado y la integración ya operativa en su ERP.<br><br>**E8 (DA)** — Cobertura del riesgo cambiario: diseño con componentes sustituibles y proveedores alternos para no depender de un solo importador. |

*Tabla 2. Matriz de estrategias cruzadas (FODA) de MachineGuard.*

A continuación se detallan las estrategias priorizadas junto con las tácticas concretas que las materializan.

#### Estrategia 1 — Liderazgo en costos dentro de un nicho definido (E1)

MachineGuard no compite por ser la plataforma más completa, sino por ser la única que una PyME de manufactura o almacenaje de Lima puede desplegar sin someter la decisión a un comité de inversiones. El foco deliberado en ese nicho evita la confrontación directa con Testo Saveris en el terreno donde este es imbatible, y con Monnit en el terreno del alcance industrial.

**Tácticas:**

- Publicar precios por punto de monitoreo de forma abierta y transparente en la Landing Page, incluyendo el costo total del primer año.
- Ofrecer un paquete de arranque de dos puntos de monitoreo con instalación asistida remota y sin costo de puesta en marcha.
- Diseñar planes de suscripción escalonados por número de puntos, de modo que el cliente crezca sin renegociar el contrato.
- Mantener el nodo sensor sobre componentes de amplia disponibilidad local para preservar el margen y el precio.

#### Estrategia 2 — Diferenciación por integración abierta (E2)

Ninguno de los tres competidores directos acompaña al cliente peruano en la integración con el sistema de gestión que ya utiliza. MachineGuard convierte esa carencia en su principal argumento: el dato no vive en una isla, llega al ERP donde el jefe de almacén ya toma decisiones.

**Tácticas:**

- Documentar y publicar la API pública con ejemplos de consumo listos para usar y una colección de pruebas descargable.
- Establecer acuerdos con proveedores de ERP y software administrativo del mercado local para ofrecer el monitoreo como módulo complementario.
- Incluir exportación a formatos que el cliente ya maneja (hoja de cálculo y PDF) como paso previo a la integración formal.
- Comunicar el mensaje "se integra con lo que ya tienes" como eje central de la Landing Page y del discurso comercial.

#### Estrategia 3 — Diferenciación técnica por Edge Computing (E3)

En un almacén de Lima la conectividad es intermitente. Que la medición continúe y se sincronice después es una ventaja concreta que los sensores puramente cloud no ofrecen, y que además protege el valor probatorio del histórico frente a auditorías.

**Tácticas:**

- Garantizar y comunicar una ventana mínima de autonomía del Edge Service ante pérdida de conexión, con sincronización posterior sin pérdida de datos.
- Mostrar en el dashboard el estado de conectividad de cada nodo y la última sincronización efectuada, como señal de confianza.
- Demostrar el escenario de corte de internet en la demostración comercial y en el video About-the-Product.

#### Estrategia 4 — Reducción de la fricción de adopción (E1, E5)

El obstáculo principal no es el precio del sensor, sino el miedo del cliente a un proyecto de instalación. La estrategia consiste en que el propio personal del almacén pueda desplegar la solución.

**Tácticas:**

- Entregar el nodo preconfigurado y vincularlo a la zona de monitoreo mediante un flujo guiado desde la aplicación móvil.
- Publicar guías visuales cortas y videos de instalación paso a paso en español.
- Ofrecer acompañamiento remoto durante la primera instalación y una revisión a los treinta días.
- Mantener valores de umbral sugeridos por tipo de producto para que el cliente no parta de una configuración en blanco.

#### Estrategia 5 — Confianza y trazabilidad progresiva (E5, E6, E7)

Frente a Testo Saveris, MachineGuard no puede prometer hoy precisión certificada. La respuesta es construir confianza por otra vía —evidencia continua, historial íntegro y transparencia sobre los límites del instrumento— y trazar una ruta explícita hacia la trazabilidad certificada para acompañar al cliente cuando su propio mercado se lo exija.

**Tácticas:**

- Registrar un historial inalterable de mediciones y alertas, con marca temporal y con el detalle de quién reconoció cada alerta.
- Generar reportes de trazabilidad por zona y período, listos para adjuntar a una auditoría.
- Declarar de forma explícita en la documentación comercial la exactitud del sensor y los usos para los que resulta apropiado.
- Incorporar en la hoja de ruta del producto un nodo de gama superior con calibración trazable para clientes en proceso de certificación.

#### Estrategia 6 — Posicionamiento local y educación del mercado (E4, E5)

El competidor real es la ronda manual. Antes de vender un producto hay que hacer visible un costo que hoy la empresa no mide: la merma silenciosa por condiciones fuera de rango.

**Tácticas:**

- Producir contenido en LinkedIn y en comunidades peruanas de logística y aseguramiento de la calidad sobre pérdidas por humedad y temperatura.
- Incluir en la Landing Page una calculadora que estime la merma anual evitada a partir del valor del inventario del visitante.
- Documentar y difundir casos de clientes piloto con cifras de reducción de tiempo de respuesta ante desviaciones.
- Participar en ferias y eventos de PyMEs industriales de Lima con una demostración física del nodo funcionando.

---

## 2.2. Entrevistas

Esta sección documenta la investigación primaria realizada por el equipo mediante entrevistas semiestructuradas a representantes de los dos segmentos objetivo. El propósito no fue validar la solución que el equipo ya tenía en mente, sino comprender cómo se controla hoy la temperatura y la humedad en un almacén o planta de una PyME, qué cuesta ese control, dónde falla y qué consecuencias reales tiene cuando falla. Toda la información recolectada constituye la base documental de los arquetipos y demás artefactos de Needfinding presentados en la sección 2.3.

---

### 2.2.1. Diseño de entrevistas

El equipo diseñó dos guiones de entrevista, uno por segmento. Ambos comparten la misma estructura —presentación, contexto y situación actual, problema y consecuencias, tecnología y canales, cierre proyectivo— y se diferencian en el foco: el primero indaga en la operación y la continuidad del negocio, el segundo en la trazabilidad y el cumplimiento normativo.

**Buenas prácticas aplicadas en el diseño:**

- Consentimiento informado explícito al inicio: se explica el propósito académico, se solicita autorización para grabar y se ofrece anonimizar la razón social de la empresa.
- Estructura de embudo: se parte de preguntas amplias sobre el día a día y se desciende progresivamente hacia el problema específico, evitando anclar al entrevistado desde el inicio.
- Preguntas abiertas y no inductivas: se evita formular la pregunta de modo que sugiera la respuesta esperada. Se pregunta "¿cómo controla hoy la temperatura del almacén?" en lugar de "¿no le resulta difícil controlar la temperatura manualmente?".
- Prioridad al hecho concreto sobre la opinión: se pide relatar el último episodio real vivido ("cuénteme la última vez que se perdió mercadería por humedad") antes de preguntar por preferencias o hipótesis.
- Postergación de las preguntas sobre la solución: las preguntas proyectivas sobre una herramienta digital se ubican al final, para no contaminar el relato del problema.
- Registro de características demográficas y actitudinales necesarias para los arquetipos: edad, distrito de residencia, situación familiar, ocupación y antigüedad en el cargo, personalidad, marcas e influencias, dispositivos y navegador de preferencia, canales digitales de interacción, objetivos y frustraciones.
- Duración acotada de entre doce y veinte minutos, con repreguntas de profundización ("¿por qué?", "¿podría darme un ejemplo?") en lugar de preguntas nuevas.

**Datos generales que se recogen en toda entrevista (ambos segmentos).** Antes del bloque temático se registran, con autorización del entrevistado, los siguientes datos que alimentan directamente las fichas de User Persona: nombres y apellidos, edad, género, distrito de residencia, estado civil y composición familiar, nivel educativo, cargo actual, sector y tamaño aproximado de la empresa, y antigüedad en el puesto.

#### Segmento 1: Jefes de Almacén y Gerentes de Operaciones

**Propósito declarado al entrevistado:** *"Queremos entender cómo se controlan hoy las condiciones de temperatura y humedad en su almacén o planta, qué dificultades encuentra en ese control y qué impacto tiene sobre su operación."*

**A. Preguntas de presentación**

1. ¿Podría indicarnos su nombre completo, su edad y el distrito donde vive?
2. ¿Cuál es su cargo actual y desde hace cuánto tiempo lo ocupa?
3. ¿A qué se dedica la empresa y aproximadamente cuántas personas trabajan en ella?
4. ¿Cómo describiría un día normal de trabajo suyo, desde que llega hasta que se retira?

**B. Situación actual y proceso vigente**

1. ¿Qué tipo de mercadería o insumos almacena y cuáles son sensibles a la temperatura o la humedad?
2. ¿Cómo se controla hoy la temperatura y la humedad en esas zonas? ¿Quién lo hace y con qué instrumentos?
3. ¿Con qué frecuencia se realiza ese control y en qué momentos del día?
4. ¿Dónde queda registrada esa medición? ¿Qué se hace después con ese registro?
5. ¿Existen rangos definidos de temperatura y humedad para cada tipo de producto? ¿Quién los estableció?
6. ¿Qué ocurre cuando la persona encargada de la ronda falta, está ocupada o se olvida de hacerla?

**C. Problema y consecuencias**

1. ¿Ha tenido pérdidas de mercadería o insumos por condiciones ambientales inadecuadas? Cuénteme el último caso que recuerde.
2. ¿Cómo se enteró de que había ocurrido? ¿Cuánto tiempo pasó entre que el problema comenzó y usted lo detectó?
3. ¿Qué hizo cuando lo detectó y a quién tuvo que informar?
4. ¿Puede estimar cuánto representó esa pérdida en dinero o en porcentaje del inventario?
5. ¿Con qué frecuencia diría que ocurren situaciones de este tipo a lo largo del año?
6. De todo lo que implica este control, ¿qué es lo que más le incomoda o le quita tiempo?

**D. Tecnología, canales y hábitos digitales**

1. ¿Qué sistemas o programas utiliza para gestionar el almacén (ERP, hoja de cálculo, sistema propio)?
2. ¿Desde qué dispositivo trabaja habitualmente y cuál usa cuando no está en la planta? ¿Qué marca y sistema operativo?
3. ¿Qué navegador utiliza en la computadora del trabajo?
4. ¿Por qué canal se entera normalmente de una urgencia operativa: llamada, WhatsApp, correo, presencial?
5. ¿Recibe hoy algún tipo de alerta automática de algún sistema? ¿Qué opina de ellas?
6. ¿Qué aplicaciones o herramientas digitales usa a diario, dentro y fuera del trabajo?
7. ¿Sigue alguna marca, gremio, medio o referente del sector logístico o industrial?
8. ¿Ha participado antes en la compra o implementación de alguna herramienta tecnológica para el almacén? ¿Cómo fue esa experiencia?

**E. Preguntas proyectivas y de cierre**

1. Si pudiera conocer en cualquier momento y desde su celular la temperatura y humedad de cada zona del almacén, ¿qué cambiaría en su forma de trabajar?
2. ¿Qué tendría que cumplir una herramienta así para que usted confíe en ella y la use todos los días?
3. ¿Qué le haría desconfiar o abandonarla?
4. ¿Quién tomaría la decisión de contratar algo así en su empresa y qué necesitaría para aprobarlo?
5. ¿Le resultaría útil que esa información llegara al sistema que ya usa, en lugar de tener que entrar a otra plataforma?
6. ¿Hay algo que no le hayamos preguntado y que considere importante sobre este tema?

#### Segmento 2: Encargados de Control de Calidad

**Propósito declarado al entrevistado:** *"Queremos entender cómo se documentan hoy las condiciones ambientales de almacenamiento, cómo se sustenta esa información ante una auditoría y qué dificultades encuentra en ese proceso."*

**A. Preguntas de presentación**

1. ¿Podría indicarnos su nombre completo, su edad y el distrito donde vive?
2. ¿Cuál es su formación profesional y su cargo actual?
3. ¿Desde hace cuánto tiempo trabaja en aseguramiento o control de la calidad?
4. ¿Qué tipo de producto elabora o almacena la empresa y bajo qué normas o certificaciones trabaja?

**B. Situación actual y proceso vigente**

1. ¿Qué variables ambientales debe controlar y documentar en las zonas de almacenamiento o producción?
2. ¿Cómo se toma y se registra hoy esa información? ¿En qué formato queda?
3. ¿Quién es responsable de tomar el dato y quién de consolidarlo?
4. ¿Cómo se definen los rangos aceptables y cada cuánto se revisan?
5. ¿Qué instrumentos se utilizan y cómo se verifica que estén correctamente calibrados?
6. ¿Cuánto tiempo le toma preparar el reporte de condiciones ambientales de un período?

**C. Auditorías, evidencia y consecuencias**

1. ¿Qué le solicitan exactamente en una auditoría respecto de las condiciones de almacenamiento?
2. Cuénteme cómo fue la última auditoría o inspección en la que participó. ¿Cómo preparó la evidencia?
3. ¿Ha recibido observaciones o no conformidades relacionadas con registros ambientales? ¿En qué consistieron?
4. ¿Qué ocurre cuando falta un registro o cuando hay un vacío en la planilla?
5. ¿Ha tenido que rechazar o dar de baja un lote por condiciones fuera de rango? ¿Cómo lo sustentó?
6. ¿Un cliente le ha exigido alguna vez evidencia de las condiciones de conservación? ¿Cómo respondió?
7. ¿Qué parte de este proceso considera más frágil o más propensa al error?

**D. Tecnología, canales y hábitos digitales**

1. ¿Con qué herramientas digitales trabaja para llevar los registros y elaborar los reportes?
2. ¿Qué dispositivo y sistema operativo utiliza en el trabajo y cuál en su vida personal? ¿Qué navegador prefiere?
3. ¿Por qué canal se comunica con producción, con almacén y con la gerencia?
4. ¿Dónde se informa sobre normativa, buenas prácticas o actualizaciones del sector?
5. ¿Qué marcas de instrumentos o proveedores de servicios de calidad conoce o le inspiran confianza?
6. ¿Ha trabajado antes con algún sistema de registro automatizado? ¿Qué le pareció?

**E. Preguntas proyectivas y de cierre**

1. Si el registro de temperatura y humedad se generara solo, de forma continua y sin intervención humana, ¿qué cambiaría en su trabajo?
2. ¿Qué características tendría que tener ese registro para que un auditor lo acepte como evidencia válida?
3. ¿Qué nivel de exactitud del instrumento consideraría suficiente y a partir de qué punto exigiría un certificado de calibración?
4. ¿Qué le preocuparía de un sistema automático de este tipo?
5. ¿Cómo preferiría recibir el reporte para una auditoría?
6. ¿Hay algo que no le hayamos preguntado y que considere importante sobre este tema?

---

### 2.2.2. Registro de entrevistas

Se realizaron cinco entrevistas por segmento, para un total de diez entrevistas. Todas fueron grabadas en video con autorización de los participantes y editadas en un único archivo publicado en Microsoft Stream, cuyo enlace se consigna a continuación. Para cada entrevista se indica el minuto de inicio dentro de ese video consolidado y su duración, además del resumen descriptivo de las respuestas obtenidas.

**URL del video consolidado de entrevistas:** *[ Insertar enlace de Microsoft Stream / Clipchamp ]*

> **⚠️ Sección que requiere evidencia real del equipo**
>
> Las fichas y el cuadro resumen que siguen contienen la estructura exigida por el enunciado del trabajo final. Los datos de los entrevistados (nombres, edad, distrito), los screenshots del video, los enlaces y los timings deben ser completados por el equipo con la información de las entrevistas efectivamente realizadas: no pueden inventarse, porque el docente contrasta el registro con el video de evidencia durante la sustentación.
>
> Cada resumen debe redactarse de forma descriptiva y cubrir explícitamente las características objetivas y subjetivas del entrevistado —personalidad, marcas e influencias, tecnología, canales de interacción, navegador y dispositivos—, de modo que toda característica que después aparezca en las fichas de User Persona pueda rastrearse hasta una entrevista concreta.

#### Cuadro resumen de entrevistas

| N.° | Nombres y apellidos | Edad | Distrito | Cargo / Empresa | Inicio | Duración |
|:---:|---|:---:|---|---|:---:|:---:|
| **Segmento 1 — Jefes de Almacén y Gerentes de Operaciones** ||||||
| 1 | | | | | | |
| 2 | | | | | | |
| 3 | | | | | | |
| 4 | | | | | | |
| 5 | | | | | | |
| **Segmento 2 — Encargados de Control de Calidad** ||||||
| 1 | | | | | | |
| 2 | | | | | | |
| 3 | | | | | | |
| 4 | | | | | | |
| 5 | | | | | | |

*Tabla 3. Cuadro resumen de las entrevistas realizadas por segmento.*

#### Segmento 1 · Jefe de Almacén / Gerente de Operaciones — Entrevista #1

| Campo | Dato |
|---|---|
| Nombres y apellidos | |
| Edad | |
| Distrito de residencia | |
| Cargo y empresa | |
| Sector y tamaño de la empresa | |
| Fecha de la entrevista | |
| Inicio en el video (timing) | |
| Duración | |
| Entrevistador (equipo) | |

> **[ INSERTAR IMAGEN ]** — Screenshot de un cuadro del video de la entrevista
> `![Entrevista S1-1](../assets/img/chapter-2/entrevistas/s1-e1.png)`

**Resumen de la entrevista:** *[ Redactar aquí el resumen descriptivo. Debe cubrir: proceso actual de control y quién lo ejecuta, frecuencia de las rondas, instrumentos utilizados, dónde se registra la información, último incidente de pérdida y cómo se detectó, tiempo transcurrido hasta la detección, impacto económico estimado, principal frustración expresada, sistemas y ERP en uso, dispositivo y sistema operativo, navegador, canales por los que recibe urgencias, marcas y referentes que sigue, rasgos de personalidad observados, objetivos declarados y condiciones que pondría para confiar en una solución digital. ]*

#### Segmento 1 · Jefe de Almacén / Gerente de Operaciones — Entrevista #2

| Campo | Dato |
|---|---|
| Nombres y apellidos | |
| Edad | |
| Distrito de residencia | |
| Cargo y empresa | |
| Sector y tamaño de la empresa | |
| Fecha de la entrevista | |
| Inicio en el video (timing) | |
| Duración | |
| Entrevistador (equipo) | |

> **[ INSERTAR IMAGEN ]** — Screenshot de un cuadro del video de la entrevista

**Resumen de la entrevista:** *[ Redactar. Mismos puntos a cubrir que la entrevista #1. ]*

#### Segmento 1 · Jefe de Almacén / Gerente de Operaciones — Entrevista #3

| Campo | Dato |
|---|---|
| Nombres y apellidos | |
| Edad | |
| Distrito de residencia | |
| Cargo y empresa | |
| Sector y tamaño de la empresa | |
| Fecha de la entrevista | |
| Inicio en el video (timing) | |
| Duración | |
| Entrevistador (equipo) | |

> **[ INSERTAR IMAGEN ]** — Screenshot de un cuadro del video de la entrevista

**Resumen de la entrevista:** *[ Redactar. Mismos puntos a cubrir que la entrevista #1. ]*

#### Segmento 1 · Jefe de Almacén / Gerente de Operaciones — Entrevista #4

| Campo | Dato |
|---|---|
| Nombres y apellidos | |
| Edad | |
| Distrito de residencia | |
| Cargo y empresa | |
| Sector y tamaño de la empresa | |
| Fecha de la entrevista | |
| Inicio en el video (timing) | |
| Duración | |
| Entrevistador (equipo) | |

> **[ INSERTAR IMAGEN ]** — Screenshot de un cuadro del video de la entrevista

**Resumen de la entrevista:** *[ Redactar. Mismos puntos a cubrir que la entrevista #1. ]*

#### Segmento 1 · Jefe de Almacén / Gerente de Operaciones — Entrevista #5

| Campo | Dato |
|---|---|
| Nombres y apellidos | |
| Edad | |
| Distrito de residencia | |
| Cargo y empresa | |
| Sector y tamaño de la empresa | |
| Fecha de la entrevista | |
| Inicio en el video (timing) | |
| Duración | |
| Entrevistador (equipo) | |

> **[ INSERTAR IMAGEN ]** — Screenshot de un cuadro del video de la entrevista

**Resumen de la entrevista:** *[ Redactar. Mismos puntos a cubrir que la entrevista #1. ]*

#### Segmento 2 · Encargado de Control de Calidad — Entrevista #1

| Campo | Dato |
|---|---|
| Nombres y apellidos | |
| Edad | |
| Distrito de residencia | |
| Cargo y empresa | |
| Sector y tamaño de la empresa | |
| Fecha de la entrevista | |
| Inicio en el video (timing) | |
| Duración | |
| Entrevistador (equipo) | |

> **[ INSERTAR IMAGEN ]** — Screenshot de un cuadro del video de la entrevista

**Resumen de la entrevista:** *[ Redactar aquí el resumen descriptivo. Debe cubrir: variables que debe documentar y bajo qué norma, forma actual de registro y consolidación, responsables del dato, definición y revisión de rangos, verificación de calibración, tiempo de preparación de reportes, experiencia en la última auditoría, observaciones o no conformidades recibidas, casos de lote rechazado, herramientas digitales utilizadas, dispositivo, sistema operativo y navegador, canales de comunicación, fuentes de información normativa, marcas de instrumentos que le inspiran confianza, rasgos de personalidad observados, objetivos declarados y exigencias que plantearía a un registro automático para considerarlo evidencia válida. ]*

#### Segmento 2 · Encargado de Control de Calidad — Entrevista #2

| Campo | Dato |
|---|---|
| Nombres y apellidos | |
| Edad | |
| Distrito de residencia | |
| Cargo y empresa | |
| Sector y tamaño de la empresa | |
| Fecha de la entrevista | |
| Inicio en el video (timing) | |
| Duración | |
| Entrevistador (equipo) | |

> **[ INSERTAR IMAGEN ]** — Screenshot de un cuadro del video de la entrevista

**Resumen de la entrevista:** *[ Redactar. Mismos puntos a cubrir que la entrevista #1 del segmento 2. ]*

#### Segmento 2 · Encargado de Control de Calidad — Entrevista #3

| Campo | Dato |
|---|---|
| Nombres y apellidos | |
| Edad | |
| Distrito de residencia | |
| Cargo y empresa | |
| Sector y tamaño de la empresa | |
| Fecha de la entrevista | |
| Inicio en el video (timing) | |
| Duración | |
| Entrevistador (equipo) | |

> **[ INSERTAR IMAGEN ]** — Screenshot de un cuadro del video de la entrevista

**Resumen de la entrevista:** *[ Redactar. Mismos puntos a cubrir que la entrevista #1 del segmento 2. ]*

#### Segmento 2 · Encargado de Control de Calidad — Entrevista #4

| Campo | Dato |
|---|---|
| Nombres y apellidos | |
| Edad | |
| Distrito de residencia | |
| Cargo y empresa | |
| Sector y tamaño de la empresa | |
| Fecha de la entrevista | |
| Inicio en el video (timing) | |
| Duración | |
| Entrevistador (equipo) | |

> **[ INSERTAR IMAGEN ]** — Screenshot de un cuadro del video de la entrevista

**Resumen de la entrevista:** *[ Redactar. Mismos puntos a cubrir que la entrevista #1 del segmento 2. ]*

#### Segmento 2 · Encargado de Control de Calidad — Entrevista #5

| Campo | Dato |
|---|---|
| Nombres y apellidos | |
| Edad | |
| Distrito de residencia | |
| Cargo y empresa | |
| Sector y tamaño de la empresa | |
| Fecha de la entrevista | |
| Inicio en el video (timing) | |
| Duración | |
| Entrevistador (equipo) | |

> **[ INSERTAR IMAGEN ]** — Screenshot de un cuadro del video de la entrevista

**Resumen de la entrevista:** *[ Redactar. Mismos puntos a cubrir que la entrevista #1 del segmento 2. ]*

---

### 2.2.3. Análisis de entrevistas

El análisis se realizó de forma independiente para cada segmento, consolidando las respuestas registradas en los resúmenes de la sección anterior. Dado que se entrevistó a cinco personas por segmento, cada entrevistado representa un 20 % de la muestra, de modo que los porcentajes consignados en los cuadros siguientes se obtienen contando cuántos entrevistados presentan cada característica sobre el total de cinco. Se trata, por tanto, de una muestra cualitativa y no probabilística: los porcentajes describen la composición de la muestra y sirven para justificar las decisiones tomadas al construir los arquetipos, no para extrapolar al universo de PyMEs de Lima.

> **⚠️ Sección que requiere completarse con los datos reales**
>
> Los cuadros siguientes listan las dimensiones que deben cuantificarse. El equipo debe llenar la columna de resultado y la de porcentaje a partir de los diez resúmenes de la sección 2.2.2, y luego redactar bajo cada cuadro el párrafo interpretativo. Cada característica que aparezca en las fichas de User Persona de la sección 2.3.1 debe poder rastrearse hasta una fila de estos cuadros.

#### Segmento 1: Jefes de Almacén y Gerentes de Operaciones

| Característica analizada | Resultado observado en la muestra | Porcentaje |
|---|---|:---:|
| **Características demográficas (objetivas)** |||
| Rango de edad predominante | | |
| Distribución por género | | |
| Distritos de residencia más frecuentes | | |
| Nivel educativo alcanzado | | |
| Antigüedad en el cargo | | |
| Sector económico de la empresa | | |
| Tamaño de la empresa (número de trabajadores) | | |
| **Prácticas actuales y problema (objetivas)** |||
| Método actual de control ambiental (ronda manual, datalogger, ninguno) | | |
| Frecuencia declarada de las rondas de inspección | | |
| Soporte del registro (papel, hoja de cálculo, sistema) | | |
| Existencia de rangos formalmente definidos por tipo de producto | | |
| Han sufrido pérdidas por condiciones fuera de rango en los últimos 12 meses | | |
| Tiempo promedio entre el inicio de la desviación y su detección | | |
| Rango estimado del impacto económico del último incidente | | |
| Cuentan con climatización o deshumidificación en las zonas críticas | | |
| **Tecnología y canales (objetivas)** |||
| Sistema de gestión en uso (ERP, hoja de cálculo, sistema propio) | | |
| Dispositivo móvil de preferencia y sistema operativo | | |
| Navegador utilizado en el equipo de trabajo | | |
| Canal por el que se comunican las urgencias operativas | | |
| Reciben actualmente algún tipo de alerta automatizada | | |
| Experiencia previa en la implementación de herramientas tecnológicas | | |
| **Características subjetivas y actitudinales** |||
| Rasgos de personalidad predominantes | | |
| Principal frustración expresada | | |
| Principal objetivo declarado en el puesto | | |
| Actitud frente a la adopción de nueva tecnología | | |
| Marcas, gremios o referentes del sector que siguen | | |
| Condición indispensable para confiar en una solución digital | | |
| Disposición a pagar una suscripción mensual | | |
| Rol declarado en la decisión de compra | | |

*Tabla 4. Análisis cuantificado de las entrevistas del segmento Jefes de Almacén y Gerentes de Operaciones (n = 5).*

**Interpretación:** *[ Redactar el análisis narrativo del segmento, resaltando las características que superan el 60 % de coincidencia, las que resultaron dispersas y las implicancias de cada una para el diseño del arquetipo y de la solución. ]*

#### Segmento 2: Encargados de Control de Calidad

| Característica analizada | Resultado observado en la muestra | Porcentaje |
|---|---|:---:|
| **Características demográficas (objetivas)** |||
| Rango de edad predominante | | |
| Distribución por género | | |
| Distritos de residencia más frecuentes | | |
| Nivel educativo alcanzado | | |
| Antigüedad en el cargo | | |
| Sector económico de la empresa | | |
| Tamaño de la empresa (número de trabajadores) | | |
| **Prácticas actuales y problema (objetivas)** |||
| Normas o certificaciones bajo las que opera la empresa | | |
| Variables ambientales que están obligados a documentar | | |
| Soporte actual del registro de condiciones ambientales | | |
| Frecuencia con que se consolidan los registros | | |
| Tiempo declarado para preparar el reporte de un período | | |
| Han recibido observaciones o no conformidades por registros ambientales | | |
| Han tenido que rechazar o dar de baja lotes por condiciones fuera de rango | | |
| Un cliente les ha exigido evidencia de condiciones de conservación | | |
| Procedimiento vigente de verificación o calibración de instrumentos | | |
| **Tecnología y canales (objetivas)** |||
| Herramientas digitales utilizadas para registros y reportes | | |
| Dispositivo y sistema operativo de preferencia | | |
| Navegador utilizado en el equipo de trabajo | | |
| Canales de comunicación con producción, almacén y gerencia | | |
| Fuentes consultadas para normativa y buenas prácticas | | |
| Experiencia previa con sistemas de registro automatizado | | |
| **Características subjetivas y actitudinales** |||
| Rasgos de personalidad predominantes | | |
| Principal frustración expresada | | |
| Principal objetivo declarado en el puesto | | |
| Nivel de exactitud considerado suficiente en el instrumento | | |
| Exigencia de certificado de calibración trazable | | |
| Marcas de instrumentos o proveedores que les inspiran confianza | | |
| Requisitos que plantean para aceptar un registro automático como evidencia | | |
| Preocupaciones expresadas frente a la automatización del registro | | |

*Tabla 5. Análisis cuantificado de las entrevistas del segmento Encargados de Control de Calidad (n = 5).*

**Interpretación:** *[ Redactar el análisis narrativo del segmento, resaltando las características que superan el 60 % de coincidencia, las que resultaron dispersas y las implicancias de cada una para el diseño del arquetipo y de la solución. ]*

#### Hallazgos transversales y su traducción en decisiones de producto

Una vez completados ambos cuadros, el equipo debe consolidar los hallazgos comunes y divergentes entre segmentos. El siguiente cuadro relaciona cada hallazgo esperado con la decisión de producto que se desprende de él, y constituye el puente explícito entre la investigación y la especificación de requisitos del Capítulo III.

| Hallazgo (a confirmar con los datos) | Implicancia para MachineGuard | Segmento afectado |
|---|---|---|
| El control depende de que una persona ejecute la ronda; cuando falta o está ocupada, la medición simplemente no ocurre. | La captura debe ser automática y continua, sin depender de una acción humana. La frecuencia de muestreo es un parámetro del sistema, no del turno. | Ambos |
| La desviación se detecta horas después de haber comenzado, cuando el daño ya se produjo. | El valor central del producto es la latencia de la alerta, no la riqueza del dashboard. La notificación push es prioridad de primer sprint. | Segmento 1 |
| El registro vive en papel o en hojas de cálculo dispersas y presenta vacíos. | El historial debe ser íntegro, continuo y exportable, con marca temporal por medición. | Segmento 2 |
| La empresa ya opera un sistema de gestión y sus responsables se resisten a incorporar una plataforma más. | La API pública de integración deja de ser un extra y se vuelve un requisito de adopción. | Ambos |
| La conectividad del almacén es intermitente. | Se confirma la necesidad del Edge Service con almacenamiento local y sincronización diferida. | Ambos |
| El encargado de calidad exige poder demostrar la validez del dato ante un auditor. | Se requiere registro de calibración por nodo, trazabilidad de quién reconoció cada alerta y reportes por período listos para auditoría. | Segmento 2 |
| La decisión de compra la aprueba la gerencia, pero la impulsa el jefe de almacén. | El producto necesita una vista de resumen orientada a gerencia y material que permita al usuario operativo sustentar la inversión internamente. | Segmento 1 |

*Tabla 6. Hallazgos transversales de las entrevistas y su traducción en decisiones de producto.*

---

## 2.3. Needfinding

El Needfinding constituye la fase de síntesis de la investigación: convierte el material disperso de las entrevistas y del análisis competitivo en artefactos que el equipo puede consultar durante todo el ciclo de vida del proyecto. En esta sección se presentan cuatro artefactos encadenados. Los User Personas condensan en un arquetipo las características recurrentes de cada segmento; el User Task Matrix identifica qué hacen realmente esas personas para cumplir sus objetivos, con independencia de que exista o no una solución de software; los User Journey Maps describen el recorrido As-Is de cada arquetipo con sus emociones y puntos de dolor; y los Empathy Maps profundizan en el mundo interno de cada uno para exponer sus pains y gains.

Los cuatro artefactos fueron elaborados en UXPressia, herramienta indicada para el curso, de modo que las fichas de User Persona quedan vinculadas a sus respectivos Journey Maps y Empathy Maps dentro del mismo proyecto. En este informe se incluye tanto la captura de imagen del artefacto en la herramienta como su transcripción en texto, para facilitar la lectura y la trazabilidad.

---

### 2.3.1. User Personas

Se elaboró una ficha de User Persona por cada segmento objetivo. Cada característica consignada proviene del análisis de entrevistas de la sección 2.2.3 —edad, distrito, formación, antigüedad, dispositivos, canales, personalidad, marcas de referencia, objetivos y frustraciones— y no de suposiciones del equipo. Del análisis competitivo de la sección 2.1 provienen, además, las expectativas y los puntos de comparación que cada arquetipo trae consigo: el Jefe de Almacén compara contra la ronda manual que hoy ejecuta, mientras que el Encargado de Control de Calidad compara contra el estándar de evidencia que un sistema profesional como Testo Saveris establece en el sector.

> **Recordatorio para el equipo:** antes de la entrega debe reemplazarse cada marcador de imagen por la captura del artefacto elaborado en UXPressia, y ajustarse los valores de las fichas si el análisis de entrevistas arroja resultados distintos a los aquí propuestos. La transcripción en texto se mantiene como respaldo y para facilitar la lectura en la versión PDF del informe.

#### User Persona 1 — Segmento: Jefes de Almacén y Gerentes de Operaciones

> **[ INSERTAR IMAGEN ]** — Ficha de User Persona "Ricardo Salazar" elaborada en UXPressia
> `![User Persona Ricardo Salazar](../assets/img/chapter-2/personas/ricardo-salazar.png)`

| **Ricardo Salazar Núñez — Jefe de Almacén de una PyME de insumos alimentarios** ||
|---|---|
| **Datos demográficos** ||
| Edad | 38 años |
| Género | Masculino |
| Distrito de residencia | Ate, Lima |
| Estado civil y familia | Casado, dos hijos en edad escolar |
| Nivel educativo | Técnico en Administración Industrial (SENATI), con un diplomado en gestión logística |
| Ocupación | Jefe de Almacén en una empresa de distribución de insumos alimentarios de 45 trabajadores |
| Antigüedad en el cargo | 6 años en el cargo, 11 años en el rubro |
| Ingreso aproximado | Entre S/ 3 500 y S/ 5 000 mensuales |
| **Biografía y contexto** ||
| Background | Ricardo empezó como auxiliar de almacén y llegó a jefe por conocimiento del terreno, no por formación en tecnología. Conoce cada rincón de la nave, sabe qué producto se malogra primero y en qué zona pega el sol por la tarde.<br><br>Responde por dos naves de almacenamiento y una zona de acondicionamiento. Su desempeño se mide por el porcentaje de merma mensual y por el cumplimiento de los despachos, de modo que cualquier lote perdido lo afecta directamente.<br><br>Llega antes que el turno para revisar cómo amaneció el almacén y suele quedarse conectado al WhatsApp del equipo incluso los fines de semana, porque sabe que una falla del sistema de ventilación un domingo no la va a detectar nadie. |
| **Perfil actitudinal** ||
| Personalidad | Pragmático y resolutivo: prefiere una herramienta simple que funcione a una completa que deba aprender.<br>Escéptico ante promesas tecnológicas por experiencias previas con sistemas que nadie terminó de usar.<br>Orientado al detalle operativo, con memoria fuerte de los incidentes vividos.<br>Comunicativo con su equipo y directo con la gerencia. |
| Habilidades | Gestión de inventarios y control de stock.<br>Manejo intermedio de hojas de cálculo.<br>Coordinación de personal operativo y de turnos.<br>Negociación con transportistas y proveedores. |
| Motivaciones | Que el mes cierre sin mermas atribuibles a su gestión.<br>Reconocimiento de la gerencia y estabilidad en el puesto.<br>Reducir el desgaste de estar permanentemente pendiente del almacén fuera del horario. |
| **Objetivos y frustraciones** ||
| Objetivos | Mantener la merma mensual por condiciones ambientales por debajo de la meta fijada por gerencia.<br>Enterarse de cualquier desviación en minutos y no al día siguiente.<br>Tener visibilidad del estado del almacén cuando no está físicamente en la planta.<br>Sustentar ante gerencia, con datos y no con impresiones, la necesidad de mejorar la climatización. |
| Frustraciones | Se entera de los problemas cuando el daño ya ocurrió y no hay nada que corregir.<br>Las planillas de control se llenan tarde, con letra ilegible o directamente no se llenan.<br>Depende de que el personal cumpla la ronda; cuando alguien falta, el control se cae ese día.<br>No puede demostrar con evidencia qué pasó durante la noche o el fin de semana.<br>Los presupuestos para equipamiento del almacén son los primeros en recortarse. |
| **Tecnología, marcas y canales** ||
| Dispositivos de preferencia | Smartphone Android de gama media (Samsung Galaxy serie A), su herramienta principal fuera de la oficina.<br>Laptop con Windows en la oficina del almacén.<br>No usa tablet ni reloj inteligente. |
| Software y sistemas que utiliza | ERP administrativo local para inventarios y despachos.<br>Microsoft Excel para los formatos de control y los reportes a gerencia.<br>WhatsApp para la coordinación diaria con el equipo.<br>Navegador Google Chrome. |
| Canales digitales de interacción | WhatsApp (canal principal de urgencias, incluso fuera de horario).<br>Llamada telefónica para lo crítico.<br>Correo corporativo para lo formal con gerencia.<br>LinkedIn y grupos de Facebook de logística, con uso ocasional. |
| Marcas e influencias | Samsung y WhatsApp en lo cotidiano.<br>Sodimac y Promart para la compra de equipamiento menor del almacén.<br>Contenido de gremios y ferias del sector logístico e industrial peruano.<br>Confía en la recomendación de colegas del rubro por encima de la publicidad. |
| Nivel de alfabetización digital | Medio: usa con soltura lo que ya conoce, pero necesita que una herramienta nueva sea evidente desde la primera pantalla. |
| **Relación con la solución** ||
| Rol en la decisión de compra | Impulsor y usuario principal. No firma la orden de compra, pero es quien detecta la necesidad, evalúa la alternativa y debe sustentarla ante la gerencia. |
| Cita representativa | *"Yo me entero de que la humedad subió cuando ya se me arruinó el saco. Para ese momento ya perdí la plata."* |

*Tabla 7. Ficha de User Persona del segmento Jefes de Almacén y Gerentes de Operaciones.*

#### User Persona 2 — Segmento: Encargados de Control de Calidad

> **[ INSERTAR IMAGEN ]** — Ficha de User Persona "Andrea Quispe" elaborada en UXPressia
> `![User Persona Andrea Quispe](../assets/img/chapter-2/personas/andrea-quispe.png)`

| **Andrea Quispe Rojas — Coordinadora de Aseguramiento de la Calidad** ||
|---|---|
| **Datos demográficos** ||
| Edad | 31 años |
| Género | Femenino |
| Distrito de residencia | Los Olivos, Lima |
| Estado civil y familia | Soltera, vive con sus padres y aporta al hogar |
| Nivel educativo | Ingeniera de Industrias Alimentarias, con certificación en HACCP y formación continua en inocuidad |
| Ocupación | Coordinadora de Aseguramiento de la Calidad en una PyME de procesamiento de alimentos |
| Antigüedad en el cargo | 3 años en el cargo, 6 años en el área de calidad |
| Ingreso aproximado | Entre S/ 3 000 y S/ 4 500 mensuales |
| **Biografía y contexto** ||
| Background | Andrea es la responsable de que la empresa pueda demostrar, con documentos, que sus productos se conservaron en condiciones adecuadas. Su trabajo se vuelve visible únicamente cuando algo falla o cuando llega una auditoría.<br><br>Sostiene el sistema de calidad prácticamente sola, apoyándose en formatos que ella misma diseñó en hojas de cálculo. Cada auditoría le implica varios días de reconstrucción documental a partir de planillas que le entrega el personal de almacén.<br><br>Aspira a que la empresa alcance una certificación internacional, consciente de que hoy no aprobaría una auditoría exigente por los vacíos de sus registros ambientales. |
| **Perfil actitudinal** ||
| Personalidad | Metódica y detallista: revisa dos veces antes de firmar.<br>Analítica, cómoda con datos, tablas y evidencia documental.<br>Cautelosa frente al riesgo: prefiere el rechazo de un lote a una observación en auditoría.<br>Persistente en la insistencia al personal operativo para que cumpla los registros. |
| Habilidades | Interpretación y aplicación de normativa sanitaria y de inocuidad.<br>Manejo avanzado de hojas de cálculo y elaboración de reportes.<br>Documentación de procedimientos y no conformidades.<br>Capacitación al personal operativo. |
| Motivaciones | Superar auditorías sin observaciones.<br>Ser reconocida como la profesional que ordenó el sistema de calidad de la empresa.<br>Avanzar hacia una certificación que abra mercados al negocio. |
| **Objetivos y frustraciones** ||
| Objetivos | Contar con un registro continuo, íntegro y verificable de las condiciones ambientales de cada zona.<br>Reducir a horas el tiempo de preparación de la evidencia para una auditoría.<br>Eliminar los vacíos y las inconsistencias del registro manual.<br>Poder sustentar ante un cliente o un auditor qué ocurrió en una fecha determinada. |
| Frustraciones | Reconstruir el histórico la noche previa a una auditoría a partir de planillas incompletas.<br>Registros con horarios sospechosamente redondos, llenados de memoria al final del turno.<br>No poder demostrar las condiciones de los períodos no cubiertos por las rondas.<br>Depender de terceros que no comparten su urgencia por el cumplimiento.<br>Que se cuestione la validez de un instrumento sin certificado de calibración vigente. |
| **Tecnología, marcas y canales** ||
| Dispositivos de preferencia | Smartphone (iPhone o Android de gama media) para correo y coordinación.<br>Laptop con Windows como herramienta principal de trabajo.<br>Impresora y archivador físico, todavía necesarios para la documentación firmada. |
| Software y sistemas que utiliza | Microsoft Excel y Google Sheets para formatos y consolidación.<br>Correo corporativo y Microsoft Teams o Google Meet para reuniones.<br>Documentos en PDF para la entrega formal de reportes.<br>Navegador Google Chrome. |
| Canales digitales de interacción | Correo electrónico como canal formal y trazable.<br>WhatsApp para coordinación rápida con producción y almacén.<br>Videollamadas con auditores y clientes.<br>LinkedIn y portales de normativa sanitaria para actualizarse. |
| Marcas e influencias | Marcas de instrumentación reconocidas en el sector, asociadas a confiabilidad metrológica.<br>Normativa y comunicaciones de las autoridades sanitarias nacionales.<br>Estándares internacionales de inocuidad como referente de buenas prácticas.<br>Comunidades profesionales de calidad e inocuidad en LinkedIn. |
| Nivel de alfabetización digital | Alto: adopta con facilidad herramientas nuevas siempre que pueda auditar de dónde sale cada dato. |
| **Relación con la solución** ||
| Rol en la decisión de compra | Influenciadora técnica y validadora. No decide la compra, pero su objeción sobre la validez del dato puede bloquearla, y su respaldo la acelera. |
| Cita representativa | *"Si no lo puedo demostrar con un registro, para el auditor simplemente no pasó."* |

*Tabla 8. Ficha de User Persona del segmento Encargados de Control de Calidad.*

---

### 2.3.2. User Task Matrix

El siguiente cuadro concentra las tareas que ambos User Personas ejecutan para cumplir sus objetivos. Se trata de tareas del dominio, es decir, actividades que Ricardo y Andrea realizan hoy con independencia de que exista MachineGuard: no se han incluido opciones de menú ni características de software. Para cada tarea se consigna la frecuencia con que se ejecuta y la importancia que el propio arquetipo le atribuye, en una escala de tres niveles (Alta, Media, Baja).

| Tarea (User Task) | Ricardo · Frecuencia | Ricardo · Importancia | Andrea · Frecuencia | Andrea · Importancia |
|---|:---:|:---:|:---:|:---:|
| Verificar las condiciones de temperatura y humedad de las zonas de almacenamiento | Alta | Alta | Media | Alta |
| Registrar la lectura ambiental en una planilla o formato de control | Alta | Media | Media | Alta |
| Detectar una condición fuera del rango permitido y comunicarla | Media | Alta | Baja | Alta |
| Coordinar y ejecutar la acción correctiva (ventilar, ajustar climatización, reubicar mercadería) | Media | Alta | Baja | Media |
| Definir y actualizar los rangos permitidos por tipo de producto | Baja | Media | Baja | Alta |
| Verificar el funcionamiento y la calibración de los instrumentos de medición | Baja | Media | Media | Alta |
| Supervisar y capacitar al personal en el procedimiento de control ambiental | Media | Media | Media | Alta |
| Consolidar el histórico de mediciones del período | Baja | Media | Alta | Alta |
| Identificar y cuantificar la merma o el producto no conforme | Media | Alta | Media | Alta |
| Elaborar el reporte de condiciones ambientales para la gerencia | Media | Media | Alta | Media |
| Preparar la evidencia documental para una auditoría o inspección | Baja | Media | Alta | Alta |
| Sustentar ante un auditor o cliente una desviación ocurrida | Baja | Alta | Media | Alta |
| Documentar la no conformidad y su acción correctiva | Baja | Media | Alta | Alta |
| Justificar ante la gerencia la inversión en mejoras de almacenamiento | Baja | Alta | Baja | Media |

*Tabla 9. User Task Matrix de los segmentos objetivo de MachineGuard.*

**Lectura del cuadro.** Solo una tarea alcanza frecuencia e importancia altas de forma simultánea en ambos arquetipos: verificar las condiciones ambientales de las zonas de almacenamiento. Es la tarea nuclear del dominio y, en consecuencia, la que MachineGuard debe automatizar primero: todo lo demás se construye sobre ella.

**Coincidencias.** Ambos arquetipos comparten la verificación de condiciones y el registro de la lectura, y ambos otorgan importancia alta a identificar y cuantificar el producto afectado. Esto confirma que un único flujo de captura automática sirve a los dos segmentos, y que la diferenciación debe darse en la capa de presentación y no en la de datos.

**Diferencias.** El contraste es nítido en el eje temporal. Ricardo concentra su importancia en tareas de reacción inmediata —detectar la desviación, coordinar la acción correctiva, sustentar la inversión— que ejecuta con frecuencia media pero bajo presión de tiempo. Andrea concentra la suya en tareas de reconstrucción y sustentación —consolidar el histórico, preparar la evidencia, documentar la no conformidad— de frecuencia baja pero de altísimo impacto cuando ocurren. En términos de producto, Ricardo necesita latencia mínima en la notificación, mientras que Andrea necesita integridad y completitud del histórico.

**Tareas de frecuencia baja e importancia alta.** Merecen atención especial las tareas que casi nunca se ejecutan pero que resultan críticas cuando se presentan: sustentar una desviación ante un auditor y justificar una inversión ante gerencia. Al ser esporádicas, hoy nadie las prepara con anticipación, y es justamente ahí donde un historial automático genera un valor desproporcionado respecto de su costo.

---

### 2.3.3. User Journey Mapping

Se elaboró un User Journey Map por cada User Persona, en su versión As-Is, es decir, describiendo el recorrido tal como ocurre hoy, sin que exista MachineGuard. El objetivo es exponer el end-to-end journey con sus fases, acciones, pensamientos y emociones, y localizar con precisión los momentos de mayor fricción, que son los que la solución deberá intervenir. Cada mapa está vinculado en UXPressia a la ficha de User Persona correspondiente elaborada en la sección 2.3.1.

#### User Journey Map (As-Is) — Ricardo Salazar, Jefe de Almacén

> **[ INSERTAR IMAGEN ]** — User Journey Map As-Is de Ricardo Salazar elaborado en UXPressia
> `![Journey Map Ricardo](../assets/img/chapter-2/journeys/ricardo-as-is.png)`

| Fase | 1. Inicio de turno | 2. Ronda de inspección | 3. Detección tardía de la desviación | 4. Respuesta y acción correctiva | 5. Registro y cierre del turno | 6. Reporte a gerencia |
|---|---|---|---|---|---|---|
| **Doing (acciones)** | Llega temprano y recorre las naves.<br>Revisa a ojo si algo se ve húmedo o si el ambiente está cargado.<br>Verifica que la ventilación esté encendida. | Un auxiliar toma la lectura con el termohigrómetro portátil en dos o tres puntos.<br>Anota los valores en la planilla del turno.<br>Si el auxiliar está ocupado, la ronda se posterga. | Nota mercadería con signos de daño o recibe el aviso de un operario.<br>Va al punto, mide y confirma que el valor está fuera de rango.<br>Revisa la planilla y encuentra vacíos en las horas previas. | Enciende o ajusta la ventilación, abre puertas, reubica la mercadería afectada.<br>Llama al técnico si el equipo falló.<br>Separa el producto comprometido. | Completa la planilla del día, a veces de memoria.<br>Envía un mensaje por WhatsApp al grupo del equipo.<br>Archiva la hoja en el file del almacén. | Digita las planillas en Excel.<br>Arma el reporte mensual de merma.<br>Sustenta ante gerencia por qué se perdió el lote. |
| **Thinking (pensamientos)** | *"Ojalá no haya pasado nada anoche."*<br>*"Con este calor, la zona del fondo siempre es la que sufre."* | *"Dos puntos no representan toda la nave."*<br>*"Si hoy no la hacen, no me entero de nada."* | *"¿Desde cuándo está así? No tengo cómo saberlo."*<br>*"Otra vez me entero cuando ya no hay nada que hacer."* | *"Tengo que salvar lo que se pueda."*<br>*"¿Cuánto de esto voy a poder recuperar?"* | *"No sé si estos números son de la hora que dice."*<br>*"Igual nadie va a revisar esta hoja."* | *"Voy a tener que explicar una merma que no pude prevenir."*<br>*"Sin datos, esto parece descuido mío."* |
| **Feeling (emociones)** | Expectante<br>Con cierta inquietud | Resignado<br>Inseguro sobre la cobertura del control | Frustrado<br>Impotente | Bajo presión<br>Apurado | Escéptico sobre el valor del registro<br>Cansado | Expuesto<br>A la defensiva |
| **Touchpoints** | Recorrido físico por la nave<br>Equipo de ventilación | Termohigrómetro portátil<br>Planilla en papel<br>Auxiliar de almacén | Mercadería dañada<br>Operario que avisa<br>Planilla incompleta | Equipo de climatización<br>Técnico externo<br>WhatsApp | Planilla en papel<br>WhatsApp del equipo<br>File del almacén | Excel<br>Correo corporativo<br>Reunión con gerencia |
| **Pain points** | La inspección visual no detecta una desviación que aún no produjo daño. | Cobertura parcial en espacio y en tiempo.<br>El control depende por completo de la disponibilidad de una persona. | Latencia de horas o días entre el inicio del problema y su detección.<br>Imposible reconstruir cuándo comenzó. | La acción correctiva llega cuando el daño ya se produjo.<br>No hay forma de verificar si la condición se normalizó. | Registros llenados de memoria, con vacíos y sin valor probatorio. | Digitación manual que consume horas.<br>Ausencia de datos para sustentar mejoras. |
| **Oportunidades** | Estado de todas las zonas disponible al llegar, en una sola pantalla. | Medición automática y continua en todos los puntos, sin intervención humana. | Alerta push en el momento en que se cruza el umbral, con el histórico del evento. | Confirmación automática de la normalización y registro de la acción correctiva. | Registro generado por el sistema, íntegro y con marca temporal. | Reporte listo para gerencia con la merma evitada y las desviaciones del período. |

*Tabla 10. User Journey Map As-Is de Ricardo Salazar (Jefe de Almacén).*

#### User Journey Map (As-Is) — Andrea Quispe, Encargada de Control de Calidad

> **[ INSERTAR IMAGEN ]** — User Journey Map As-Is de Andrea Quispe elaborado en UXPressia
> `![Journey Map Andrea](../assets/img/chapter-2/journeys/andrea-as-is.png)`

| Fase | 1. Definición de estándares | 2. Recolección de registros | 3. Consolidación y digitación | 4. Detección de vacíos | 5. Auditoría o inspección | 6. Cierre de no conformidades |
|---|---|---|---|---|---|---|
| **Doing (acciones)** | Revisa la normativa y las fichas técnicas de los productos.<br>Define los rangos aceptables por zona.<br>Diseña el formato de control y capacita al personal. | Solicita las planillas del período a almacén y producción.<br>Recibe hojas físicas, algunas incompletas o ilegibles.<br>Insiste por WhatsApp por las que faltan. | Digita las planillas en su hoja de cálculo.<br>Ordena las lecturas por fecha y zona.<br>Arma los gráficos de tendencia. | Identifica días sin registro y horarios sin cobertura.<br>Detecta valores idénticos repetidos que delatan llenado de memoria.<br>Consulta al personal qué ocurrió esos días. | Presenta la carpeta de evidencias al auditor.<br>Responde preguntas sobre períodos específicos.<br>Recibe observaciones sobre la continuidad del registro. | Redacta la no conformidad y el plan de acción.<br>Refuerza la capacitación al personal.<br>Programa el seguimiento. |
| **Thinking (pensamientos)** | *"Esto solo funciona si el personal lo cumple todos los días."*<br>*"El formato ya está; el problema es la disciplina."* | *"Otra vez tengo que perseguir las hojas."*<br>*"¿Y los días que faltan cómo los explico?"* | *"Estoy transcribiendo datos que ni siquiera sé si son ciertos."*<br>*"Dos días completos en algo que debería ser automático."* | *"Estas lecturas son demasiado parejas para ser reales."*<br>*"Si el auditor pregunta por esta semana, no tengo respuesta."* | *"Espero que no pida el detalle de las noches."*<br>*"Todo mi trabajo del mes se juega en esta hora."* | *"El próximo período va a pasar exactamente lo mismo."*<br>*"El problema no es el formato, es que depende de personas."* |
| **Feeling (emociones)** | Motivada<br>Con sentido de propósito | Fastidiada<br>Dependiente de terceros | Agotada<br>Con sensación de trabajo estéril | Preocupada<br>Vulnerable | Tensa<br>Expuesta | Resignada<br>Frustrada por la recurrencia |
| **Touchpoints** | Normativa y fichas técnicas<br>Formato de control<br>Capacitación al personal | Planillas en papel<br>WhatsApp<br>Personal de almacén | Excel<br>Laptop<br>Archivador físico | Planillas con vacíos<br>Consultas al personal<br>Correo | Carpeta de evidencias<br>Auditor<br>Reporte impreso | Formato de no conformidad<br>Reunión de seguimiento<br>Correo a gerencia |
| **Pain points** | Los rangos quedan en el papel; nada garantiza que se respeten en el terreno. | Depende de la voluntad de terceros para obtener su insumo de trabajo.<br>Registros ilegibles o extraviados. | Días de digitación manual sin valor agregado.<br>Riesgo de error de transcripción. | Vacíos imposibles de subsanar de forma retroactiva.<br>Sospecha fundada sobre la veracidad del dato. | El registro manual carece de valor probatorio frente a un auditor exigente.<br>No puede responder por períodos no cubiertos. | La causa raíz —la dependencia del registro manual— nunca se corrige. |
| **Oportunidades** | Umbrales configurados en el sistema, vigentes y verificables por zona y producto. | El dato se genera solo: desaparece la recolección y la dependencia del personal. | Histórico consolidado y disponible al instante, sin digitación. | Cobertura continua sin vacíos, con registro del estado de conexión de cada nodo. | Reporte de trazabilidad por período, exportable y con marca temporal por medición. | Trazabilidad de la desviación, de quién la reconoció y de la acción correctiva aplicada. |

*Tabla 11. User Journey Map As-Is de Andrea Quispe (Encargada de Control de Calidad).*

**Síntesis de ambos recorridos.** Los dos journeys colapsan en el mismo punto de origen: el dato ambiental depende de que una persona lo tome y lo anote. De esa dependencia se derivan la latencia que frustra a Ricardo y los vacíos documentales que exponen a Andrea. La intervención de MachineGuard es, por tanto, una sola —automatizar la captura y la custodia del dato— aunque se manifieste de forma distinta en cada segmento: como alerta inmediata para uno y como histórico íntegro para el otro.

---

### 2.3.4. Empathy Mapping

El equipo realizó una sesión colaborativa de Empathy Mapping por cada User Persona, utilizando la herramienta indicada para el curso. El proceso siguió las etapas habituales de la técnica: preparación de la sesión y selección del arquetipo; colocación del User Persona en el centro del lienzo; aporte individual de observaciones por parte de cada integrante del equipo a partir del material de las entrevistas; agrupación y depuración de las observaciones repetidas; y, finalmente, derivación de los Pains y Gains a partir del conjunto. Las observaciones responden a las preguntas guía de la técnica: ¿con quién estamos empatizando?, ¿qué necesita hacer?, ¿qué está diciendo?, ¿qué está viendo?, ¿qué está haciendo?, ¿qué está escuchando? y ¿cómo se siente y qué piensa?

Es importante señalar que ninguna observación fue incorporada por intuición del equipo: cada una remite a algo efectivamente dicho, mostrado o hecho por los entrevistados y registrado en los resúmenes de la sección 2.2.2.

#### Empathy Map — Ricardo Salazar, Jefe de Almacén

> **[ INSERTAR IMAGEN ]** — Empathy Map de Ricardo Salazar elaborado en UXPressia
> `![Empathy Map Ricardo](../assets/img/chapter-2/empathy/ricardo.png)`

| **Empathy Map — Ricardo Salazar Núñez** ||
|---|---|
| **¿Con quién empatizamos?** | Ricardo, jefe de almacén de una PyME de insumos alimentarios en Ate, responsable de dos naves y de la meta mensual de merma.<br>Su situación: debe garantizar la conservación del inventario con recursos limitados y sin instrumentación permanente. |
| **¿Qué necesita hacer?** | Saber en qué condición están sus zonas de almacenamiento en cualquier momento, también fuera de su horario.<br>Reaccionar antes de que la mercadería se dañe, no después.<br>Demostrar con datos a la gerencia qué ocurrió y qué se necesita para evitar que se repita.<br>Dejar de depender de que un auxiliar recuerde hacer la ronda. |
| **¿Qué ve?** | Naves amplias donde solo se controlan dos o tres puntos de forma esporádica.<br>Planillas de papel colgadas junto a la puerta, con casilleros en blanco.<br>Mercadería con signos de daño que ya no se puede recuperar.<br>Equipos de ventilación antiguos que a veces se apagan sin que nadie lo note.<br>Colegas de otras empresas que tampoco tienen una solución mejor. |
| **¿Qué dice?** | *"Yo me entero de que la humedad subió cuando ya se me arruinó el saco."*<br>*"Si el chico no hace la ronda, ese día no hay control y nadie se entera."*<br>*"Necesito algo simple; con otro sistema complicado no voy a poder."*<br>*"A mí me miden por la merma, así que el problema termina siendo mío."* |
| **¿Qué hace?** | Recorre el almacén apenas llega y revisa a criterio las zonas más expuestas.<br>Delega la medición al auxiliar de turno y la verifica cuando puede.<br>Coordina todo por WhatsApp, incluso los fines de semana.<br>Improvisa acciones correctivas cuando detecta un problema: ventila, abre puertas, reubica mercadería.<br>Digita las planillas en Excel al final del mes para el reporte a gerencia. |
| **¿Qué escucha?** | A la gerencia reclamando por el porcentaje de merma del mes.<br>A los operarios avisando tarde de que "algo huele raro" en una zona.<br>A colegas del rubro comentando sus propias pérdidas por humedad.<br>A proveedores que le ofrecen equipos costosos que su empresa no aprobaría. |
| **¿Qué piensa y siente?** | Piensa que el control actual es insuficiente, pero que no tiene alternativa a su alcance.<br>Siente frustración por enterarse siempre tarde y responsabilidad personal por una pérdida que no pudo prevenir.<br>Desconfía de las soluciones tecnológicas por implementaciones previas que quedaron a medias.<br>Le preocupa quedar expuesto ante la gerencia sin datos con los que defenderse. |
| **Pains (¿qué le preocupa?)** | Enterarse de las desviaciones cuando el daño ya es irreversible.<br>No tener ninguna visibilidad durante noches, fines de semana y feriados.<br>Depender de la disciplina de terceros para que el control se ejecute.<br>Registros sin valor probatorio que no le permiten sustentar nada.<br>Ser evaluado por una merma que no cuenta con medios para prevenir.<br>Temor a que una nueva herramienta sea compleja y termine abandonada. |
| **Gains (¿qué puede resolver su problema y convencerlo de que somos la alternativa correcta?)** | Una alerta en su celular en el momento exacto en que una zona sale de rango, con la zona identificada.<br>Ver el estado de todas las zonas en una sola pantalla, desde donde esté.<br>Cobertura continua que no dependa de que alguien recuerde hacer la ronda.<br>Un historial que le permita mostrar a la gerencia qué pasó y cuándo.<br>Instalación que su propio personal pueda ejecutar, sin obra ni técnicos.<br>Un costo que pueda aprobarse sin un proceso de inversión formal.<br>Que la información llegue al sistema que ya usa, sin sumar otra plataforma a su día. |

*Tabla 12. Empathy Map del User Persona Ricardo Salazar.*

#### Empathy Map — Andrea Quispe, Encargada de Control de Calidad

> **[ INSERTAR IMAGEN ]** — Empathy Map de Andrea Quispe elaborado en UXPressia
> `![Empathy Map Andrea](../assets/img/chapter-2/empathy/andrea.png)`

| **Empathy Map — Andrea Quispe Rojas** ||
|---|---|
| **¿Con quién empatizamos?** | Andrea, coordinadora de aseguramiento de la calidad de una PyME de procesamiento de alimentos en Lima Norte.<br>Su situación: sostiene prácticamente sola el sistema de calidad y responde por la evidencia documental ante auditorías y clientes. |
| **¿Qué necesita hacer?** | Contar con un registro continuo y verificable de las condiciones ambientales de cada zona.<br>Preparar la evidencia de una auditoría sin días de reconstrucción documental.<br>Poder responder con precisión qué ocurrió en una fecha y una zona determinadas.<br>Asegurar que el dato que firma sea confiable y auditable. |
| **¿Qué ve?** | Planillas con casilleros vacíos y valores repetidos que delatan llenado de memoria.<br>Hojas de cálculo con cientos de filas digitadas a mano.<br>Auditores que piden el detalle de períodos que ella no puede cubrir.<br>Instrumentos sin etiqueta de calibración vigente.<br>Empresas competidoras que ya exhiben certificaciones que su empresa aún no alcanza. |
| **¿Qué dice?** | *"Si no lo puedo demostrar con un registro, para el auditor simplemente no pasó."*<br>*"Me paso dos días digitando planillas que ni siquiera sé si son ciertas."*<br>*"Necesito saber de dónde sale cada dato para poder firmarlo."*<br>*"El formato está bien hecho; el problema es que depende de que alguien lo llene."* |
| **¿Qué hace?** | Diseña formatos de control y capacita al personal en su uso.<br>Persigue por WhatsApp y correo las planillas que le faltan.<br>Digita, ordena y grafica manualmente los datos del período.<br>Contrasta lecturas buscando inconsistencias antes de una auditoría.<br>Documenta no conformidades y da seguimiento a los planes de acción. |
| **¿Qué escucha?** | Al auditor preguntando por la continuidad y la trazabilidad del registro.<br>A clientes que exigen evidencia de las condiciones de conservación.<br>A la gerencia pidiendo la certificación sin asignar recursos adicionales.<br>Al personal de almacén explicando por qué no llenó la planilla ese día. |
| **¿Qué piensa y siente?** | Piensa que el sistema documental es frágil y que una auditoría exigente lo dejaría en evidencia.<br>Siente que invierte su tiempo en transcribir en lugar de analizar y mejorar.<br>Le preocupa que se cuestione la validez de un dato que ella respalda con su firma.<br>Aspira a ordenar el sistema de calidad y a que la empresa alcance la certificación. |
| **Pains (¿qué le preocupa?)** | Vacíos en el registro que resultan imposibles de subsanar de forma retroactiva.<br>Días completos consumidos en digitación manual sin valor agregado.<br>Sospecha permanente sobre la veracidad de los datos que consolida.<br>Registros manuales sin peso probatorio frente a un auditor.<br>Dependencia de terceros que no comparten su urgencia por el cumplimiento.<br>Riesgo de una no conformidad que comprometa una certificación o un cliente. |
| **Gains (¿qué puede resolver su problema y convencerla de que somos la alternativa correcta?)** | Un registro automático, continuo y con marca temporal por medición, sin intervención humana.<br>Reportes de trazabilidad por zona y período, exportables y listos para presentar a un auditor.<br>Historial íntegro que evidencie también los períodos sin conexión y su posterior sincronización.<br>Trazabilidad de cada alerta: cuándo se generó, quién la reconoció y qué acción correctiva se aplicó.<br>Registro del estado de calibración de cada nodo, para sustentar la validez del instrumento.<br>Recuperar el tiempo de digitación y dedicarlo al análisis de tendencias y a la mejora del proceso.<br>Umbrales configurados por producto y zona, vigentes y verificables en el sistema. |

*Tabla 13. Empathy Map del User Persona Andrea Quispe.*

---

## 2.4. Big Picture EventStorming

El equipo llevó a cabo una sesión colaborativa de Big Picture EventStorming con el objetivo de construir una primera comprensión compartida del dominio de negocio de MachineGuard. A diferencia de los artefactos anteriores, centrados en el usuario, este ejercicio se enfoca en el landscape del negocio: qué sucede a lo largo del tiempo, quién lo provoca, qué sistemas externos participan y en qué puntos existen dudas, tensiones u oportunidades que el equipo aún no ha resuelto. El resultado es la base sobre la que se construirá el Design-Level EventStorming y el descubrimiento de bounded contexts del Capítulo IV.

### Preparación y desarrollo de la sesión

La sesión se realizó de forma remota sobre un lienzo compartido, con la participación de los siete integrantes del equipo y una duración aproximada de dos horas. Se adoptó la convención de colores estándar de la técnica:

| Elemento | Significado en la sesión |
|---|---|
| **Nota naranja** | Domain Event: algo relevante que ya ocurrió en el dominio. Se redacta siempre en tiempo pasado ("Umbral Excedido", "Alerta Reconocida"). |
| **Nota azul claro** | Pivotal Event: evento que marca un cambio de fase y permite dividir la línea de tiempo en bloques con significado propio. |
| **Nota amarilla** | Actor: persona o rol del dominio que provoca o consume el evento. |
| **Nota rosada** | Sistema externo: componente ajeno al dominio con el que este interactúa. |
| **Nota morada (hot spot)** | Punto caliente: duda, tensión, riesgo o decisión pendiente que el equipo no logró resolver durante la sesión. |
| **Nota verde** | Oportunidad: idea de mejora o de negocio detectada durante la exploración. |

*Tabla 14. Convención de notas utilizada en la sesión de Big Picture EventStorming.*

El desarrollo siguió las cinco etapas del método:

1. **Kick-off y exploración desordenada.** Cada integrante escribió, sin coordinarse con los demás, todos los eventos de dominio que reconocía en el negocio del monitoreo ambiental, colocándolos libremente en el lienzo. Se obtuvieron más de setenta notas con abundante duplicación.
2. **Enforcing the timeline.** El equipo ordenó cronológicamente las notas, fusionó los duplicados, corrigió la redacción para dejar todos los eventos en tiempo pasado y descartó aquellos que resultaron ser acciones o intenciones en lugar de hechos consumados.
3. **Identificación de pain points y hot spots.** Sobre la línea de tiempo ya ordenada se marcaron con notas moradas las zonas donde el equipo discrepaba o donde no existía una respuesta clara, evitando la tentación de resolverlas en el momento.
4. **Identificación de pivotal events y división en fases.** Se seleccionaron los eventos que cambian el estado del negocio de forma irreversible y se utilizaron como frontera para dividir la línea de tiempo en seis fases.
5. **Walkthrough y validación.** El equipo recorrió la línea de tiempo en voz alta, de principio a fin, verificando que la narración fuera coherente y que cada evento tuviera un actor identificable. Este recorrido produjo los primeros términos que se formalizan en el Ubiquitous Language de la sección 2.5.

> **[ INSERTAR IMAGEN ]** — Captura del lienzo completo del Big Picture EventStorming
> `![Big Picture EventStorming](../assets/img/chapter-2/eventstorming/lienzo-completo.png)`
>
> *Figura 1. Lienzo consolidado del Big Picture EventStorming de MachineGuard.*

> **Recordatorio para el equipo:** deben reemplazarse los marcadores de imagen por capturas reales del lienzo: una vista general y, si el detalle no se aprecia, una captura por fase. El enunciado exige incluir capturas y explicaciones de las etapas del proceso, no únicamente el resultado final.

### Línea de tiempo del dominio

La tabla siguiente transcribe la línea de tiempo resultante. Los eventos se presentan en el orden en que fueron ordenados durante la sesión, agrupados por las fases que delimitan los pivotal events, identificados en la tabla con la marca «(pivotal event)».

| Fase | Domain Events (en pasado) | Actores | Sistemas externos |
|---|---|---|---|
| **Fase 1**<br>Provisión y despliegue | Cliente Registrado<br>Plan de Suscripción Contratado<br>Instalación Registrada<br>Zona de Monitoreo Creada<br>Umbrales de la Zona Configurados<br>**Nodo Sensor Vinculado a la Zona (pivotal event)**<br>Nodo Sensor Activado | Gerente de Operaciones<br>Jefe de Almacén<br>Encargado de Control de Calidad<br>Equipo comercial de MachineGuard | Pasarela de pagos<br>Servicio de correo transaccional |
| **Fase 2**<br>Captura y procesamiento en el borde | Lectura Capturada<br>Lectura Calibrada<br>Lectura Descartada por Anomalía<br>Lote de Lecturas Almacenado Localmente<br>Conexión con la Nube Restablecida<br>**Lote de Lecturas Sincronizado (pivotal event)** | Nodo sensor (dispositivo)<br>Servicio de borde | Red del cliente / proveedor de internet |
| **Fase 3**<br>Evaluación y alerta | Medición Registrada<br>Umbral Excedido<br>**Alerta Generada (pivotal event)**<br>Notificación Enviada<br>Notificación Entregada<br>Alerta Reconocida<br>Alerta Escalada por Falta de Respuesta | Jefe de Almacén<br>Gerente de Operaciones<br>Encargado de Control de Calidad | Servicio de notificaciones push<br>Servicio de mensajería / SMS<br>Servicio de correo<br>Servicio meteorológico externo |
| **Fase 4**<br>Respuesta operativa | Acción Correctiva Registrada<br>Condición Ambiental Normalizada<br>**Alerta Cerrada (pivotal event)**<br>Incidente Documentado<br>Producto No Conforme Identificado<br>Merma Registrada | Jefe de Almacén<br>Operario de almacén<br>Encargado de Control de Calidad | ERP del cliente |
| **Fase 5**<br>Trazabilidad y cumplimiento | Historial de Mediciones Consultado<br>**Reporte de Trazabilidad Generado (pivotal event)**<br>Reporte Exportado<br>Datos Consumidos por el ERP del Cliente<br>Evidencia Presentada en Auditoría<br>No Conformidad Levantada | Encargado de Control de Calidad<br>Auditor externo<br>Cliente de la empresa | ERP del cliente<br>Sistema documental del auditor |
| **Fase 6**<br>Continuidad del servicio | Nodo Reportado Sin Conexión<br>Batería Baja Detectada<br>Calibración del Nodo Vencida<br>Nodo Recalibrado<br>Nodo Reemplazado<br>**Suscripción Renovada (pivotal event)**<br>Suscripción Cancelada | Jefe de Almacén<br>Soporte de MachineGuard<br>Gerente de Operaciones | Pasarela de pagos<br>Servicio de correo transaccional |

*Tabla 15. Línea de tiempo del Big Picture EventStorming de MachineGuard.*

### Hot spots identificados

Los siguientes puntos calientes quedaron registrados durante la sesión. No se resolvieron en ese momento —la técnica prescribe explícitamente no hacerlo— pero constituyen la agenda de decisiones que el equipo debe cerrar antes del diseño táctico del Capítulo IV, y varios de ellos anticipan reglas de negocio que se convertirán en criterios de aceptación de las User Stories del Capítulo III.

| N.° | Hot spot (duda o tensión no resuelta) | Por qué importa |
|:---:|---|---|
| 1 | ¿Cuánto tiempo puede operar el servicio de borde sin conexión antes de que se considere pérdida de datos? | Define la capacidad de almacenamiento local exigida al Edge Service y el compromiso de continuidad que se puede prometer comercialmente. |
| 2 | ¿Quién está autorizado a modificar los umbrales de una zona: el jefe de almacén o solo el encargado de calidad? | Determina el modelo de roles y permisos, y tiene implicancias sobre el valor probatorio del registro ante una auditoría. |
| 3 | Si una condición se normaliza sola, ¿la alerta se cierra automáticamente o requiere reconocimiento humano? | Impacta directamente en la trazabilidad: un cierre automático simplifica la operación, pero elimina la evidencia de que alguien tomó conocimiento del evento. |
| 4 | ¿Cuántos reintentos de notificación se realizan y tras cuánto tiempo sin reconocimiento se escala la alerta? | Define la política de escalamiento y evita tanto la desatención de un evento crítico como la saturación del usuario. |
| 5 | ¿Cómo se distingue una desviación real de una lectura anómala producida por un sensor descalibrado o defectuoso? | Es el mayor riesgo del producto: las falsas alarmas erosionan la confianza más rápido que cualquier otra falla. |
| 6 | ¿Qué ocurre con el historial cuando el cliente cancela su suscripción? | Involucra una decisión de negocio, una expectativa del usuario y una obligación de protección de datos. |
| 7 | ¿Qué peso probatorio tiene el historial ante un auditor si el nodo no cuenta con calibración certificada? | Delimita hasta dónde puede llegar la promesa comercial dirigida al segmento de control de calidad. |
| 8 | ¿Debe el sistema registrar la acción correctiva aplicada o basta con dejar constancia de la normalización? | Condiciona el alcance del bounded context de incidentes y la profundidad del reporte de trazabilidad. |
| 9 | ¿La información meteorológica externa se usa solo como contexto o llega a alimentar alertas predictivas? | Define el rol del servicio de terceros dentro del dominio y el alcance de una futura funcionalidad predictiva. |
| 10 | ¿Un nodo sin conexión debe generar una alerta propia y de qué severidad? | Un sensor silencioso puede ser más peligroso que uno que reporta valores fuera de rango, porque genera una falsa sensación de normalidad. |

*Tabla 16. Hot spots registrados durante la sesión de Big Picture EventStorming.*

### Oportunidades detectadas

Durante el recorrido final de la línea de tiempo, el equipo identificó además las siguientes oportunidades de negocio y de producto, que se recogen como insumo para el Impact Mapping del Capítulo III:

- El evento Merma Registrada permite calcular de forma automática el retorno de la inversión del cliente, lo que convierte el propio producto en su mejor argumento de renovación.
- El evento Calibración del Nodo Vencida abre la puerta a un servicio recurrente de verificación y recalibración, con ingresos adicionales a la suscripción.
- El evento Datos Consumidos por el ERP del Cliente sugiere un modelo de alianza con proveedores de ERP locales, en el que MachineGuard opera como módulo complementario.
- La acumulación de mediciones por sector y por zona geográfica habilita, a mediano plazo, la generación de valores de referencia comparativos entre empresas del mismo rubro.
- El evento Evidencia Presentada en Auditoría revela la posibilidad de ofrecer plantillas de reporte preconfiguradas según el estándar de inocuidad que aplique a cada cliente.

---

## 2.5. Ubiquitous Language

El siguiente glosario recoge los términos y conceptos del dominio de negocio de MachineGuard —el monitoreo ambiental de zonas de almacenamiento y producción— con definiciones unívocas acordadas por el equipo durante el walkthrough del Big Picture EventStorming. Siguiendo la recomendación de Eric Evans, se trata exclusivamente de lenguaje del dominio: no se incluyen términos técnicos de ingeniería de software, que corresponden al diseño de la solución y se abordan en el Capítulo IV. Los términos se consignan en inglés, con su equivalente en español entre paréntesis, y su definición en español.

Mantener este glosario vigente permite que todos los integrantes del equipo y los stakeholders se refieran a lo mismo con las mismas palabras, y que esas palabras sean las que después aparezcan en las User Stories, en los nombres de los eventos de dominio y en la interfaz del producto. El glosario se irá ampliando a lo largo del ciclo de vida del proyecto conforme el equipo profundice en el dominio.

| Término (Term) | Definición |
|---|---|
| **Monitored Facility** (Instalación monitoreada) | Sede física del cliente —planta de producción, almacén o centro de acopio— dentro de la cual se despliega el servicio de monitoreo. Una instalación agrupa una o más zonas de monitoreo. |
| **Monitoring Zone** (Zona de monitoreo) | Espacio delimitado dentro de una instalación que comparte un mismo comportamiento ambiental esperado y un mismo conjunto de umbrales, por ejemplo una cámara, un pasillo de estantería o una sala de acondicionamiento. |
| **Monitoring Point** (Punto de monitoreo) | Ubicación específica dentro de una zona donde se instala un nodo sensor. Es la unidad sobre la que se calcula el precio del servicio. |
| **Sensor Node** (Nodo sensor) | Dispositivo instalado en un punto de monitoreo que captura de forma periódica los valores de las variables ambientales de ese punto. |
| **Environmental Variable** (Variable ambiental) | Magnitud física del entorno cuya evolución afecta la conservación del producto almacenado. En el alcance actual: temperatura y humedad relativa. |
| **Reading** (Lectura) | Valor bruto capturado por un nodo sensor en un instante determinado, antes de ser calibrado o validado. |
| **Measurement** (Medición) | Lectura ya calibrada y validada, asociada a un punto de monitoreo y a una marca temporal, que se incorpora al historial del cliente. |
| **Sampling Interval** (Intervalo de muestreo) | Tiempo que transcurre entre dos capturas consecutivas de un mismo nodo sensor. Determina la resolución del historial y la rapidez con que puede detectarse una desviación. |
| **Calibration** (Calibración) | Procedimiento mediante el cual se contrasta la respuesta de un nodo sensor contra una referencia conocida y se determina el ajuste que debe aplicarse a sus lecturas. |
| **Calibration Offset** (Ajuste de calibración) | Corrección que se aplica a la lectura bruta de un nodo para obtener la medición definitiva, resultante del procedimiento de calibración. |
| **Threshold** (Umbral) | Valor límite, superior o inferior, definido para una variable ambiental dentro de una zona. Su cruce constituye el hecho que da origen a una alerta. |
| **Safe Range** (Rango seguro) | Intervalo comprendido entre el umbral inferior y el superior de una variable, dentro del cual las condiciones se consideran adecuadas para el producto almacenado. |
| **Deviation** (Desviación) | Situación en la que una medición se sitúa fuera del rango seguro definido para su zona. |
| **Excursion** (Excursión) | Período completo durante el cual una variable permaneció fuera del rango seguro, delimitado por el momento en que salió y aquel en que retornó. Es la unidad que se evalúa en una auditoría, más que la medición aislada. |
| **Alert** (Alerta) | Aviso generado por el sistema al detectarse una desviación, dirigido a los responsables de la zona afectada. |
| **Alert Severity** (Severidad de la alerta) | Nivel de criticidad asignado a una alerta según la magnitud de la desviación y el tiempo que lleva sostenida. |
| **Acknowledgement** (Reconocimiento) | Acto por el cual un responsable declara haber tomado conocimiento de una alerta, quedando registrados su identidad y el momento en que lo hizo. |
| **Escalation** (Escalamiento) | Elevación de una alerta hacia un responsable de mayor jerarquía cuando no ha sido reconocida dentro del plazo previsto. |
| **Corrective Action** (Acción correctiva) | Intervención ejecutada para devolver una zona a su rango seguro, como ajustar la climatización, ventilar o reubicar la mercadería. |
| **Incident** (Incidente) | Registro consolidado de una excursión: incluye la desviación detectada, la alerta generada, su reconocimiento, la acción correctiva aplicada y su resultado. |
| **Shrinkage** (Merma) | Pérdida de producto o insumo atribuible a condiciones ambientales fuera de rango, expresada en unidades o en valor monetario. |
| **Non-conforming Product** (Producto no conforme) | Producto que, por haber estado expuesto a condiciones fuera de rango, no cumple los requisitos de calidad establecidos y debe ser separado, reprocesado o descartado. |
| **Batch** (Lote) | Conjunto de unidades de producto elaboradas o recibidas bajo las mismas condiciones, que constituye la unidad de trazabilidad frente a un cliente o un auditor. |
| **Traceability** (Trazabilidad) | Capacidad de reconstruir las condiciones ambientales a las que estuvo expuesto un producto durante todo el período en que permaneció almacenado. |
| **Measurement History** (Historial de mediciones) | Serie temporal completa e íntegra de las mediciones de un punto de monitoreo, conservada durante el período de retención contratado. |
| **Traceability Report** (Reporte de trazabilidad) | Documento generado a partir del historial que resume las condiciones de una zona en un período determinado, junto con las excursiones ocurridas y su tratamiento. Es la evidencia que se presenta en una auditoría. |
| **Audit** (Auditoría) | Revisión, interna o externa, en la que se verifica que la empresa cumple los estándares comprometidos, incluida la conservación adecuada de sus productos. |
| **Non-conformity** (No conformidad) | Hallazgo formal de una auditoría que señala el incumplimiento de un requisito y obliga a la empresa a ejecutar y documentar un plan de acción. |
| **Inspection Round** (Ronda de inspección) | Recorrido manual periódico en el que un operario mide las condiciones ambientales con un instrumento portátil y las anota en un formato. Es el procedimiento vigente que la solución sustituye. |
| **Thermohygrometer** (Termohigrómetro) | Instrumento portátil de medición puntual de temperatura y humedad relativa, utilizado en las rondas de inspección manuales. |
| **Cold Chain** (Cadena de frío) | Secuencia ininterrumpida de etapas de almacenamiento y transporte en condiciones controladas de temperatura que debe mantener un producto sensible desde su origen hasta su destino. |
| **Offline Node** (Nodo sin conexión) | Nodo sensor que ha dejado de reportar mediciones dentro del intervalo esperado, lo que genera un vacío en el historial de su punto de monitoreo. |
| **Subscription Plan** (Plan de suscripción) | Modalidad contratada por el cliente que determina el número de puntos de monitoreo habilitados, el período de retención del historial y los canales de notificación disponibles. |
| **Retention Period** (Período de retención) | Tiempo durante el cual el historial de mediciones permanece disponible para consulta y exportación, según el plan contratado. |
