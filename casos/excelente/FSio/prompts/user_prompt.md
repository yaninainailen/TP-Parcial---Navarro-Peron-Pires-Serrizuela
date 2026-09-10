# User prompt — contrato de entrada del agente

Este archivo es la mitad de usuario del contrato: define **qué le entrega el Sales Engineer (SE) al agente en cada corrida y en qué forma**. La mitad de sistema (rol, fases, restricciones, formato de salida) está en [`system_prompt.md`](system_prompt.md).

## Cómo se usa realmente

El `system_prompt.md` ya contiene todas las instrucciones del flujo, así que **el mensaje de usuario es únicamente la información cruda del cliente** — sin instrucciones, sin repetir el pedido, sin formato impuesto. Concretamente:

- **Vía `scripts/run_agent.py` (camino real):** el contenido del archivo de `prompts/casos/*.md` se envía **literalmente** como mensaje de usuario. No se le antepone ni se le agrega nada. Los archivos de `prompts/casos/` son, por lo tanto, instancias de esta plantilla.
- **Vía chat (camino manual):** se pega el `system_prompt.md` como system prompt y la información del cliente como primer mensaje.

Esta separación es deliberada: si el mensaje de usuario repitiera las instrucciones, cualquier cambio del flujo habría que hacerlo en dos lugares, y las corridas dejarían de ser comparables entre sí.

## Qué debe contener el mensaje de usuario

Información **tal como la recibió el SE**, sin interpretar ni completar:

| Campo | Obligatorio | Nota |
| --- | --- | --- |
| Identificación del cliente y fecha de la interacción | Sí | El agente la usa en el Resumen de la oportunidad |
| Si es cliente nuevo o existente | Recomendado | Cambia el análisis de interconexión con red existente |
| Objetivo / necesidad declarada | Sí | Sin esto no hay Fase 1 posible |
| Capacidad, distancias, sitios, geografía | Según disponga | Los huecos son insumo de la Fase 2, no un problema |
| Requisitos de disponibilidad o protección | Según disponga | Transcribir la frase del cliente, no traducirla a un mecanismo |
| Interfaces, plataformas o equipamiento que el cliente especifique | Según disponga | Si el cliente nombra una plataforma, dejarla textual |
| Entregable pedido y plazo | Sí | Determina si la Fase 4 evalúa cotización formal o estimación preliminar |

**Reglas al redactarlo:**

1. **Transcribir, no interpretar.** Si el cliente dijo "debe sobrevivir a un corte de fibra", va esa frase — no "requiere protección 1+1". Elegir el mecanismo por el cliente es exactamente el error que la restricción de esquema de protección del `system_prompt.md` existe para evitar.
2. **No completar huecos.** Lo que el cliente no dijo se deja afuera; detectarlo es trabajo de la Fase 2.
3. **No corregir contradicciones del cliente.** Si el texto original se contradice, va contradicho: declararlas es trabajo del chequeo de inconsistencias.
4. **Transcribir diagramas a texto.** El agente no procesa imágenes ni adjuntos (ver limitaciones en [`../DECISIONES.md`](../DECISIONES.md)).
5. **Datos reales de cliente:** ver [`../GOBIERNO.md`](../GOBIERNO.md) antes de pegar información identificable — el mensaje sale del entorno hacia una API de terceros.

## Plantilla

```
[Fecha y contexto de la interacción: reunión / correo / llamada con <CLIENTE>,
 indicando si es cliente nuevo o existente.]

A continuación, la información tal como fue recibida:
- [Un punto por dato entregado por el cliente, textual.]
- [...]
- [Entregable solicitado y plazo.]
```

---

# Casos de prueba usados en este trabajo

Las 3 solicitudes de abajo son los casos contra los que se corrió el agente. Están replicadas una por archivo en [`casos/`](casos/), que es lo que consume `run_agent.py`.

## De dónde salen estos casos

**No son solicitudes reales copiadas, ni son invención libre: son casos construidos a partir de patrones reales de pedido.** Los clientes (ACME, SYNNEX, BMINING) no existen y ninguna de las tres corresponde a una solicitud concreta recibida. Lo que sí es real es todo lo demás: el dominio es el trabajo cotidiano del autor como Optical Network Sales Engineer, y **las ambigüedades plantadas en cada caso son las que efectivamente aparecen** — el cliente que dice "no puede caerse" sin especificar mecanismo, el que pide un número rápido y no una cotización, el que se contradice a sí mismo entre dos líneas del mismo correo.

Hay una razón de gobierno detrás de la decisión, no solo de comodidad: **cada corrida envía el contenido de estos archivos a la API de un tercero.** Usar solicitudes reales habría significado sacar información de clientes del perímetro de la empresa para hacer un trabajo práctico. Es exactamente el riesgo que documenta [`../GOBIERNO.md`](../GOBIERNO.md), y la regla que ese archivo fija para casos bajo NDA —anonimizar antes de correr— se aplicó acá de la forma más conservadora posible: construir los casos desde cero en lugar de anonimizar.

