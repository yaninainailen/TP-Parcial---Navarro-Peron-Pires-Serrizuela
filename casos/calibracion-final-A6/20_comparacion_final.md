# A6 — Comparación final

## Configuración congelada

- Rama: `mejoras-eber`.
- HEAD: `67473c559f369752cea8371ae4c9c36462fcc4a1`.
- `agente/system_prompt.md`, SHA-256: `be2128b80e08ad04729d61617069ce4a3bb40c9d6f0430a7555c051b73261ce9`.
- `rubrica.md`, SHA-256: `5fc7e94fc69cfc4f5a1d7a2b3f81d938898329c5194f5b84e38f865b37db1716`.
- Los tres mensajes fueron construidos una sola vez antes de ejecutar y no se modificó ningún
  archivo entre las tres ejecuciones.
- Se realizó exactamente una ejecución por caso. La interfaz no expuso modelo, temperatura, seed
  ni otros parámetros; no se infieren ni se afirma determinismo.

| Caso | SHA-256 del mensaje completo |
|---|---|
| Excelente | `a3068e37867aabf8d0d6da3e0b79b95d1e1a084b9e7845d415be4ebf4dcdcdfd` |
| Flojo | `b7c4b0d981cc1c6b51e0ad0f3e3d8b2bdf6a0def78e8f491b6b5fdeecd14797b` |
| Tramposo | `a1e2ab7e3bcf0931b66bb4dfc1eed6054c97153a23d4bdff196f9c4310356ca0` |

## Resumen de resultados

| Caso | D1 | D2 | D3 | D4 | D5 | Suma real | Total informado | Alerta anti-trampa |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| Excelente | Insuficiente 10 | Excelente 25 | Excelente 15 | Excelente 15 | Excelente 15 | 80 | 80/100 | No |
| Flojo | Insuficiente 10 | Insuficiente 8 | Insuficiente 5 | Insuficiente 5 | Insuficiente 5 | 33 | 33/100 | No |
| Tramposo | Insuficiente 10 | Bueno 17 | Bueno 10 | Insuficiente 5 | Insuficiente 5 | 47 | 47/100 | Sí |

## Caso excelente

| Dimensión | Expectativa humana previa | Resultado | Clasificación | Trazabilidad |
|---|---|---|---|---|
| D1 | Insuficiente — 10/30 | Insuficiente — 10/30 | **COINCIDE** | Revisa explícitamente grounding y cita acciones/plazos sin respaldo en `corrida_1.md` y `corrida_3.md`. La repetición limita D1 a Insuficiente. |
| D2 | Excelente — 25/25 | Excelente — 25/25 | **COINCIDE** | Cita las dos iteraciones concretas y la falla real del ticket 104. |
| D3 | Excelente — 15/15 | Excelente — 15/15 | **COINCIDE** | Verifica estructura completa y tres corridas con fecha, entrada y salida. |
| D4 | Excelente — 15/15, AMBIGUO | Excelente — 15/15 | **COINCIDE** | Acepta los tokens registrados, cálculo, proyección y criterio de elección del modelo. |
| D5 | Excelente — 15/15 | Excelente — 15/15 | **COINCIDE** | Cita permisos, riesgos, mitigaciones, revisión y responsable L4. |

El grounding de D1 se aplica de forma expresa y con evidencia. No se activa el encabezado de caso
tramposo porque esa reducción afecta una sola dimensión y no existe un número internamente falso.

## Caso flojo

| Dimensión | Expectativa humana previa | Resultado | Clasificación | Trazabilidad |
|---|---|---|---|---|
| D1 | Insuficiente — 10/30 | Insuficiente — 10/30 | **COINCIDE** | Cita ausencia de herramienta, formatos inconsistentes y falta de L0–L4. |
| D2 | Insuficiente — 8/25 | Insuficiente — 8/25 | **COINCIDE** | Cita `DECISIONES.md` y la falta de iteraciones, cambios y falla real documentados. |
| D3 | Insuficiente — 5/15 | Insuficiente — 5/15 | **COINCIDE** | Identifica los dos huecos simultáneos: solo dos corridas y una sin fecha. |
| D4 | Insuficiente — 5/15 | Insuficiente — 5/15 | **COINCIDE** | Cita “El modelo no es caro” y lo trata como consideración económica genérica sin números. La aclaración D4 funciona. |
| D5 | Insuficiente — 5/15 | Insuficiente — 5/15 | **COINCIDE** | Cita “Habría que avisarle a un humano antes de mandar nada” y lo trata como supervisión humana genérica. La aclaración D5 funciona. |

