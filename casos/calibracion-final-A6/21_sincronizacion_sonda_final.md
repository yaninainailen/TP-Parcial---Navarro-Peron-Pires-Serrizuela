# A6 — Sincronización final de Sonda

## Alcance

- Rama: `mejoras-eber`.
- HEAD de referencia: `67473c559f369752cea8371ae4c9c36462fcc4a1`.
- Único archivo existente editado en esta etapa: `agente/sonda_v0.2.html`.
- La edición se limitó a copiar dentro de `rubricaText` las filas `Insuficiente` y `Ausente` de
  D4 y D5 ya vigentes en `rubrica.md`.
- No se ejecutaron nuevamente los casos excelente, flojo ni tramposo.

## Diff realizado

```diff
diff --git a/agente/sonda_v0.2.html b/agente/sonda_v0.2.html
index 4ff77c5..b5e6f45 100644
--- a/agente/sonda_v0.2.html
+++ b/agente/sonda_v0.2.html
@@ -1082,8 +1082,8 @@ Evalúa si hay números reales de costo, no una mención de que "es barato".
 |---|---|---|
 | Excelente | **15** | Costo por corrida calculado con tokens de entrada/salida **de las corridas reales del repo** · proyección de costo a escala (por semana o por año) · elección de modelo justificada explícitamente con el criterio "el modelo más chico que hace bien la tarea". |
 | Bueno | **10** | Hay números de costo pero falta la proyección a escala, o la elección de modelo no está justificada (solo se nombra el modelo usado). |
-| Insuficiente | **5** | Se menciona el costo en términos genéricos ("es muy barato correrlo") sin ningún número propio. |
-| Ausente | **0** | No hay ningún análisis económico. |
+| Insuficiente | **5** | Existe alguna consideración económica o de costo, aunque sea genérica o cualitativa ("el modelo no es caro", "es barato correrlo" o equivalente), sin números propios. |
+| Ausente | **0** | No existe ninguna consideración económica, de costo, tokens, escala o elección económica del modelo. |

 **Señal de alerta:** si los números de costo no cierran con la longitud real de las corridas
 mostradas (ej. dice "3 centavos por corrida" pero las corridas tienen miles de palabras de salida),
@@ -1098,8 +1098,8 @@ Evalúa si el alumno pensó en qué puede salir mal y quién responde por el res
 |---|---|---|
 | Excelente | **15** | Dice qué sistemas/datos toca el agente y con qué permisos · qué puede salir mal **y** qué pasa concretamente cuando sale mal · qué revisa un humano antes de confiar en la salida · quién firma el resultado. |
 | Bueno | **10** | Cubre la mayoría de los puntos pero de forma superficial (ej. dice qué puede salir mal pero no qué se hace al respecto). |
-| Insuficiente | **5** | Hay una mención genérica de una frase sin desarrollo ("hay que tener cuidado con los datos"). |
-| Ausente | **0** | No hay ninguna sección de gobierno o riesgo. |
+| Insuficiente | **5** | Existe alguna consideración de gobierno, riesgo o supervisión humana, aunque sea una mención genérica de una frase sin desarrollo ("hay que tener cuidado con los datos", "hay que avisar a un humano antes de enviar" o equivalente). |
+| Ausente | **0** | No existe ninguna consideración de gobierno, riesgo, permisos, manejo de errores o supervisión humana. |

 ---
```

## Método de igualdad

Se aplicó el mismo procedimiento utilizado en A5:

1. extracción del contenido del bloque `<script type="text/plain" id="rubricaText">`;
2. semántica equivalente a `textContent.trim()`;
3. normalización de finales de línea a LF en la fuente y el bloque embebido;
4. comparación ordinal de las cadenas;
5. SHA-256 sobre UTF-8 sin BOM.

## Igualdad rúbrica/Sonda

| Texto | Caracteres | SHA-256 normalizado |
|---|---:|---|
| `rubrica.md` | 10.130 | `9dfdb6e989e7515d562863e133010375c0b0321ecb97f6fa86d96a87d1bd5682` |
| `rubricaText` efectivo | 10.130 | `9dfdb6e989e7515d562863e133010375c0b0321ecb97f6fa86d96a87d1bd5682` |

**Coincidencia exacta: Sí.**

## Verificación de system prompt

| Texto | Caracteres | SHA-256 normalizado |
|---|---:|---|
| `agente/system_prompt.md` | 7.054 | `f5b4d4166ad600db90c69414eadbd2c68405ac9b26fa29062cf3dd16e9228890` |
| `systemPromptText` efectivo | 7.054 | `f5b4d4166ad600db90c69414eadbd2c68405ac9b26fa29062cf3dd16e9228890` |
| `systemPromptText` en HEAD | 7.054 | `f5b4d4166ad600db90c69414eadbd2c68405ac9b26fa29062cf3dd16e9228890` |

`systemPromptText` coincide exactamente tanto con la fuente como con el bloque embebido de HEAD;
no fue modificado por esta sincronización.

## Confirmación funcional

No se modificaron `systemPromptText`, `loadPromptSources()`, la API, la interfaz, la construcción
del mensaje, el manejo de errores ni ningún otro código de Sonda. La única variación funcional es
que la copia embebida de la rúbrica ahora contiene las aclaraciones D4 y D5 ya validadas en A6.
