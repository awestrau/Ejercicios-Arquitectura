---
title: "Arquitectura de Software de Spotify"
sub_title: "Investigación de Arquitectura y Rol del Arquitecto de Software"
authors:
  - Daniel Alberto Serrano Mora
  - Andrés Jose Westra Ureña
  - Carlos Andres Fuentes Santos
  - Erick Andre Campos Sibaja
  - Michael Ramírez González
theme:
  name: dark
options:
  implicit_slide_ends: false
  end_slide_shorthand: false
  auto_render_languages:
    - mermaid
---

<!-- new_lines: 1 -->
# Agenda

- Sección 1: Investigación de arquitectura de Spotify
- Sección 2: Rol del arquitecto de software actual
- Sección 3: El rol del arquitecto en el futuro

<!--
speaker_note: |
  Buenas. Hoy presentamos nuestra investigación sobre la arquitectura de software de Spotify y el rol del arquitecto de software.
  El trabajo se estructura en tres secciones: la arquitectura actual de Spotify, el estado del rol del arquitecto en el mercado laboral, y cómo evolucionará este rol en la próxima década.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Objetivo del Trabajo

- Investigar la **arquitectura de software de Spotify** como plataforma digital de escala global
- Analizar el **rol actual del arquitecto de software** en el mercado laboral
- Proponer la **evolución del rol en los próximos 10 años** considerando inteligencia artificial y computación cuántica

<!--
speaker_note: |
  El objetivo de este trabajo es triple. Primero, investigar a fondo la arquitectura de Spotify, una de las plataformas de streaming más grandes del mundo.
  Segundo, analizar el rol actual del arquitecto de software mediante perfiles reales del mercado laboral.
  Y tercero, proyectar cómo evolucionará este rol en la próxima década frente a la inteligencia artificial y la computación cuántica.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Introducción

El presente trabajo analiza la **arquitectura de software de Spotify** desde tres perspectivas:

- **Sección 1**: Arquitectura actual — microservicios, tecnologías, datos, seguridad
- **Sección 2**: Rol del arquitecto de software en el mercado laboral actual
- **Sección 3**: Evolución del rol en la próxima década (IA, computación cuántica)

<!--
speaker_note: |
  Spotify es una de las plataformas de streaming de audio más grandes del mundo.
  Con más de 600 millones de usuarios activos mensuales y un catálogo superior a los 100 millones de pistas, representa un caso de estudio paradigmático sobre cómo una arquitectura bien diseñada habilita innovación continua y escalabilidad global.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Spotify en Cifras

- **600M+** usuarios activos mensuales
- **100M+** pistas en el catálogo
- **~1.500** microservicios
- **250+** squads autónomos
- **1 billón+** de eventos diarios procesados
- Infraestructura sobre **Google Cloud Platform**

<!--
speaker_note: |
  Estas cifras nos dan una idea de la escala a la que opera Spotify.
  Mil quinientos microservicios gestionados por más de 250 equipos autónomos, procesando más de un billón de eventos al día.
  Toda esta infraestructura corre sobre Google Cloud Platform desde 2018.

  Términos clave:
  - Microservicio: Servicio pequeño, autónomo e independiente que implementa una funcionalidad de negocio específica. Se despliega y escala de forma aislada.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 1: Arquitectura de Spotify
## Tipo de Arquitectura

- **Microservicios** (~1.500 servicios independientes)
- **Event-Driven** con Apache Kafka como backbone
- **Comunicación dual**: REST/gRPC (síncrono) + Kafka/PubSub (asíncrono)
- **Backend for Frontend**: APIs por tipo de cliente
- **API Gateway**: Apollo GraphQL
- **Strangler Fig**: migración progresiva desde el monolito

<!--
speaker_note: |
  Entremos en la primera sección. Spotify opera bajo una arquitectura de microservicios con un modelo event-driven.
  La comunicación entre sus 1.500 servicios se resuelve mediante canales síncronos como REST y gRPC, y asíncronos con Apache Kafka como backbone principal.
  Utilizan Apollo GraphQL como API Gateway unificado para todos los clientes y aplican el patrón Strangler Fig para migrar progresivamente desde el monolito original.

  Términos clave:
  - Arquitectura event-driven: Estilo donde los componentes se comunican publicando y consumiendo eventos en lugar de llamadas directas. Favorece el desacoplamiento.
  - REST: Estilo de API sobre HTTP que usa verbos (GET, POST, PUT, DELETE) para operar sobre recursos.
  - gRPC: Framework de comunicación de alta eficiencia desarrollado por Google. Usa Protocol Buffers para serialización binaria, más rápido que REST con JSON.
  - API Gateway: Punto único de entrada para todos los clientes que enruta peticiones a los microservicios internos y maneja autenticación, rate limiting y composición de respuestas.
  - Strangler Fig (patrón): Estrategia de migración donde nuevas funcionalidades se construyen como microservicios y el tráfico se redirige incrementalmente desde el monolito hasta que este queda vacío.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 1: Arquitectura de Spotify
## Patrones Arquitectónicos

- **Microservicios**: despliegue y BD independientes por servicio
- **Event-Driven**: Kafka procesa billones de eventos/día
- **CQRS**: separación lectura/escritura en catálogo y playlists
- **Saga / Coreografía**: transacciones distribuidas sin orquestador
- **CDN / Edge Delivery**: distribución global multi-proveedor

<!--
speaker_note: |
  Entre los patrones identificados destacan CQRS para separar modelos de lectura y escritura en servicios de alta demanda como el catálogo y las playlists.
  Las transacciones distribuidas se coordinan mediante el patrón Saga con coreografía de eventos, sin un orquestador central.
  La distribución de contenido se apoya en una CDN multi-proveedor con puntos de presencia propios.

  Términos clave:
  - CQRS (Command Query Responsibility Segregation): Patrón que separa el modelo de escritura (comandos) del de lectura (consultas), permitiendo optimizar cada uno de forma independiente.
  - Saga / Coreografía: Mecanismo para manejar transacciones distribuidas sin un orquestador central. Cada servicio publica eventos y los demás reaccionan; si algo falla, se emiten eventos compensatorios.
  - CDN (Content Delivery Network): Red de servidores distribuidos globalmente que almacenan copias del contenido cerca de los usuarios para reducir latencia.
  - Edge Delivery: Estrategia de servir contenido desde el punto de presencia más cercano al usuario, en el borde de la red.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 1: Arquitectura de Spotify
## Tecnologías Principales — Lenguajes

- **Java** + Spring Boot: backend predominante
- **Python**: ML, ciencia de datos, automatización
- **C++**: motor de streaming, reproductores nativos
- **Scala** (Scio): pipelines de datos sobre Apache Beam
- **TypeScript / Node.js**: frontend web y servicios ligeros

