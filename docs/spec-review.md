# Revisión de spec · HuecoCero

> Evaluación con la skill `evaluate-spec` (`referencia/harness/skills/cierre-de-clase/SKILL_Evaluate.md`), fecha 2026-11-27, rama `clase-03-prd`. **Cuarta pasada**, después de agregar el catálogo de reglas BR, los estados oficiales y los datos del cliente.
> Artefactos revisados: `docs/ficha-idea.md`, `docs/prd.md`, `docs/historias/HU-001` a `HU-006`, `CONTEXT.md`, `PLAN.md` y `README.md`.
> No existen todavía `specs/`, `design.md`, `AGENTS.md` ni código. Quedan registrados como vacíos.

## 1. Olas y nota

| Ola | Alcance | Nota | Justificación |
|---|---|---|---|
| **Ola 1 · Reservas y agenda** | HU-001, HU-002, HU-003, HU-005 | **C** | Producto y pruebas listos: 15 escenarios, reglas con ID y datos del cliente definidos. Quedan 3 reglas menores sin decidir (P4 parcial, se cierran con supuestos) y falta la arquitectura (D1). |
| **Ola 2 · Oferta y vencimiento** | HU-004, HU-006 | **C** | Producto y pruebas **completos**: 11 escenarios que cubren aceptar, rechazar, vencer, recortar el plazo y la lista agotada. Lo único que la frena es la arquitectura (D1). |

**Cambio respecto de la tercera pasada:**
- En la ola 2, las reglas de negocio (P4) y los casos de prueba (T2) pasan a **Cumple**.
- En la ola 1, se resolvieron los datos del cliente y la concurrencia.

Las dos olas solo esperan la arquitectura de la clase 4. Con ella, la ola 2 subiría directamente a **A**, y la ola 1 a **B** si se aprueban los supuestos de la §6.

## 2. Recomendación

- **Aprobar los 3 supuestos de la ola 1 (§6)** para cerrar su P4. Con eso, el producto queda completo en las dos olas.
- **Siguiente paso: la clase 4.** Ahí se define la arquitectura mínima: modelo de datos, mecanismo de vencimiento, reloj inyectable y cómo se garantiza BR-RES-02 ante la concurrencia. Ese es el último crítico que falta.
- Orden de planificación: la ola 1 (HU-001 → HU-005 → HU-003 → HU-002) y luego la ola 2 (HU-004 → HU-006), como dice `docs/prd.md` §5.

## 3. Evaluación por lente

### Producto

| Criterio | Estado | Evidencia | Corrección mínima |
|---|---|---|---|
| P1 (*) Problema y usuario | Cumple | `docs/prd.md` §2 y §3 | — |
| P2 (*) Alcance | Cumple | `docs/prd.md` §5: 5 Must, flujo de punta a punta y orden de construcción | — |
| P3 (*) Fuera de alcance | Cumple | `docs/prd.md` §6 | — |
| P4 (*) Reglas de negocio | **Ola 1: Parcial · Ola 2: Cumple** | Hay un catálogo de 14 reglas con ID en `docs/prd.md` §7, citadas en todas las HU. Ola 2: todos los bordes tienen regla (BR-OF-02 recorte, BR-OF-05 lista agotada, BR-OF-06 oferta vencida). Ola 1: quedan sin regla los duplicados en la lista y el conflicto de horario (`HU-002` · Preguntas abiertas), y la hora límite para cancelar (`HU-003` · Preguntas abiertas). | Aprobar los supuestos de la §6 y pasarlos al catálogo. |
| P5 Criterio de éxito | Cumple | `docs/prd.md` §4; BR-RES-04 (cancelación tardía) | — |
| P6 Decisiones pendientes | Cumple | "Preguntas abiertas" en cada HU; riesgos en `docs/prd.md` §7 | — |

### TDD

| Criterio | Estado | Evidencia | Corrección mínima |
|---|---|---|---|
| T1 (*) Aceptación observable | Cumple | Cada HU tiene "Criterios de aceptación" y cada escenario cita la regla que prueba. | — |
| T2 (*) Casos de prueba | **Cumple** | Ola 1: 15 escenarios, incluidos la falta de teléfono y la concurrencia (`HU-001`). Ola 2: 11 escenarios, incluidos el recorte del plazo y la lista agotada (`HU-004`, `HU-006`). | Cuando se aprueben los supuestos de la ola 1, agregar sus escenarios. |
| T3 Runner y comando | Falta | `PLAN.md` menciona Vitest y Playwright (clases 5 y 6); no hay `package.json` ni comando. | Se cierra con el esqueleto del proyecto. |
| T4 Declaración TDD | Parcial | `README.md` (clase 5: "primera historia con TDD"); `HU-006` pide poder inyectar la hora. | Escribir dónde viven las pruebas y con qué comando se corren. |

