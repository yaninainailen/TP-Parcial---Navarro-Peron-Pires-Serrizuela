# A6 — Comparación de la aclaración específica de D5

## Único cambio probado

Se reemplazaron exclusivamente las filas `Insuficiente` y `Ausente` de D5 en `rubrica.md` por el
texto solicitado. La aclaración provisional de D4 se mantuvo exactamente igual. No se modificaron
el system prompt, Sonda, el caso, la composición del mensaje ni ninguna otra regla.

## Comparación con R1 y R2

| Campo | R1 | R2 | Después de D5 |
|---|---:|---:|---:|
| D1 · Sistema completo y funcionando | Insuficiente — 10/30 | Insuficiente — 10/30 | Insuficiente — 10/30 |
| D2 · Proceso documentado | Insuficiente — 8/25 | Insuficiente — 8/25 | Insuficiente — 8/25 |
| D3 · Formato y reproducibilidad | Insuficiente — 5/15 | Insuficiente — 5/15 | Insuficiente — 5/15 |
| D4 · Análisis económico | Insuficiente — 5/15 | Insuficiente — 5/15 | Insuficiente — 5/15 |
| D5 · Gobierno y riesgo | Insuficiente — 5/15 | Ausente — 0/15 | Insuficiente — 5/15 |
| Suma real de las filas | 33/100 | 28/100 | 33/100 |
| Total informado | 33/100 | 28/100 | 33/100 |
| Alerta anti-trampa | No | No | Sí |

## D5

La aclaración resolvió la frontera en esta ejecución. El evaluador asignó
`Insuficiente — 5/15`, citó “Si es un reclamo, avisame” y la mención de `corrida_2.md` a avisar a
un humano antes de enviar. Explicó que existe supervisión identificable, aunque faltan permisos,
riesgos concretos, manejo de errores, revisión definida y responsable de firma.

## Otras dimensiones y total

D1, D2, D3 y D4 conservaron los mismos niveles y puntajes de R1. D4 permaneció en
`Insuficiente — 5/15`. El total informado, `33/100`, coincide con la suma de las cinco filas.

## Variación material

La ejecución activó por primera vez la alerta obligatoria
`⚠️ Posible caso de trabajo tramposo detectado`. R1 y R2 no la habían activado bajo la misma
evidencia. La alerta se justificó con afirmaciones y faltas de evidencia de D1 y D2, no con el
cambio de frontera de D5. Como la única modificación textual estaba en D5, esta nueva alerta no
puede atribuirse causalmente a un cambio sustantivo en D1 o D2 y constituye una variación material
no explicada por el objetivo del experimento.

## Diagnóstico

**B) D5 se corrige, pero aparece otra variación material.**

La frontera D5 funcionó como se esperaba y D1–D4 permanecieron estables, pero apareció una alerta
anti-trampa nueva. Conforme al protocolo, no se realiza una segunda ejecución ni se introduce
otro cambio.
