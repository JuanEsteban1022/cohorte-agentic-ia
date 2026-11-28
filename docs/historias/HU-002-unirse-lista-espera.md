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
```

## Preguntas abiertas

- ¿Se puede anotar a un cliente dos veces en la misma lista, o en la lista de un cupo que él mismo ya reservó?
- ¿Se puede anotar a Andrés en la lista de un cupo a las 19:00 si ya tiene otra reserva a esa misma hora?
