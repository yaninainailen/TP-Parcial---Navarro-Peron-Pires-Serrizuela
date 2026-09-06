# A6 — Configuración congelada y expectativa final previa

Este documento se creó antes de ejecutar la validación final. Conserva la expectativa humana de
`00_configuracion_y_expectativa_previa.md` y solo incorpora las dos aclaraciones empíricamente
justificadas para el caso flojo: D4 y D5 en `Insuficiente — 5/15`.

## Configuración congelada

- Fecha: 2026-09-06 (America/Buenos_Aires).
- Rama: `mejoras-eber`.
- HEAD: `67473c559f369752cea8371ae4c9c36462fcc4a1`.
- `agente/system_prompt.md`, SHA-256 de los bytes actuales:
  `be2128b80e08ad04729d61617069ce4a3bb40c9d6f0430a7555c051b73261ce9`.
- `rubrica.md`, SHA-256 de los bytes actuales:
  `5fc7e94fc69cfc4f5a1d7a2b3f81d938898329c5194f5b84e38f865b37db1716`.
- El contenido Git de `agente/system_prompt.md` coincide con HEAD.
- El único diff de `rubrica.md` respecto de HEAD son las aclaraciones de las filas
  `Insuficiente`/`Ausente` de D4 y D5.
- Los tres casos originales no presentan diferencias ni archivos nuevos respecto de HEAD.

Los hashes de archivo anteriores preservan los bytes y finales de línea actuales. No se infieren
modelo, temperatura, seed u otros parámetros que la interfaz no expone.

## Casos congelados

| Caso | Ruta | Árbol Git en HEAD | Archivos enviados |
|---|---|---|---:|
| Excelente | `casos/excelente/Caso-Ej-1/` | `ae429c957565373e70108e3f12b18469b0646c2a` | 12 |
| Flojo | `casos/flojo/Caso-Ej-2/` | `882123591f6b14a9b83412001958b0a15aaa847d` | 6 |
| Tramposo | `casos/tramposo/Caso-Ej-3/` | `0919669508d1ac711c1217933316f2523542ea13` | 9 |

## Mensajes congelados antes de ejecutar

Cada mensaje se construyó una sola vez con la estructura completa, el contenido íntegro de los
archivos listados, el mismo system prompt, la misma rúbrica vigente y la misma instrucción final.
Las cadenas quedan conservadas en memoria sin reconstruirse entre ejecuciones.

| Caso | Caracteres | SHA-256 del mensaje completo |
|---|---:|---|
| Excelente | 36.912 | `a3068e37867aabf8d0d6da3e0b79b95d1e1a084b9e7845d415be4ebf4dcdcdfd` |
| Flojo | 19.818 | `b7c4b0d981cc1c6b51e0ad0f3e3d8b2bdf6a0def78e8f491b6b5fdeecd14797b` |
| Tramposo | 22.429 | `a1e2ab7e3bcf0931b66bb4dfc1eed6054c97153a23d4bdff196f9c4310356ca0` |

## Expectativa humana final — caso excelente

| Dimensión | Nivel y puntos esperados | Evidencia humana concreta |
|---|---|---|
| D1 · Sistema completo y funcionando | **Insuficiente — 10/30** | El objetivo, prompts, CSV usados, formato y L0–L4 están presentes, pero las corridas repiten afirmaciones positivas materiales no respaldadas: consulta a logística y respuesta en 24 horas; características de camperas y derivaciones; cambio iniciado, reposición y plazo a Bariloche. El grounding repetidamente fallido corresponde a Insuficiente. |
| D2 · Proceso documentado | **Excelente — 25/25** | `DECISIONES.md` narra dos iteraciones concretas con problema, cambio y resultado, y conserva una falla real. |
| D3 · Formato y reproducibilidad | **Excelente — 15/15** | Están todos los componentes obligatorios y hay tres corridas fechadas con entradas y salidas completas. |
| D4 · Análisis económico | **Excelente — 15/15 — AMBIGUO** | Usa tokens anotados en las corridas, calcula costo, proyecta a escala y justifica el modelo más chico. Es discutible si los tokens autodeclarados bastan sin log del proveedor, requisito que la rúbrica no exige expresamente. |
| D5 · Gobierno y riesgo | **Excelente — 15/15** | `GOBIERNO.md` identifica datos, permisos, riesgos, mitigaciones, revisión humana, niveles L0–L4 y responsable de firma. |

- **Total esperado:** 80/100.
- **Alerta anti-trampa obligatoria:** No. El grounding reduce solo D1 y no hay un número falso.

## Expectativa humana final — caso flojo

| Dimensión | Nivel y puntos esperados | Evidencia humana concreta |
|---|---|---|
| D1 · Sistema completo y funcionando | **Insuficiente — 10/30** | Prompts incompletos, sin herramienta real, formatos distintos y afirmaciones operativas no respaldadas. |
| D2 · Proceso documentado | **Insuficiente — 8/25** | `DECISIONES.md` es una frase genérica sin iteraciones, antes/después ni falla concreta. |
| D3 · Formato y reproducibilidad | **Insuficiente — 5/15** | Solo hay dos corridas y `corrida_1.md` no tiene fecha: dos huecos menores combinados. |
| D4 · Análisis económico | **Insuficiente — 5/15** | “El modelo no es caro” es una consideración económica genérica sin números, contemplada por la aclaración D4. |
| D5 · Gobierno y riesgo | **Insuficiente — 5/15** | “Habría que avisarle a un humano antes de mandar nada” es una consideración genérica de supervisión humana sin desarrollo, contemplada por la aclaración D5. |

- **Total esperado:** 33/100.
- **Alerta anti-trampa obligatoria:** No. Las carencias determinan niveles bajos por los
  descriptores ordinarios; no hay dos dimensiones reducidas específicamente por anti-trampa ni un
  número falso.

## Expectativa humana final — caso tramposo

| Dimensión | Nivel y puntos esperados | Evidencia humana concreta |
|---|---|---|
| D1 · Sistema completo y funcionando | **Insuficiente — 10/30** | Afirma Gmail/OAuth y producción sin evidencia; carece de contrato y supervisión concretos y repite acciones no respaldadas. |
| D2 · Proceso documentado | **Insuficiente — 8/25** | Afirma 47 iteraciones y 98% sin mostrar antes/después, falla ni iteración verificable. |
| D3 · Formato y reproducibilidad | **Insuficiente — 5/15 — AMBIGUO** | Ninguna de las tres corridas tiene fecha. La lectura alternativa `Bueno — 10/15` también es defendible si se considera un único tipo de hueco. |
| D4 · Análisis económico | **Ausente — 0/15** | Las cifras son internamente incompatibles: USD 0,002 por 500 tickets proyecta aproximadamente USD 0,48, no USD 0,05, para 120.000 tickets/año. |
| D5 · Gobierno y riesgo | **Insuficiente — 5/15** | Enumera estándares y supervisión en abstracto, sin permisos, riesgos, mitigaciones, revisión ni firmante específicos. |

- **Total esperado:** 28/100; con la lectura alternativa de D3, 33/100.
- **Alerta anti-trampa obligatoria:** Sí. Hay afirmaciones fuertes no verificables y D4 contiene
  un número internamente falso que activa por sí solo el encabezado obligatorio.
