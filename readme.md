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
- **ORM / Models:** SQLModel (Postgres) *o* ODM (si eliges Mongo full)  
- **DB principal:** PostgreSQL (recomendado)  
- **Audit logs / events:** MongoDB (opcional o complemento)  
- **Frontend:** Astro + React + Tailwind *o* React + Vite + Tailwind  
- **Storage de archivos:** local `uploads/` (MVP) → MinIO / S3 a producción  
- **Contenerización:** Docker / docker-compose  
- **Tests:** pytest, pytest-asyncio

> Nota: Si tu equipo prefiere MongoDB, se puede implementar todo en Mongo: viable para MVP pero exigiría disciplina (transacciones, validaciones JSON Schema, índices, políticas de auditoría).

---

## Estructura sugerida del repo
```
.
├─ api/                 # routers / endpoints
├─ core/                # configuración, database init
├─ models/              # SQLModel / ODM models
├─ repositories/        # capa persistencia
├─ services/            # lógica de negocio
├─ utils/               # utilidades (storage, qr, etc.)
├─ assets/              # archivos estáticos
├─ tests/               # pruebas unitarias / integración
├─ docker-compose.yml
├─ requirements.txt
└─ README.md
```

---

## ⚙️ Requisitos previos
- Python 3.11+  
- Docker & docker-compose (recomendado para dev)  
- (Opcional) Node.js para frontend

---

## 🛠️ Instalación local (rápida)
```bash
# clonar
git clone https://github.com/<tu-org>/lims-qc.git
cd lims-qc

# crear venv
python -m venv venv
source venv/bin/activate    # linux/mac
# venv\Scripts\activate     # windows

# instalar dependencias
pip install -r requirements.txt
```

---

## ⚙️ Variables de entorno (ejemplo)
Crea `core/config.env` con (MVP):

```env
# DB engine: "mongodb" or "postgresql"
DB_ENGINE=mongodb

# Mongo
MONGODB_URI_DEV_LAB_TEST=mongodb://user:pass@localhost:27017
MONGODB_NAME=lims_qc_dev

# Postgres (si usas postgres)
POSTGRES_URI=postgresql+asyncpg://user:pass@localhost:5432/lims_qc_dev

# JWT
JWT_SECRET_KEY=supersecreto
JWT_ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=60

# App
PROJECT_NAME=LIMS-QC
API_PREFIX=/api/v1
CORS_ORIGINS=http://localhost:3000
```

---

## 🚀 Levantar con Docker (recomendado)
Hay un `docker-compose.yml` ejemplo que levanta Postgres + Mongo + app.  
```bash
docker-compose up --build
# luego
# acceder: http://localhost:8000/docs
```

---

## Ejecutar la app en dev
```bash
# activar venv
uvicorn main:app --reload --host 0.0.0.0 --port 8000
# OpenAPI: http://localhost:8000/api/v1/openapi.json
# Docs: http://localhost:8000/docs
```

---

## Migraciones (Postgres)
Si usas SQLModel + Alembic:
```bash
# inicializar alembic (si no existe)
alembic init alembic
# generar
alembic revision --autogenerate -m "create models"
# aplicar
alembic upgrade head
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

Asegúrate de configurar fixtures para DB (usar instancias docker o bases de datos temporales).

---

## 📦 CI / CD
- GitHub Actions sugerido:
  - Test matrix (py versions)
  - Linting (ruff/flake8)
  - Build + push image (si se usa)
- Deploy: Render / Railway / VPS + Docker

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

---

## 📞 Contacto / Créditos
- Proyecto: **LIMS-QC**  
- Equipo: [Tu nombre] (Owner)  
- Cliente: Yaneth Cardoza

---

## 📜 Licencia
MIT — ver `LICENSE` (añade archivo si aplica).
