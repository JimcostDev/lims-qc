# Backlog Inicial - Sistema SGQ / LIMS (MVP)
**Versión:** 0.1
**Prioridad:** Ordenada de alta a baja

## Epics
- EPIC-1: Gestión Documental (R1)
- EPIC-2: Gestión de Usuarios y Seguridad (R2)
- EPIC-3: Gestión de Muestras (R3)
- EPIC-4: Auditoría y Logs (R4)
- EPIC-5: Reportes y Dashboards (R5)
- EPIC-6: Equipos y Calibraciones (R6)
- EPIC-7: Integraciones (R7)

## Backlog (items prioritizados)
### EPIC-1: Gestión Documental
- BL-001: HU-03 - Subir documento (metadata + archivo) **(MVP)**
- BL-002: HU-03 - Versionar documento y mantener historial **(MVP)**
- BL-003: BL-001 - Descargar versión específica **(MVP)**
- BL-004: BL-001 - Listado filtrable y alertas vencimiento

### EPIC-2: Gestión Usuarios y Seguridad
- BL-005: HU-05 - Crear usuarios y asignar roles **(MVP)**
- BL-006: BL-005 - Login JWT y refreshtokens **(MVP)**
- BL-007: BL-005 - Endpoint para roles/permisos (admin)

### EPIC-3: Gestión Muestras
- BL-008: HU-01 - Generar sample_code único y QR **(MVP)**
- BL-009: HU-02 - Registrar patient + order + take sample **(MVP)**
- BL-010: BL-009 - Registrar incidencias y rechazos **(MVP)**
- BL-011: BL-008 - Consultar historial por QR/barcode **(MVP)**

### EPIC-4: Auditoría y Logs
- BL-012: HU-04 - Registrar actions CRUD en audit_logs **(MVP)**
- BL-013: BL-012 - Endpoint para consultar logs (admin)
- BL-014: BL-012 - Retención y export logs a CSV/PDF

### EPIC-5: Reportes y Dashboards
- BL-015: Reporte PDF de resultados por muestra
- BL-016: Dashboard KPIs básicos (muestras por estado, rechazadas)

### EPIC-6: Equipos y Calibraciones
- BL-017: CRUD equipos e historial de calibración
- BL-018: Alertas calibración próxima

### EPIC-7: Integraciones
- BL-019: API para sincronizar con RQCLAB (fase 2)
- BL-020: Importer/Exporter CSV de órdenes

## Sugerencia de etiquetas (GitHub Issues)
- `mvp`, `bug`, `enhancement`, `backend`, `frontend`, `infra`, `security`, `doc`

## Sugerencia de columnas (GitHub Projects)
- Backlog, To do (sprint), In progress, Code review, QA, Done

---
*Fin Backlog inicial*