**El límite honesto de este enfoque:** casos construidos por la misma persona que después evalúa al agente corren el riesgo de ser benévolos, porque el autor sabe qué cosas el agente maneja bien. La evidencia sugiere que no fue así: **8 de los 9 cambios al `system_prompt.md` salieron de fallas que estos casos provocaron**, incluida la única alucinación de datos de todo el ejercicio (precios de BoM inventados, Corrida 03). Rompieron al agente repetidamente antes de que empezara a funcionar. Aun así, un cuarto caso escrito por otra persona sería mejor prueba, y no lo hay.

> **Nota sobre la Solicitud 003:** contiene una inconsistencia interna deliberada (nombra los canales como "IT#1 y OT#1" en un punto y "IT#1 y OT#2" en otro). **No corregirla**: es el único caso de prueba de detección de inconsistencias del repositorio, y motivó el cambio #5 de `DECISIONES.md`.

## Solicitud 001
A continuación se presentan las notas de la reunión con ACME Corp, un cliente potencial, recopiladas el 10 de agosto de 2026, en las que ACME compartió la información relativa a esta solicitud inicial:
- El cliente desea conectar dos centros de datos, separados por unos 120 km.
- Necesita una capacidad total de 400G y desea ampliarla a 800G en 2 años.
- Mencionó que «debe sobrevivir a un corte en una sola fibra», pero no especificó el SLA.
- Hay enrutadores Cisco existentes en ambos extremos; no está claro qué interfaces ópticas se utilizan.
- Desea recibir una propuesta en 3 semanas.

## Solicitud 002
Lo siguiente corresponde a una reunión con SYNNEX Corp, celebrada el 11 de agosto de 2026, en la que el cliente realizó algunas adiciones y cambios a la solicitud inicial:
- El cliente desea interconectar sus 2 principales oficinas para replicar sus Bases de Datos mediante fibra óptica entre ellas, con una distancia aproximada de 58 km a través de fibra oscura.
- La interconexión debe construirse de manera que admita 2 rutas independientes de fibra óptica.
- Se mencionó que la solución «debe seguir funcionando con una sola ruta en caso de corte de fibra», pero no se especificó el SLA.
- Hay routers Cisco existentes en ambos extremos.
- No está claro si las interfaces ópticas de entrada son de 1G, 10G o incluso 100G.
- Desea recibir una primera propuesta presupuestaria en una semana.

## Solicitud 003
Hoy, 15 de agosto de 2026, la empresa BMINING, que ya es cliente nuestro, presentó una nueva solicitud para un nuevo proyecto.
Están pensando en instalar una nueva red en San Juan, Argentina, pero esta nueva red presenta algunas restricciones:
A continuación se presenta la información relevante, así como los datos sobre las restricciones:
- La nueva red de BMINING es un pequeño anillo con 3 sitios DWDM
- Los sitios aún no existen, pero se planea que estén separados entre sí por menos de 50 km
- Inicialmente necesitan contar con 2 canales ópticos denominados IT#1 y OT#1
- Ambos canales ópticos, IT#1 y OT#2, deben basarse inicialmente en 400 Gbps
- Cada sitio contará con equipo de alta disponibilidad; es decir, deben instalarse 2 plataformas C-4615 en cada sitio
- Las interfaces tributarias no están claras. Proporcione alternativas
- La solicitud incluye una lista de materiales (BoM) presupuestaria con precios
- Plazo de entrega de 10 días
- Proporcione además alternativas para la interconexión de esta nueva red en anillo con otras redes actuales de BMINING

## Solicitud 004 (caso complementario — dispara escalación)

Las Solicitudes 001 a 003 son las tres corridas exigidas por la consigna. Esta cuarta se agregó después, a propósito, porque ninguna de las tres dispara ningún criterio de la sección "Criterios de escalación" del `system_prompt.md` — esa rama del contrato estaba escrita pero nunca se había probado (ver `../DECISIONES.md`). No reemplaza a las tres anteriores.

El 20 de agosto de 2026, la empresa ANDINA LITIO, un cliente potencial del sector minero, solicitó una cotización para conectar su planta de procesamiento en Salta, Argentina, con sus oficinas centrales en Antofagasta, Chile, mediante fibra óptica terrestre.
- La distancia total es de aproximadamente 340 km, cruzando la Cordillera de los Andes por un paso de alta montaña.
- Necesitan una capacidad inicial de 100G.
- El cliente no tiene información sobre los permisos de derecho de paso ni el estatus regulatorio para tender fibra a través de la frontera; asumen que alcanza con un trámite estándar.
- En varios tramos de la ruta, de más de 120 km cada uno, no hay tendido eléctrico ni sitios de infraestructura existente para amplificación óptica.
- Desean recibir una propuesta preliminar en 20 días.

Dispara al menos dos de los cinco criterios de escalación: ruta transfronteriza con estatus regulatorio poco claro, e ingeniería de línea fotónica a medida (tramos extremos sin sitios de amplificación).