### Diseño

| Criterio | Estado | Evidencia | Corrección mínima |
|---|---|---|---|
| D1 (*) Arquitectura | **Falta** | No existe `specs/` ni ningún documento de arquitectura. | Clase 4: modelo de datos (cancha, bloque, reserva, cliente, lista de espera, oferta), vencimiento automático, reloj inyectable y garantía de BR-RES-02 ante concurrencia. |
| D2 Contratos | Falta | Sin APIs ni esquemas. | Clase 4. |
| D3 design.md | Falta | No existe; el producto tiene interfaz. | `design.md` mínimo antes de las tareas de interfaz. |
| D4 Restricciones | Parcial | Los datos personales ya están identificados (nombre y teléfono, BR-RES-01), y `docs/prd.md` §7 · Riesgos dice que su protección va al diseño. Faltan los requisitos no funcionales (precisión de los plazos, despliegue). | Definir en la clase 4 la protección de datos y la precisión del vencimiento. |

## 4. Coherencia entre artefactos

- **Una decisión duplicada que no está al día:** `docs/prd.md` §7 · Decisiones del MVP · "Reasignación" (línea 91) describe los plazos de 15 y 5 minutos pero **no menciona el recorte a la hora de inicio**, que sí está en BR-OF-02. La corrección mínima es que esa línea remita a BR-OF-02.
- **Una pregunta abierta que ya está resuelta:** `HU-006` · Preguntas abiertas pregunta qué pasa si el plazo vence mientras Carlos lee el WhatsApp. BR-OF-06 ya lo responde (una oferta vencida no se puede aceptar), y lo cubre el escenario "Intentar aceptar una oferta vencida".
- **Mensajes de error sin decisión:** los textos de los `Entonces` no salen del PRD.
- **Duración de los bloques:** los escenarios usan bloques de 1 hora; ni el PRD ni el seed la definen.
- **Nombre de archivo:** el slug de `HU-002-unirse-lista-espera.md` no coincide con su título ("Anotar a un cliente…").
- **`CONTEXT.md` desactualizado:** el Estado actual sigue en la clase 2.
- **Referencias fuera del repo:** la skill cita `create-implementation-plan` y `estacion6/ejemplos/spec-review-entrevista-ai.md`, que no existen aquí.
- **Dependencias por verificar:** el vencimiento automático necesita un mecanismo temporal, y la regla BR-RES-02 ante concurrencia necesita un mecanismo de exclusión (restricción única, transacción o similar). **Verificar** en la clase 4 que el stack los soporta.

## 5. Los tres vacíos más graves

1. **Arquitectura (D1).** Es el único crítico que bloquea las dos olas.
2. **Tres reglas menores sin decidir (P4, ola 1):** duplicados en la lista, conflicto de horario y hora límite para cancelar. Se cierran con los supuestos de la §6.
3. **Coherencia del PRD:** la línea de "Reasignación" de la §7 no recoge el recorte del plazo, y HU-006 tiene una pregunta ya resuelta. Son correcciones de una línea, pero un agente podría leer la versión vieja de la regla.

## 6. Supuestos propuestos (pendientes de aprobación)

| Para | Supuesto propuesto | Estado |
|---|---|---|
| P4 · HU-002 | Un cliente no puede estar dos veces en la misma lista de espera, ni en la lista de un cupo que él mismo tiene reservado. | **Pendiente de aprobación** |
| P4 · HU-002 | Un cliente sí puede estar en la lista de un cupo aunque tenga otra reserva a la misma hora en otra cancha. Carlos lo decide al avisarle. | **Pendiente de aprobación** |
| P4 · HU-003 | No se puede cancelar una reserva después de su hora de inicio. | **Pendiente de aprobación** |
| Coherencia | Los bloques del seed duran 1 hora. | **Pendiente de aprobación** |
| Coherencia | Los textos de los mensajes de error son ilustrativos y se ajustan en el diseño. | **Pendiente de aprobación** |
| Coherencia · HU-005 | La agenda no muestra la lista de espera de cada cupo en el MVP. | **Pendiente de aprobación** |
