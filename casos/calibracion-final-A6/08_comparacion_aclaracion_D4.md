# A6 — Comparación de la aclaración específica de D4

## Único cambio probado

Se aclararon exclusivamente los descriptores `Insuficiente` y `Ausente` de D4 en `rubrica.md`.
No se modificaron los puntajes, otros niveles de D4, otras dimensiones, la regla anti-trampa, el
system prompt, Sonda ni el caso flojo.

## Antes vs. después

El “antes” es la línea base A6 guardada en `02_resultado_flojo.md`, ya que el cambio fallido del
system prompt fue retirado antes de esta prueba.

| Dimensión | Antes | Después | Resultado |
|---|---:|---:|---|
| D1 · Sistema completo y funcionando | Insuficiente — 10/30 | Insuficiente — 10/30 | Sin cambio |
| D2 · Proceso documentado | Insuficiente — 8/25 | Insuficiente — 8/25 | Sin cambio |
| D3 · Formato y reproducibilidad | Insuficiente — 5/15 | Insuficiente — 5/15 | Sin cambio |
| D4 · Análisis económico | Ausente — 0/15 | Insuficiente — 5/15 | Corregido |
| D5 · Gobierno y riesgo | Insuficiente — 5/15 | Insuficiente — 5/15 | Sin cambio |
| **Total informado** | **28/100** | **31/100** | El total posterior es aritméticamente incorrecto |
| **Suma verificable de las filas** | **28/100** | **33/100** | Incremento esperado de 5 puntos |

## D4

La nueva evaluación asignó `Insuficiente — 5/15`, citó la frase del `README.md` “El modelo no es
caro así que se puede usar tranquilamente para esto” y explicó correctamente que no aporta
números verificables, tokens, costo por corrida, proyección ni justificación económica del modelo.
Esto coincide con el descriptor aclarado.

## Otras dimensiones

D1, D2, D3 y D5 conservaron exactamente sus niveles y puntajes de la línea base. Tampoco apareció
una alerta anti-trampa nueva: la salida declaró explícitamente que no se cumplía ninguna condición
de activación.

Sin embargo, el total informado fue `31/100`. La suma simple obligatoria de las filas es
`10 + 8 + 5 + 5 + 5 = 33`. Este error afecta el resultado final aunque la clasificación por
dimensión sea coherente.

## Diagnóstico

**B) Corrección parcial.** D4 se corrigió con evidencia y las otras dimensiones permanecieron
estables, pero apareció una regresión material en el total informado. Conforme al protocolo, no
se introduce un segundo cambio y no se actualizan Sonda, `README.md` ni `calibracion.md`.
