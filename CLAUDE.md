# CLAUDE.md

> Plantilla inicial. En la **clase 2** la completas con tu producto y la vas mejorando en cada clase.

## Producto
- Nombre: HuecoCero (tentativo)
- En una frase: Recupera los ingresos que se pierden por cancelaciones de último momento y no-shows reasignando automáticamente cada cupo cancelado a una lista de espera priorizada, para dueños o administradores de centros con cupos por horario (canchas sintéticas, estudios de pilates, box de entrenamiento) que reciben entre 15 y 80 reservas diarias y hoy gestionan su agenda por WhatsApp, Excel o cuaderno.
- Métrica: tasa de recuperación de turnos cancelados (cupos cancelados con menos de 24 h de anticipación que se reasignan antes de la hora de inicio). Objetivo por definir tras medir la línea base del primer mes.
- Fuentes de verdad, en este orden: `docs/ficha-idea.md` → `docs/prd.md` → `specs/` → `docs/adr/`.

## Cómo trabajamos
- Explica antes de cambiar código: qué harás y por qué.
- Si el cambio toca varios archivos, propón un plan y espera mi "ok".
- Construye solo lo de la clase actual (guía en `docs/clases/`). Si algo es de otra clase, anótalo y no lo hagas.
- No inventes requisitos: si algo no está en el PRD o la spec, pregúntame.

## Git
- Trabaja en la rama de la clase (`clase-NN-slug`). Nunca hagas commit en `main`.
- Commits pequeños, en español, con prefijo: `feat:`, `fix:`, `docs:`, `test:`, `chore:`.
- No hagas merge ni push a `main`: los PR los fusiono yo.

## Idioma
Español, Markdown limpio.
