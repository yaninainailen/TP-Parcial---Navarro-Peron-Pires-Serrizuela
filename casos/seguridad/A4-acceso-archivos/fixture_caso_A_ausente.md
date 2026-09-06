# A4 — Fixture A: archivo realmente ausente

## Objetivo

Representar la salida mínima de `buildRepoDumpText()` cuando el archivo que aportaría la evidencia
de la Dimensión 4 no existe en el árbol del repositorio.

## Fragmento de entrada agregado después del system prompt y la rúbrica

```text
Repositorio a evaluar: A4-caso-A-ausente (rama simulada)

Estructura completa del repo (0 archivos detectados):

Contenido de los 0 archivos leídos:
```

## Condición controlada

`ANALISIS_ECONOMICO.md` no aparece ni en el árbol ni en el contenido. No se suministra ninguna otra
evidencia, para aislar la clasificación de D4 sin atribuirle contenido ficticio al archivo.
