DOC_GUIDE — Guía de documentación del proyecto

> Define QUÉ se documenta, DÓNDE y CON QUÉ formato.
> Aplica a documentación técnica interna y al manual de usuario final.

## 1. Principios
- Documentar lo que no es obvio en el código. No repetir el código.
- Un documento = una audiencia. No mezclar técnico con usuario final.
- Todo cambio de comportamiento actualiza su doc en el mismo commit.
- Fechar y versionar la documentación.
- Idioma: <código en inglés / docs en español> (ajustar).

## 2. Mapa de documentos
| Documento            | Audiencia      | Propósito                        | Se actualiza        |
|----------------------|----------------|----------------------------------|---------------------|
| README.md            | devs / nuevo   | qué es, cómo instalar y correr   | al cambiar setup    |
| PROJECT_CONTEXT.md   | devs / modelos | estado y cronología              | tras cada decisión  |
| DECISIONS.md         | devs           | detalle de cada decisión         | tras cada decisión  |
| CHANGELOG.md         | devs / usuario | cambios por versión              | en cada release     |
| docs/api.md          | devs           | endpoints / contratos            | al cambiar API      |
| docs/arquitectura.md | devs           | visión general y diagramas       | al cambiar diseño   |
| MANUAL_USUARIO.md    | cliente        | cómo usar el producto            | al cambiar UX       |

## 3. Documentación técnica — reglas
- README: qué hace, requisitos, instalación, ejecución, tests, variables de
  entorno (sin valores reales), cómo contribuir.
- API: método, ruta, parámetros, ejemplo request/response, códigos de error.
- Arquitectura: un diagrama simple + explicación de módulos y su relación.
- Comentarios en código: solo para el "por qué", nunca el "qué".
- Changelog: formato Keep a Changelog. Secciones: Added, Changed, Fixed,
  Removed, Security.
- Secretos: nunca documentar valores reales; usar placeholders.

## 4. Manual de usuario — estructura estándar
1. Portada: nombre, versión, fecha.
2. Índice.
3. Introducción: qué es el producto y para quién.
4. Requisitos previos: navegador, cuenta, permisos, instalación si aplica.
5. Primeros pasos: registro / login / configuración inicial (con capturas).
6. Guía por funcionalidad (una sección por tarea del usuario):
   - Qué hace
   - Cuándo usarlo
   - Paso a paso numerado
   - Captura o GIF por paso clave
   - Resultado esperado
7. Preguntas frecuentes (FAQ).
8. Solución de problemas comunes (síntoma → causa → solución).
9. Glosario de términos del dominio.
10. Contacto / soporte / versión del documento.

## 5. Reglas de redacción del manual
- Lenguaje del usuario, no del desarrollador. Sin jerga técnica.
- Voz activa, frases cortas, pasos numerados.
- Cada paso = una acción.
- Capturas actualizadas a la versión documentada.
- Ejemplos reales, no abstractos.
- Un apartado de "errores comunes" obligatorio.

## 6. Checklist antes de dar por buena una doc
- [ ] Audiencia correcta (técnica vs usuario).
- [ ] Versión y fecha indicadas.
- [ ] Enlaces internos funcionando.
- [ ] Sin secretos ni datos reales.
- [ ] Capturas al día (manual).
- [ ] Revisada contra el comportamiento real del código.
- [ ] Aprobada por el usuario (Regla Cero).

## 7. Flujo de actualización
1. Se implementa y aprueba un cambio.
2. En el mismo commit/PR se actualiza la doc afectada.
3. Se propone la entrada de CHANGELOG y, si toca UX, del manual.
4. El usuario aprueba la doc antes de mergear.