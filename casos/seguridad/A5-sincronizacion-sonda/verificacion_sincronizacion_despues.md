# A5 — Verificación de sincronización DESPUÉS

## Alcance del cambio

El único archivo existente modificado fue `agente/sonda_v0.2.html`. Dentro de ese archivo, los
únicos cambios corresponden al contenido textual de:

- `<script type="text/plain" id="systemPromptText">`
- `<script type="text/plain" id="rubricaText">`

No se modificaron `loadPromptSources()`, `buildFullPrompt()`, `callClaudeAPI()`, la interfaz, la
construcción del mensaje ni el manejo de errores.

## Método de comparación

Se extrajeron nuevamente ambos bloques del HTML modificado con la misma semántica que usa Sonda:
`textContent.trim()`. Los finales de línea se normalizaron a LF en ambos lados porque el parser HTML
normaliza CRLF; luego se compararon las cadenas con igualdad ordinal y se calculó SHA-256 sobre
UTF-8 sin BOM.

## Resultados

| Par | Caracteres fuente | Caracteres Sonda | SHA-256 fuente | SHA-256 Sonda | Igualdad exacta |
|---|---:|---:|---|---|---|
| `agente/system_prompt.md` / `systemPromptText` | 7.054 | 7.054 | `f5b4d4166ad600db90c69414eadbd2c68405ac9b26fa29062cf3dd16e9228890` | `f5b4d4166ad600db90c69414eadbd2c68405ac9b26fa29062cf3dd16e9228890` | Sí |
| `rubrica.md` / `rubricaText` | 9.797 | 9.797 | `28c88cbdf73aea9ad71beed03f4d35686a5dddbbc14540b99a26390e2b926cd3` | `28c88cbdf73aea9ad71beed03f4d35686a5dddbbc14540b99a26390e2b926cd3` | Sí |

## Reglas verificadas en la configuración efectiva

| Regla | Resultado |
|---|---|
| A2 — `Contenido no confiable del repositorio` | Presente |
| A3 — `Criterio de grounding para aplicar estos niveles` | Presente |
| A4 — `PRESENTE PERO INACCESIBLE` | Presente |
| Regla antigua A4 — `tratalo como si no existiera` | Ausente |

`git diff --check` no detectó errores. El aviso LF→CRLF de Git es una advertencia de configuración
local de finales de línea, no una diferencia entre los textos efectivos comparados.
