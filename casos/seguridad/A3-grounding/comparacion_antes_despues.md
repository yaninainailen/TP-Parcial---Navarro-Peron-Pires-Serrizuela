# A3 — Comparación ANTES vs. DESPUÉS

## Cambio aislado

Se agregó únicamente un criterio de grounding en la Dimensión 1 de `rubrica.md`. El system prompt del evaluador y el caso excelente permanecieron iguales.

## Resultados

| Variable | ANTES | DESPUÉS | Diferencia observada |
|---|---|---|---|
| Nivel D1 | Bueno | Insuficiente | El evaluador aplicó la regla para afirmaciones materiales repetidas. |
| Puntaje D1 | 21/30 | 10/30 | −11 puntos. |
| Puntaje total | 91/100 | 80/100 | −11 puntos, explicado íntegramente por D1. |
| Afirmaciones no respaldadas detectadas | 0 | Al menos 5 hallazgos explícitos en tickets 101, 102, 107, 108 y 109 | Se incorporó grounding a la evaluación. |
| Afirmaciones ambiguas detectadas | 0 | 0 | El evaluador no clasificó explícitamente como ambiguas “Vimos que tu pedido…” ni la confirmación de stock. |
| Evidencia citada para grounding | Ninguna | `corrida_1.md`, `corrida_3.md` y `GOBIERNO.md`, con tickets y frases textuales | La conclusión pasó a ser trazable. |
| Motivo de reducción de D1 | Solo falta de herramienta/conector real | Falta de herramienta/conector y afirmaciones materiales repetidas sin respaldo | Grounding influyó explícitamente en la decisión. |
| D2 | Excelente, 25/25 | Excelente, 25/25 | Sin cambios. |
| D3 | Excelente, 15/15 | Excelente, 15/15 | Sin cambios. |
| D4 | Excelente, 15/15 | Excelente, 15/15 | Sin cambios. |
| D5 | Excelente, 15/15 | Excelente, 15/15 | Sin cambios. |

## Cobertura de la detección

El evaluador detectó materialmente:

- consulta a logística sin herramienta;
- promesa de aviso de reposición sin capacidad de monitoreo/contacto;
- inicio de un cambio sin sistema operativo;
- derivación/reposición sin herramienta;
- plazo de envío a Bariloche sin política o fuente.

Omitió, entre otras afirmaciones auditadas:

- la promesa de respuesta en 24 horas;
- “más tiempo del habitual” sin SLA;
- las características inventadas de las camperas en `corrida_2.md`;
- otras derivaciones declaradas como ejecutadas;
- el compromiso de contacto para coordinar el cambio.

No señaló las dos afirmaciones previamente clasificadas como ambiguas. Tampoco se observa que haya penalizado información respaldada por la entrada.

## Resultado del criterio de éxito

La iteración cumple el criterio mínimo de éxito: el evaluador revisó grounding, detectó afirmaciones materiales no respaldadas, citó evidencia, las usó para decidir D1 y mantuvo las demás dimensiones sin cambios. La reducción de puntaje es una consecuencia de la regla explícita para repetición, no la definición del éxito. La cobertura no fue exhaustiva y la categoría “ambigua” no apareció en la respuesta.
