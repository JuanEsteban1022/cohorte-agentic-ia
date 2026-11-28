# HU-006 · Vencimiento del plazo y paso al siguiente

**Como** dueño (Carlos)
**quiero** que, si nadie responde la oferta a tiempo, pase sola al siguiente de la lista de espera
**para** que el cupo siga buscando quién lo tome sin que yo tenga que estar pendiente del reloj.

## Prioridad y dependencias

- **Prioridad:** Must (parte del Must 4, junto con HU-004). Es la última en construirse.
- **Depende de:** HU-004.
- **La necesitan:** ninguna.

## Notas: reglas de negocio

Reglas del catálogo (`docs/prd.md` §7):

- **BR-OF-02:** plazo de 15 min, o de 5 min si falta menos de 1 h, recortado a la hora de inicio.
- **BR-OF-04:** si vence, la oferta pasa automáticamente al siguiente.
- **BR-OF-05:** si la lista se agota, el cupo vuelve a "libre".
- **BR-OF-06:** una oferta "vencida" no se puede aceptar, aunque el cliente haya dicho que sí por WhatsApp mientras Carlos leía el mensaje.

## Criterios de aceptación

```gherkin
Antecedentes:
  Dado que la reserva de Felipe de la "Cancha 1" del 2 de diciembre de 2026 de 19:00 a 20:00 fue cancelada
  Y en la lista de espera de ese cupo están, en este orden, Andrés y luego Camila

Escenario: Vence el plazo de 15 minutos y la oferta pasa al siguiente (BR-OF-04)
  Dado que el cupo está "ofertado" a Andrés con plazo hasta las 15:15
  Cuando son las 15:15 y nadie respondió en nombre de Andrés
  Entonces la oferta de Andrés queda "vencida"
  Y el cupo queda "ofertado" a Camila con plazo hasta las 15:30

Escenario: Vence el plazo de 5 minutos cerca del turno (BR-OF-02, BR-OF-04)
  Dado que el cupo está "ofertado" a Andrés con plazo hasta las 18:35
  Cuando son las 18:35 y nadie respondió en nombre de Andrés
  Entonces la oferta de Andrés queda "vencida"
  Y el cupo queda "ofertado" a Camila con plazo hasta las 18:40

Escenario: El plazo del siguiente se recorta a la hora de inicio (BR-OF-02)
  Dado que el cupo está "ofertado" a Andrés con plazo hasta las 18:57
  Cuando son las 18:57 y nadie respondió en nombre de Andrés
  Entonces el cupo queda "ofertado" a Camila con plazo hasta las 19:00

Escenario: La lista se agota y el cupo vuelve a libre (BR-OF-05)
  Dado que la oferta de Andrés quedó "vencida"
  Y el cupo está "ofertado" a Camila con plazo hasta las 15:30
  Cuando son las 15:30 y nadie respondió en nombre de Camila
  Entonces la oferta de Camila queda "vencida"
  Y el cupo queda "libre"

Escenario: Intentar aceptar una oferta vencida (BR-OF-06)
  Dado que la oferta de Andrés venció a las 15:15
  Cuando Carlos intenta aceptar la oferta en nombre de Andrés a las 15:16
  Entonces se rechaza con el mensaje "Esta oferta ya venció"
  Y el cupo no queda a nombre de Andrés
```

## Preguntas abiertas

- Las pruebas necesitan controlar el reloj ("cuando son las 15:15"). El diseño debe permitir inyectar la hora (clase 4 o 5).
