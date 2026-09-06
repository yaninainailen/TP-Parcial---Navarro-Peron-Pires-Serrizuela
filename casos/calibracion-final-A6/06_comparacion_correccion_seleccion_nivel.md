# A6 — Comparación de la corrección de selección de nivel

## Único cambio probado

Se agregó al paso de aplicación de la rúbrica en `agente/system_prompt.md` la indicación de usar
`Ausente` solo cuando ninguna evidencia satisface un descriptor superior y de asignar
`Insuficiente` cuando evidencia débil o genérica coincide con ese descriptor.

No se modificaron la rúbrica, Sonda, el caso flojo ni ningún otro componente de evaluación.

## Antes vs. después

| Dimensión | Antes | Después | Resultado |
|---|---:|---:|---|
| D1 · Sistema completo y funcionando | Insuficiente — 10/30 | Insuficiente — 10/30 | Sin cambio |
| D2 · Proceso documentado | Insuficiente — 8/25 | Insuficiente — 8/25 | Sin cambio |
| D3 · Formato y reproducibilidad | Insuficiente — 5/15 | Insuficiente — 5/15 | Sin cambio |
| D4 · Análisis económico | Ausente — 0/15 | Ausente — 0/15 | No se corrigió |
| D5 · Gobierno y riesgo | Insuficiente — 5/15 | Ausente — 0/15 | Variación material no buscada |
| **Total** | **28/100** | **23/100** | **−5 puntos** |

## D4

El evaluador volvió a citar la evidencia exacta del `README.md`: “El modelo no es caro así que
se puede usar tranquilamente para esto”. También reconoció que no contiene números propios, pero
volvió a asignar `Ausente — 0/15` en lugar de `Insuficiente — 5/15`, pese a que el descriptor de
la rúbrica contempla una mención genérica equivalente.

## D5

Sin que cambiara el caso ni existiera una regla nueva sobre gobierno, D5 pasó de
`Insuficiente — 5/15` a `Ausente — 0/15`. La nueva salida sí citó “Si es un reclamo, avisame”,
pero no consideró esa evidencia débil suficiente para `Insuficiente`. Esta variación no puede
atribuirse razonablemente al objetivo específico del cambio y muestra variabilidad residual de
la ejecución.

## Alerta anti-trampa

- Antes: no activada.
- Después: no activada.

No apareció una alerta anti-trampa nueva. La mención de huecos de evidencia incluida en la salida
no usa el encabezado obligatorio de alerta ni afirma que se haya activado esa regla.

## Diagnóstico

**C) No corrige D4.** El cambio mínimo no consiguió que el evaluador aplicara el descriptor
`Insuficiente` a la evidencia económica genérica. D1, D2 y D3 permanecieron estables y no hubo
una alerta nueva, pero D5 varió además de forma no buscada. Conforme al protocolo, la prueba se
detiene aquí: no se introduce un segundo cambio ni se actualizan `README.md`, `calibracion.md` o
Sonda.