<!--
speaker_note: |
  Spotify es notablemente políglota. Java con Spring Boot domina el backend de los microservicios core. Python, el lenguaje original del monolito, hoy se usa en machine learning. C++ maneja los componentes de baja latencia como el motor de streaming. Scala, mediante Scio, procesa los pipelines de datos a escala.

  Términos clave:
  - Políglota (en arquitectura): Enfoque donde cada componente usa el lenguaje o tecnología más adecuado para su caso de uso, en lugar de imponer uno único para todo el sistema.
  - Spring Boot: Framework de Java que simplifica la creación de microservicios con configuración automática y servidor embebido.
  - Scio: Librería desarrollada por Spotify sobre Apache Beam para procesamiento de datos a gran escala en Scala.
  - Apache Beam: Modelo unificado de programación para definir pipelines de procesamiento de datos que pueden ejecutarse en múltiples motores como Dataflow, Spark o Flink.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 1: Arquitectura de Spotify
## Tecnologías — Infraestructura

- **Google Cloud Platform**: migración total desde datacenters propios (2016-2018)
- **Kubernetes** (GKE): orquestación de contenedores
- **Docker**: empaquetado estándar
- **CDN multi-proveedor**: puntos de presencia propios
- **CI/CD**: Jenkins + Spinnaker

<!--
speaker_note: |
  Entre 2016 y 2018 Spotify migró desde aproximadamente 5.000 servidores físicos en datacenters propios hacia Google Cloud Platform, en una de las migraciones a la nube más significativas de la industria.
  Hoy opera sobre Kubernetes con Docker, utiliza una CDN multi-proveedor y pipelines de CI/CD con Jenkins y Spinnaker.
  Para orquestación de batch jobs y flujos de datos utilizan Luigi y Styx, herramientas internas que automatizan procesos de ETL a escala global.

  Términos clave:
  - Kubernetes (K8s): Plataforma de código abierto para orquestar contenedores: automatiza despliegue, escalado y manejo de cargas de trabajo.
  - Docker: Tecnología que empaqueta aplicaciones con todas sus dependencias en contenedores ligeros y portables que se ejecutan de forma aislada.
  - CI/CD (Continuous Integration / Continuous Delivery): Prácticas de automatización: la integración continua valida cada cambio con pruebas automáticas; la entrega continua despliega a producción de forma automatizada.
  - Jenkins: Servidor de automatización de código abierto para construir, probar y desplegar software (pipelines CI/CD).
  - Spinnaker: Plataforma de entrega continua multi-cloud creada por Netflix. Soporta despliegues canary, blue/green y rollbacks.
  - ETL (Extract, Transform, Load): Proceso de mover datos desde fuentes externas, transformarlos a un formato adecuado y cargarlos en el destino.
  - Luigi: Framework de Python creado por Spotify para construir pipelines de datos complejos con manejo de dependencias, reintentos y visualización.
  - Styx: Orquestador de workflows de datos desarrollado internamente por Spotify para ejecutar pipelines de ETL a escala.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 1: Arquitectura de Spotify
## Tecnologías — Almacenamiento y Mensajería

- **Apache Cassandra** (~3.000 nodos): catálogo, metadatos
- **PostgreSQL**: usuarios, cuentas, facturación
- **BigTable / BigQuery**: métricas y analítica
- **Redis / Memcached**: caché distribuida
- **Elasticsearch**: búsqueda full-text
- **Apache Kafka**: >1 billón de eventos/día

<!--
speaker_note: |
  La estrategia de almacenamiento es poliglota: Cassandra con unos 3.000 nodos aloja los metadatos del catálogo, PostgreSQL gestiona datos relacionales, Redis y Memcached proporcionan la capa de caché esencial para latencias inferiores a 200 ms, y Elasticsearch indexa el catálogo para búsqueda.
  Apache Kafka es el sistema nervioso de la plataforma, procesando más de un billón de eventos al día.

  Términos clave:
  - Apache Cassandra: Base de datos NoSQL distribuida y masterless (sin nodo maestro) diseñada para alta disponibilidad, escalabilidad horizontal y replicación multirregión.
  - PostgreSQL: Sistema de base de datos relacional de código abierto con soporte ACID, extensibilidad y fuerte consistencia.
  - Redis: Base de datos en memoria usada como caché distribuida y broker de mensajería. Proporciona latencias de microsegundos.
  - Memcached: Sistema de caché distribuido en memoria para acelerar aplicaciones web reduciendo la carga sobre bases de datos.
  - Elasticsearch: Motor de búsqueda y analítica distribuido basado en Apache Lucene. Indexa datos en tiempo real para búsquedas full-text.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 1: Arquitectura de Spotify
## Componentes de la Solución

<!-- new_lines: 4 -->
```mermaid +render
mindmap
  root((Spotify))
    Catálogo Musical
    Motor de Recomendaciones
    Streaming de Audio
    Publicidad
    Social y Playlists
    Autenticación y Usuarios
    Plataforma de Datos
    Content Platform
```

<!--
speaker_note: |
  La funcionalidad de Spotify se articula en ocho dominios de servicios core.
  Veamos brevemente cada uno.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 1: Arquitectura de Spotify
## Catálogo y Recomendaciones

<!-- column_layout: [1, 1] -->

<!-- column: 0 -->

**Catálogo Musical**
- Metadata Service sobre Cassandra
- Audio Features Pipeline (ML)
- Search Service con Elasticsearch
- Cover Art Service

<!-- column: 1 -->

**Motor de Recomendaciones**
- BaRT: Bandits multi-armados
- Discover Weekly / Release Radar
- Daily Mix
- Taste Profile: vectores densos
- DJ AI (2023): IA generativa

<!-- reset_layout -->

<!--
speaker_note: |
  El catálogo musical con más de 100 millones de pistas se apoya en Cassandra para metadatos y Elasticsearch para búsqueda.
  El motor de recomendaciones es el diferenciador competitivo más importante: BaRT equilibra explotación y exploración, y productos como Discover Weekly combinan filtrado colaborativo con procesamiento de lenguaje natural.
  Spotify utiliza un enfoque "Algotorial" que fusiona curaduría editorial humana con señales algorítmicas, logrando un balance entre relevancia computacional y criterio experto.
  En 2023 lanzaron DJ AI, que utiliza inteligencia artificial generativa y voz sintética.

  Términos clave:
  - BaRT (Bandits for Recommendations as Treatments): Sistema de recomendación basado en bandits multi-armados que balancean explotación (recomendar lo conocido) y exploración (descubrir nuevo contenido).
  - Filtrado colaborativo: Técnica de recomendación que predice preferencias de un usuario basándose en patrones de usuarios similares.
  - Algotorial: Enfoque híbrido de Spotify que combina curaduría editorial humana con señales algorítmicas de machine learning.
  - Taste Profile: Representación vectorial densa de las preferencias musicales de un usuario, generada por redes neuronales profundas.
  - DJ AI: Funcionalidad de Spotify (2023) que usa IA generativa y voz sintética como curador personalizado que selecciona y comenta canciones.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 1: Arquitectura de Spotify
