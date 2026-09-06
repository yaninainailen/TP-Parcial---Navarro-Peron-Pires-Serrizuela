# Resultado completo A6 — Replicación D4 R1

## Condiciones de ejecución

- Fecha: 2026-09-06 (America/Buenos_Aires).
- Interfaz: ChatGPT anónimo en `chatgpt.com`, conversación nueva.
- Entrada: exactamente la misma utilizada en R2, construida una sola vez y reutilizada sin
  cambios.
- SHA-256 del mensaje completo enviado:
  `a2e360d6bca37f4248e35fbc03ba27d7af60870d7cae894d564c9b0cf468e368`.
- System prompt: contenido vigente de `agente/system_prompt.md`, idéntico a HEAD
  `67473c559f369752cea8371ae4c9c36462fcc4a1`.
- Rúbrica: `rubrica.md` con la aclaración específica de D4 mantenida provisionalmente.
- Caso: `casos/flojo/Caso-Ej-2`, sin cambios.
- El nombre exacto del modelo, temperatura, seed y otros parámetros no fueron expuestos por la
  interfaz; no se infieren.

## Respuesta completa

# Corrección — Caso-Ej-2

## Puntaje por dimensión

| Dimensión | Nivel asignado | Puntos | Evidencia citada |
|---|---|---|---|
| Sistema completo y funcionando | Insuficiente | 10/30 | `prompts/system_prompt.md` y `prompts/user_prompt.md` existen, pero no hay herramienta/conector real ni niveles L0–L4 de supervisión. Además, las dos corridas no mantienen un formato estructurado común. |
| Proceso documentado | Insuficiente | 8/25 | `DECISIONES.md` contiene solo: “Probé el prompt varias veces hasta que anduvo más o menos bien. No hubo grandes problemas.” No documenta dos iteraciones concretas ni una falla real. |
| Formato y reproducibilidad | Insuficiente | 5/15 | Existen `README.md`, `prompts/`, `corridas/` y `DECISIONES.md`, pero hay solo 2 corridas y `corridas/corrida_1.md` no tiene fecha. Se combinan dos huecos menores. |
| Análisis económico | Insuficiente | 5/15 | `README.md` solo afirma: “El modelo no es caro así que se puede usar tranquilamente para esto.” No hay costos, tokens, proyección ni justificación del modelo. |
| Gobierno y riesgo | Insuficiente | 5/15 | En `corridas/corrida_2.md` se menciona “Habría que avisarle a un humano antes de mandar nada”, pero no se detallan sistemas, permisos, manejo de fallas, revisión formal ni quién firma. |

## Puntaje total: 33/100

## Justificación por dimensión

**Sistema completo y funcionando:** `system_prompt.md` define un agente de atención y pide
clasificar y responder, pero no define un formato de salida completo ni una herramienta real.
Tampoco establece supervisión L0–L4; la única referencia a un humano aparece informalmente en
`corrida_2.md`.

**Proceso documentado:** `DECISIONES.md` se limita a “Probé el prompt varias veces hasta que
anduvo más o menos bien”. No permite reconstruir qué se probó, qué falló ni qué cambios se
hicieron, y no contiene una falla concreta reconocida.

**Formato y reproducibilidad:** La estructura principal está presente, pero solo hay
`corrida_1.md` y `corrida_2.md`, cuando se exigen 3 corridas. Además, `corrida_1.md` no tiene
fecha, mientras que `corrida_2.md` sí incluye “2026-08-28”.

**Análisis económico:** `README.md` dice que “El modelo no es caro”, pero no aporta ningún número
propio. No hay tokens, costo por corrida, proyección a escala ni criterio explícito de elegir el
modelo más chico que resuelva bien la tarea.

**Gobierno y riesgo:** El caso de reclamo de `corrida_2.md` reconoce que “Habría que avisarle a un
humano antes de mandar nada”, pero no existe una política concreta de permisos, tratamiento de
errores, revisión humana o firma del resultado.

## Señales de alerta

## Sugerencia concreta de mejora

Completar `DECISIONES.md` con dos iteraciones verificables (entrada, falla observada y cambio
realizado) y documentar en el prompt una política L0–L4 de supervisión; además, agregar una
tercera corrida fechada y un análisis económico con tokens, costo por corrida y proyección a
escala.
