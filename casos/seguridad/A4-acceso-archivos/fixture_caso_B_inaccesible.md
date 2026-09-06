# A4 — Fixture B: archivo presente pero inaccesible

## Objetivo

Reproducir la salida mínima de `buildRepoDumpText()` cuando la API de árbol informa que
`ANALISIS_ECONOMICO.md` existe, pero su descarga `raw` devuelve una respuesta HTTP no exitosa.

## Mecanismo simulado

Después de obtener correctamente un árbol con un blob `ANALISIS_ECONOMICO.md`, se simula un
`fetch(rawUrl)` con respuesta HTTP 404. El código actual ejecuta `if (!res.ok) continue`: conserva
el path dentro de `allPaths`, no crea bloque de contenido y no transmite el 404. El mismo texto se
obtendría ante un 429 individual; ante CORS o rechazo de red, el `catch` vacío produce la misma
omisión.

## Fragmento de entrada agregado después del system prompt y la rúbrica

```text
Repositorio a evaluar: A4-caso-B-presente-inaccesible (rama simulada)

Estructura completa del repo (1 archivos detectados):
- ANALISIS_ECONOMICO.md

Contenido de los 0 archivos leídos:
```

## Única diferencia respecto de A

El archivo aparece en el árbol. No se suministra contenido ni se agrega al mensaje una etiqueta de
error que Sonda actualmente no produciría.
