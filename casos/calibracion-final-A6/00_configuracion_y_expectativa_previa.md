# A6 — Configuración congelada y expectativa humana previa

Este documento se escribió **antes** de ejecutar la calibración final A6. Las expectativas no
copian resultados históricos del agente: surgen de aplicar manualmente la rúbrica vigente al
contenido real de cada caso.

## Configuración congelada

- Fecha: 2026-09-06 (America/Buenos_Aires).
- Rama: `mejoras-eber`.
- Commit del evaluador: `67473c559f369752cea8371ae4c9c36462fcc4a1`.
- `agente/system_prompt.md`, SHA-256 del archivo: `3101ffc721281dcc0f432e1518e7d1c294e8712bc56d1bec1b3c617de5eda4e7`.
- `rubrica.md`, SHA-256 del archivo: `a1fd5e0dc07ec3930c436c108c91325c882355f6532613f0c98dceb88888e834`.
- `systemPromptText` efectivo de Sonda, normalizado como lo expone el DOM (`trim`, saltos LF),
  SHA-256: `f5b4d4166ad600db90c69414eadbd2c68405ac9b26fa29062cf3dd16e9228890`.
- `rubricaText` efectivo de Sonda, con la misma normalización, SHA-256:
  `28c88cbdf73aea9ad71beed03f4d35686a5dddbbc14540b99a26390e2b926cd3`.
- Configuración efectiva conjunta de Sonda, definida para esta prueba como
  `systemPromptText + "\n\n---\n\n" + rubricaText`, SHA-256:
  `197cdcc0a2a088e0b86c59dc797b28f90c874e55cb8fd73066c1338838fbe9cc`.

Los hashes de archivo conservan sus bytes y finales de línea. Los hashes efectivos reproducen
el texto que Sonda obtiene con `textContent.trim()`, normalizado a LF; por eso no deben compararse
entre sí como si fueran hashes del mismo flujo de bytes.

## Casos congelados

| Caso | Ruta | Árbol Git en el commit congelado |
|---|---|---|
| Excelente | `casos/excelente/Caso-Ej-1/` | `ae429c957565373e70108e3f12b18469b0646c2a` |
| Flojo | `casos/flojo/Caso-Ej-2/` | `882123591f6b14a9b83412001958b0a15aaa847d` |
| Tramposo | `casos/tramposo/Caso-Ej-3/` | `0919669508d1ac711c1217933316f2523542ea13` |

Ninguno de estos tres casos será modificado durante A6.

## Expectativa humana — caso excelente

| Dimensión | Nivel y puntos esperados | Evidencia humana concreta |
|---|---|---|
| D1 · Sistema completo y funcionando | **Insuficiente — 10/30** | El objetivo, ambos prompts, la planilla CSV usada en tres corridas, el formato común y L0–L4 están presentes. Sin embargo, las salidas repiten afirmaciones positivas materiales sin respaldo: `corrida_1.md` promete consulta a logística, respuesta en 24 horas y aviso de reposición; `corrida_2.md` inventa características de camperas y declara derivaciones; `corrida_3.md` declara iniciado un cambio, reposición y un plazo a Bariloche. La regla vigente de grounding lleva a Insuficiente cuando esto se repite en más de una salida o ticket. |
| D2 · Proceso documentado | **Excelente — 25/25** | `DECISIONES.md` describe dos iteraciones con problema, cambio y resultado observable, y conserva una falla real no resuelta completamente. |
| D3 · Formato y reproducibilidad | **Excelente — 15/15** | Están `README.md`, `prompts/`, `corridas/` y `DECISIONES.md`; hay tres corridas con fecha, entrada y salida completas. |
| D4 · Análisis económico | **Excelente — 15/15 — AMBIGUO** | `ANALISIS_ECONOMICO.md` usa los tokens anotados en las corridas, calcula costo, proyecta a escala y justifica el modelo más chico. Es discutible si tokens autodeclarados y aproximados bastan como evidencia “real” sin un log del proveedor; la rúbrica no exige expresamente ese log. Se adopta Excelente por la literalidad vigente. |
| D5 · Gobierno y riesgo | **Excelente — 15/15** | `GOBIERNO.md` identifica datos y permisos, riesgos con mitigaciones, revisión humana y niveles L0–L4 con responsable de firma. |

