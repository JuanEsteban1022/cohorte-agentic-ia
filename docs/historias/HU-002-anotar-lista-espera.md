# HU-002 · Anotar a un cliente en la lista de espera

**Como** dueño (Carlos)
**quiero** anotar a un cliente en la lista de espera de un cupo que ya está ocupado
**para** que ese cliente reciba el cupo si quien lo reservó cancela.

## Prioridad y dependencias

- **Prioridad:** Must (2 de 5).
- **Depende de:** HU-001, porque solo hay lista de espera para un cupo ya reservado.
- **La necesitan:** HU-004.

## Notas: reglas de negocio

Reglas del catálogo (`docs/prd.md` §7):

- **BR-RES-01:** el cliente se anota con su nombre y teléfono (sin cuenta de cliente).
- **BR-LE-01:** solo un cupo "reservado" admite lista de espera.
- **BR-LE-02:** FIFO estricto (`created_at` ascendente).
- **BR-LE-03:** un cliente no puede estar dos veces en la misma lista, ni en la de un cupo que él mismo tiene reservado.
- **BR-LE-04:** un cliente sí puede estar en la lista aunque tenga otra reserva a la misma hora en otra cancha.

## Criterios de aceptación

```gherkin
Escenario: Anotar a un cliente en la lista de espera de un cupo ocupado (BR-LE-01)
  Dado que la "Cancha 1" el 2 de diciembre de 2026 de 19:00 a 20:00 está "reservado" a nombre de Felipe
  Y la lista de espera de ese cupo está vacía
  Cuando Carlos anota a Andrés, teléfono "300 123 4567", en la lista de espera de ese cupo el 1 de diciembre de 2026 a las 10:00
  Entonces Andrés queda en la posición 1 de la lista de espera con el teléfono "300 123 4567"

Escenario: El orden de la lista es por llegada (BR-LE-02)
  Dado que la "Cancha 1" el 2 de diciembre de 2026 de 19:00 a 20:00 está "reservado" a nombre de Felipe
  Y Andrés fue anotado en la lista de espera el 1 de diciembre de 2026 a las 10:00
  Cuando Carlos anota a Camila en la lista de espera el 1 de diciembre de 2026 a las 10:05
  Entonces Andrés queda en la posición 1 y Camila en la posición 2

Escenario: Intentar anotar a alguien en la lista de un cupo libre (BR-LE-01)
  Dado que la "Cancha 2" el 2 de diciembre de 2026 de 19:00 a 20:00 está "libre"
  Cuando Carlos intenta anotar a Andrés en la lista de espera de ese cupo
  Entonces se rechaza con el mensaje "Este cupo está libre: puedes reservarlo directamente"

Escenario: Intentar anotar dos veces al mismo cliente en la misma lista (BR-LE-03)
  Dado que la "Cancha 1" el 2 de diciembre de 2026 de 19:00 a 20:00 está "reservado" a nombre de Felipe
  Y Andrés ya está en la posición 1 de la lista de espera de ese cupo
  Cuando Carlos intenta anotar otra vez a Andrés en esa lista
  Entonces se rechaza con el mensaje "Andrés ya está en la lista de espera de este cupo"

Escenario: Intentar anotar al titular en la lista de su propio cupo (BR-LE-03)
  Dado que la "Cancha 1" el 2 de diciembre de 2026 de 19:00 a 20:00 está "reservado" a nombre de Felipe
  Cuando Carlos intenta anotar a Felipe en la lista de espera de ese cupo
  Entonces se rechaza con el mensaje "Felipe ya tiene reservado este cupo"

Escenario: Anotar a un cliente que tiene otra reserva a la misma hora (BR-LE-04)
  Dado que la "Cancha 2" el 2 de diciembre de 2026 de 19:00 a 20:00 está "reservado" a nombre de Andrés
  Y la "Cancha 1" el 2 de diciembre de 2026 de 19:00 a 20:00 está "reservado" a nombre de Felipe
  Cuando Carlos anota a Andrés en la lista de espera de la "Cancha 1" a las 19:00
  Entonces Andrés queda en la lista de espera de la "Cancha 1"
```

## Preguntas abiertas

- Ninguna por ahora.
