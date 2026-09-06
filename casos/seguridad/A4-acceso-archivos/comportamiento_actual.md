# A4 — Comportamiento actual de acceso a archivos

## Alcance revisado

- `agente/system_prompt.md`
- `agente/sonda_v0.2.html`
- Rama: `mejoras-eber`
- Revisión de partida: `53c3b1f35d927bd9187ccb22e69c2833c7464190`

## Regla que recibe el evaluador

`agente/system_prompt.md` dice:

> Si no podés leer algún archivo referenciado, tratalo como si no existiera (no asumas su contenido).

La regla evita inventar el contenido, pero equipara expresamente la falta de evidencia con una
falla de lectura. No define un estado `INACCESIBLE`, una política de reintento ni una forma de
separar el puntaje académico de una limitación técnica.

## Cómo construye Sonda la entrada

`buildRepoDumpText()` recibe el árbol de blobs obtenido por la API de GitHub y arma dos fuentes de
información:

1. `Estructura completa del repo`, creada con todos los paths del árbol (`allPaths`).
2. `Contenido de los N archivos leídos`, que solo incluye los archivos que superaron los filtros y
   cuya descarga `raw` terminó con una respuesta HTTP exitosa.

Esto produce los siguientes estados observables:

| Situación | Representación actual | Consecuencia |
|---|---|---|
| Archivo realmente ausente | No aparece en `allPaths` ni como bloque de contenido. | El evaluador lo considera ausente. |
| Archivo presente y legible | Aparece en `allPaths` y tiene bloque `### path` con contenido. | El evaluador puede usarlo como evidencia. |
| Descarga `raw` con HTTP no exitoso, incluido 404 o 429 | Aparece en `allPaths`; `if (!res.ok) continue` omite silenciosamente su contenido. | Se conserva la presencia, pero se pierde código HTTP, causa y posibilidad de distinguir error permanente/transitorio. |
| Excepción de red, timeout que rechaza el `fetch` o error CORS | Aparece en `allPaths`; el `catch` vacío omite silenciosamente el archivo. | Se conserva la presencia, pero no se informa la causa. No hay timeout propio ni `AbortController`; un `fetch` que no resuelve puede dejar la operación esperando. |
| Extensión en `SKIP_EXT` | Aparece en `allPaths`, pero se excluye antes de descargar. | No se indica al evaluador que la exclusión fue por tipo no admitido. |
| Tamaño mayor a 60.000 bytes | Aparece en `allPaths`, se excluye y se lista bajo `Archivos NO leídos por ser demasiado grandes (tratalos como si no existieran)`. | La causa sí se informa, pero la propia Sonda ordena equipararla con ausencia. |
| Presupuesto total superior a 300.000 caracteres | Se detiene la lectura y se agrega un aviso general de truncamiento. | Se informa que hay contenido no incluido, pero no se enumeran explícitamente todos los archivos afectados por el corte. |
| 404/403/429/error de red al consultar metadatos o árbol por la API de GitHub | `githubJson()` lanza una excepción y no llega a construirse el prompt. | No hay evaluación ni puntaje; la interfaz muestra un error general. El texto de ayuda especializa 404 y 403, pero no 429 de GitHub. |
| 429/error/timeout de la API de Anthropic | La evaluación se interrumpe antes de obtener una corrección válida. | No se confunde con ausencia de un archivo porque no se genera resultado. |

`copyExtras()` repite el patrón de omisión silenciosa para las copias locales posteriores: una
descarga no exitosa o una excepción provoca `continue` sin dejar un registro por archivo.

## Señal disponible para el evaluador

Para un fallo individual de descarga, el evaluador puede inferir que el archivo **estaba presente**
si compara el árbol con los bloques de contenido. Sin embargo, Sonda no transmite un estado
estructurado ni la causa (`HTTP 404`, `HTTP 429`, CORS, red, timeout o tipo excluido). Además, el
system prompt le indica convertir cualquier imposibilidad de lectura en inexistencia a efectos de
la evaluación.

## Limitación adicional de versión

`loadPromptSources()` lee `systemPromptText` y `rubricaText` embebidos dentro del HTML, no los
archivos externos en tiempo de ejecución. En la revisión analizada, esos bloques no incorporan
todo el texto vigente de A2/A3. Para esta línea base se usaron explícitamente
`agente/system_prompt.md` y `rubrica.md` actuales, como pidió el procedimiento. La regla relevante
para A4 —tratar lo no legible como inexistente— sí está presente tanto en el archivo actual como en
el bloque embebido de Sonda.