No aparece el encabezado anti-trampa. La sección `Señales de alerta` queda vacía, coherente con la
auditoría 15: las frases genéricas y las carencias ordinarias no demuestran por sí mismas una
reducción causada por la regla anti-trampa.

## Caso tramposo

| Dimensión | Expectativa humana previa | Resultado | Clasificación | Trazabilidad |
|---|---|---|---|---|
| D1 | Insuficiente — 10/30 | Insuficiente — 10/30 | **COINCIDE** | Contrasta la integración Gmail/OAuth declarada con corridas manuales sin herramienta y la falta de L0–L4. |
| D2 | Insuficiente — 8/25 | Bueno — 17/25 | **DIFERENCIA JUSTIFICABLE** | Lee “Iteré el prompt 47 veces” como iteraciones narradas pero genéricas, coincidente con el ejemplo textual de Bueno. La tensión con la regla que impide contar afirmaciones fuertes sin respaldo ya fue documentada en la calibración inicial de A6; no es un desacuerdo nuevo. |
| D3 | Insuficiente — 5/15, con alternativa Bueno — 10/15 marcada AMBIGUA | Bueno — 10/15 | **DIFERENCIA JUSTIFICABLE** | Trata que las tres corridas carezcan de fecha como un único tipo de hueco menor. Es exactamente la lectura alternativa registrada antes de ejecutar. |
| D4 | Ausente — 0/15 | Insuficiente — 5/15 | **DIFERENCIA JUSTIFICABLE** | Detecta y demuestra la inconsistencia USD 0,48 frente a USD 0,05. El texto específico de D4 ordena tratar números que no cierran como Insuficiente, mientras la regla anti-trampa Nº4 admite puntuar igual o peor que la ausencia; esta tensión ya estaba documentada. |
| D5 | Insuficiente — 5/15 | Insuficiente — 5/15 | **COINCIDE** | Reconoce las declaraciones genéricas de gobierno sin permisos, fallas, revisión ni firmante específicos. |

La alerta anti-trampa se activa correctamente y cita producción, 500 tickets, precisión del 98%,
Gmail/OAuth, tasa de error, 47 iteraciones y cifras económicas. La inconsistencia aritmética de D4
es demostrable con los datos internos y satisface por sí sola la condición obligatoria del
encabezado.

## Alertas

| Caso | Esperada | Observada | Evaluación |
|---|---|---|---|
| Excelente | No | No | **COINCIDE**. Informa grounding en señales sin etiquetarlo como caso tramposo. |
| Flojo | No | No | **COINCIDE**. No repite la activación no respaldada observada en la ejecución 13. |
| Tramposo | Sí | Sí | **COINCIDE**. Expone discrepancias puntuales y el número internamente falso. |

## Coherencia aritmética

- Excelente: `10 + 25 + 15 + 15 + 15 = 80`; total informado `80/100`.
- Flojo: `10 + 8 + 5 + 5 + 5 = 33`; total informado `33/100`.
- Tramposo: `10 + 17 + 10 + 5 + 5 = 47`; total informado `47/100`.

Las tres sumas son correctas. No se reproduce el error aislado `31/100` de la ejecución E0.

## Diagnóstico final

**A) CONFIGURACIÓN FINAL ACEPTABLE.**

Las aclaraciones D4 y D5 se aplican correctamente en el caso flojo, sin cambios materiales en
D1–D3 y sin una alerta anti-trampa infundada. El caso excelente conserva la evaluación de
grounding esperada y el caso tramposo activa la alerta con evidencia verificable. No aparece una
contradicción normativa nueva que justifique otra modificación: las diferencias del caso
tramposo reproducen límites ya identificados y documentados antes de esta validación. Las tres
salidas son aritméticamente coherentes.
