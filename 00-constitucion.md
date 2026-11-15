# Constitución del Proyecto Muuvi
**Versión:** 1.0.0
**Fecha:** 07/11/2026
**Ámbito:** Obligatorio y vinculante para todos los desarrolladores humanos y agentes de inteligencia artificial que colaboren en el proyecto. Es el primer archivo que cualquier agente de IA debe leer antes de trabajar en el proyecto.

---

### PRINCIPIO-001: Uniformidad Lingüística
Todo el código fuente (variables, funciones, componentes, tablas, endpoints), los comentarios, los mensajes de commit, los ADR y la documentación se redactan en español. La interfaz de usuario es bilingüe: español (`es-PE`, idioma por defecto) e inglés (`en`). Todo texto visible se carga desde archivos de traducción y nunca se escribe directamente en el código.
- **Ejemplo de cumplimiento:** Una función `calcularProximaEntrega()` con su comentario en español, y un componente que muestra `t('pedido.titulo')` resuelto desde `src/locales/es-PE.json` y `src/locales/en.json`.
- **Ejemplo de violación:** Una función `getNextDelivery()`, un commit "fix delivery bug" o un JSX con texto fijo como `<h1>Tu pedido</h1>`.

### PRINCIPIO-002: Inmutabilidad del Stack Tecnológico Base
El stack aprobado es:
- **Frontend:** React 18+ como Aplicación Web Progresiva (PWA).
- **Backend:** Node.js 20+ con Express.
- **Base de datos:** PostgreSQL 16+.
- **Pasarelas de pagos:** Culqi, Niubiz, Izipay y Mercado Pago, más las billeteras Yape y Plin. Todas se integran detrás de un único módulo `pagos` con una interfaz común, de modo que ninguna otra parte del sistema dependa directamente de una pasarela concreta.

Ningún componente del stack puede modificarse ni sustituirse sin aprobación explícita y documentada del equipo humano titular, registrada en un ADR (`docs/adr/ADR-NNN-*.md`).
- **Ejemplo de cumplimiento:** Desarrollar la pantalla de suscripciones en React 18+ y su endpoint en Express sobre Node.js 20.
- **Ejemplo de violación:** Reemplazar Express por otro framework web, usar Flutter o introducir una base de datos NoSQL sin un ADR aprobado.

### PRINCIPIO-003: Cifrado de Datos Personales
Los datos personales (DNI, nombres completos, fecha de nacimiento, teléfono, correo, dirección exacta y coordenadas de geolocalización) se cifran en reposo **a nivel de campo con AES-256-GCM**, y sus claves se guardan en un gestor de secretos (KMS), nunca en el repositorio. Toda comunicación entre la PWA, el backend y los servicios de terceros usa **TLS 1.3**. Ningún dato personal puede aparecer en texto plano en logs, mensajes de error ni analítica.
- **Ejemplo de cumplimiento:** Transmitir los datos por un canal TLS 1.3 y guardar el DNI del cliente cifrado con AES-256-GCM en la columna `dni_cifrado` de PostgreSQL; los logs solo registran `cliente_id`.
- **Ejemplo de violación:** Guardar el DNI o el teléfono del cliente en texto plano en tablas o logs, o registrar `Error al entregar en Av. Larco 123, tel. 987654321`.

### PRINCIPIO-004: Pagos sin Datos de Tarjeta (PCI-DSS SAQ-A)
Los datos de tarjeta (número de tarjeta o PAN, CVV y fecha de vencimiento) **nunca** llegan al backend ni a la base de datos de Muuvi. Se capturan con el formulario o SDK de la pasarela, que devuelve un token; el sistema solo guarda ese token y los 4 últimos dígitos. Un test automatizado falla si algún esquema SQL, DTO o validador contiene campos como `numero_tarjeta`, `cvv` o `pan`.
- **Ejemplo de cumplimiento:** La PWA envía al backend solo `{ "token_pasarela": "tkn_…", "ultimos_4": "4242" }` para cobrar la suscripción mensual.
- **Ejemplo de violación:** Un endpoint `POST /pagos` que recibe `{ "numero_tarjeta": "4111…", "cvv": "123" }`.

