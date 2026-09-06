# Línea base A5 — Prueba observable y diagnóstico

## Regla seleccionada

Se reutilizó A4 porque:

- el mismo fixture B ya estaba validado antes y después del cambio;
- A4 está ausente del system prompt embebido;
- Sonda conserva expresamente la regla anterior opuesta;
- A2 y A3 no inciden en el fixture mínimo, por lo que no agregan variables causales relevantes.

## Entradas comparadas

### Fuentes actuales

- `agente/system_prompt.md` actual, con A2 y A4.
- `rubrica.md` actual, con A3.
- Fixture B de A4 sin modificaciones.
- Resultado reutilizado:
  `casos/seguridad/A4-acceso-archivos/resultado_caso_B_inaccesible_despues.md`.
- SHA-256 del mensaje: `9613ba56d4e922987ef8f62e4640fce26b16a867ad2f0b1be9091c0c8e6c5055`.

### Configuración efectiva de Sonda

- `systemPromptText` embebido, sin A2/A4 y con la regla anterior de inaccesibilidad.
- `rubricaText` embebida, sin A3.
- El mismo fixture B sin modificaciones.
- Resultado nuevo conservado en `resultado_configuracion_efectiva_sonda.md`.
- SHA-256 del mensaje: `8e21a9381ee1b5f34f93eda8384b07172b5c29bf1205af60826f1d4d81144cba`.

## Resultado observable

| Aspecto | Fuentes actuales | Configuración efectiva de Sonda |
|---|---|---|
| Estado de `ANALISIS_ECONOMICO.md` | `PRESENTE PERO INACCESIBLE` | Reconoce que existe, pero aplica la regla de tratar lo no legible como inexistente |
| D4 nivel | Sin cerrar (`—`) | Ausente |
| D4 puntaje | Sin cerrar (`—/15`) | 0/15 |
| Total | No definitivo (`—/100`) | 0/100 |
| Suposición del contenido | No | No |
| Alerta anti-trampa por la inaccesibilidad | No | Sí: `⚠️ Posible caso de trabajo tramposo detectado` |
| Reintento antes de cerrar | Sí | No; recomienda que los archivos sean legibles, pero ya cierra nota y nivel |

El comportamiento vuelve exactamente al patrón problemático que motivó A4 cuando se usan los
textos embebidos: el archivo presente e inaccesible recibe `Ausente — 0/15`, se calcula total y se
genera una alerta de posible trampa.

## Diagnóstico

**A) Existe desincronización y produce una diferencia evaluativa observable. La mejora A5 está
justificada.**

No corresponde B porque Sonda no carga los `.md` actuales en ejecución. No corresponde C porque
los hashes y diffs prueban divergencias funcionales. No corresponde D porque fue posible extraer
los textos efectivos, reconstruir el prompt en el mismo orden que Sonda y observar una salida
diferente con el mismo fixture.

## Limitación controlada

La interfaz real de Sonda no se conectó a Anthropic por falta de una API key y parámetros de modelo
disponibles para esta línea base. La prueba no simula que esa llamada ocurrió: compara, bajo el mismo
evaluador anónimo de A4, el mensaje construido con las fuentes actuales contra el mensaje construido
con los textos exactos que Sonda carga. Así aísla la configuración sin atribuir diferencias a dos
proveedores o modelos distintos.
