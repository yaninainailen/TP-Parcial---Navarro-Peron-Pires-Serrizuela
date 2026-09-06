# A6 — Reconciliación documental final

## Alcance

- Rama: `mejoras-eber`.
- HEAD base: `67473c559f369752cea8371ae4c9c36462fcc4a1`.
- Diagnóstico de partida: `A) CONFIGURACIÓN FINAL ACEPTABLE`.
- Archivos existentes modificados en esta etapa: `README.md` y `calibracion.md`.
- Este archivo es la única evidencia nueva creada.
- No se modificaron `rubrica.md`, `agente/system_prompt.md`, `agente/sonda_v0.2.html`, casos
  originales ni evidencia A2–A6 previa.

## Actualización de README

- Se reemplazó la afirmación de salida idéntica y determinismo por mayor consistencia y
  reproducibilidad controlada, reconociendo variabilidad residual del LLM.
- Se marcaron como vigentes los resultados finales: excelente 80/100, flojo 33/100 y tramposo
  47/100, con alerta anti-trampa solo en el tramposo.
- El 100/100 del caso excelente quedó identificado como resultado histórico anterior al criterio
  de grounding.
- Se resumieron A2–A6 con el alcance empírico de cada mejora, incluida la limitación de A2 a la
  forma directa de prompt injection ensayada.
- Se documentó que Sonda está sincronizada actualmente con el system prompt y la rúbrica final.
- Se actualizaron las limitaciones: variabilidad D5 y de alerta, error aritmético aislado 31/100,
  mantenimiento manual de copias embebidas, uso de `default_branch`, señal técnica incompleta de
  archivos inaccesibles y ausencia de una cobertura exhaustiva de prompt injection.

## Actualización de calibracion.md

- Se rotuló la calibración original como histórica sin eliminar sus resultados, corridas,
  desacuerdos, pruebas contra repos reales ni stress test.
- Se renombró su tabla de cierre como `Resultado histórico de esta etapa`.
- Se agregó una sección independiente `Recalibración final A6`.
- La nueva sección registra configuración congelada, expectativa humana previa por dimensión,
  resultados actuales y comparación dimensión por dimensión.
- Se documentaron el desacuerdo D4 inicial, la iteración fallida del system prompt y su reversión,
  la aclaración D4, la replicación, la variabilidad D5, la aclaración D5 y la auditoría de la alerta
  anti-trampa.
- Se dejó explícito que el 31/100 fue aritméticamente incorrecto pero no se reprodujo en las dos
  repeticiones posteriores.
- Se referenciaron los archivos de evidencia A6 pertinentes sin copiar sus transcripciones
  completas.
- La conclusión vigente quedó registrada como `A) CONFIGURACIÓN FINAL ACEPTABLE`.

## Historial preservado

La documentación conserva como resultados de versiones previas:

- excelente 100/100 antes de incorporar grounding;
- flojo 28/100 después del ajuste histórico de formato;
- tramposo 28/100 con la alerta anti-trampa histórica;
- los desacuerdos y ajustes de la primera calibración;
- las pruebas adicionales sobre repos reales;
- el stress test de una única dimensión económica fabricada;
- la iteración fallida A6 sobre `agente/system_prompt.md`;
- todas las ejecuciones y auditorías A6 00–21.

Estos valores no se presentan como comportamiento vigente.

## Resultados vigentes

| Caso | D1 | D2 | D3 | D4 | D5 | Total | Alerta anti-trampa |
|---|---:|---:|---:|---:|---:|---:|---|
| Excelente | 10 | 25 | 15 | 15 | 15 | **80/100** | No |
| Flojo | 10 | 8 | 5 | 5 | 5 | **33/100** | No |
| Tramposo | 10 | 17 | 10 | 5 | 5 | **47/100** | Sí |

Las diferencias del tramposo respecto de la expectativa humana primaria quedaron clasificadas y
explicadas como justificables; no se modificó retroactivamente la expectativa para forzar una
coincidencia.

## Limitaciones finales documentadas

1. Las reglas discretas mejoran la consistencia, pero el LLM conserva variabilidad residual.
2. Sonda mantiene copias embebidas actualmente sincronizadas que deben verificarse tras futuros
   cambios.
3. Sonda evalúa la rama por defecto y no fija una rama o commit incluido en la URL.
4. A4 corrige el tratamiento evaluativo, pero Sonda aún puede omitir la causa técnica precisa de
   fallos de descarga, exclusiones o truncamiento.
5. A2 prueba resistencia y detección para un ataque directo controlado, no inmunidad universal a
   prompt injection.
6. El error 31/100 se conserva como variación aritmética aislada no reproducida en dos
   repeticiones posteriores.