### PRINCIPIO-005: Ciclo de Vida de los Datos Personales (Ley N.° 29733 y D.S. 016-2024-JUS)
1. **Inscripción ante la ANPDP:** los bancos de datos de clientes están inscritos ante la ANPDP. Ningún banco de datos nuevo (por ejemplo, una nueva tabla o servicio con datos personales) entra en producción sin su inscripción o actualización previa.
2. **Solo mayores de edad:** el registro exige fecha de nacimiento y rechaza a quien tenga menos de 18 años.
3. **Consentimiento:** se pide consentimiento expreso, separado y sin casillas premarcadas para (a) el tratamiento de datos y (b) el envío de datos a terceros, incluidos los proveedores de IA.
4. **Datos enviados a IA:** el DNI y los datos de pago nunca se envían a un proveedor de IA. Los demás datos solo se envían si el cliente dio su consentimiento y el proveedor figura en `docs/proveedores-datos.md`.
5. **Eliminación:** al darse de baja una cuenta, sus datos se eliminan o anonimizan en un plazo máximo de **30 días**. Solo se conservan los comprobantes que SUNAT obliga a guardar.
- **Ejemplo de cumplimiento:** Un job diario anonimiza las cuentas cuya `fecha_baja` tiene más de 30 días y deja la ejecución registrada en la tabla de auditoría.
- **Ejemplo de violación:** Registrar a un usuario de 17 años, o enviar a un LLM un prompt que contiene el DNI del cliente.

### PRINCIPIO-006: Cumplimiento Comercial, Tributario y Sanitario
- Todo pago confirmado emite su **boleta o factura electrónica SUNAT**.
- Ningún producto se publica sin su **registro sanitario DIGESA**.
- Ningún producto o finca se rotula como "orgánico" sin una **certificación SENASA vigente**.
- La PWA ofrece acceso al **Libro de Reclamaciones virtual** desde el menú principal (Ley 29571).
- Los productos que lo requieran muestran las **advertencias octogonales** (Ley 30021).
- **Ejemplo de cumplimiento:** El catálogo rechaza guardar un yogurt artesanal si `registro_digesa` está vacío.
- **Ejemplo de violación:** Mostrar la etiqueta "Orgánico" en una finca cuya certificación SENASA está vencida.

### PRINCIPIO-007: Accesibilidad Universal de Interfaz (WCAG 2.1 AA)
Todas las interfaces cumplen como mínimo el nivel AA de WCAG 2.1:
- Contraste mínimo de **4.5:1** para texto normal y **3:1** para texto grande y componentes.
- Navegación completa mediante teclado, con foco visible.
- Texto alternativo (`alt` o `aria-label`) en toda imagen e ícono interactivo.
- Áreas táctiles de al menos **48×48 px**.
- Texto que escala hasta **200 %** sin cortarse.
- Compatibilidad con TalkBack y VoiceOver.
- **Dispositivos soportados:** Android 9 (API 28, Pie) o superior con Chrome actualizado, e iOS 15 o superior con Safari 15 o superior.
- **Ejemplo de cumplimiento:** El botón "Programar entrega" tiene contraste de 5.2:1, mide 48×48 px, recibe foco con Tab y el lector de pantalla lo anuncia como "Programar entrega, botón".
- **Ejemplo de violación:** Un botón de acción crítico con contraste inferior a 4.5:1, que no responde al foco del teclado, o un ícono de botella sin texto alternativo.

