# HU-004 · Ofrecer el cupo cancelado y responder la oferta

**Como** dueño (Carlos)
**quiero** que el cupo cancelado se ofrezca automáticamente al primero de la lista de espera y poder aceptarlo o rechazarlo en su nombre
**para** no perder el ingreso de ese turno ni escribirle a nadie uno por uno.

## Prioridad y dependencias

- **Prioridad:** Must (4 de 5). Es el núcleo del producto. El vencimiento del plazo está en HU-006.
- **Depende de:** HU-002 y HU-003.
- **La necesitan:** HU-006 y HU-005, que muestra los estados "ofertado" y "reasignado".

## Notas: reglas de negocio

Reglas del catálogo (`docs/prd.md` §7):

- **BR-OF-01:** al cancelar, el cupo se ofrece al primero de la lista y pasa a "ofertado".
- **BR-OF-02:** plazo de 15 min, o de 5 min si falta menos de 1 h, recortado a la hora de inicio.
- **BR-OF-03:** solo se acepta o rechaza en nombre del titular de la oferta.
- **BR-OF-04:** si se rechaza, la oferta pasa al siguiente.
- **BR-OF-05:** si la lista está vacía, el cupo vuelve a "libre".
- **BR-OF-07:** si se acepta, el cupo queda "reasignado".
- **BR-RES-03:** mientras está "ofertado", el cupo no se puede reservar directamente.

Otras notas:

- La oferta se ve en la app. Carlos avisa al cliente por WhatsApp a mano y responde en su nombre.

## Criterios de aceptación

```gherkin
Antecedentes:
  Dado que la "Cancha 1" el 2 de diciembre de 2026 de 19:00 a 20:00 está "reservado" a nombre de Felipe
  Y en la lista de espera de ese cupo están, en este orden, Andrés y luego Camila

Escenario: El cupo cancelado se ofrece al primero y Carlos acepta por él (BR-OF-01, BR-OF-07)
  Cuando Carlos cancela la reserva de Felipe el 2 de diciembre de 2026 a las 15:00
  Entonces el cupo queda "ofertado" a Andrés con plazo hasta las 15:15
  Cuando Carlos acepta la oferta en nombre de Andrés a las 15:10
  Entonces la oferta queda "aceptada"
  Y el cupo queda "reasignado" a nombre de Andrés

Escenario: Si falta menos de 1 hora para el turno, el plazo es de 5 minutos (BR-OF-02)
  Cuando Carlos cancela la reserva de Felipe el 2 de diciembre de 2026 a las 18:30
  Entonces el cupo queda "ofertado" a Andrés con plazo hasta las 18:35

Escenario: El plazo se recorta a la hora de inicio (BR-OF-02)
  Cuando Carlos cancela la reserva de Felipe el 2 de diciembre de 2026 a las 18:57
  Entonces el cupo queda "ofertado" a Andrés con plazo hasta las 19:00

Escenario: Carlos rechaza en nombre del primero y la oferta pasa al siguiente (BR-OF-04)
  Dado que el cupo está "ofertado" a Andrés con plazo hasta las 15:15
  Cuando Carlos rechaza la oferta en nombre de Andrés a las 15:05
  Entonces la oferta de Andrés queda "rechazada"
  Y el cupo queda "ofertado" a Camila con plazo hasta las 15:20

Escenario: Intentar aceptar la oferta a nombre de quien no la tiene (BR-OF-03)
  Dado que el cupo está "ofertado" a Andrés con plazo hasta las 15:15
  Cuando Carlos intenta aceptar la oferta en nombre de Camila a las 15:05
  Entonces se rechaza con el mensaje "Esta oferta es para Andrés"
  Y el cupo sigue "ofertado" a Andrés

Escenario: Cancelar con la lista de espera vacía (BR-OF-05)
  Dado que la "Cancha 2" el 2 de diciembre de 2026 de 19:00 a 20:00 está "reservado" a nombre de Luis
  Y la lista de espera de ese cupo está vacía
  Cuando Carlos cancela la reserva de Luis el 2 de diciembre de 2026 a las 15:00
  Entonces el cupo queda "libre"
```

## Riesgo conocido

- El plazo de 5 minutos con aviso manual por WhatsApp se mantiene y se valida con la línea base del primer mes (`docs/prd.md` §7, Riesgos).
