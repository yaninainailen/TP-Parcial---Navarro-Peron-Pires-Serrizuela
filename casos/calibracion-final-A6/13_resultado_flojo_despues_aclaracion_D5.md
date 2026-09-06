# Resultado completo A6 — Caso flojo después de la aclaración específica de D5

## Condiciones de ejecución

- Fecha: 2026-09-06 (America/Buenos_Aires).
- Interfaz: ChatGPT anónimo en `chatgpt.com`, conversación nueva.
- Se realizó una única ejecución.
- SHA-256 del mensaje completo enviado:
  `b7c4b0d981cc1c6b51e0ad0f3e3d8b2bdf6a0def78e8f491b6b5fdeecd14797b`.
- System prompt: contenido vigente de `agente/system_prompt.md`, idéntico a HEAD
  `67473c559f369752cea8371ae4c9c36462fcc4a1`.
- Rúbrica: `rubrica.md` con la aclaración provisional de D4 y la única aclaración nueva de D5.
- Caso: `casos/flojo/Caso-Ej-2`, sin cambios y con el mismo orden de archivos e instrucción final.
- El nombre exacto del modelo, temperatura, seed y otros parámetros no fueron expuestos por la
  interfaz; no se infieren.

## Respuesta completa

# Corrección — Caso-Ej-2

## Puntaje por dimensión

| Dimensión | Nivel asignado | Puntos | Evidencia citada |
|---|---|---|---|
| Sistema completo y funcionando | Insuficiente | 10/30 | `system_prompt.md` y `user_prompt.md` existen, pero no hay herramienta o conector real. Las salidas de `corrida_1.md` y `corrida_2.md` tampoco mantienen un mismo formato estructurado. La supervisión solo aparece como “Si es un reclamo, avisame” y, en `corrida_2.md`, “Habría que avisarle a un humano”. |
| Proceso documentado | Insuficiente | 8/25 | `DECISIONES.md` contiene únicamente “Probé el prompt varias veces hasta que anduvo más o menos bien. No hubo grandes problemas.” No documenta dos iteraciones concretas ni una falla real con antes/después. |
| Formato y reproducibilidad | Insuficiente | 5/15 | Existen `README.md`, `prompts/`, `corridas/` y `DECISIONES.md`, pero solo hay 2 corridas en lugar de 3 y `corrida_1.md` no tiene fecha. Se combinan dos huecos de reproducibilidad. |
| Análisis económico | Insuficiente | 5/15 | `README.md` solo afirma “El modelo no es caro así que se puede usar tranquilamente para esto”, sin costos, tokens, proyección ni justificación del modelo. |
| Gobierno y riesgo | Insuficiente | 5/15 | `system_prompt.md` indica “Si es un reclamo, avisame” y `corrida_2.md` menciona avisar a un humano antes de enviar algo, pero no define permisos, riesgos concretos, manejo de errores, revisión ni responsable que firme. |

## Puntaje total: 33/100

## Justificación por dimensión

**Sistema completo y funcionando:** El repositorio contiene los dos prompts, pero no evidencia
ninguna herramienta o conector real ni su uso en las corridas. Además, `corrida_1.md` usa
“Categoría: Consulta de producto. Respuesta sugerida: ...”, mientras `corrida_2.md` presenta una
respuesta en prosa, por lo que no hay un formato estructurado común.

**Proceso documentado:** `DECISIONES.md` solo dice “Probé el prompt varias veces hasta que anduvo
más o menos bien”. No hay dos iteraciones concretas, cambios documentados ni una falla reconocida
con evidencia textual.

**Formato y reproducibilidad:** La estructura principal está presente, pero solo existen
`corrida_1.md` y `corrida_2.md`; falta una tercera corrida. Además, `corrida_1.md` no incluye
fecha, mientras `corrida_2.md` sí está fechada como 2026-08-28. Por la combinación de ambos
huecos corresponde Insuficiente.

**Análisis económico:** `README.md` afirma que “El modelo no es caro”, pero no aporta ningún
número propio de costo o tokens. Tampoco existe proyección a escala ni una elección de modelo
justificada mediante el criterio del modelo más chico que hace bien la tarea.

**Gobierno y riesgo:** La única supervisión identificable es la instrucción “Si es un reclamo,
avisame” en `system_prompt.md` y la mención de `corrida_2.md` a avisar a un humano. No hay
evidencia sobre sistemas o datos afectados, permisos, qué ocurre ante errores, qué revisa
concretamente el humano ni quién firma.

## Señales de alerta

⚠️ Posible caso de trabajo tramposo detectado

- `README.md` afirma que el agente “Anda bien, contesta los mensajes que le paso”, pero las
  corridas verificables muestran formatos de salida diferentes y no existe evidencia de una
  herramienta o conector real.
- `DECISIONES.md` afirma que “No hubo grandes problemas”, pero no aporta las iteraciones ni
  evidencia concreta necesaria para reconstruir el proceso.
- La falta de evidencia afecta al menos dos dimensiones en las que las afirmaciones del
  repositorio no alcanzan para respaldar lo declarado: Sistema completo y funcionando, y Proceso
  documentado.

## Sugerencia concreta de mejora

Agregar una tercera corrida fechada y hacer que las tres usen exactamente el mismo formato
estructurado, documentar en `DECISIONES.md` al menos dos iteraciones con ejemplos antes/después y
una falla real, e incorporar números de tokens/costo y una política L0–L4 de supervisión humana
respaldada por una herramienta o conector efectivamente utilizado.
