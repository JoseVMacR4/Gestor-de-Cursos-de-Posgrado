PROJECT_CONTEXT — Estado y cronología del desarrollo

> Documento vivo. Se actualiza tras cada decisión implementada.
> Propósito: que cualquier modelo o desarrollador nuevo retome el proyecto
> sabiendo qué se hizo, en qué orden, por qué y en qué punto está.

## 1. Resumen del proyecto
- Nombre: Sistema de Gestión de Cursos de Posgrado (`Gestor-de-Cursos-de-Posgrado`)
- Objetivo (1-3 líneas): Aplicación de escritorio local para centralizar el registro de libros, tomos, folios y números operativos de cursos de posgrado, importar y gestionar informes finales en formato `.docx`, y automatizar la generación y exportación masiva de certificados en PDF.
- Stack y versiones fijas:
  - Lenguaje: Python 3.12
  - Interfaz gráfica (UI): PySide6 6.7+
  - Base de datos y ORM: SQLite 3 (`PRAGMA foreign_keys = ON`, `PRAGMA journal_mode = WAL`) + SQLAlchemy 2.0+ (esquema gestionado con `create_all` y control de versión de esquema vía `PRAGMA user_version` / scripts incrementales simples)
  - Procesamiento de documentos e informes: `python-docx` 1.1+ (lectura/parseo de `.docx` para INF-01)
  - Generación de certificados PDF: `ReportLab` 4.2+ (renderizado vectorial para certificados y maquetación de impresión de libros/cuartillas; exportación masiva en `.zip`)
  - Seguridad local: `bcrypt` / `argon2-cffi` (hashing de credenciales locales)
  - Empaquetado / Distribución: `PyInstaller` 6.x (generación de ejecutable/portable offline)
  - Calidad y Testing: `pytest`, `ruff`
- Repositorio / rama principal: Gestor-de-Cursos-de-Posgrado / main
- Entorno de despliegue: Local (Windows 10 64-bit / Windows 11 — Ejecutable / Portable 100% offline, compatible con ejecución desde carpeta local o USB, con base de datos SQLite y respaldos locales)
- Estrategia de rutas y directorios de datos: Resolución centralizada en `src/utils/app_paths.py` mediante `sys.frozen` y `Path(sys.executable).parent` en producción (carpeta portable autocontenida con `data/` para SQLite, plantillas y firmas, y `backups/` para respaldos `.zip` generados con `sqlite3.Connection.backup()`), y directorio raíz del proyecto en desarrollo.

## 2. Estado actual (actualizar en cada sesión)
- Fecha de última actualización: 2026-11-24
- Fase actual: Fase 1 — Diseño y arquitectura (Iniciando SYS-01: Modelado de Base de Datos)
- Rama de trabajo activa: main (pendiente crear rama feature para materializar contexto y/o iniciar SYS-01)
- Último commit aprobado (hash + mensaje): Inicialización de documentación base y definición de stack tecnológico
- Qué está terminado y aprobado:
  - Planificación PERT y cronograma por fases (F1: 10/12/26, F2: 11/01/27, F3: 29/01/27).
  - Gobernanza del asistente (`MODEL_ROL.md`) y guía de documentación (`DOC_GUIDE.md`).
  - Definición completa de stack (`PySide6`), rutas portables, SQLite/respaldos, jerarquía `Libro -> Tomo -> Folio (25 espacios, 200 folios/tomo = 5,000 espacios) -> Número`, reglas de numeración por informe y `audit_log` ligero.
- Qué está en curso (y en qué paso del protocolo): Listo para iniciar el Paso 1 del protocolo con `SYS-01` (Modelado de Base de Datos).
- Qué está pendiente y aún NO aprobado:
  - Fase 1 (Entrega 10/12/26): SYS-01 Modelado BD, SYS-02 Arquitectura del Proyecto, SYS-03 Seguridad y Login Local, MAE-01 CRUD Libros de Registro (y tomos asociados), MAE-02 CRUD Folios Operativos (y números/posiciones asociados).
  - Fase 2 (Entrega 11/01/27): INF-01 Importar Informe Final (`.docx`), INF-02 Registrar Informe Final, INF-03 Mostrar Informe Final, INF-04 Editar Informe Final, SYS-04 Motor de Respaldo Local (`.zip` con `sqlite3.Connection.backup()`).
  - Fase 3 (Entrega 29/01/27): CER-01 Motor de Generación PDF Certificados, CER-02 Gestión de Datos y Firmas Certificados, CER-03 Exportación Masiva de Certificados, SYS-05 Instalador Ejecutable / Portable.
- Bloqueos o riesgos abiertos:
  - Ninguno para Fase 1.
  - Para Fase 2 (`INF-01`): Se requerirá una muestra real de Informe Final `.docx` para mapear su estructura y extraer profesor principal, profesores y cursistas.
  - Para Fase 3 (`CER-01`): Se requerirá el diseño oficial del certificado, formato de impresión de cuartillas/libros y las imágenes de firmas/sellos.

## 3. Decisiones implementadas (cronológico, más reciente arriba)

