# A6 — Comparación y diagnóstico de la recalibración final

## Resumen

| Caso | Total humano previo | Total del evaluador | Alerta humana | Alerta del evaluador |
|---|---:|---:|---|---|
| Excelente | 80/100 | 80/100 | No | No |
| Flojo | 28/100 | 28/100 | No | No |
| Tramposo | 28/100; 33/100 bajo la lectura alternativa declarada para D3 | 47/100 | Sí | Sí |

La coincidencia del total flojo es accidental: dos diferencias de cinco puntos se compensan entre
D4 y D5. Por eso no se utiliza el total como sustituto de la comparación dimensión por dimensión.

## Caso excelente

| Dimensión | Expectativa humana previa | Resultado | Clasificación | Comparación de evidencia |
|---|---|---|---|---|
| D1 | Insuficiente, 10/30 | Insuficiente, 10/30 | **COINCIDE** | Detectó grounding repetido y citó logística, cambio y plazo a Bariloche. Agregó que los CSV no son herramienta real, lectura discutible frente al ejemplo de nivel alto de la rúbrica, pero sin efecto adicional sobre el nivel ya limitado por grounding. |
| D2 | Excelente, 25/25 | Excelente, 25/25 | **COINCIDE** | Citó las dos iteraciones y la falla real de `DECISIONES.md`. |
| D3 | Excelente, 15/15 | Excelente, 15/15 | **COINCIDE** | Citó estructura completa, tres fechas, entradas y salidas. |
| D4 | Excelente, 15/15, marcado AMBIGUO por tokens autodeclarados | Excelente, 15/15 | **COINCIDE** | Aceptó los tokens anotados en las corridas, la aritmética, la proyección y la elección del modelo. Adoptó la misma lectura literal que la expectativa humana. |
| D5 | Excelente, 15/15 | Excelente, 15/15 | **COINCIDE** | Citó permisos, riesgos, mitigaciones, revisión muestral y firma L4. |

El evaluador informó las afirmaciones materiales no respaldadas sin activar el encabezado de caso
tramposo. Coincide con la expectativa: el grounding reduce D1, pero no satisface por sí solo las
condiciones específicas de la alerta obligatoria.

## Caso flojo

| Dimensión | Expectativa humana previa | Resultado | Clasificación | Comparación de evidencia |
|---|---|---|---|---|
| D1 | Insuficiente, 10/30 | Insuficiente, 10/30 | **COINCIDE** | Citó contrato incompleto, ausencia de herramienta, cambio de formato y falta de L0–L4. |
| D2 | Insuficiente, 8/25 | Insuficiente, 8/25 | **COINCIDE** | Reconoció que `DECISIONES.md` no tiene iteraciones ni antes/después verificable. |
| D3 | Insuficiente, 5/15 | Insuficiente, 5/15 | **COINCIDE** | Identificó los dos huecos combinados: solo dos corridas y una sin fecha. |
| D4 | Insuficiente, 5/15 | Ausente, 0/15 | **DESACUERDO REAL** | El propio evaluador citó “El modelo no es caro”. Esa frase coincide casi literalmente con el descriptor vigente de Insuficiente: “Se menciona el costo en términos genéricos ('es muy barato correrlo') sin ningún número propio”. No hay ambigüedad suficiente para justificar Ausente. |
| D5 | Ausente, 0/15 | Insuficiente, 5/15 | **DIFERENCIA JUSTIFICABLE** | La expectativa exigió una sección de gobierno; el evaluador contó como mención genérica “Si es un reclamo, avisame” y “Habría que avisarle a un humano”. La rúbrica permite leer una mención genérica como Insuficiente, aunque no exista archivo dedicado, por lo que el límite no está completamente cerrado. |

El total coincide en 28/100 solo porque D4 pierde cinco puntos y D5 gana cinco. La alerta coincide:
no se activó el encabezado anti-trampa.

## Caso tramposo

