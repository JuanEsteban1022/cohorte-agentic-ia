# CONTEXT.md

Memoria del proyecto entre clases. El agente lee la sección **Estado actual** al iniciar (desde la clase 5, con un hook). Mantenla corta.

## Estado actual

| Campo | Valor |
|---|---|
| Producto | HuecoCero |
| Clase en curso | 3 · Tu PRD con IA |
| Última clase completada | 2 · Tu entorno y tu producto |
| Fecha | 2026-11-27 |
| Siguiente paso | Abrir el PR de `clase-03-prd` y pasar a la clase 4 (spec y arquitectura) |
| Progreso | 2 / 7 |

---

## Mi perfil

- Rol actual y experiencia: _(p. ej., backend 3 años, frontend 1 año)_
- Lo que quiero lograr en la cohorte: _(una frase)_
- Horas por semana fuera de clase: _(número)_

## Stack

Por defecto: Next.js + TypeScript, Vitest, Playwright, GitHub Actions, Docker, Terraform, k6. Si cambias algo, enlaza el ADR.

## Bitácora

Entradas nuevas **arriba**. No se reescriben las anteriores.

```
### Clase N · Título · AAAA-MM-DD
- Entregable: …
- Decisiones: … (enlace al ADR si aplica)
- Aprendí: …
- Pendiente: …
```

### Clase 3 · Tu PRD con IA · 2026-11-27
- Entregable: `docs/prd.md` con 7 secciones, 6 historias en `docs/historias/` con 30 escenarios Gherkin y `docs/spec-review.md` (nota C en las dos olas; solo falta la arquitectura).
- Decisiones: el MVP lo opera el dueño (registra, avisa por WhatsApp y acepta en nombre del cliente). Catálogo de 17 reglas BR y estados oficiales. Se partió HU-004 en HU-004 y HU-006.
- Pendiente: clase 4 (spec viva y arquitectura: modelo de datos, vencimiento de ofertas, reloj inyectable, concurrencia).

### Clase 2 · Tu entorno y tu producto · 2026-11-27
- Entregable: ficha de la idea HuecoCero en `docs/ficha-idea.md`, a partir de una entrevista de 6 preguntas, y sección "Producto" de `CLAUDE.md` actualizada; fusionado en `main` con el PR #1.
- Decisiones: se descartó la idea de cosmética. El problema es recuperar los cupos cancelados a última hora en canchas, pilates y box, y la métrica es la tasa de recuperación de turnos cancelados (objetivo por definir).
- Pendiente: completar Estado actual y Mi perfil de este archivo. El siguiente paso es la clase 3 (PRD).
