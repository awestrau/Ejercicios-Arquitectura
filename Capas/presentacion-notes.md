# Speaker Notes — Arquitectura en Capas Distribuidas (Sistema de Tareas)

## Glosario de Acrónimos

- **ADR** — Architecture Decision Record: documento que registra una decisión arquitectónica, su contexto, alternativas y consecuencias.
- **C4** — Context, Containers, Components, Code: modelo de 4 niveles de diagramas para describir arquitectura de software.
- **DDD** — Domain-Driven Design: enfoque de diseño donde el dominio de negocio es el centro del software y dicta la estructura del código.
- **DIP** — Dependency Inversion Principle: principio SOLID que establece que los módulos de alto nivel no deben depender de los de bajo nivel; ambos deben depender de abstracciones.
- **DMZ** — Demilitarized Zone: segmento de red que aísla los servidores expuestos a internet de la red interna protegida.
- **DTO** — Data Transfer Object: objeto plano sin lógica que transporta datos entre capas o a través de la red.
- **JPA** — Jakarta/Java Persistence API: especificación de Java para mapear objetos a bases de datos relacionales (implementada por Hibernate).
- **SLA** — Service Level Agreement: acuerdo que define métricas de servicio como tiempo de respuesta, disponibilidad y prioridad.
- **SPA** — Single Page Application: aplicación web que carga una sola página HTML y actualiza dinámicamente el contenido sin recargar el navegador.

---

## Slide 2: Agenda
Bienvenidos. Hoy presentamos la arquitectura en capas distribuidas para el Sistema de Tareas.
La agenda cubre el ADR con la decisión justificada, los diagramas C4 de contenedores y componentes, la explicación detallada de cada capa y un flujo de ejemplo que las conecta todas.
Vamos paso a paso, empezando por el contexto del sistema.

## Slide 3: Escenario y Requerimientos
El Sistema de Tareas es una plataforma para una empresa mediana con equipos distribuidos geográficamente.
Los requerimientos clave son: soportar de 100 a 10,000 usuarios concurrentes, gestionar tareas con su ciclo de vida completo, notificar cambios por correo y SMS, y generar reportes de productividad.
A nivel no funcional, necesitamos consultas rápidas, alta disponibilidad y separación clara de roles entre frontend, backend y base de datos.

## Slide 4: ADR 001 — Decisión
La decisión fue una arquitectura de 3 tiers físicos.
El tier de presentación es una SPA en React servida por Nginx, expuesta a internet.
El tier de aplicación contiene la lógica de negocio en Spring Boot, en una subred protegida.
El tier de infraestructura aísla PostgreSQL y los servicios de notificación externos.
La comunicación entre tiers usa REST con HTTP/JSON, y a nivel interno seguimos DIP: todas las dependencias apuntan hacia el dominio, nunca al revés.

## Slide 5: Alternativas Consideradas
Evaluamos dos alternativas antes de decidirnos por capas distribuidas.
El monolito en capas ofrecía simplicidad y transacciones directas, pero un único despliegue impedía escalar el frontend independientemente y no permitía el aislamiento de seguridad por zonas de red que requería la organización.
Los microservicios daban escalado granular y equipos autónomos, pero para un equipo mediano con un dominio de tareas acotado, la complejidad operativa de orquestar contenedores, manejar consistencia distribuida y configurar observabilidad era sobre-ingeniería.

## Slide 6: ADR — Consecuencias
Toda decisión arquitectónica tiene consecuencias.
Del lado positivo: podemos escalar el frontend horizontalmente sin tocar el backend, tenemos aislamiento de seguridad por zonas de red, cada equipo mantiene su tier sin interferencias y las responsabilidades están claramente separadas.
Del lado negativo: cada salto de red introduce latencia, los fallos parciales requieren manejo de errores en cada capa, y ahora tenemos tres superficies para monitorear y desplegar.
El trade-off se acepta porque la ganancia en escalabilidad, seguridad y organización supera estos costos para nuestro contexto.

## Slide 7: C4 Nivel 2 — Diagrama de Contenedores
Este es el diagrama C4 Nivel 2 de contenedores. Muestra los tres tiers físicos y las tecnologías seleccionadas.
El usuario accede por HTTPS a través de un balanceador Nginx. El frontend es una SPA en React que consume la API REST del backend.
El backend en Spring Boot orquesta la lógica de negocio y se comunica con PostgreSQL para persistencia y con servicios externos de notificación como SendGrid y Twilio.
Noten la separación física: el frontend está expuesto, el backend en subred protegida, y la base de datos en la subred más restringida.

## Slide 8: C4 Nivel 3 — Componentes del Backend
Este es el C4 Nivel 3 simplificado. Muestra las 4 capas lógicas dentro del backend.
La Presentación expone el TareaController con endpoints REST y DTOs.
La Aplicación coordina los casos de uso con TareaApplicationService y ReporteService.
El Dominio contiene la entidad Tarea con sus patrones State y Strategy, y define los contratos TareaRepository y NotificacionGateway.
La Infraestructura implementa esos contratos: SqlTareaRepository con JPA, EmailAdapter con SendGrid, SMSAdapter con Twilio, y AuditoriaService escuchando eventos.
Las líneas punteadas muestran DIP: la infraestructura implementa interfaces definidas por el dominio.