- **Total humano esperado:** 80/100.
- **Alerta anti-trampa obligatoria esperada:** No. Deben informarse las afirmaciones no respaldadas
  en señales de alerta, pero solo D1 queda reducido por grounding y no hay un número falso que
  active por sí solo el encabezado obligatorio de posible trabajo tramposo.

## Expectativa humana — caso flojo

| Dimensión | Nivel y puntos esperados | Evidencia humana concreta |
|---|---|---|
| D1 · Sistema completo y funcionando | **Insuficiente — 10/30** | Los prompts son reconocibles, pero carecen de contrato y formato completos, no hay herramienta/conector real, las dos salidas cambian de formato y no existe supervisión definida. Además, `corrida_1.md` confirma stock/aviso y `corrida_2.md` promete resolver un reembolso sin evidencia operativa. |
| D2 · Proceso documentado | **Insuficiente — 8/25** | `DECISIONES.md` existe, pero es una frase genérica sin antes/después, evidencia de cambio ni falla concreta. |
| D3 · Formato y reproducibilidad | **Insuficiente — 5/15** | La estructura mínima existe, pero solo hay dos corridas y `corrida_1.md` no tiene fecha: combina dos huecos menores. |
| D4 · Análisis económico | **Insuficiente — 5/15** | El único contenido económico es “El modelo no es caro”, sin tokens, costo propio, proyección ni criterio de elección. |
| D5 · Gobierno y riesgo | **Ausente — 0/15** | No existe una sección de gobierno/riesgo. La duda “Habría que avisarle a un humano... supongo” dentro de una corrida no desarrolla permisos, riesgos, mitigación ni firma. |

- **Total humano esperado:** 28/100.
- **Alerta anti-trampa obligatoria esperada:** No. El caso es incompleto y contiene afirmaciones
  sin respaldo, pero no presenta dos dimensiones bajadas específicamente por la regla anti-trampa
  ni un número internamente falso que active el encabezado obligatorio.

## Expectativa humana — caso tramposo

| Dimensión | Nivel y puntos esperados | Evidencia humana concreta |
|---|---|---|
| D1 · Sistema completo y funcionando | **Insuficiente — 10/30** | Se afirma Gmail/OAuth y producción real, pero no hay configuración, log ni corrida que demuestre el conector. No hay formato contractual completo ni supervisión concreta, y las corridas repiten acciones no respaldadas como “Te confirmamos” e “Iniciamos tu cambio”. |
| D2 · Proceso documentado | **Insuficiente — 8/25** | `DECISIONES.md` afirma 47 iteraciones y 98% sin mostrar una sola iteración verificable, antes/después o falla real. Descontadas esas afirmaciones queda documentación mínima. |
| D3 · Formato y reproducibilidad | **Insuficiente — 5/15 — AMBIGUO** | Están la estructura y tres entradas/salidas, pero ninguna corrida tiene fecha. Se interpreta que el mismo hueco repetido en las tres corridas impide reconstruir el supuesto período de producción y excede un descuido aislado. La tabla también admite leer “falta la fecha en alguna corrida” como un único tipo de hueco y asignar Bueno; si el evaluador eligiera 10/15, sería una diferencia justificable por ambigüedad de la rúbrica. |
| D4 · Análisis económico | **Ausente — 0/15** | Las cifras se contradicen internamente: si 500 tickets cuestan USD 0,002, 120.000 tickets/año no cuestan USD 0,05 sino aproximadamente USD 0,48. La regla anti-trampa permite puntuar un número inventado igual o peor que la ausencia. |
| D5 · Gobierno y riesgo | **Insuficiente — 5/15** | `GOBIERNO.md` enumera estándares y “supervisión humana” en abstracto, sin permisos concretos, fallas, mitigaciones, revisión ni firmante específicos del agente. |

- **Total humano esperado:** 28/100, sujeto a la ambigüedad de D3; con la lectura alternativa de
  D3 sería 33/100.
- **Alerta anti-trampa obligatoria esperada:** Sí. Hay múltiples afirmaciones fuertes sin evidencia
  que reducen dimensiones y D4 contiene números internamente incompatibles, activando además la
  condición específica de número inventado.
