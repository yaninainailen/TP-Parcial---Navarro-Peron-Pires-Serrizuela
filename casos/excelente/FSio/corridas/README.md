# Corridas — cómo leer esta carpeta

Acá conviven **dos cosas distintas** que se hicieron en momentos distintos y con métodos distintos. Esta página existe para que no se confundan.

| | `corrida_0X_*.md` | `raw/` |
| --- | --- | --- |
| **Qué es** | El registro del **desarrollo**: cómo se construyó el contrato | La **evidencia de ejecución**: las corridas guardadas tal como salieron |
| **Cuándo** | 2026-08-31 | 2026-09-01 |
| **Cómo se ejecutó** | Manual (copiar y pegar entre un chat y el archivo) | `scripts/run_agent.py` → API de Anthropic |
| **Qué contiene** | Iteraciones, errores encontrados, **fragmentos citados** de la salida y el cambio aplicado al `system_prompt.md` por cada error | El texto completo de la respuesta, sin editar, con cabecera de modelo, fecha, tokens y costo |
| **Para qué sirve** | Ítem 4 de la consigna (la historia del proceso) | Ítem 2 de la consigna (tres corridas reales reconstruibles) e insumo de `ANALISIS_ECONOMICO.md` |

**Si venís a verificar que el sistema corre de verdad, mirá `raw/`.** Los `corrida_0X_*.md` citan fragmentos, no salidas completas: son un cuaderno de laboratorio, no evidencia de ejecución. Esa distinción es deliberada y está contada así porque las 3 corridas de desarrollo son anteriores a que existiera el runner — el orden real fue *construir el contrato a mano, después automatizar*, y el repositorio lo refleja en vez de disimularlo.

## Las 3 corridas de desarrollo

Se ejecutaron en orden y **cada una parte del `system_prompt.md` que dejó la anterior**, así que las mejoras se acumulan.

| # | Corrida | Caso | Iteraciones | Qué estresó |
| --- | --- | --- | --- | --- |
| 01 | [`corrida_01_ACME.md`](corrida_01_ACME.md) | Solicitud 001 | 3 | Supuesto redactado como hecho (esquema de protección) |
| 02 | [`corrida_02_SYNNEX.md`](corrida_02_SYNNEX.md) | Solicitud 002 | 3 | Pregunta genérica vs. específica; cotización vs. estimación |
| 03 | [`corrida_03_BMINING.md`](corrida_03_BMINING.md) | Solicitud 003 | 4 | Inconsistencia interna, plataforma del cliente, precios inventados, pedido sin sección propia |

El consolidado de todos los cambios y su motivo está en [`../DECISIONES.md`](../DECISIONES.md).

## La evidencia de ejecución (`raw/`)

**20 archivos, en tres generaciones del contrato.** El `system_prompt.md` cambió dos veces de forma sustancial (2026-09-05, dos revisiones) y en las dos ocasiones se volvió a correr todo en vez de dejar la evidencia describiendo un contrato que ya no existe. Ningún archivo se editó ni se borró — las tres generaciones conviven como registro de qué cambió y qué efecto tuvo, medido y no estimado.

| Generación | Casos | Corridas | sha256 | Estado |
| --- | --- | --- | --- | --- |
| 1 — 2026-09-01 | 001–003 | 6 | *(campo no existía)* | Histórica |
| 2 — 2026-09-05 (temprano) | 001–003 | 6 | `ad45bc6c055a` | Histórica, superada |
| 3 — 2026-09-05 (tarde) | 001–004 | 8 | `76d91e443f52` | **Vigente** |

### Generación 3 (vigente) — contrato con anti-fuga + caso de escalación

Incluye el caso 004 (ANDINA LITIO), agregado para probar la rama de escalación del contrato, que ninguno de los 3 casos originales ejercitaba. Las 8 corridas comparten sha256 y terminaron con `stop_reason: end_turn`.

| Caso | `claude-sonnet-5` (modelo elegido) | `claude-haiku-4-5` (comparación) |
| --- | --- | --- |
| 001 ACME | [salida](raw/solicitud_001_acme_claude-sonnet-5_20260905T221823.md) — 19959/4520 tok | [salida](raw/solicitud_001_acme_claude-haiku-4-5_20260905T221908.md) — 15050/3981 tok |
| 002 SYNNEX | [salida](raw/solicitud_002_synnex_claude-sonnet-5_20260905T222003.md) — 20037/6197 tok | [salida](raw/solicitud_002_synnex_claude-haiku-4-5_20260905T222038.md) — 15099/2760 tok |
| 003 BMINING | [salida](raw/solicitud_003_bmining_claude-sonnet-5_20260905T222206.md) — 20161/10079 tok | [salida](raw/solicitud_003_bmining_claude-haiku-4-5_20260905T222302.md) — 15191/4886 tok |
| 004 ANDINA (escalación) | [salida](raw/solicitud_004_andina_claude-sonnet-5_20260905T222427.md) — 20037/8658 tok | [salida](raw/solicitud_004_andina_claude-haiku-4-5_20260905T222502.md) — 15098/3056 tok |