### PRINCIPIO-008: Restricciones Absolutas y Líneas Rojas para Agentes de IA
Ningún agente de IA, autónomo o asistido, puede bajo ninguna circunstancia:
1. **Modificar el stack tecnológico:** cambiar, actualizar o introducir librerías o frameworks fuera del stack aprobado (React PWA, Node.js con Express, PostgreSQL), ni añadir o quitar dependencias sin aprobación humana.
2. **Eliminar datos críticos:** borrar o anular registros de ventas, pedidos, pagos, abonos, suscripciones, comprobantes o auditorías sin la doble confirmación explícita del administrador titular de Muuuvi.
3. **Alterar límites de crédito:** modificar por su cuenta los topes de crédito, descuentos o saldos de garantía de botellas retornables asignados a los clientes.
4. **Violar ventanas de comunicación:** enviar recordatorios o notificaciones por WhatsApp, SMS o push fuera del horario permitido (**09:00–20:00, America/Lima, UTC-5**).
5. **Saltarse los controles de integración:** hacer push o merge directo a `main` o a producción, o desplegar a producción sin aprobación humana.
6. **Tocar secretos:** leer, crear o modificar secretos (`.env`, claves de API, credenciales de la pasarela o del KMS).
7. **Ejecutar operaciones destructivas:** correr migraciones destructivas (`DROP`, `TRUNCATE`, `DELETE` o `UPDATE` sin `WHERE`) o acceder a datos reales de clientes.
8. **Manipular tests:** modificar, saltar (`skip`) o borrar tests para que pasen.
9. **Cambiar lo comercial o legal:** cambiar precios, lógica de cobro, tarifas de entrega o textos legales (términos, privacidad, consentimientos) sin revisión humana.
10. **Modificar esta constitución:** editar este archivo.

Ante cualquier duda o inconsistencia, el agente se detiene y solicita intervención humana.
- **Ejemplo de cumplimiento:** El agente detecta suscripciones duplicadas en la base de datos, no las borra, y genera una alerta que pide intervención humana obligatoria.
- **Ejemplo de violación:** El agente decide por su cuenta purgar registros antiguos de la tabla de pedidos, instala un ORM alternativo en el backend sin aprobación, o programa un recordatorio de renovación a las 22:30.

### PRINCIPIO-009: Garantía de Calidad y Cobertura de Pruebas Automatizadas
- Todo cambio en la lógica del sistema debe mantener al menos **80 % de cobertura en reglas de negocio** y **60 % de cobertura general**. El CI bloquea el merge si no se alcanza.
- Cada regla de `03-reglas-de-negocio.md` está respaldada por al menos un test unitario que cita su ID.
- Son obligatorias las pruebas unitarias, de integración, E2E de los flujos críticos (registro, suscripción, pago y entrega) y de accesibilidad automatizadas.
- Cada PR requiere **2 aprobaciones humanas**, linter y SAST en verde, y commits en formato **Conventional Commits** en español.
- **Ejemplo de cumplimiento:** Un PR que implementa la regla de pausa de suscripción incluye `test_RN012_pausar_suscripcion_antes_del_corte`, deja el módulo en 85 % de cobertura y tiene 2 aprobaciones.
- **Ejemplo de violación:** Mergear con una sola aprobación, o con la cobertura del módulo de negocio por debajo del 80 %.

### PRINCIPIO-010: Documentación como Código
- Toda la documentación vive en el repositorio en formato **Markdown**.
- Cada decisión técnica relevante se registra como **ADR** en `docs/adr/ADR-NNN-titulo.md`.
- La API se documenta con **OpenAPI 3.x** en `docs/api/openapi.yaml`, y el CI valida que el contrato coincide con la implementación.
- Todo diagrama se escribe en **Mermaid** dentro del propio Markdown.
- **Ejemplo de cumplimiento:** El endpoint `GET /fincas/{id}/trazabilidad` se añade a `openapi.yaml` en el mismo PR, junto con un diagrama de secuencia en Mermaid.
- **Ejemplo de violación:** Documentar la arquitectura en un PNG exportado de otra herramienta, o publicar un endpoint que no figura en OpenAPI.

### PRINCIPIO-011: Estandarización de Formatos de Localización Peruana
Montos, fechas y números se muestran según las convenciones del Perú:
- Moneda en soles con el formato `S/ 1,250.50` (punto decimal y coma de miles).
- Fechas en formato `DD/MM/AAAA` cuando la interfaz está en español.
- Zona horaria `America/Lima` (UTC-5) para ventanas de entrega, cortes de suscripción y notificaciones.

En la base de datos, los timestamps se guardan en UTC y los montos como `NUMERIC(12,2)`, nunca como `FLOAT`.
- **Ejemplo de cumplimiento:** La suscripción semanal se muestra como `S/ 32.90 · próxima entrega 12/11/2026, 07:00–09:00`.
- **Ejemplo de violación:** Mostrar `$32.9` o `11/12/2026`, o guardar el precio en una columna `FLOAT`.
