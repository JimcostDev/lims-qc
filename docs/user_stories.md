# Historias de Usuario (Formato GitHub Issue / Sprint)

**Convención:** HU-XXX - Título breve

Cada historia incluye criterios de aceptación (AC) y tareas sugeridas.

---

## HU-01 - Generar código QR único para muestra (Técnico)
**Descripción**: Como técnico quiero generar un código QR/Barcode para cada muestra para poder trazarla desde la toma hasta la emisión del informe.

**Criterios de aceptación (AC):**
- AC1: Al registrar una nueva muestra se genera `sample_code` único.
- AC2: Se genera y devuelve un QR o barcode legible (imagen o URL) en la respuesta.
- AC3: Consultar `/samples/{sample_code}` devuelve historial parcial (registro preanalítico y estado actual).

**Tareas sugeridas:**
- Backend: Endpoint `POST /api/v1/samples` (crear muestra) + generar sample_code.
- Backend: Generar QR (librería qrcode) y almacenar ruta en metadata.
- Frontend: Formulario registrar muestra y mostrar QR.
- Tests: Pruebas unitarias y e2e básicas.

---

## HU-02 - Registrar toma de muestra (Técnico)
**Descripción**: Como técnico quiero registrar la toma de muestra (fecha/hora, responsable, condiciones) para garantizar trazabilidad preanalítica.

**AC:**
- AC1: El registro incluye timestamp server-side, user_id y condiciones (campo libre y tags como hemolizada/insuficiente).
- AC2: Estado de la muestra cambia a `taken` y queda en histórico.

**Tareas:**
- Backend: Endpoint `POST /api/v1/samples/{id}/take`.
- Frontend: UI para marcar muestra como tomada y añadir condiciones.
- Tests: Validaciones y cambios de estado.

---

## HU-03 - Subir y versionar documento (Responsable de Calidad)
**Descripción**: Como responsable de calidad quiero subir procedimientos y versionarlos para tener control documental.

**AC:**
- AC1: Endpoint `POST /api/v1/documents` acepta metadata + archivo.
- AC2: Subir nueva versión crea `document_version` con version_number incrementado.
- AC3: `GET /api/v1/documents/{id}` muestra versiones y metadata.

**Tareas:**
- Backend: Endpoints CRUD documentos y versiones.
- Storage: Guardar archivos en `uploads/` (MVP) y metadata en DB.
- Frontend: Formulario upload y vista historial.
- Tests: Upload tests and version increment.

---

## HU-04 - Registrar auditoría / historial (Auditor)
**Descripción**: Como auditor quiero poder consultar el historial de acciones (quién, cuándo, qué) para evidenciar cumplimiento.

**AC:**
- AC1: Todas las operaciones CRUD registran un documento en `audit_logs`.
- AC2: Endpoint `GET /api/v1/logs?from=&to=&user=&action=` devuelve paginado.

**Tareas:**
- Backend: Middleware o servicio que registre logs en MongoDB.
- Frontend: Página de logs con filtros.
- Tests: Validar creación de logs en cada operación.

---

## HU-05 - Crear usuarios y asignar roles (Admin)
**Descripción**: Como administrador quiero crear usuarios y asignar roles para controlar accesos.

**AC:**
- AC1: Endpoint `POST /api/v1/users` crea usuario con role.
- AC2: Endpoint de login devuelve JWT con claims de rol.
- AC3: Endpoints protegidos por middleware RBAC.

**Tareas:**
- Backend: Auth endpoints (register/login), RBAC middleware.
- Frontend: UI admin para gestionar usuarios.
- Tests: Auth flows y permisos.

---

## HU-06 - Alertas de calibración y vencimiento (Responsable)
**Descripción**: Como responsable quiero recibir alertas de calibraciones y vencimientos de documentos para planificar acciones.

**AC:**
- AC1: Sistema crea alertas cuando fecha próxima < threshold (configurable).
- AC2: Endpoint `GET /api/v1/alerts` lista alertas activas.

**Tareas:**
- Backend: Job periódico (cron) que evalúe fechas y cree alertas.
- Frontend: Panel de alertas y notificaciones.
- Tests: Simulación de fechas y generación de alertas.

---

# Prioridad inicial
- Alta: HU-01, HU-02, HU-03, HU-05, HU-04
- Media: HU-06
- Baja: Integraciones (fase 2)