## Streaming y Publicidad

<!-- column_layout: [1, 1] -->

<!-- column: 0 -->

**Streaming de Audio**
- Códecs: OGG Vorbis, AAC
- VBR adaptativo a red
- Spotify Connect
- Offline con cifrado local

<!-- column: 1 -->

**Publicidad (Free Tier)**
- Ad Insertion dinámico
- Segmentación de audiencias
- Subasta en tiempo real
- Analytics de atribución

<!-- reset_layout -->

<!--
speaker_note: |
  El streaming utiliza códecs OGG Vorbis y AAC con bitrate variable que se adapta dinámicamente a las condiciones de red.
  Spotify Connect sincroniza el estado entre dispositivos.
  En el nivel gratuito, el Ad Insertion Service inserta anuncios dinámicamente con segmentación avanzada y subasta en tiempo real.

  Términos clave:
  - OGG Vorbis: Códec de audio comprimido de código abierto y sin patentes. Ofrece mejor calidad que MP3 a la misma tasa de bits.
  - AAC (Advanced Audio Coding): Códec de audio con pérdida diseñado como sucesor del MP3. Usado por YouTube, Apple y Spotify.
  - VBR (Variable Bitrate): Técnica de codificación que ajusta dinámicamente la tasa de bits según la complejidad del audio.
  - Spotify Connect: Protocolo que permite sincronizar y transferir la reproducción entre dispositivos de forma fluida.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 1: Arquitectura de Spotify
## Social, Auth y Plataforma de Datos

<!-- column_layout: [1, 1] -->

<!-- column: 0 -->

**Social y Playlists**
- Playlist Service + CRDTs
- Social Graph
- Collaborative Playlists
- Activity Feed

**Auth y Usuarios**
- OAuth 2.0 + SSO
- Planes: Free, Premium, Duo, Family
- Payment Service multi-país

<!-- column: 1 -->

**Plataforma de Datos**
- ~1 billón eventos/día
- Scio (Scala + Apache Beam)
- Feature Store centralizado
- ABBA: experimentación A/B masiva

<!-- reset_layout -->

<!--
speaker_note: |
  Las playlists colaborativas utilizan CRDTs para permitir edición simultánea sin servidor central de coordinación.
  La plataforma ABBA permite ejecutar experimentos A/B a escala de cientos de millones de usuarios simultáneamente, siendo una de las infraestructuras de experimentación más grandes del mundo.

  Términos clave:
  - CRDT (Conflict-free Replicated Data Type): Estructura de datos replicada que permite ediciones concurrentes por múltiples usuarios sin necesidad de coordinación central.
  - Experimentos A/B: Metodología que compara dos versiones de una funcionalidad con usuarios reales para medir cuál produce mejores resultados.
  - Feature Store: Repositorio centralizado de características para entrenar y servir modelos de machine learning.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 1: Arquitectura de Spotify
## Content Platform

<!-- column_layout: [1, 1] -->

<!-- column: 0 -->

**Ingestión de Contenido**
- Ingestion Pipeline automatizado
- Recepción de sellos discográficos
- Distribuidores digitales integrados

<!-- column: 1 -->

**Herramientas para Creadores**
- Spotify for Artists
- Spotify for Labels
- Spotify for Podcasters
- Rights Management (regalías, licencias)

<!-- reset_layout -->

<!--
speaker_note: |
  Spotify opera también una plataforma de contenido completa. El Ingestion Pipeline automatiza la recepción de material desde sellos discográficos y distribuidores digitales.
  Spotify for Artists, Labels y Podcasters son los portales donde los creadores gestionan su presencia en la plataforma.
  El Rights Management gestiona los derechos de autor, licencias, y el cálculo y liquidación de regalías a los titulares de derechos — una pieza crítica del ecosistema.

  Términos clave:
  - Ingestion Pipeline: Flujo automatizado que recibe, valida, transforma y almacena contenido desde fuentes externas.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 1: Arquitectura de Spotify
## Evolución Histórica

- **Fase 1 — Monolito (2006-2012)**: Python/Django, C++, PostgreSQL
- **Fase 2 — SOA incipiente (2012-2015)**: Cassandra, Kafka, Docker, squads
- **Fase 3 — Cloud + microservicios (2016-2018)**: GCP, Kubernetes, ~1.500 servicios
- **Fase 4 — Madurez (2018-2020)**: GraphQL, gRPC, Backstage open source
- **Fase 5 — Expansión e IA (2021-presente)**: Podcast, DJ AI, >1B eventos/día

<!--
speaker_note: |
  La arquitectura de Spotify ha transitado por cinco fases diferenciadas.
  Partió de un monolito Python/Django en 2006, evolucionó hacia SOA con Cassandra y Kafka, migró integralmente a Google Cloud entre 2016 y 2018, maduró con GraphQL y Backstage, y hoy se expande con IA generativa y nuevos formatos como podcast y audiolibros.

  Términos clave:
  - Monolito: Aplicación donde toda la funcionalidad reside en un solo codebase y se despliega como una unidad. Simple al inicio, difícil de escalar con equipos grandes.
  - SOA (Service-Oriented Architecture): Estilo arquitectónico donde las funcionalidades se dividen en servicios que se comunican mediante protocolos estándar. Precursor de los microservicios.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 1: Arquitectura de Spotify
## Manejo de Datos

- **PostgreSQL**: perfiles, suscripciones, facturación
- **Apache Cassandra**: historial de reproducción, eventos masivos
- **Apache Kafka**: cada interacción genera eventos → consumidores
- Procesamiento en **tiempo real** sin afectar experiencia del usuario
- Desacoplamiento total entre microservicios

<!--
speaker_note: |
  Spotify utiliza una arquitectura de datos distribuida donde cada motor se especializa en un tipo específico de información.
  Cada interacción del usuario genera eventos en Kafka que alimentan sistemas analíticos, motores de recomendación y procesos de machine learning, todo en tiempo real y sin afectar la experiencia del usuario.

  Términos clave:
  - Machine Learning (ML): Rama de la IA donde los sistemas aprenden patrones a partir de datos sin ser explícitamente programados.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 1: Arquitectura de Spotify
