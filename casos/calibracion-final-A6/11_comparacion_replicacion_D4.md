# A6 — Replicación de la aclaración D4

## Entrada congelada

R1 y R2 recibieron exactamente la misma cadena, construida una sola vez y reutilizada sin
cambios. SHA-256 del mensaje completo:
`a2e360d6bca37f4248e35fbc03ba27d7af60870d7cae894d564c9b0cf468e368`.

El system prompt, la rúbrica, el caso, el orden de archivos y la instrucción final no cambiaron
entre ambas ejecuciones.

## Comparación

| Campo | E0 | R1 | R2 |
|---|---:|---:|---:|
| D1 · Sistema completo y funcionando | Insuficiente — 10/30 | Insuficiente — 10/30 | Insuficiente — 10/30 |
| D2 · Proceso documentado | Insuficiente — 8/25 | Insuficiente — 8/25 | Insuficiente — 8/25 |
| D3 · Formato y reproducibilidad | Insuficiente — 5/15 | Insuficiente — 5/15 | Insuficiente — 5/15 |
| D4 · Análisis económico | Insuficiente — 5/15 | Insuficiente — 5/15 | Insuficiente — 5/15 |
| D5 · Gobierno y riesgo | Insuficiente — 5/15 | Insuficiente — 5/15 | Ausente — 0/15 |
| Suma real de las filas | 33/100 | 33/100 | 28/100 |
| Total informado | 31/100 | 33/100 | 28/100 |
| Total correcto | No | Sí | Sí |
| Alerta anti-trampa | No | No | No |

## Respuestas a las preguntas de replicación

1. **Sí.** D4 permanece en `Insuficiente — 5/15` en E0, R1 y R2.
2. **No.** D1–D4 son estables, pero D5 cambia en R2 de `Insuficiente — 5/15` a
   `Ausente — 0/15`.
3. **Sí.** R1 informa 33/100 y sus filas suman 33.
4. **Sí.** R2 informa 28/100 y sus filas suman 28.
5. **No.** El error 31 vs. 33 de E0 no se reproduce en R1 ni R2.
6. **Sí.** Existe una variación material en D5. Las diferencias de redacción y sugerencia no se
   consideran por sí mismas una inestabilidad relevante para esta prueba.

## D4

Las tres ejecuciones con la rúbrica aclarada citan la consideración económica “El modelo no es
caro” y la clasifican como `Insuficiente — 5/15` por carecer de números, tokens, proyección y
justificación económica del modelo. La aplicación de D4 es estable en esta muestra.

## Variabilidad observada

El error aritmético de E0 aparece como puntual dentro de esta muestra: las dos réplicas calculan
correctamente su propio total. No obstante, R2 deja D5 en `Ausente — 0/15`, mientras E0 y R1
valoran las menciones de aviso/revisión humana como evidencia genérica suficiente para
`Insuficiente — 5/15`. Esa diferencia de nivel y cinco puntos es material.

## Diagnóstico

**D) Aparecen variaciones materiales adicionales que impiden concluir estabilidad global.**

La evidencia permite afirmar que D4 se mantuvo corregido en tres ejecuciones y que el error
31/100 no se reprodujo. Sin embargo, no corresponde clasificar el conjunto como A porque D5 no
fue estable bajo entradas idénticas. No se introduce ninguna solución ni cambio adicional.
