# A4 — Comparación ANTES vs. DESPUÉS

## Único cambio

Se reemplazó en `agente/system_prompt.md` la regla que obligaba a tratar todo archivo no legible
como inexistente por tres estados explícitos: `AUSENTE`, `PRESENTE Y LEÍDO` y
`PRESENTE PERO INACCESIBLE`. No se modificaron la rúbrica, la Sonda ni los fixtures.

## Caso A — Archivo realmente ausente

| Aspecto | ANTES | DESPUÉS |
|---|---|---|
| Tratamiento | Ausente | Ausente |
| D4 | Ausente, 0/15 | Ausente, 0/15 |
| Total | 0/100 | 0/100 |
| Justificación | No existe contenido económico verificable | No hay archivo ni dato económico verificable |
| Alerta anti-trampa | No | No; aclara que solo faltan archivos |
| Reintento/revisión humana | No corresponde | No corresponde |

El control permanece estable: la nueva regla no convirtió una ausencia real en una falla técnica.

## Caso B — Archivo presente pero inaccesible

| Aspecto | ANTES | DESPUÉS |
|---|---|---|
| Tratamiento del archivo | Reconocido como existente, pero evaluado igual que ausente | `PRESENTE PERO INACCESIBLE` |
| D4 nivel | Ausente | Sin cerrar (`—`) |
| D4 puntaje | 0/15 | Sin cerrar (`—/15`) |
| Total | 0/100 | No definitivo (`—/100`) |
| Justificación | Sin contenido verificable, por lo tanto 0 | No puede evaluarse técnicamente; no corresponde asignar `Ausente` ni inferir contenido |
| Uso de `Ausente` para D4 | Sí | No |
| Inaccesibilidad identificada | Sí, solo en prosa | Sí, como estado explícito y causa de dimensión sin cerrar |
| Alerta anti-trampa | Sí: `⚠️ Posible caso de trabajo tramposo detectado` | No; dice expresamente que una falla técnica no es señal anti-trampa |
| Reintento/revisión humana | No | Sí: pide reintentar el acceso antes de cerrar la corrección |

## Evaluación del criterio de éxito

- Caso A sigue tratándose como realmente ausente: **cumplido**.
- Caso B se reconoce como presente pero inaccesible: **cumplido**.
- Caso B no se trata como inexistente: **cumplido**.
- D4 no recibe `Ausente` ni 0 únicamente por la falla: **cumplido**.
- No se genera alerta anti-trampa por inaccesibilidad: **cumplido**.
- Se solicita reintento antes de cerrar el resultado: **cumplido**.

El modelo no reprodujo literalmente la etiqueta `No evaluable técnicamente` en la celda de nivel:
usó `—` y explicó en la evidencia y justificación que el contenido no pudo evaluarse técnicamente.
Esto respeta la instrucción de no inventar un nivel ni puntaje y produce el estado evaluativo
diferente buscado.

## Resultado

La iteración A4 cumple el criterio de éxito funcional. El único cambio del system prompt preservó
el tratamiento de una ausencia real y evitó que una inaccesibilidad técnica se transformara en una
penalización académica o en una señal de posible trampa.
