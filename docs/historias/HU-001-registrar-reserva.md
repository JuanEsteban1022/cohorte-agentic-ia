# HU-001 · Registrar una reserva a nombre de un cliente

**Como** dueño (Carlos)
**quiero** registrar en la app la reserva que un cliente me pide por WhatsApp
**para** tener la agenda completa sin depender de que mis clientes usen la app.

## Prioridad y dependencias

- **Prioridad:** Must (1 de 5). Es la primera en construirse.
- **Depende de:** ninguna. Usa las canchas y los bloques horarios precargados (seed).
- **La necesitan:** HU-002, HU-003 y HU-005.

## Notas: reglas de negocio

Reglas del catálogo (`docs/prd.md` §7):

- **BR-RES-01:** el dueño registra la reserva a nombre del cliente, con su nombre y teléfono (sin cuenta de cliente).
- **BR-RES-02:** no se puede reservar un cupo "reservado".
- **BR-RES-03:** no se puede reservar un cupo "ofertado".

Otras notas:

- La reserva directa del cliente desde la app es Should.
- Las canchas y los bloques horarios vienen del seed.

## Criterios de aceptación

```gherkin
Escenario: Registrar una reserva en un cupo libre (BR-RES-01)
  Dado que la "Cancha 1" tiene el bloque del 2 de diciembre de 2026 de 19:00 a 20:00
  Y ese bloque está "libre"
  Cuando Carlos registra la reserva de la "Cancha 1" el 2 de diciembre de 2026 a las 19:00 a nombre de Andrés, teléfono "300 123 4567"
  Entonces el cupo queda "reservado" a nombre de Andrés
  Y la reserva queda "activa" con el teléfono "300 123 4567"

Escenario: Intentar registrar una reserva sin teléfono (BR-RES-01)
  Dado que la "Cancha 1" el 2 de diciembre de 2026 de 19:00 a 20:00 está "libre"
  Cuando Carlos intenta registrar la reserva de ese cupo a nombre de Andrés sin teléfono
  Entonces la reserva se rechaza con el mensaje "Falta el teléfono del cliente"
  Y el cupo sigue "libre"

Escenario: Dos registros llegan al mismo cupo al mismo tiempo (BR-RES-02)
  Dado que la "Cancha 1" el 2 de diciembre de 2026 de 19:00 a 20:00 está "libre"
  Cuando se registran al mismo tiempo una reserva a nombre de Andrés y otra a nombre de Luis para ese cupo
  Entonces solo una de las dos reservas queda "activa"
  Y la otra se rechaza con el mensaje "Ese cupo ya está ocupado"

Escenario: Intentar reservar un cupo ocupado (BR-RES-02)
  Dado que la "Cancha 1" el 2 de diciembre de 2026 de 19:00 a 20:00 está "reservado" a nombre de Felipe
  Cuando Carlos intenta registrar la reserva de ese cupo a nombre de Andrés
  Entonces la reserva se rechaza con el mensaje "Ese cupo ya está ocupado"
  Y el cupo sigue "reservado" a nombre de Felipe

Escenario: Intentar reservar un cupo con oferta en curso (BR-RES-03)
  Dado que la "Cancha 1" el 2 de diciembre de 2026 de 19:00 a 20:00 está "ofertado" a Camila con plazo hasta las 18:35
  Cuando Carlos intenta registrar la reserva de ese cupo a nombre de Luis a las 18:32
  Entonces la reserva se rechaza con el mensaje "Este cupo tiene una oferta en curso para la lista de espera"
  Y el cupo sigue "ofertado" a Camila
```

## Preguntas abiertas

- Cómo se garantiza que solo un registro gane en caso de concurrencia es una decisión de arquitectura (clase 4).
