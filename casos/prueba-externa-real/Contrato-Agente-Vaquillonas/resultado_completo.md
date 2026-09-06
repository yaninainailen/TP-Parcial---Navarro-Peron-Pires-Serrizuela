# Resultado completo — Contrato-Agente-Vaquillonas

La siguiente respuesta se conserva tal como fue proporcionada, sin correcciones manuales.

---

# Corrección — Contrato-Agente-Vaquillonas

## Puntaje por dimensión

| Dimensión | Nivel asignado | Puntos | Evidencia citada |
| --- | --- | --- | --- |
| Sistema completo y funcionando | Insuficiente | 10/30 | Existen `system_prompt.md` y `user_prompt.md`, pero la estructura detectada no contiene `prompts/` ni una herramienta/conector real verificable usado en las corridas. |
| Proceso documentado | Ausente | 0/25 | No existe `DECISIONES.md`; las iteraciones están documentadas dentro de `README.md`. |
| Formato y reproducibilidad | Insuficiente | 5/15 | El repo tiene 9 archivos, pero faltan las carpetas obligatorias `prompts/`, `corridas/` y el archivo `DECISIONES.md`; además las corridas no documentan fecha. |
| Análisis económico | Ausente | 0/15 | No se presenta análisis de tokens, costo por corrida, proyección a escala ni justificación económica del modelo. |
| Gobierno y riesgo | Ausente | 0/15 | No hay una sección verificable que defina permisos, manejo de errores, niveles L0–L4, revisión humana y quién firma el resultado. |

## Puntaje total: 15/100

## Justificación por dimensión

Sistema completo y funcionando:
El trabajo sí contiene un contrato técnico reconocible: `system_prompt.md` define el rol de apoyo técnico, las variables y los criterios reproductivos, y el README explicita el objetivo del agente. Sin embargo, la rúbrica exige además una herramienta o conector real usado en las corridas y supervisión humana L0–L4; la estructura reportada contiene solamente nueve archivos y no evidencia esos componentes.

Proceso documentado:
El README contiene un proceso de iteración sustantivo, pero esto no puede puntuar esta dimensión. La rúbrica dice expresamente que si las iteraciones están en `README.md` y no existe `DECISIONES.md` dedicado en la raíz, la dimensión debe calificarse Ausente.

Formato y reproducibilidad:
La estructura obligatoria exige `README.md`, `prompts/`, `corridas/` y `DECISIONES.md`, además de tres corridas con entrada, salida y fecha. La estructura efectivamente detectada tiene `README.md`, prompts y salidas como archivos en la raíz, pero no las carpetas `prompts/` y `corridas/`, ni `DECISIONES.md`.

Análisis económico:
No se encuentra evidencia de costo por corrida calculado con tokens reales, proyección semanal/anual ni elección del modelo con el criterio económico exigido. Por lo tanto, corresponde Ausente (0/15) según la rúbrica, que requiere una consideración económica verificable para obtener puntaje.

Gobierno y riesgo:
Aunque el contrato tiene restricciones técnicas sobre qué hacer ante datos faltantes o inconsistentes, no se documenta el esquema de gobierno exigido: sistemas/datos y permisos, qué ocurre concretamente ante fallas, qué revisa un humano y quién firma el resultado. Por ello no alcanza la evidencia necesaria para puntuar esta dimensión.

## Señales de alerta

No se detecta evidencia de números deliberadamente inventados ni un intento de manipular al evaluador. Las principales diferencias son estructurales: el README documenta mucho más de lo que la rúbrica permite reconocer cuando esa evidencia no está ubicada en los archivos y carpetas obligatorios.

## Sugerencia concreta de mejora

Reestructurar el mismo trabajo sin rehacer el agente: crear `prompts/` y mover allí `system_prompt.md` y `user_prompt.md`; crear `corridas/` y documentar allí tres corridas con entrada + salida + fecha; y crear `DECISIONES.md` en la raíz trasladando allí las dos iteraciones que actualmente están muy bien explicadas en el README.