## Slide 9: Dirección de Dependencias (DIP)
Este diagrama resume la dirección de dependencias según el principio DIP.
Presentación depende de Aplicación, Aplicación depende de Dominio. El Dominio no depende de nadie.
Infraestructura implementa contratos definidos por el Dominio, invirtiendo la dependencia tradicional.
Esto significa que el Dominio —el núcleo del sistema— no sabe nada de Spring, Hibernate, SendGrid ni Twilio. Es completamente portable.

## Slide 10: Capa de Presentación
Empecemos con la capa de Presentación. Su única responsabilidad es traducir entre protocolos externos y los casos de uso internos.
El TareaController expone los endpoints REST y recibe DTOs de entrada. Valida el formato pero no decide nada de negocio.
Usa el patrón Front Controller de Spring y DTOs para el transporte de datos.
Es importante entender lo que NO hace: nunca contiene lógica de negocio, nunca accede directamente a la base de datos y nunca decide si una tarea puede cambiar de estado. Todo eso lo delega hacia abajo.

## Slide 11: Capa de Aplicación
La capa de Aplicación es la directora de orquesta. No toca instrumentos, solo coordina.
El TareaApplicationService ejecuta el flujo completo de un caso de uso: carga la tarea, resuelve políticas, delega al dominio, persiste y notifica. Pero en ningún momento contiene reglas de negocio.
El ReporteService coordina consultas agregadas usando el repositorio.
Los patrones clave son Application Service —cada método es un caso de uso— y Facade para ofrecer una API simple a la capa de presentación.

## Slide 12: Capa de Dominio — Reglas de Negocio
Llegamos al corazón del sistema: la capa de Dominio.
La entidad Tarea es el agregado raíz que garantiza la consistencia de sus reglas internas. Encapsula todo el ciclo de vida.
El patrón State se usa para modelar las transiciones: Pendiente solo puede pasar a EnProgreso o Cancelada, EnProgreso solo a Completada o Cancelada, y los estados terminales no permiten más cambios.
Los eventos de dominio como TareaCreada o TareaCompletada permiten que la infraestructura reaccione sin que el dominio sepa quién está escuchando.

## Slide 13: Capa de Dominio — Patrones
Veamos los patrones que hacen potente a esta capa.
Aggregate de DDD: Tarea es la raíz que protege las reglas de consistencia.
State: cada estado es una clase separada que implementa una interfaz común; la entidad solo delega.
Strategy para asignación: un líder puede asignar a cualquiera, un miembro solo puede auto-asignarse. Esta política es intercambiable sin modificar la entidad.
Strategy para SLA: la prioridad se calcula dinámicamente según el tiempo restante; si faltan menos de 2 horas, la tarea sube a crítica.
Domain Events permiten que la auditoría y las notificaciones reaccionen sin acoplar el dominio.
Y lo más importante: los contratos TareaRepository y NotificacionGateway se definen aquí, en el dominio. La infraestructura los implementa, nunca al revés.

## Slide 14: Capa de Infraestructura
La capa de Infraestructura es la que interactúa con el mundo real.
SqlTareaRepository implementa el contrato definido en el dominio usando JPA y Hibernate, mapeando entidades a tablas en PostgreSQL.
EmailAdapter y SMSAdapter implementan NotificacionGateway, cada uno traduciendo la interfaz del dominio a la API propietaria de su proveedor. Esto permite cambiar de SendGrid a otro proveedor sin tocar el dominio.
AuditoriaService escucha los eventos de dominio y registra cada cambio con marca de tiempo, usuario y detalle, sin bloquear el flujo principal.
Los patrones aquí son puramente técnicos: Repository, Data Mapper, Adapter y Event Listener.

## Slide 15: Flujo de Ejemplo — Cambio de Estado
Veamos cómo colaboran las cuatro capas en un caso de uso concreto: marcar una tarea como completada.
El controlador recibe el PATCH, valida y delega. El servicio de aplicación carga la entidad y la orquesta.
El dominio valida la transición —si el estado actual es EnProgreso, puede pasar a Completada—, muta el estado y emite el evento de dominio.
La aplicación persiste el cambio y dispara la notificación.
La infraestructura ejecuta el UPDATE en PostgreSQL, envía el correo por SendGrid, y de forma asíncrona el AuditoriaService registra la trazabilidad al recibir el evento.
Todo esto ocurre sin que el dominio sepa nada de HTTP, SQL, SMTP ni frameworks.

## Slide 16: Resumen de Capas
Para cerrar, este es el resumen de las cuatro capas.
Presentación traduce protocolos, Aplicación coordina casos de uso, Dominio contiene las reglas de negocio, Infraestructura implementa los detalles técnicos.
Cada capa tiene sus patrones específicos y responsabilidades claras.
Y la regla de oro: las dependencias siempre apuntan hacia el Dominio. El Dominio es el centro inmutable del sistema. Todo lo demás es un detalle que puede cambiar.
¿Alguna pregunta antes de pasar a la discusión?
