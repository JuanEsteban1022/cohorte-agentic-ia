# HU-003 · Cancelar la reserva de un cliente

**Como** dueño (Carlos)
**quiero** cancelar en la app la reserva de un cliente que me avisa por WhatsApp
**para** liberar el cupo y que el sistema lo ofrezca a la lista de espera.

## Prioridad y dependencias

- **Prioridad:** Must (3 de 5).
- **Depende de:** HU-001.
- **La necesitan:** HU-004, porque la cancelación es lo que dispara la oferta a la lista de espera (BR-OF-01).

## Notas: reglas de negocio

Reglas del catálogo (`docs/prd.md` §7):

- **BR-RES-04:** una cancelación con menos de 24 h queda marcada como "tardía" y cuenta para la métrica.
- **BR-RES-05:** no se puede cancelar una reserva ya "cancelada".

Otras notas:

- Las penalizaciones por cancelar están fuera del MVP (Should).

## Criterios de aceptación

```gherkin
Escenario: Cancelar una reserva con menos de 24 horas de anticipación (BR-RES-04)
  Dado que la "Cancha 1" el 2 de diciembre de 2026 de 19:00 a 20:00 está "reservado" a nombre de Felipe
  Cuando Carlos cancela la reserva de Felipe el 2 de diciembre de 2026 a las 18:30
  Entonces la reserva de Felipe queda "cancelada"
  Y la cancelación queda marcada como "tardía"

Escenario: Cancelar una reserva con más de 24 horas de anticipación (BR-RES-04)
  Dado que la "Cancha 1" el 2 de diciembre de 2026 de 19:00 a 20:00 está "reservado" a nombre de Felipe
  Cuando Carlos cancela la reserva de Felipe el 30 de noviembre de 2026 a las 12:00
  Entonces la reserva de Felipe queda "cancelada"
  Y la cancelación no queda marcada como "tardía"

Escenario: Intentar cancelar una reserva ya cancelada (BR-RES-05)
  Dado que la reserva de Felipe de la "Cancha 1" del 2 de diciembre de 2026 a las 19:00 está "cancelada"
  Cuando Carlos intenta cancelar de nuevo esa reserva
  Entonces se rechaza con el mensaje "Esta reserva ya fue cancelada"
```

## Preguntas abiertas

- ¿Hay una hora límite para cancelar (por ejemplo, no se puede cancelar después de la hora de inicio)?
