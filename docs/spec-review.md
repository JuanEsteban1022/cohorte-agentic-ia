# Revisión de spec · HuecoCero

> Evaluación con la skill `evaluate-spec` (`referencia/harness/skills/cierre-de-clase/SKILL_Evaluate.md`), fecha 2026-11-27, rama `clase-03-prd`. **Quinta pasada**, después de aprobar los supuestos y corregir las incoherencias de la cuarta.
> Artefactos revisados: `docs/ficha-idea.md`, `docs/prd.md`, `docs/historias/HU-001` a `HU-006`, `CONTEXT.md`, `PLAN.md` y `README.md`.
> No existen todavía `specs/`, `design.md`, `AGENTS.md` ni código. Quedan registrados como vacíos.

## 1. Olas y nota

| Ola | Alcance | Nota | Justificación |
|---|---|---|---|
| **Ola 1 · Reservas y agenda** | HU-001, HU-002, HU-003, HU-005 | **C** | Producto y pruebas **completos**: 19 escenarios y 17 reglas con ID, sin preguntas de negocio abiertas. Lo único que la frena es la arquitectura (D1). |
| **Ola 2 · Oferta y vencimiento** | HU-004, HU-006 | **C** | Producto y pruebas **completos**: 11 escenarios que cubren aceptar, rechazar, vencer, recortar el plazo y la lista agotada. Lo único que la frena es la arquitectura (D1). |

**Cambio respecto de la cuarta pasada:**
- Se aprobaron los supuestos: BR-RES-06, BR-LE-03 y BR-LE-04, bloques de 1 hora, mensajes ilustrativos y agenda sin lista de espera.
- En la ola 1, las reglas de negocio (P4) pasan a **Cumple**.
- Se corrigieron las incoherencias de la cuarta pasada.

Las dos olas solo esperan la arquitectura de la clase 4. Con ella, las dos subirían a **A**.

## 2. Recomendación

- **El producto ya está completo en las dos olas.**
- **Siguiente paso: la clase 4.** Ahí se define la arquitectura mínima: modelo de datos, mecanismo de vencimiento, reloj inyectable y cómo se garantiza BR-RES-02 ante la concurrencia. Ese es el último crítico que falta.
- Orden de planificación: la ola 1 (HU-001 → HU-005 → HU-003 → HU-002) y luego la ola 2 (HU-004 → HU-006), como dice `docs/prd.md` §5.

## 3. Evaluación por lente

### Producto

| Criterio | Estado | Evidencia | Corrección mínima |
|---|---|---|---|
| P1 (*) Problema y usuario | Cumple | `docs/prd.md` §2 y §3 | — |
| P2 (*) Alcance | Cumple | `docs/prd.md` §5: 5 Must, flujo de punta a punta y orden de construcción | — |
| P3 (*) Fuera de alcance | Cumple | `docs/prd.md` §6 | — |
| P4 (*) Reglas de negocio | **Cumple** | Hay un catálogo de 17 reglas con ID en `docs/prd.md` §7, todas citadas en las HU. Ola 1: BR-RES-06 (no se cancela después del inicio), BR-LE-03 (sin duplicados) y BR-LE-04 (otra reserva a la misma hora). Ola 2: BR-OF-02, BR-OF-05 y BR-OF-06 cubren los bordes. | — |
| P5 Criterio de éxito | Cumple | `docs/prd.md` §4; BR-RES-04 (cancelación tardía) | — |
| P6 Decisiones pendientes | Cumple | "Preguntas abiertas" en cada HU; riesgos en `docs/prd.md` §7 | — |

### TDD

| Criterio | Estado | Evidencia | Corrección mínima |
|---|---|---|---|
| T1 (*) Aceptación observable | Cumple | Cada HU tiene "Criterios de aceptación" y cada escenario cita la regla que prueba. | — |
| T2 (*) Casos de prueba | **Cumple** | Ola 1: 19 escenarios, incluidos la falta de teléfono, la concurrencia (`HU-001`), los duplicados (`HU-002`) y la cancelación después del inicio (`HU-003`). Ola 2: 11 escenarios, incluidos el recorte del plazo y la lista agotada (`HU-004`, `HU-006`). | — |
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

- **Corregido en esta pasada:**
  - La línea "Reasignación" de `docs/prd.md` §7 ahora remite a BR-OF-02, BR-OF-04 y BR-OF-05.
  - Se quitó de `HU-006` la pregunta ya resuelta por BR-OF-06.
  - Los mensajes de error y la duración de los bloques (1 hora) quedaron como decisiones en `docs/prd.md` §7.
  - `HU-002` pasó a llamarse `HU-002-anotar-lista-espera.md`.
  - Se actualizó el Estado actual de `CONTEXT.md`.
- **Referencias fuera del repo:** la skill cita `create-implementation-plan` y `estacion6/ejemplos/spec-review-entrevista-ai.md`, que no existen aquí.
- **Dependencias por verificar:** el vencimiento automático necesita un mecanismo temporal, y la regla BR-RES-02 ante concurrencia necesita un mecanismo de exclusión (restricción única, transacción o similar). **Verificar** en la clase 4 que el stack los soporta.

## 5. Los tres vacíos más graves

1. **Arquitectura (D1).** Es el único crítico que bloquea las dos olas.
2. **Runner y declaración TDD (T3/T4):** no son críticos, pero sin comando de pruebas ni reloj inyectable no se pueden ejecutar los escenarios de plazo. Se cierran con el esqueleto del proyecto.
3. **Contratos, design.md y restricciones (D2–D4):** no son críticos, pero las tareas de interfaz y la protección de los datos del cliente los necesitan antes de construir.

## 6. Supuestos

| Para | Supuesto | Estado |
|---|---|---|
| P4 · HU-002 | Un cliente no puede estar dos veces en la misma lista de espera, ni en la lista de un cupo que él mismo tiene reservado. | **Aprobado** → BR-LE-03 |
| P4 · HU-002 | Un cliente sí puede estar en la lista de un cupo aunque tenga otra reserva a la misma hora en otra cancha. Carlos lo decide al avisarle. | **Aprobado** → BR-LE-04 |
| P4 · HU-003 | No se puede cancelar una reserva después de su hora de inicio. | **Aprobado** → BR-RES-06 |
| Coherencia | Los bloques del seed duran 1 hora. | **Aprobado** → `docs/prd.md` §7 |
| Coherencia | Los textos de los mensajes de error son ilustrativos y se ajustan en el diseño. | **Aprobado** → `docs/prd.md` §7 |
| Coherencia · HU-005 | La agenda no muestra la lista de espera de cada cupo en el MVP. | **Aprobado** → `docs/prd.md` §7 y `HU-005` |

No quedan supuestos pendientes.
