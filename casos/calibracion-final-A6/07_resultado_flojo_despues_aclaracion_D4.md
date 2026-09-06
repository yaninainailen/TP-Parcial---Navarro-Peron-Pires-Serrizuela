# Resultado completo A6 — Caso flojo después de la aclaración específica de D4

## Condiciones de ejecución

- Fecha: 2026-09-06 (America/Buenos_Aires).
- Interfaz: ChatGPT anónimo en `chatgpt.com`, conversación nueva.
- Caso: el mismo `casos/flojo/Caso-Ej-2` utilizado en la línea base A6, sin cambios.
- System prompt: contenido de `agente/system_prompt.md` idéntico a HEAD
  `67473c559f369752cea8371ae4c9c36462fcc4a1`.
- Rúbrica: contenido de `rubrica.md` con la única aclaración de los descriptores Insuficiente y
  Ausente de D4 documentada en esta iteración.
- Composición de entrada: la misma estructura, orden de archivos e instrucción final utilizadas
  en la línea base A6.
- El nombre exacto del modelo, temperatura, seed y otros parámetros no fueron expuestos por la
  interfaz; no se infieren.

## Respuesta completa

# Corrección — Caso-Ej-2

## Puntaje por dimensión

| Dimensión | Nivel asignado | Puntos | Evidencia citada |
|---|---|---|---|
| Sistema completo y funcionando | Insuficiente | 10/30 | `prompts/system_prompt.md` y `prompts/user_prompt.md` están presentes, pero no hay herramienta o conector real. Además, solo hay 2 corridas y sus formatos de salida no son estructurados ni consistentes. |
| Proceso documentado | Insuficiente | 8/25 | `DECISIONES.md` contiene solo: “Probé el prompt varias veces hasta que anduvo más o menos bien. No hubo grandes problemas.” No documenta 2 iteraciones concretas ni cambios antes/después, y tampoco reconoce una falla real. |
| Formato y reproducibilidad | Insuficiente | 5/15 | Están `README.md`, `prompts/`, `corridas/` y `DECISIONES.md`, pero hay solo 2 corridas en vez de 3 y `corrida_1.md` no tiene fecha. Se combinan dos huecos menores. |
| Análisis económico | Insuficiente | 5/15 | `README.md` solo afirma: “El modelo no es caro así que se puede usar tranquilamente para esto.” No hay costos, tokens, proyección ni justificación del modelo. |
| Gobierno y riesgo | Insuficiente | 5/15 | `system_prompt.md` solo indica “Si es un reclamo, avisame.” `corrida_2.md` agrega “Habría que avisarle a un humano antes de mandar nada, supongo”, pero no define permisos, tratamiento de fallas, revisión formal ni quién firma. |

## Puntaje total: 31/100

## Justificación por dimensión

**Sistema completo y funcionando:** Hay prompts reconocibles y un objetivo básico en
`system_prompt.md`: atender consultas de clientes y clasificarlas. Sin embargo, no existe ninguna
herramienta o conector real y las dos corridas no presentan una salida estructurada común:
`corrida_1.md` usa “Categoría...” mientras `corrida_2.md` entrega un párrafo libre.

**Proceso documentado:** `DECISIONES.md` existe, pero su única evidencia es “Probé el prompt
varias veces hasta que anduvo más o menos bien”. No identifica qué se probó, qué falló, qué texto
se cambió ni permite reconstruir iteraciones concretas; tampoco documenta una falla real.

**Formato y reproducibilidad:** La estructura principal está presente, pero solo hay
`corrida_1.md` y `corrida_2.md`; falta la tercera corrida exigida. Además, `corrida_1.md` no tiene
fecha, mientras `corrida_2.md` sí figura como “2026-08-28”, por lo que se acumulan dos huecos
menores.

**Análisis económico:** `README.md` dice que “El modelo no es caro”, pero no aporta ningún número
verificable. No hay tokens de entrada/salida, costo por corrida, proyección a escala ni criterio
explícito de elegir “el modelo más chico que hace bien la tarea”.

**Gobierno y riesgo:** La única regla explícita es “Si es un reclamo, avisame” en
`system_prompt.md`. En `corrida_2.md` aparece la idea condicional de avisar a un humano, pero no se
establecen niveles L0–L4, permisos, sistemas/datos involucrados, procedimiento ante errores o
responsable que firme el resultado.

## Señales de alerta

No se cumple ninguna de las dos condiciones obligatorias para abrir con ⚠️ Posible caso de
trabajo tramposo detectado: la regla anti-trampa no baja 2 o más dimensiones, y no hay ningún
número económico verificablemente falso. Las carencias observadas son omisiones, no
contradicciones matemáticas ni afirmaciones fuertes desmentidas por las corridas.

## Sugerencia concreta de mejora

Agregar una tercera corrida fechada y rehacer `DECISIONES.md` con al menos dos iteraciones
verificables —incluyendo el texto del problema, el cambio aplicado y una falla real— y documentar
en el prompt un formato de salida único junto con supervisión L0–L4, mientras se incorporan una
herramienta real y un cálculo de costo basado en tokens de las corridas.

## Observación de preservación

La respuesta anterior se conserva sin corregir: informó `31/100`. La suma aritmética de los cinco
puntajes que ella misma asignó es `10 + 8 + 5 + 5 + 5 = 33`, no 31.
