# A5 — Trazabilidad de la ejecución de Sonda

## System prompt

`loadPromptSources()` ejecuta:

```js
var systemPrompt = document.getElementById('systemPromptText').textContent.trim();
```

El texto proviene del bloque `systemPromptText` incluido dentro de
`agente/sonda_v0.2.html`. El navegador no lee `agente/system_prompt.md` en tiempo de ejecución.
Modificar el `.md` por sí solo no cambia el comportamiento de Sonda.

## Rúbrica

La misma función ejecuta:

```js
var rubrica = document.getElementById('rubricaText').textContent.trim();
```

La rúbrica proviene del bloque `rubricaText` del HTML. No existe un `fetch`, importación ni lectura
de `rubrica.md` durante la evaluación. Modificar `rubrica.md` por sí solo tampoco actualiza Sonda.

## Repositorio evaluado

1. `fetchRepoTree(owner, repo)` consulta la API de GitHub para obtener la rama por defecto.
2. Consulta el árbol recursivo de esa rama y conserva sus blobs.
3. `buildRepoDumpText()` filtra directorios, extensiones y tamaños.
4. Descarga los archivos elegibles desde `raw.githubusercontent.com` y concatena árbol y bloques
   de contenido.
5. Los fallos individuales de descarga se omiten mediante `continue` o un `catch` vacío.

## Prompt efectivo enviado al modelo

`buildFullPrompt()` concatena en este orden:

1. `INSTRUCCIONES (system prompt del corrector):` + system prompt embebido.
2. `RÚBRICA A APLICAR:` + rúbrica embebida.
3. Identificación del repositorio y rama por defecto.
4. Árbol y contenido descargado del repositorio evaluado.

`callClaudeAPI()` envía toda esa concatenación como un único mensaje con rol `user`:

```js
messages: [{ role: 'user', content: prompt }]
```

No usa un canal `system` separado. Los `.md` externos son archivos de referencia/versionado, pero
no son dependencias de ejecución de la Sonda actual.

## Consecuencia de trazabilidad

La fuente que determina el comportamiento operativo está duplicada dentro del HTML. Un commit que
solo cambie `agente/system_prompt.md` o `rubrica.md` puede quedar correctamente versionado y probado
por fuera de Sonda, pero no alterar las evaluaciones ejecutadas desde ella.