## Seguridad

- **OAuth 2.0**: autenticación de usuarios y apps de terceros
- **Mínimo privilegio**: cada microservicio accede solo a lo necesario
- **TLS + HTTPS**: cifrado en tránsito
- **AES-256**: cifrado en reposo
- **DRM**: protección de contenido con licencias y restricciones geográficas

<!--
speaker_note: |
  La seguridad es un pilar fundamental. Spotify implementa OAuth 2.0 para autenticación, políticas de mínimo privilegio entre microservicios, cifrado AES-256 en reposo y TLS en tránsito, además de DRM para proteger los derechos de autor del contenido.

  Términos clave:
  - OAuth 2.0: Protocolo de autorización (RFC 6749) que permite a aplicaciones acceder a recursos protegidos mediante tokens temporales sin exponer credenciales.
  - AES-256 (Advanced Encryption Standard): Algoritmo de cifrado simétrico con clave de 256 bits. Estándar gubernamental para proteger datos en reposo.
  - TLS (Transport Layer Security): Protocolo criptográfico que garantiza comunicación segura sobre redes (HTTPS). Cifra datos en tránsito y autentica servidores.
  - DRM (Digital Rights Management): Tecnologías que controlan el acceso y uso de contenido digital protegido por derechos de autor.
  - Mínimo privilegio (principio de): Cada componente o usuario debe tener solo los permisos estrictamente necesarios para su función.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 1: Arquitectura de Spotify
## Escalabilidad y Disponibilidad

<!-- column_layout: [1, 1] -->

<!-- column: 0 -->

**Escalabilidad**
- Kubernetes con autoescalado
- CDN multi-proveedor global
- Sharding de bases de datos
- Escalado independiente por servicio

<!-- column: 1 -->

**Disponibilidad**
- Replicación multi-datacenter
- Self-healing con Kubernetes
- Prometheus + Grafana
- Backups periódicos
- Recuperación ante desastres

<!-- reset_layout -->

<!--
speaker_note: |
  Para la escalabilidad, Spotify utiliza autoescalado de Kubernetes, CDN global y sharding de bases de datos.
  Para la disponibilidad, replica datos en múltiples centros distribuidos geográficamente, Kubernetes reinicia servicios fallidos automáticamente, y herramientas como Prometheus y Grafana permiten monitoreo continuo y detección rápida de incidentes.
  En observabilidad, Spotify desarrolló herramientas internas como Heroic y Ffwd —liberadas como open source— para manejar series temporales a la escala de cientos de millones de usuarios.

  Términos clave:
  - Autoescalado (autoscaling): Capacidad de añadir o quitar automáticamente recursos computacionales según la demanda. Kubernetes lo gestiona nativamente.
  - Sharding: Técnica de particionado de bases de datos donde los datos se dividen en fragmentos distribuidos entre múltiples servidores.
  - Prometheus: Sistema de monitoreo y alertas de código abierto que recolecta métricas en series temporales. Ampliamente usado en entornos cloud nativos.
  - Grafana: Plataforma de visualización para métricas, logs y trazas. Se integra con Prometheus y permite crear dashboards interactivos.
  - Heroic: Sistema de series temporales desarrollado por Spotify sobre Cassandra y Elasticsearch. Liberado como open source.
  - Ffwd: Herramienta interna de Spotify para manejo de series temporales a gran escala. Liberada como open source.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 1: Arquitectura de Spotify
## Diagrama de Arquitectura

![Diagrama Parte1](img/spotify_architecture_part1.png)

<!--
speaker_note: |
  Este es el diagrama de arquitectura de Spotify, que muestra la estructura de microservicios, los flujos de datos y las integraciones entre los distintos componentes de la plataforma.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 1: Arquitectura de Spotify
## Diagrama de Arquitectura (2)

![Diagrama Parte2](img/spotify_architecture_part2.png)

<!--
speaker_note: |
  La segunda parte del diagrama detalla componentes adicionales de la plataforma, incluyendo los sistemas de recomendación, procesamiento de datos y distribución de contenido.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 1: Arquitectura de Spotify
## Decisiones Arquitectónicas Clave
### ADR-1: Monolito → Microservicios

- **Contexto**: monolito Python/Django era cuello de botella organizacional
- **Decisión**: descomponer en microservicios independientes
- **Resultado**: 250+ squads despliegan en paralelo, time-to-market reducido
- **Contrapartida**: complejidad en orquestación, service discovery, consistencia eventual

<!--
speaker_note: |
  Ahora analizamos las decisiones arquitectónicas más importantes que dieron forma a Spotify, documentadas como Architecture Decision Records.
  Hacia 2012, un solo codebase impedía que múltiples equipos desplegaran de forma independiente. La descomposición en microservicios permitió que más de 250 squads desarrollaran en paralelo.
  La contrapartida fue una complejidad operativa significativa que Spotify gestionó construyendo herramientas como Backstage.

  Términos clave:
  - ADR (Architecture Decision Record): Documento breve que registra una decisión arquitectónica, su contexto, alternativas y consecuencias. Vive en el repositorio junto al código.
  - Codebase: Conjunto completo de código fuente que compone un proyecto. En un monolito, todos los equipos comparten un solo codebase.
  - Backstage: Portal de desarrolladores open source creado por Spotify (CNCF). Centraliza catálogo de servicios, documentación viva y plantillas.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 1: Arquitectura de Spotify
## Decisiones Arquitectónicas Clave
### ADR-2: Google Cloud | ADR-3: Cassandra

<!-- column_layout: [1, 1] -->

<!-- column: 0 -->

**ADR-2: Google Cloud**
- 5.000 servidores → GCP
- Reducción de TCO
- Acceso a servicios managed
- Riesgo: vendor lock-in

<!-- column: 1 -->

**ADR-3: Cassandra**
- PostgreSQL no escalaba horizontalmente
- Arquitectura masterless
- Replicación multirregión
- Contrapartida: consistencia eventual

<!-- reset_layout -->

<!--
speaker_note: |
  Mantener datacenters propios desviaba talento hacia tareas de bajo valor. La migración a GCP permitió reducir costos y acceder a BigQuery y Dataflow.
  PostgreSQL no escalaba al ritmo del catálogo, así que Cassandra aportó escalabilidad horizontal sin punto único de fallo, aunque a costa de un modelo de consistencia eventual.

  Términos clave:
  - TCO (Total Cost of Ownership): Costo total de poseer y operar un sistema durante toda su vida útil: hardware, licencias, personal, energía, mantenimiento.
  - BigQuery: Data warehouse serverless de Google Cloud que ejecuta consultas SQL analíticas sobre petabytes de datos.
  - Dataflow: Servicio gestionado de Google Cloud para ejecutar pipelines de Apache Beam en tiempo real o batch.
  - Vendor lock-in: Dependencia excesiva de un solo proveedor que dificulta migrar a otra plataforma.
  - Consistencia eventual: Modelo donde las réplicas de datos se sincronizan con cierto retraso pero garantizan que eventualmente convergerán.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 1: Arquitectura de Spotify
