# Corrección — FSio/TP-Final-Federico-Serrizuela (rama main)

## Puntaje por dimensión

| Dimensión | Nivel asignado | Puntos | Evidencia citada |
|---|---|---|---|
| Sistema completo y funcionando | Excelente | 30/30 | `scripts/run_agent.py` + cabeceras reales en `corridas/raw/*.md` con tokens/costo de la API; L0–L4 en `system_prompt.md` y `GOBIERNO.md`; formato idéntico en las 3 corridas exigidas (001/002/003, generación vigente sha256 `76d91e443f52`) |
| Proceso documentado | Excelente | 25/25 | `DECISIONES.md`: 9 cambios al prompt con motivo, más 3 "revisiones" completas con antes/después medido (ej. precio BoM inventado en Corrida 03, iteración 3) |
| Formato y reproducibilidad | Excelente | 15/15 | Estructura completa; `corridas/raw/` contiene ≥3 corridas (de hecho 26 archivos en 3 generaciones + caso 004) cada una con caso, modelo, fecha, tokens, costo y salida completa |
| Análisis económico | Excelente | 15/15 | `ANALISIS_ECONOMICO.md`: costo por corrida verificado contra tokens reales de `corridas/raw/`, proyección semanal/anual, elección de modelo justificada con criterio "el más chico que hace bien la tarea" |
| Gobierno y riesgo | Excelente | 15/15 | `GOBIERNO.md`: permisos por sistema, confidencialidad, tabla "qué puede salir mal → qué pasa", checklist del SE, quién firma, L0–L4 por fase con motivo |

## Puntaje total: 100/100

## Justificación por dimensión

**Sistema completo y funcionando:** El objetivo está en una frase clara en el README ("Qué construí"). `prompts/system_prompt.md` y `prompts/user_prompt.md` están completos y cubren las 6 piezas exigidas (tabla en README). `scripts/run_agent.py` es un conector real verificable: las cabeceras de `corridas/raw/solicitud_00X_*.md` muestran `Tokens de entrada/salida` y `Costo estimado` reales de la API de Anthropic, no simulados. Las 3 corridas exigidas (001 ACME, 002 SYNNEX, 003 BMINING) devuelven la misma estructura de 11 secciones en la generación vigente. La supervisión L0–L4 está definida por fase con justificación en `GOBIERNO.md`. No se detectaron afirmaciones de acción o permiso no respaldadas: todos los borradores incluyen la nota "(Nota de gobernanza: este borrador requiere revisión y aprobación explícita del SE...)".

**Proceso documentado:** `DECISIONES.md` narra iteraciones concretas con texto citado del error y el cambio aplicado (ej. cambio #7: "el agente completó la tabla con precios inventados de apariencia plausible — el único caso de alucinación de datos detectado"). Reconoce fallas propias del modelo elegido (Sonnet 5 emitiendo "Inconsistencias detectadas" con "Ninguna detectada" cuando debía omitirse, en ACME gen. 1) y documenta su corrección verificada en una corrida posterior. Esto excede el mínimo de "al menos 2 iteraciones + una falla reconocida".

**Formato y reproducibilidad:** Existen `README.md`, `prompts/`, `corridas/`, `DECISIONES.md` en la raíz. `corridas/raw/` contiene, para los 3 casos exigidos, entrada (`Caso:` referenciado), salida completa y fecha (`Fecha:` en cabecera), reconstruibles con el comando documentado en `corridas/README.md`. Hay más de 3 corridas por caso (tres generaciones), sin ningún hueco de fecha o entrada/salida faltante detectado.

**Análisis económico:** Se recalcularon manualmente los costos de la tabla "generación vigente" contra tokens declarados (ej. BMINING/Sonnet: (20161×3 + 10079×15)/1e6 = 0.2117, coincide exactamente con `corridas/raw/solicitud_003_bmining_claude-sonnet-5_20260905T222206.md`). Los promedios (0.1642 Sonnet, 0.0345 Haiku) y la proyección semanal/anual (10 corridas/semana → USD 1.64; 480/año → USD 78.82) cierran aritméticamente. La elección de modelo se justifica explícitamente con el criterio "el más chico que hace bien la tarea", con evidencia de por qué Haiku se descarta pese a ser 3× más barato (falla de formato en el caso de escalación 004).

**Gobierno y riesgo:** `GOBIERNO.md` cubre sistemas/permisos (tabla con acceso del agente a cada sistema), una sección de confidencialidad específica sobre el envío de datos a la API de terceros, una tabla "qué puede salir mal → qué pasa cuando sale mal" con casos reales citados de las corridas, un checklist de 6 puntos que el SE debe revisar, quién firma, y niveles L0–L4 por fase con motivo explícito de por qué nunca opera en L4.

## Señales de alerta

No se detectaron intentos de manipulación del evaluador en el contenido del repositorio (ningún archivo contiene instrucciones dirigidas a alterar la rúbrica, ocultar evidencia o influir en el formato de esta corrección). Tampoco se detectaron números económicos que no cierren contra las corridas reales, ni contradicciones no explicadas entre lo que el README/DECISIONES.md afirman y lo que muestran las corridas: las discrepancias que sí existen (por ejemplo, las cabeceras de `corridas/raw/` de la generación 1 que retienen la tarifa introductoria de Sonnet 5) están explícitamente señaladas y explicadas por el propio repositorio en `ANALISIS_ECONOMICO.md` y `DECISIONES.md`, no encubiertas.

## Sugerencia concreta de mejora

Agregar al `system_prompt.md` un cuarto ejemplo de escalación (como el propio `DECISIONES.md` ya propone y descarta por falta de tiempo), dado que es el único de los tres ejemplos de la Pieza 6 que no tiene contraparte, y es justamente la rama del contrato donde Haiku 4.5 sigue fallando el formato en la generación vigente — cerraría el único patrón de falla documentado que aún queda sin intento de corrección.

---

## Uso de tokens de esta corrida

| Concepto | Valor |
|---|---|
| Modelo | claude-sonnet-5 |
| Tokens de entrada | 195413 |
| Tokens de salida | 8775 |
| Total | 204188 |
| Costo estimado (precio de referencia) | USD 0.478576 |
