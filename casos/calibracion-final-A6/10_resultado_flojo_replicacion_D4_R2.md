# Resultado completo A6 — Replicación D4 R2

## Condiciones de ejecución

- Fecha: 2026-09-06 (America/Buenos_Aires).
- Interfaz: ChatGPT anónimo en `chatgpt.com`, conversación nueva.
- Entrada: exactamente la misma utilizada en R1, construida una sola vez y reutilizada sin
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
| Sistema completo y funcionando | Insuficiente | 10/30 | `system_prompt.md` describe clasificación y respuesta, pero no hay ninguna herramienta o conector real. Tampoco hay supervisión humana definida; solo se indica “Si es un reclamo, avisame”. |
| Proceso documentado | Insuficiente | 8/25 | `DECISIONES.md` contiene únicamente: “Probé el prompt varias veces hasta que anduvo más o menos bien. No hubo grandes problemas.” No documenta iteraciones concretas, cambios ni fallas reales. |
| Formato y reproducibilidad | Insuficiente | 5/15 | Están presentes `README.md`, `prompts/`, `corridas/` y `DECISIONES.md`, pero solo existen 2 corridas en lugar de 3 y `corrida_1.md` no tiene fecha. |
| Análisis económico | Insuficiente | 5/15 | `README.md` afirma: “El modelo no es caro así que se puede usar tranquilamente para esto”, pero no aporta números de costo, tokens, proyección ni justificación del modelo. |
| Gobierno y riesgo | Ausente | 0/15 | No existe una sección o evidencia que indique sistemas/datos y permisos, riesgos, tratamiento de errores, revisión humana o quién firma el resultado. |

## Puntaje total: 28/100

## Justificación por dimensión

**Sistema completo y funcionando:** `system_prompt.md` define al agente como uno que atiende
consultas de una tienda y pide clasificar y responder, pero no hay herramienta o conector real en
la estructura ni en las corridas. La supervisión se limita a “Si es un reclamo, avisame”, sin
niveles L0–L4, revisión ni responsable de firma.

**Proceso documentado:** `DECISIONES.md` solo dice “Probé el prompt varias veces hasta que anduvo
más o menos bien”. No permite reconstruir qué se probó, qué falló ni qué cambio se realizó, por lo
que no alcanza el nivel de proceso documentado.

**Formato y reproducibilidad:** La estructura básica requerida está presente, pero hay dos
huecos: solo se muestran `corrida_1.md` y `corrida_2.md`, y la primera no incluye fecha. Por la
combinación de ambos incumplimientos corresponde Insuficiente.

**Análisis económico:** El único análisis económico está en `README.md`, donde se afirma que “El
modelo no es caro”. No hay costos calculados a partir de tokens de las corridas reales, proyección
a escala ni criterio explícito de elección del modelo.

**Gobierno y riesgo:** No hay evidencia de permisos o sistemas/datos involucrados, manejo
concreto de errores, revisión humana previa ni responsable que firme el resultado. La mención de
avisar ante un reclamo en `system_prompt.md` no desarrolla estos requisitos.

## Señales de alerta

## Sugerencia concreta de mejora

Completar `DECISIONES.md` con al menos dos iteraciones verificables —incluyendo el texto de una
falla y el cambio aplicado— y agregar una tercera corrida fechada que muestre el formato final;
además, documentar en el mismo repositorio costos basados en tokens reales, niveles L0–L4 de
supervisión y el manejo concreto de reclamos y errores.
