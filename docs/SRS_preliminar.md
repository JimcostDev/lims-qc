# SRS - Sistema de Gestión de Calidad y LIMS (MVP)
**Versión:** 0.1
**Autor:** Equipo de Desarrollo
**Fecha:** 2025-09-17

## 1. Introducción
### 1.1 Propósito
Este documento especifica los requisitos del Sistema de Gestión de Calidad y Gestión de Muestras (LIMS) para un laboratorio clínico, alineado con ISO 15189:2022. Sirve como base para el desarrollo del MVP y para planificar el backlog y sprints.

### 1.2 Alcance
**MVP (Fase 1)**:
- Módulo de Gobernanza / Gestión Documental (control documental, versionado, aprobaciones básicas).
- Módulo de Gestión de Muestras (preanalítico: registro paciente, generación de código QR/barcode; trazabilidad básica; registro de incidencias; emisión de informe básico en PDF).
- Gestión de usuarios y roles (Director, Responsable de Calidad, Técnico, Auditor, Viewer).
- Logs de auditoría (append-only) y dashboard básico de indicadores.
**Fase 2+**: Integraciones (RQCLAB), firma electrónica avanzada, portal paciente, gestión avanzada de equipos e inventario.

## 2. Definiciones, Acrónimos y Abreviaturas
- **LIMS**: Laboratory Information Management System
- **SGQ**: Sistema de Gestión de la Calidad
- **HU**: Historia de Usuario
- **R#**: Requisito numerado en este SRS

## 3. Stakeholders
- Cliente: Yaneth Cardoza (Director / Representante del laboratorio)
- Usuarios: Técnicos, Responsables de Calidad, Auditores, Dirección, Médicos solicitantes
- Equipo de desarrollo: Backend, Frontend, QA, DevOps

## 4. Requisitos funcionales (priorizados)
### R1 - Gestión Documental (Alta)
**Descripción:** Controlar documentos, versiones, responsables y vencimientos.
**Criterios de aceptación:**
- Subir documento con metadatos (título, tipo, responsable, fecha).
- Crear versiones y mantener historial (version_number, uploaded_by, note).
- Descargar versiones específicas.
- Listado filtrable y alertas por vencimiento.

### R2 - Gestión de Usuarios y Roles (Alta)
**Descripción:** CRUD de usuarios y asignación de roles y permisos (RBAC).
**Criterios de aceptación:**
- Admin puede crear/editar/desactivar usuarios.
- Roles: admin, quality, technician, auditor, viewer.
- Endpoints protegidos por permisos.

### R3 - Registro y Trazabilidad de Muestras (Alta - MVP)
**Descripción:** Registro preanalítico de muestras con código único (QR/barcode) y trazabilidad.
**Criterios de aceptación:**
- Registrar paciente y orden, generar `sample_code` único.
- Registrar toma de muestra (fecha/hora, responsable, conditions).
- Vínculo de muestra -> historial (trace) consultable por QR/barcode.
- Registrar incidencias (hemólisis, insuficiente, etc.).

### R4 - Auditoría y Logs (Alta)
**Descripción:** Registrar todas las acciones clave en logs append-only.
**Criterios de aceptación:**
- Cada acción CRUD registra user_id, action, timestamp, target, details.
- Logs consultables por rango de fecha y usuario.

### R5 - Reportes e Informes (Media)
**Descripción:** Exportes PDF/Excel e informes básicos.
**Criterios de aceptación:**
- Generar informe de resultados en PDF con metadatos.
- Exportar listados a Excel.

### R6 - Gestión de Equipos/Calibraciones (Media)
**Descripción:** Inventario de equipos, mantenimientos y alertas de calibración.
**Criterios de aceptación:**
- Registrar equipo, registrar mantenimiento/calibración y generar alertas.

### R7 - Integraciones (Baja)
**Descripción:** API para integrar con RQCLAB u otros sistemas.
**Criterios de aceptación:**
- Endpoints documentados para sincronización (fase 2).

## 5. Requisitos no funcionales
- **Seguridad:** HTTPS, JWT, RBAC, contraseñas seguras, cifrado at-rest (si es posible).
- **Disponibilidad:** Objetivo 99% en horario laboral (definir SLA con cliente).
- **Rendimiento:** Consultas de lista < 500ms en condiciones normales.
- **Escalabilidad:** Arquitectura modular, posibilidad de Migrar storage a S3/MinIO.
- **Mantenibilidad:** Código en Git, CI con tests automáticos, documentación en /docs.
- **Backups:** Respaldo diario de DBs y archivos.
- **Privacidad:** Retención y anonimización según norma local.

## 6. Modelado de datos (resumen)
Tablas/colecciones recomendadas (Postgres dominante):
- users, documents, document_versions, samples, sample_results, equipment, audit_logs, nc_actions

## 7. Interfaces externas y APIs
- API REST JSON (`/api/v1/...`) con OpenAPI (FastAPI).
- Integración opcional con S3/MinIO para archivos.
- Web frontend (Astro/React/Tailwind o React/Vite/Tailwind).

## 8. Criterios de aceptación y pruebas
Cada HU del backlog debe incorporar criterios de aceptación claros y pruebas automatizadas (pytest/pytest-asyncio).

## 9. Suposiciones y dependencias
- Cliente proveerá lista inicial de contratos/medicos para demo.
- Integración con RQCLAB queda para fase 2.
- Firma electrónica sujeta a requisitos legales del país.

## 10. Matriz de trazabilidad (resumen)
| Req ID | Requisito | HU relacionado | Sprint |
|---|---:|---|---:|
| R1 | Gestión documental | HU-03 | Sprint 1 |
| R2 | Usuarios y roles | HU-05 | Sprint 1 |
| R3 | Trazabilidad muestras | HU-01, HU-02 | Sprint 2 |
| R4 | Auditorías/Logs | HU-04 | Sprint 1/3 |

## 11. Plan de Sprints (sugerido)
- Sprint 0: Entorno, Docker, repo, config.
- Sprint 1 (2 semanas): Auth/RBAC + CRUD Documentos + audit logs.
- Sprint 2 (2 semanas): Registro pacientes + muestras + QR generation.
- Sprint 3 (2 semanas): Versioning completo + alertas vencimiento + tests.
- Sprint 4 (2 semanas): Results + PDF reports.
- Sprint 5 (2 semanas): Auditorías y NC workflows.

---
*Fin SRS preliminar (v0.1)*