## Decisiones Arquitectónicas Clave
### ADR-4: Apache Kafka | ADR-5: Apollo GraphQL

<!-- column_layout: [1, 1] -->

<!-- column: 0 -->

**ADR-4: Apache Kafka**
- Desacopla productores y consumidores
- Garantiza orden y durabilidad
- Permite reprocesamiento histórico
- Contrapartida: complejidad operativa

<!-- column: 1 -->

**ADR-5: Apollo GraphQL**
- Cada cliente declara sus datos
- Optimiza ancho de banda
- Reduce latencia percibida
- Contrapartida: DataLoaders necesarios

<!-- reset_layout -->

<!--
speaker_note: |
  Con cientos de microservicios, la comunicación punto a punto era insostenible. Kafka desacopla productores y consumidores y permite replay histórico de eventos.
  GraphQL permite que cada tipo de cliente declare exactamente los datos que necesita, optimizando ancho de banda y evitando over-fetching y under-fetching.

  Términos clave:
  - Desacoplar: Reducir dependencias entre componentes para que puedan evolucionar, fallar y escalar de forma independiente.
  - Over-fetching: Problema de APIs REST donde el servidor devuelve más datos de los que el cliente necesita. GraphQL lo resuelve.
  - Under-fetching: Problema donde el cliente recibe datos insuficientes y debe hacer múltiples llamadas adicionales (problema N+1).
  - DataLoader: Utilidad que agrupa y procesa por lotes las consultas a bases de datos para evitar el problema N+1 en resolvers de GraphQL.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 1: Arquitectura de Spotify
## Decisiones Arquitectónicas Clave
### ADR-6: Modelo de Squads | ADR-7: Backstage

<!-- column_layout: [1, 1] -->

<!-- column: 0 -->

**ADR-6: Modelo de Squads**
- 5-8 ingenieros por squad
- "You build it, you run it"
- Ley de Conway aplicada
- Riesgo: fragmentación tecnológica

<!-- column: 1 -->

**ADR-7: Backstage**
- Catálogo centralizado de servicios
- Documentación viva desde código
- Scaffolding automatizado
- Donado a CNCF en 2020

<!-- reset_layout -->

<!--
speaker_note: |
  Spotify adoptó el modelo de squads, tribes, chapters y guilds. Cada squad es dueño de una misión de negocio y de los microservicios que la implementan, bajo el principio "You build it, you run it".
  Backstage unifica la visibilidad de todo el ecosistema en un solo lugar. Spotify lo donó a la CNCF en 2020, donde hoy es un proyecto incubado.

  Términos clave:
  - Squad: Equipo autónomo de 5-8 ingenieros dueño de una misión de negocio y de los microservicios que la implementan.
  - Tribe: Agrupación de squads que trabajan en áreas relacionadas. Límite de ~100 personas (principio de Dunbar).
  - Chapter: Agrupación horizontal de personas con habilidades similares dentro de una tribe.
  - Guild: Comunidad de interés voluntaria que cruza toda la organización para compartir conocimiento sobre un tema.
  - CNCF (Cloud Native Computing Foundation): Fundación bajo la Linux Foundation que alberga proyectos cloud nativos como Kubernetes, Prometheus y Backstage.
  - Scaffolding: Generación automática de la estructura inicial de un proyecto. Backstage lo ofrece como plantillas para nuevos servicios.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 1: Arquitectura de Spotify
## Decisiones Arquitectónicas Clave
### ADR-8: Caché Distribuida (Redis + Memcached)

- **Contexto**: streaming exige latencias < 200 ms; consultar BD por reproducción es inviable
- **Decisión**: capa de caché distribuida con Redis y Memcached para sesión y catálogo frecuente
- **Resultado**: reducción drástica de carga sobre Cassandra y PostgreSQL
- **Contrapartida**: inconsistencia temporal caché-BD, gestionada con TTL e invalidación diferenciada

<!--
speaker_note: |
  El último ADR documentado aborda la caché distribuida.
  Con latencias requeridas inferiores a 200 milisegundos para una experiencia de streaming fluida, consultar la base de datos en cada reproducción sería inviable.
  Redis y Memcached proporcionan una capa de caché que reduce la carga sobre Cassandra y PostgreSQL.
  La contrapartida es la inconsistencia temporal entre caché y base de datos, gestionada mediante tiempos de expiración y estrategias de invalidación diferenciadas por tipo de dato.

  Términos clave:
  - TTL (Time To Live): Tiempo de vida de un dato en caché antes de ser considerado obsoleto y eliminado o refrescado.
  - Invalidación de caché: Estrategia para marcar datos en caché como obsoletos cuando cambian en la base de datos.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 1: Arquitectura de Spotify
## Conclusión

- Arquitectura de **microservicios event-driven** diseñada para escala global
- **Tecnologías**: Kubernetes, Kafka, Cassandra, PostgreSQL, GCP
- **Decisiones clave** con ADRs documentados y contrapartidas gestionadas
- **Alineación** entre estructura organizacional (squads) y arquitectura técnica
- Sistema **confiable, flexible y preparado** para evolucionar

<!--
speaker_note: |
  En conclusión, el éxito de Spotify se sustenta en una arquitectura de microservicios basada en eventos, con tecnologías como Kubernetes, Kafka y Cassandra, y decisiones arquitectónicas bien documentadas.
  La alineación entre squads autónomos y microservicios es una manifestación concreta de la Ley de Conway.

  Términos clave:
  - Ley de Conway: Principio: las organizaciones diseñan sistemas que reflejan su estructura de comunicación. Squads autónomos → microservicios independientes.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 2: Rol del Arquitecto de Software Actual
## Tabla Comparativa de Perfiles (1/3)

<!-- column_layout: [1, 1, 1, 1] -->

<!-- column: 0 -->

**Criterio**

Puesto
<!-- new_lines: 1 -->

Empresa

País / Modalidad

Responsabilidades

<!-- column: 1 -->

**Spotify**

Staff Software Architect

Spotify

Suecia / Híbrido

Visión técnica, guiar equipos, mitigar riesgos

<!-- column: 2 -->

**Globant**