En 004, Sonnet 5 declaró la escalación sin ninguna desviación de formato; Haiku 4.5 inventó dos encabezados que no existen en el contrato (`# ESCALACIÓN REQUERIDA`, `# Recomendaciones inmediatas`). Detalle en [`../DECISIONES.md`](../DECISIONES.md) y [`../ANALISIS_ECONOMICO.md`](../ANALISIS_ECONOMICO.md).

### Generaciones anteriores (históricas)

<details>
<summary>Generación 2 — 2026-09-05 temprano, contrato con Pieza 6 pero sin la restricción anti-fuga (sha256 <code>ad45bc6c055a</code>)</summary>

| Caso | `claude-sonnet-5` | `claude-haiku-4-5` |
| --- | --- | --- |
| 001 ACME | [salida](raw/solicitud_001_acme_claude-sonnet-5_20260905T215718.md) — 19714/4650 tok | [salida](raw/solicitud_001_acme_claude-haiku-4-5_20260905T220001.md) — 14877/4267 tok |
| 002 SYNNEX | [salida](raw/solicitud_002_synnex_claude-sonnet-5_20260905T215817.md) — 19792/6437 tok | [salida](raw/solicitud_002_synnex_claude-haiku-4-5_20260905T220052.md) — 14926/4264 tok |
| 003 BMINING | [salida](raw/solicitud_003_bmining_claude-sonnet-5_20260905T215912.md) — 19916/6265 tok | [salida](raw/solicitud_003_bmining_claude-haiku-4-5_20260905T220151.md) — 15018/5119 tok |

Acá se encontraron los dos hallazgos que motivaron la generación 3: la fuga de contenido de Haiku (BMINING y SYNNEX) y la confirmación de que el Ejemplo 1 corrigió la desviación de formato de Sonnet en ACME.

</details>

<details>
<summary>Generación 1 — 2026-09-01, contrato previo a la Pieza 6</summary>

| Caso | `claude-sonnet-5` | `claude-haiku-4-5` |
| --- | --- | --- |
| 001 ACME | [salida](raw/solicitud_001_acme_claude-sonnet-5_20260901T115654.md) — 4544/8133 tok | [salida](raw/solicitud_001_acme_claude-haiku-4-5_20260901T115745.md) — 3423/5173 tok |
| 002 SYNNEX | [salida](raw/solicitud_002_synnex_claude-sonnet-5_20260901T115956.md) — 4622/12611 tok | [salida](raw/solicitud_002_synnex_claude-haiku-4-5_20260901T120112.md) — 3472/8499 tok |
| 003 BMINING | [salida](raw/solicitud_003_bmining_claude-sonnet-5_20260901T120850.md) — 4746/11559 tok | [salida](raw/solicitud_003_bmining_claude-haiku-4-5_20260901T120926.md) — 3564/3757 tok |

Acá se detectaron los problemas estructurales originales de Haiku (formato inconsistente, abandono del flujo en BMINING), que la generación 3 confirma corregidos.

</details>

### Cómo reconstruir una corrida

Entrada, salida y fecha de cada una:

- **Entrada:** el archivo de `prompts/casos/` nombrado en el campo `Caso:` de la cabecera, enviado literalmente como mensaje de usuario.
- **Contrato:** `prompts/system_prompt.md` como system prompt — el sha256 de la cabecera permite verificar contra qué versión exacta corrió cada archivo de la generación 2 en adelante.
- **Salida y fecha:** el cuerpo del archivo y el campo `Fecha:` de la cabecera.

```bash
pip install -r scripts/requirements.txt
export ANTHROPIC_API_KEY=sk-ant-...
python scripts/run_agent.py prompts/casos/solicitud_001_acme.md --model claude-sonnet-5 --max-tokens 28000
```

**Dos salvedades honestas sobre reproducibilidad, ambas medidas y no supuestas:**

1. **La redacción varía.** Dos corridas con el mismo input y el mismo contrato siguen el mismo Formato pero varían en redacción — no hay garantía byte a byte. Observable comparando cualquier caso entre generación 2 y 3.
2. **El costo también varía, y no poco.** BMINING con Sonnet 5 costó 0.1537 en la generación 2 y 0.2117 en la generación 3 (+38%) con una diferencia de contrato de una sola línea — la causa fue que esa corrida particular exploró con más profundidad una hipótesis técnica del caso, no un cambio de contrato. Detalle en `../ANALISIS_ECONOMICO.md`.

Todas las corridas desde la generación 2 se pidieron con `--max-tokens 28000` de forma uniforme; ninguna llegó a la mitad de ese techo (la más larga, BMINING/Sonnet en generación 3, usó 10079). En la generación 1, BMINING sí necesitó ese valor para no truncarse con el `max_tokens` por defecto de esa época; el resto de esa generación corrió con el default de 16000.