### [2026-11-24] Reglas de jerarquía de registro y numeración por informe
- Contexto: Cada libro corresponde a un año, se divide en tomos (200 folios por defecto; cada folio es una cuartilla física de 25 espacios, equivalente a 5,000 espacios por tomo). Cada informe debe iniciar en el primer folio vacío, sin reutilizar huecos sobrantes de folios de informes anteriores.
- Opciones consideradas: Numeración global continua sin reinicio vs. numeración correlativa (`1..N`) reiniciada por informe con conteo de posiciones/huecos no reutilizables en los folios.
- Elegida y motivo: Cada informe numera correlativamente desde `1` hasta el total de personas (`1` = profesor principal, luego demás profesores, luego cursistas), ocupando posiciones consecutivas en folios de 25 espacios a partir del primer folio vacío; los espacios sobrantes del último folio ocupado quedan vacíos/no ocupables para otros informes, manteniendo el seguimiento de fondo de posiciones y cuartillas para su posterior impresión física.
- Archivos afectados: `PROJECT_CONTEXT.md`, `DECISIONS.md`
- Tests añadidos: No (definición de reglas de dominio)
- Aprobada por: Usuario
- Commit: pendiente
- Reversible con: Ajustar reglas del servicio de asignación en rama de trabajo antes de consolidar `MAE-02` / `INF-02`.

### [2026-11-24] Selección del stack tecnológico, rutas portables y criterios de BD/auditoría
- Contexto: Se requiere una aplicación de escritorio local con login y respaldos locales (SYS-03, SYS-04), importación de informes `.docx` (INF-01), emisión masiva de certificados PDF (CER-01, CER-03), empaquetado ejecutable/portable (SYS-05) y jerarquía de registro Libro -> Tomo -> Folio -> Número.
- Opciones consideradas:
  - UI: `PySide6` vs `CustomTkinter` (y opción web local descartada previamente).
  - Migraciones: esquema con `SQLAlchemy` + `PRAGMA user_version` vs `Alembic`.
  - Rutas: carpeta portable junto al ejecutable (`data/`, `backups/`) vs `%APPDATA%`.
  - Respaldos: `sqlite3.Connection.backup()` en `.zip` vs copia simple de archivo.
- Elegida y motivo: `PySide6` por tablas nativas, diálogos estándar y evitar empaquetado de recursos de temas; rutas junto al ejecutable para portabilidad (incluyendo USB) y respaldos fáciles; `SQLAlchemy` + `PRAGMA user_version` para evitar complejidad de `Alembic` en un empaquetado monousuario local; `sqlite3.Connection.backup()` en `.zip` para respaldos consistentes en caliente; `audit_log` ligero en operaciones clave pensando en escalabilidad futura.
- Archivos afectados: `PROJECT_CONTEXT.md`, `DECISIONS.md`
- Tests añadidos: No (definición de contexto y arquitectura inicial)
- Aprobada por: Usuario
- Commit: pendiente
- Reversible con: Ajustar la decisión en rama de trabajo antes de consolidar `SYS-01` / `SYS-02`.

## 4. Sugerencias NO implementadas (backlog de ideas)
- [ ] Validación de certificados mediante código QR local con hash de verificación — propuesta el 2026-11-24, motivo de no aprobación: fuera del alcance presupuestado inicial.
- [ ] Interfaz avanzada de consulta/exportación de bitácora de auditoría — propuesta el 2026-11-24, motivo de no aprobación: se mantiene solo registro ligero automático en `audit_log` para no ampliar el alcance de Fase 1.

## 5. Convenciones acordadas (resumen rápido)
- Idioma código / docs: Código (variables, funciones, clases, modelos, tablas BD) en inglés / Documentación, mensajes de commit, logs de usuario e interfaz gráfica en español.
- Formato de commits / ramas: Conventional Commits (`feat:`, `fix:`, `docs:`, `refactor:`, `test:`) en ramas `feature/<id-req>-descripcion` (ej. `feature/sys-01-database-model`). Prohibido hacer commits directos a `main`.
- Cobertura mínima de tests: Pruebas unitarias y de integración obligatorias con `pytest` para cada requerimiento nuevo; tests en verde antes de mergear.
- Linter / formatter: `ruff` (linter y formateador) + tipado estático básico con type hints nativos de Python 3.12.
- Estructura de carpetas:
  - `src/models/` (modelos SQLAlchemy y esquema BD)
  - `src/services/` (lógica de negocio: asignación de folios/números, importador `.docx`, generador PDF, respaldos, auditoría)
  - `src/ui/` (ventanas, tablas, componentes y vistas en `PySide6`)
  - `src/utils/` (`app_paths.py`, seguridad, validaciones, helpers)
  - `tests/` (pruebas unitarias y de integración)
  - `docs/` (`arquitectura.md`, manuales)
- Qué NO tocar: Archivos de gobernanza (`MODEL_ROL.md`, `DOC_GUIDE.md`), `.env` / secretos, configuración de empaquetado y rama `main` sin aprobación explícita (Regla Cero).

## 6. Cómo retomar (instrucciones para el próximo modelo)
1. Lee este archivo completo antes de proponer nada.
2. Lee DECISIONS.md para el detalle de cada decisión.
3. NO ejecutes nada: primero reformula el estado y propón el siguiente paso según el SYSTEM_PROMPT (protocolo de aprobación).
4. Confirma con el usuario el "Estado actual" antes de continuar.