Software Architect Cloud Solutions

Globant

EE.UU. / Remoto

Migración cloud, FinOps, auditoría seguridad

<!-- column: 3 -->

**Mercado Libre**

Arquitecto de Software Senior

Mercado Libre

Argentina / Híbrido

Microservicios, BD distribuidas, mentoría

<!-- reset_layout -->

<!--
speaker_note: |
  Pasamos a la segunda sección, donde investigamos el estado actual del rol del arquitecto de software mediante perfiles reales del mercado laboral.
  
  Spotify — Staff Software Architect (Suecia, Híbrido): Define la visión técnica a largo plazo de los sistemas de distribución de contenido, guía equipos multidisciplinares en decisiones arquitectónicas y mitiga riesgos técnicos en plataformas de alta disponibilidad.
  
  Globant — Software Architect Cloud Solutions (EE.UU., Remoto): Diseña y migra arquitecturas empresariales hacia entornos multi-nube, asegura gobernanza de datos y costos de infraestructura (FinOps), y evalúa y audita la seguridad de soluciones integradas.
  
  Mercado Libre — Arquitecto de Software Senior (Argentina, Híbrido): Diseña arquitecturas orientadas a microservicios tolerantes a fallos, optimiza bases de datos distribuidas y flujos de mensajería masiva, y mentoriza ingenieros senior definiendo estándares de calidad de código.
  
  Fuentes: LinkedIn, Glassdoor, Get on Board (2026).
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 2: Rol del Arquitecto de Software Actual
## Tabla Comparativa de Perfiles (2/3)

<!-- column_layout: [1, 1, 1, 1] -->

<!-- column: 0 -->

**Criterio**

Habilidades Técnicas
<!-- new_lines: 2 -->

Habilidades Blandas

<!-- column: 1 -->

**Spotify**

Sist. distribuidos hiperescala, event-driven, resiliencia

Liderazgo influencia, negociación, resolución conflictos

<!-- column: 2 -->

**Globant**

Cloud nativa, IaC, seguridad perimetral
<!-- new_lines: 1 -->

Comunicación clara, adaptabilidad, pensamiento analítico

<!-- column: 3 -->

**Mercado Libre**

Patrones diseño, caché, pruebas rendimiento
<!-- new_lines: 1 -->

Mentoría, empatía, trabajo colaborativo ágil

<!-- reset_layout -->

<!--
speaker_note: |
  Habilidades Técnicas:
  - Spotify: Diseño de sistemas distribuidos a gran escala (hiperescala), modelado de microservicios y arquitecturas de eventos, gestión de resiliencia y tolerancia a fallos.
  - Globant: Arquitectura cloud nativa (IaaS, PaaS, SaaS), automatización de infraestructura como código (IaC), estrategias de seguridad perimetral y gobierno de datos.
  - Mercado Libre: Patrones de diseño de software, optimización de consultas distribuidas, estrategias de caché e ingesta de datos en tiempo real, pruebas de carga y análisis de rendimiento.
  
  Habilidades Blandas:
  - Spotify: Liderazgo de influencia sin autoridad formal, comunicación estratégica y negociación con directivos, gestión y resolución de conflictos técnicos.
  - Globant: Comunicación técnica clara a audiencias no técnicas, adaptabilidad a entornos cambiantes y prioridades de clientes, pensamiento analítico orientado a objetivos de negocio.
  - Mercado Libre: Mentoría, empatía y desarrollo de talento técnico, trabajo en equipo colaborativo bajo metodologías ágiles, proactividad y toma de decisiones bajo presión.
  
  Términos clave:
  - IaC (Infrastructure as Code): Práctica de gestionar y aprovisionar infraestructura mediante archivos de configuración versionables.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 2: Rol del Arquitecto de Software Actual
## Tabla Comparativa de Perfiles (3/3)

<!-- column_layout: [1, 1, 1, 1] -->

<!-- column: 0 -->

**Criterio**

Tecnologías
<!-- new_lines: 2 -->

Salario (USD)

Fuente

<!-- column: 1 -->

**Spotify**

Java, C++, Python, GCP, K8s, Kafka, gRPC

$120K–$145K

LinkedIn (2026)

<!-- column: 2 -->

**Globant**

AWS, Azure, Terraform, Jenkins, Docker, Redis

$150K–$185K

Glassdoor (2026)

<!-- column: 3 -->

**Mercado Libre**

Go, Java, Python, AWS, Kafka, Elasticsearch

$60K–$75K

Get on Board (2026)

<!-- reset_layout -->

<!--
speaker_note: |
  Tecnologías mencionadas:
  - Spotify: Java, C++, Python, GCP, Kubernetes, Docker, Kafka, gRPC. Stack orientado a hiperescala sobre Google Cloud.
  - Globant: AWS, Azure, Terraform, Jenkins, Docker, SQL Server, PostgreSQL, Redis. Stack multi-cloud con foco en automatización.
  - Mercado Libre: Go, Java, Python, AWS, Elasticsearch, Redis, MySQL, Apache Kafka. Stack regional sobre AWS con énfasis en datos y mensajería.
  
  Brecha salarial: Norteamérica/Europa ($120K-$185K) vs Latinoamérica ($60K-$75K). Las exigencias técnicas conceptuales son muy similares, pero las compensaciones están ligadas a la ubicación geográfica y el costo de vida local de la empresa contratante.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 2: Rol del Arquitecto de Software Actual
## Coincidencias Clave

- **Cloud e Infraestructura Inmutable**: dominio obligatorio de AWS/GCP/Azure, Docker y Kubernetes
- **Sistemas de Datos**: Apache Kafka, bases distribuidas, Redis
- **Habilidades Blandas**: mentoría, negociación, liderazgo de influencia
- El arquitecto moderno pasa gran parte de su tiempo **comunicando e influenciando**

<!--
speaker_note: |
  Las tres coincidencias principales son: dominio obligatorio de cloud y contenedores en todas las geografías; experiencia en Kafka y bases distribuidas como requisito transversal; y la importancia extrema de las habilidades blandas.
  A diferencia de roles puramente técnicos, el arquitecto pasa gran parte de su tiempo comunicando, negociando e influenciando.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 2: Rol del Arquitecto de Software Actual
## Diferencias Significativas

- **Escala organizacional**: Spotify → hiperescala; Globant → consultoría y FinOps
- **Brecha salarial**: Norteamérica/Europa ($120K-$185K) vs LATAM ($60K-$75K)
- Mismas exigencias técnicas conceptuales, **compensaciones ligadas a la ubicación**

