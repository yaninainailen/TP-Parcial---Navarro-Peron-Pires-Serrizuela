# A5 — Comparación ANTES vs. DESPUÉS

## Configuración efectiva

| Aspecto | ANTES | DESPUÉS |
|---|---|---|
| System prompt embebido = fuente | No | Sí |
| Rúbrica embebida = fuente | No | Sí |
| A2 presente | No | Sí |
| A3 presente | No | Sí |
| A4 presente | No | Sí |
| Regla antigua y contradictoria de A4 | Presente | Ausente |
| Caracteres del mensaje A4-B | 15.140 | 17.115 |
| SHA-256 del mensaje A4-B | `8e21a9381ee1b5f34f93eda8384b07172b5c29bf1205af60826f1d4d81144cba` | `9613ba56d4e922987ef8f62e4640fce26b16a867ad2f0b1be9091c0c8e6c5055` |

El hash DESPUÉS coincide con el hash de la entrada A4-B construida directamente con
`agente/system_prompt.md` y `rubrica.md`, lo que aporta una verificación adicional de que la
configuración efectiva quedó sincronizada.

## Prueba observable con el mismo fixture A4-B

| Aspecto | Sonda efectiva ANTES | Sonda efectiva DESPUÉS |
|---|---|---|
| Tratamiento de `ANALISIS_ECONOMICO.md` | Existe, pero se penaliza como falta de evidencia | Presente pero inaccesible; no se asume contenido ni ausencia |
| D4 | Ausente, 0/15 | No evaluable técnicamente; nivel y puntos `—` |
| Total | 0/100 | No definitivo, `—/100` |
| Alerta anti-trampa | Sí: `⚠️ Posible caso de trabajo tramposo detectado` | No; aclara que la inaccesibilidad técnica no es manipulación |
| Reintento/revisión humana | No antes de cerrar la nota | Sí, requerido para cerrar D4 y el total |

## Criterio de éxito

1. System prompt fuente = system prompt efectivo: **cumplido**.
2. Rúbrica fuente = rúbrica efectiva: **cumplido**.
3. A2, A3 y A4 presentes: **cumplido**.
4. El fixture deja de reproducir el comportamiento anterior: **cumplido**.
5. Comportamiento A4 esperado: **cumplido** — archivo presente pero inaccesible, D4 y total sin
   cerrar, sin alerta anti-trampa por esa causa y con solicitud de reintento o revisión humana.

## Limitación conservada

La llamada real de Sonda a Anthropic no se ejecutó por falta de credenciales y parámetros
disponibles. Igual que en la línea base, se construyó el mensaje exacto de Sonda y se ejecutó en el
mismo ChatGPT anónimo usado para A4/A5, evitando comparar proveedores distintos. La identidad del
modelo, temperatura y seed no fueron expuestas por la interfaz.