| Dimensión | Expectativa humana previa | Resultado | Clasificación | Comparación de evidencia |
|---|---|---|---|---|
| D1 | Insuficiente, 10/30 | Insuficiente, 10/30 | **COINCIDE** | Contrastó Gmail/OAuth, producción y acciones externas contra tres corridas manuales sin conector. |
| D2 | Insuficiente, 8/25 | Bueno, 17/25 | **DIFERENCIA JUSTIFICABLE** | La expectativa descartó “47 iteraciones” por falta de evidencia y consideró que quedaba documentación sin sustancia. El evaluador leyó esa misma frase como iteración narrada pero genérica. El descriptor de Bueno usa como ejemplo “mejoré el prompt varias veces”, mientras la regla anti-trampa dice que una afirmación fuerte sin respaldo no cuenta; ambas reglas empujan en sentidos distintos. |
| D3 | Insuficiente, 5/15, marcado AMBIGUO | Bueno, 10/15 | **DIFERENCIA JUSTIFICABLE** | El evaluador trató la ausencia de fecha en las tres corridas como un único tipo de hueco. Esta era exactamente la lectura alternativa registrada antes de ejecutar. |
| D4 | Ausente, 0/15 | Insuficiente, 5/15 | **DIFERENCIA JUSTIFICABLE** | La rúbrica ordena tratar cifras que no cierran como Insuficiente, pero la regla anti-trampa permite que un número inventado puntúe igual o peor que la ausencia. El evaluador eligió 5 y demostró correctamente la inconsistencia USD 0,48 vs. USD 0,05. |
| D5 | Insuficiente, 5/15 | Insuficiente, 5/15 | **COINCIDE** | Identificó que los estándares y controles son declaraciones genéricas sin permisos, fallas, revisión ni firma específicas. |

El total 47/100 difiere del 28/100 esperado y de la alternativa previa 33/100, pero surge de tres
límites que la propia redacción vigente admite razonablemente. La detección central sí coincide:
el informe abrió con `⚠️ Posible caso de trabajo tramposo detectado` y enumeró producción, volumen,
precisión, Gmail/OAuth y cifras económicas sin respaldo o internamente incompatibles.

## Desacuerdos

### Desacuerdo real material

El único desacuerdo inequívoco es D4 del caso flojo. El evaluador reconoció la evidencia que define
el nivel Insuficiente, pero asignó Ausente. Que D5 compense casualmente el total no corrige el error
por dimensión ni demuestra calibración.

### Diferencias justificables

- D5 del flojo: no está definido con precisión si una mención genérica fuera de una sección de
  gobierno evita Ausente.
- D2 del tramposo: tensión entre “iteración genérica” para Bueno y “afirmación sin evidencia no
  cuenta” de la regla anti-trampa.
- D3 del tramposo: no queda cerrado si el mismo defecto repetido en tres corridas es uno o varios
  huecos.
- D4 del tramposo: el descriptor específico indica Insuficiente, mientras la regla anti-trampa
  habilita igualar o empeorar la ausencia.
- D1 del excelente: el nivel coincide por grounding, aunque la afirmación de que el CSV no es una
  herramienta contradice el ejemplo de nivel alto; no tuvo efecto numérico en esta ejecución.

## Limitaciones de la ejecución

- Las tres evaluaciones se hicieron en conversaciones anónimas nuevas con el mismo system prompt,
  rúbrica y composición textual de repositorio.
- La interfaz no expuso identidad exacta del modelo, temperatura, seed ni otros parámetros. No se
  afirma determinismo ni se infieren esos valores.
- Se realizó una ejecución final por caso, tal como pidió la fase 3. Esta calibración demuestra
  alineación o desacuerdo en esas ejecuciones, no repetibilidad estadística.

## Diagnóstico

**B) Existe un único desacuerdo material que exige otra corrección antes de documentar.**

El desacuerdo es acotado y observable: D4 del caso flojo debería ser Insuficiente 5/15 según el
descriptor literal, pero fue evaluado como Ausente 0/15. Las diferencias restantes son límites de
interpretación ya identificados o razonablemente discutibles. Conforme al procedimiento A6, no se
modifican `README.md` ni `calibracion.md` en este diagnóstico B.