<!--
speaker_note: |
  Las principales diferencias están en el contexto: Spotify se enfoca en hiperescala y resiliencia, mientras Globant tiene un matiz de consultoría y gobernanza de costos.
  La brecha salarial es marcada: entre 120 y 185 mil dólares en Norteamérica y Europa frente a 60-75 mil en Latinoamérica, pese a que las exigencias técnicas son muy similares.

  Términos clave:
  - FinOps (Financial Operations): Práctica que une finanzas, tecnología y negocio para gestionar y optimizar costos en la nube.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 2: Rol del Arquitecto de Software Actual
## Conclusión

- El arquitecto evolucionó de **diseñador técnico** a **posición estratégica**
- Conocimientos requeridos: microservicios, cloud, DevOps, ciberseguridad
- Habilidades blandas **tan importantes** como las técnicas
- Rol de **puente entre negocio y tecnología**

<!--
speaker_note: |
  El arquitecto de software ha evolucionado más allá del diseño técnico hacia una posición estratégica.
  Las empresas buscan profesionales con conocimientos en microservicios, cloud, DevOps y ciberseguridad, pero las habilidades blandas son igualmente valoradas.
  El arquitecto funciona como puente entre los objetivos del negocio y la implementación tecnológica.

  Términos clave:
  - DevOps: Cultura que integra desarrollo (Dev) y operaciones (Ops) para acortar ciclos de entrega con CI/CD, monitoreo y operación autónoma.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 3: El Rol del Arquitecto en el Futuro
## Transformación del Rol

El arquitecto pasará de **diseñador de estructuras técnicas** a **estratega tecnológico** que toma decisiones sobre sistemas:

- Más inteligentes y automatizados
- Más distribuidos
- Más regulados
- Con impacto ético, económico, ambiental y social

<!--
speaker_note: |
  En la tercera sección proyectamos la evolución del rol del arquitecto en la próxima década.
  Durante los próximos diez años, el arquitecto evolucionará de ser principalmente un diseñador técnico a convertirse en un estratega tecnológico.
  Deberá comprender no solo la infraestructura, sino también el impacto ético, económico, ambiental y social de sus decisiones.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 3: El Rol del Arquitecto en el Futuro
## Impacto de la IA

<!-- column_layout: [1, 1] -->

<!-- column: 0 -->

**Oportunidades**
- Análisis masivo de información técnica
- Comparación de patrones arquitectónicos
- Detección de deuda técnica
- Documentación automática
- Optimización de costos cloud

<!-- column: 1 -->

**Desafíos**
- Precisión y alucinaciones
- Privacidad de datos
- Falta de marcos de evaluación
- La IA apoya, no decide

<!-- reset_layout -->

<!--
speaker_note: |
  La IA transformará directamente las actividades del arquitecto. Podrá analizar grandes cantidades de información, detectar deuda técnica, generar documentación y sugerir mejoras.
  Sin embargo, la IA generativa todavía enfrenta problemas de precisión, alucinaciones y falta de marcos sólidos de evaluación. La IA debe verse como herramienta de apoyo, no como reemplazo del arquitecto.
  Fuente: Esposito et al., 2025; Software Engineering Institute, 2024.

  Términos clave:
  - Alucinación (en IA): Fenómeno donde un modelo de IA generativa produce información incorrecta o inventada presentada como factual.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 3: El Rol del Arquitecto en el Futuro
## Impacto de la Computación Cuántica

- IBM proyecta computadora cuántica tolerante a fallos hacia **2029**
- Oportunidades: optimización de distribución, recomendaciones, análisis de datos
- Adopción **gradual y limitada** a casos concretos
- **Impacto inmediato en seguridad**: migración a criptografía post-cuántica
- NIST publicó estándares de criptografía post-cuántica en 2024

<!--
speaker_note: |
  La computación cuántica podría influir en optimización y simulación en la próxima década. IBM proyecta una computadora cuántica tolerante a fallos hacia 2029.
  El impacto más inmediato estará en seguridad: el arquitecto deberá planificar migraciones hacia algoritmos resistentes a ataques cuánticos.
  El NIST ya publicó estándares de criptografía post-cuántica en 2024.

  Términos clave:
  - Criptografía post-cuántica: Algoritmos criptográficos diseñados para resistir ataques de computadoras cuánticas. Estandarizados por NIST en 2024.
  - NIST (National Institute of Standards and Technology): Agencia del gobierno de EE.UU. que desarrolla estándares tecnológicos. Publicó los primeros estándares de criptografía post-cuántica en 2024.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 3: El Rol del Arquitecto en el Futuro
## Nuevas Responsabilidades

- **Gobernanza de IA**: sistemas auditables, explicables y controlados (ISO/IEC 42001:2023)
- **Sostenibilidad**: eficiencia energética, reducción de huella de carbono (SCI Specification)
- **Ética tecnológica**: privacidad, transparencia, responsabilidad
- **Cumplimiento normativo**: regulaciones emergentes sobre IA y datos
- **Gestión de riesgos**: anticipar consecuencias no evidentes

<!--
speaker_note: |
  El arquitecto del futuro tendrá responsabilidades más amplias: gobernanza de sistemas de IA bajo ISO 42001, sostenibilidad medida con la especificación SCI de la Green Software Foundation, ética tecnológica y cumplimiento normativo.
  Ya no bastará con que el sistema funcione; deberá ser auditable, confiable y alineado con principios de privacidad y transparencia.

  Términos clave:
  - ISO/IEC 42001:2023: Estándar internacional para sistemas de gestión de inteligencia artificial.
  - SCI (Software Carbon Intensity): Especificación de la Green Software Foundation que mide la tasa de emisiones de carbono de un sistema de software.
  - Green Software Foundation: Organización sin fines de lucro que desarrolla estándares y herramientas para reducir el impacto ambiental del software.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 3: El Rol del Arquitecto en el Futuro
## Habilidades a Desarrollar

<!-- column_layout: [1, 1] -->

<!-- column: 0 -->

**Técnicas**
- Arquitectura cloud nativa
- MLOps y pipelines de ML
- Gobernanza de datos
- IA generativa
- Criptografía post-cuántica
- Observabilidad avanzada

<!-- column: 1 -->

**Humanas**
- Pensamiento crítico
- Liderazgo y negociación
- Visión de negocio
- Comunicación estratégica
- Análisis de trade-offs

<!-- reset_layout -->

