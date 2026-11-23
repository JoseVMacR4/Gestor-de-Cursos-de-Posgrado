PROJECT_CONTEXT — Estado y cronología del desarrollo

> Documento vivo. Se actualiza tras cada decisión implementada.
> Propósito: que cualquier modelo o desarrollador nuevo retome el proyecto
> sabiendo qué se hizo, en qué orden, por qué y en qué punto está.

## 1. Resumen del proyecto
- Nombre:
- Objetivo (1-3 líneas):
- Stack y versiones fijas:
- Repositorio / rama principal:
- Entorno de despliegue:

## 2. Estado actual (actualizar en cada sesión)
- Fecha de última actualización:
- Fase actual: (ej. diseño / MVP / hardening / release)
- Rama de trabajo activa:
- Último commit aprobado (hash + mensaje):
- Qué está terminado y aprobado:
- Qué está en curso (y en qué paso del protocolo):
- Qué está pendiente y aún NO aprobado:
- Bloqueos o riesgos abiertos:

## 3. Decisiones implementadas (cronológico, más reciente arriba)
Formato por entrada:

### [YYYY-MM-DD] <título corto>
- Contexto: <por qué surgió>
- Opciones consideradas: <A, B, C>
- Elegida y motivo: <X porque ...>
- Archivos afectados: <lista>
- Tests añadidos: <sí/no, cuáles>
- Aprobada por: <usuario>
- Commit: <hash>
- Reversible con: <comando o nota>

## 4. Sugerencias NO implementadas (backlog de ideas)
> Todo lo que se propuso y no se aprobó. No ejecutar sin nueva aprobación.
- [ ] <idea> — propuesta el <fecha>, motivo de no aprobación:
- [ ]

## 5. Convenciones acordadas (resumen rápido)
- Idioma código / docs:
- Formato de commits / ramas:
- Cobertura mínima de tests:
- Linter / formatter:
- Estructura de carpetas:
- Qué NO tocar:

## 6. Cómo retomar (instrucciones para el próximo modelo)
1. Lee este archivo completo antes de proponer nada.
2. Lee DECISIONS.md para el detalle de cada decisión.
3. NO ejecutes nada: primero reformula el estado y propón el siguiente paso
   según el SYSTEM_PROMPT (protocolo de aprobación).
4. Confirma con el usuario el "Estado actual" antes de continuar.