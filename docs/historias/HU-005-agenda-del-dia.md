# HU-005 · Ver la agenda del día

**Como** dueño (Carlos)
**quiero** ver la agenda del día con el estado de cada cupo
**para** saber de un vistazo qué canchas están libres, reservadas, ofertadas o reasignadas, y a quién tengo que avisar.

## Prioridad y dependencias

- **Prioridad:** Must (5 de 5). Se construye justo después de HU-001, para tener algo visible de punta a punta.
- **Depende de:** HU-001. Los estados "ofertado" y "reasignado" solo aparecen con HU-004.
- **La necesitan:** ninguna.

## Notas: reglas de negocio

Estados oficiales del cupo (`docs/prd.md` §7): libre · reservado · ofertado · reasignado.

- Un cupo "ofertado" muestra a quién se le ofreció y hasta qué hora vence el plazo, para que Carlos le avise por WhatsApp.
- Un cupo "reasignado" es el que se llenó con una oferta aceptada (BR-OF-07).
- Las canchas y los bloques horarios vienen del seed.

## Criterios de aceptación

```gherkin
Escenario: Ver la agenda del día con cupos libres y reservados
  Dado que el 2 de diciembre de 2026 la "Cancha 1" y la "Cancha 2" tienen el bloque de 19:00 a 20:00
  Y la "Cancha 1" de 19:00 a 20:00 está "reservado" a nombre de Felipe
  Y la "Cancha 2" de 19:00 a 20:00 está "libre"
  Cuando Carlos abre la agenda del 2 de diciembre de 2026
  Entonces ve la "Cancha 1" de 19:00 a 20:00 como "reservado" a nombre de Felipe
  Y ve la "Cancha 2" de 19:00 a 20:00 como "libre"

Escenario: Un cupo con oferta en curso muestra a quién y su plazo
  Dado que la "Cancha 1" del 2 de diciembre de 2026 de 19:00 a 20:00 está "ofertado" a Andrés con plazo hasta las 15:15
  Cuando Carlos abre la agenda del 2 de diciembre de 2026 a las 15:05
  Entonces ve la "Cancha 1" de 19:00 a 20:00 como "ofertado" a Andrés, con vencimiento a las 15:15

Escenario: Un cupo reasignado aparece con su nuevo titular (BR-OF-07)
  Dado que Carlos aceptó en nombre de Andrés la oferta de la "Cancha 1" del 2 de diciembre de 2026 a las 19:00
  Cuando Carlos abre la agenda del 2 de diciembre de 2026
  Entonces ve la "Cancha 1" de 19:00 a 20:00 como "reasignado" a nombre de Andrés

Escenario: Consultar un día sin bloques configurados
  Dado que el seed no tiene bloques horarios para el 25 de diciembre de 2026
  Cuando Carlos abre la agenda del 25 de diciembre de 2026
  Entonces ve el mensaje "No hay cupos configurados para este día"
```

## Preguntas abiertas

- ¿La agenda muestra también la lista de espera de cada cupo?