<!--
speaker_note: |
  El arquitecto necesitará una combinación de habilidades técnicas y humanas.
  En lo técnico: cloud, MLOps, gobernanza de datos, IA generativa y fundamentos de criptografía post-cuántica.
  En lo humano: pensamiento crítico, liderazgo, negociación y capacidad para analizar trade-offs entre rendimiento, costo, privacidad y sostenibilidad.

  Términos clave:
  - MLOps (Machine Learning Operations): Prácticas que aplican DevOps al ciclo de vida de modelos ML: CI/CD y monitoreo continuo de modelos en producción.
  - Trade-off (compensación): Sacrificio aceptado en un aspecto para ganar en otro. En arquitectura: rendimiento vs. costo, personalización vs. privacidad.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 3: El Rol del Arquitecto en el Futuro
## Automatización vs. Criterio Humano

<!-- column_layout: [1, 1] -->

<!-- column: 0 -->

**Automatizable con IA**
- Documentación técnica
- Diagramas iniciales
- Análisis de dependencias
- Detección de deuda técnica
- Pruebas y reportes

<!-- column: 1 -->

**Requiere criterio humano**
- Prioridades estratégicas
- Resolución de conflictos
- Interpretación del negocio
- Evaluación ética
- Decisiones de seguridad
- Migraciones complejas

<!-- reset_layout -->

<!--
speaker_note: |
  Varias actividades podrán automatizarse: documentación, diagramas, análisis de dependencias, detección de deuda técnica.
  Pero el criterio humano seguirá siendo indispensable para prioridades estratégicas, resolver conflictos, evaluar consecuencias éticas y decidir sobre seguridad y migraciones complejas.
  El arquitecto actuará como mediador entre la automatización y la responsabilidad.

  Términos clave:
  - Deuda técnica: Costo implícito de retrabajo causado por elegir soluciones rápidas en lugar de enfoques de mayor calidad.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 3: El Rol del Arquitecto en el Futuro
## Valor Diferencial del Arquitecto

- **Conectar** tecnología, negocio y responsabilidad social
- **Formular las preguntas correctas**, no solo generar opciones
- **Equilibrar**: innovación ↔ estabilidad, automatización ↔ control, personalización ↔ privacidad
- **Anticipar impactos** y diseñar arquitecturas adaptables
- No será solo un experto técnico: será un **líder de decisiones complejas**

<!--
speaker_note: |
  El valor diferencial del arquitecto estará en conectar tecnología, negocio y responsabilidad social.
  Mientras la IA podrá generar opciones, el arquitecto deberá formular las preguntas correctas, evaluar riesgos y asegurar que la arquitectura sea sostenible en el tiempo.
  No será solo un experto técnico, sino un líder de decisiones complejas.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 3: El Rol del Arquitecto en el Futuro
## Riesgos y Oportunidades

<!-- column_layout: [1, 1] -->

<!-- column: 0 -->

**Oportunidades**
- Herramientas más avanzadas
- Automatización de tareas repetitivas
- Más tiempo para decisiones estratégicas
- Nuevos enfoques (cuántica)

<!-- column: 1 -->

**Riesgos**
- Dependencia excesiva de la IA
- Deuda técnica acelerada
- Sesgos algorítmicos
- Aumento del consumo energético
- Dificultad regulatoria

<!-- reset_layout -->

<!--
speaker_note: |
  Las oportunidades incluyen herramientas más avanzadas, automatización de tareas repetitivas y más tiempo para lo estratégico.
  Los riesgos abarcan la dependencia excesiva de la IA sin validación, generación de deuda técnica a gran velocidad, sesgos algorítmicos y aumento del consumo energético.

  Términos clave:
  - Sesgo algorítmico: Error sistemático en un modelo de IA que produce resultados injustos o discriminatorios hacia ciertos grupos.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Sección 3: El Rol del Arquitecto en el Futuro
## Conclusión

- El arquitecto **no desaparecerá**: se volverá más estratégico, interdisciplinario y crítico
- La IA automatiza tareas, pero el **criterio humano** sigue siendo indispensable
- Deberá diseñar sistemas que integren IA, prepararse para seguridad post-cuántica y garantizar confianza
- **La diferencia**: saber cuándo usar nuevas tecnologías, cómo gobernarlas y qué consecuencias generan

<!--
speaker_note: |
  En conclusión, el rol del arquitecto de software no desaparecerá. Se volverá más estratégico, interdisciplinario y crítico.
  La diferencia no estará solo en conocer nuevas tecnologías, sino en saber cuándo usarlas, cómo gobernarlas y qué consecuencias pueden generar.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Conclusión General
## Síntesis del Trabajo

- Spotify demuestra que una **arquitectura bien diseñada** habilita escala global e innovación continua
- El arquitecto moderno es un **puente estratégico** entre negocio y tecnología
- El futuro exige **nuevas competencias**: IA, ciberseguridad, sostenibilidad, ética
- El arquitecto seguirá siendo **indispensable** como líder de decisiones complejas

<!--
speaker_note: |
  Llegamos a la conclusión general de nuestra investigación.
  Nuestro trabajo evidencia que el éxito de Spotify reside en una arquitectura cuidadosamente diseñada que equilibra escalabilidad, disponibilidad, seguridad y capacidad de innovación.
  El arquitecto moderno trasciende lo técnico para convertirse en puente estratégico.
  Y el futuro exigirá nuevas competencias sin eliminar la necesidad del criterio humano.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# Referencias

- Kniberg, H. y Ivarsson, A. (2012). *Scaling Agile @ Spotify*
- Kreitz, G. y Niemelä, F. (2010). *Spotify — Large scale, low latency, P2P streaming*
- Newman, S. (2021). *Building Microservices* (2.ª ed.). O'Reilly
- Esposito et al. (2025). *Generative AI for Software Architecture*. arXiv
- ISO/IEC 42001:2023. *AI Management System*
- NIST (2024). *Post-Quantum Cryptography Standards*
- IBM (2025). *Fault-Tolerant Quantum Computing Roadmap*
- Spotify Engineering (2024). *Engineering Blog*
- Apache Kafka, Cassandra, Kubernetes Documentation (2024)
- Google Cloud Architecture Framework (2024)

<!--
speaker_note: |
  Estas son las principales referencias consultadas para este trabajo. Se utilizaron fuentes académicas, técnicas y de la industria, incluyendo papers originales de ingenieros de Spotify, documentación oficial de tecnologías como Kafka y Cassandra, y estándares recientes de ISO y NIST.
  La lista completa de referencias en formato APA se encuentra en el documento de investigación.

  Términos clave:
  - APA (American Psychological Association): Estilo de citación y formato académico estándar. Citas autor-año en el texto y referencias completas al final.
-->

<!-- end_slide -->

<!-- new_lines: 1 -->
# ¡Gracias!

¿Preguntas?

<!--
speaker_note: |
  Muchas gracias por su atención. Quedamos abiertos a preguntas y comentarios.
-->

