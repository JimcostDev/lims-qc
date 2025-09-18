# lims-qc

**LIMS-QC** — *Laboratory Information Management & Quality Control System*  
Plantilla / repositorio inicial para el desarrollo de un SGC/LIMS alineado con **ISO 15189:2022**. Pensado para arrancar rápido (MVP) y escalar: control documental, trazabilidad de muestras y auditoría.

---

## ✅ Características (MVP)
- Gestión documental con versionado (subida, historial, descargas).  
- Registro y trazabilidad de muestras (generación de `sample_code` + QR).  
- Gestión de usuarios y roles (RBAC).  
- Audit logs append-only (motor flexible: MongoDB recomendado).  
- Reportes básicos (PDF / export Excel).  
- API REST `/api/v1` con documentación OpenAPI (FastAPI).

---

## 🧩 Stack propuesto
- **Backend:** FastAPI (Python)   
- **DB principal:** PostgreSQL (recomendado)  
- **Audit logs / events:** MongoDB (opcional o complemento)  
- **Frontend:** Astro + React + Tailwind *o* React + Vite + Tailwind  
- **Storage de archivos:** local `uploads/` (MVP) → MinIO / S3 a producción  
- **Contenerización:** Docker / docker-compose  
- **Tests:** pytest, pytest-asyncio

> Nota: Se puede implementar todo en Mongo: viable para MVP pero exigiría disciplina (transacciones, validaciones JSON Schema, índices, políticas de auditoría).

---


## 🛠️ Instalación local (rápida)
```bash
# clonar
git clone https://github.com/JimcostDev/lims-qc.git
cd lims-qc

# crear venv
python -m venv venv
source venv/bin/activate    # linux/mac
# venv\Scripts\activate     # windows

# instalar dependencias
pip install -r requirements.txt
```

---


## 🔐 Autenticación y RBAC
- Sistema con JWT (python-jose), rutas protegidas por dependencias.  
- Roles: `admin`, `quality`, `technician`, `auditor`, `viewer`.  
- Implementar middleware/dependencies para validar permisos en endpoints.

---

## 📁 Almacenamiento de archivos
- MVP: `uploads/` local.  
- Producción: MinIO o S3.  
- Guarda metadata en DB (`document_versions`) y path/URL al archivo.

---

## 🧪 Tests
```bash
pytest -q
# Para tests async
pytest -q --disable-warnings
```

---

## 📋 Roadmap (resumido)
- Sprint 1: Auth/RBAC + CRUD Documentos + Audit logs  
- Sprint 2: Registro pacientes + muestras + QR generation  
- Sprint 3: Versioning completo + alertas vencimiento + tests  
- Sprint 4: Results + PDF reports  
- Sprint 5: Auditorías / NC workflows / dashboards

---

## 🧭 Cómo contribuir
1. Crea un issue o toma una tarea del proyecto.  
2. `git checkout -b feat/<issue-id>-short-desc`  
3. Tests y lint antes de PR.  
4. PR con descripción, screenshots, y referencias a issues.


