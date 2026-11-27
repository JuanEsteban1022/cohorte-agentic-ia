# Ficha de idea

**Nombre tentativo:** HuecoCero

## 1. Problema

Los negocios pierden ingresos por los turnos vacíos que quedan cuando un cliente cancela a última hora o no se presenta (no-show). A esto se suma el caos operativo de gestionar a mano las reprogramaciones y las listas de espera.

## 2. Para quién

Administradores o dueños de centros con cupos limitados por horario, por ejemplo:

- canchas sintéticas,
- estudios de pilates,
- box de entrenamiento.

Reciben entre **15 y 80 reservas diarias** y hoy gestionan su agenda por WhatsApp, Excel o cuaderno.

## 3. Cómo lo resuelven hoy y qué les duele

**Cómo lo resuelven hoy:**

- Anotan las reservas en una hoja de cálculo o en un chat.
- Piden transferencias manuales como anticipo.
- Si alguien cancela, escriben uno por uno a sus contactos frecuentes por WhatsApp para intentar llenar el hueco.

**Qué les duele:**

- Si alguien cancela 30 minutos antes, no alcanzan a reasignar el cupo y pierden ese dinero.
- Cobrar penalizaciones genera una fricción incómoda por chat.
- Dos administradores pueden prometer el mismo horario por error (doble reserva).

## 4. Propuesta

Una plataforma de reservas por bloques horarios que **reasigna automáticamente las cancelaciones a una lista de espera priorizada**, bloquea los cupos en tiempo real para evitar dobles asignaciones y aplica reglas de penalización por incumplimiento.

**Capacidades secundarias:**

- **Bloqueo en tiempo real.** Es higiene técnica básica: cualquier base de datos con restricciones únicas o un calendario simple ya lo resuelve. No justifica por sí solo un producto nuevo.
- **Reglas de penalización.** Son un complemento administrativo. Aunque se cobren multas, si el turno queda vacío igual se pierde capacidad instalada y rotación. Además, en negocios de barrio, cobrar multas sin ofrecer antes una alternativa para llenar el cupo suele espantar clientes.

## 5. Métrica de éxito

**Tasa de recuperación de turnos cancelados:** el porcentaje de cupos cancelados con menos de 24 horas de anticipación que se reasignan con éxito a un usuario de la lista de espera antes de la hora de inicio.

- **Objetivo:** por definir, tras medir la línea base del primer mes de operación.

## 6. Qué NO es

- **No es un sistema de historia clínica ni de gestión de pacientes médicos.** Si se usa en salud, solo gestiona la agenda, no datos médicos.
- **No es una pasarela de pagos ni un software contable o de facturación electrónica.** Solo registra estados de pago, seña o saldo de penalización.
- **No es un marketplace público tipo directorio para descubrir locales nuevos.** Es una herramienta operativa para los clientes propios de cada negocio.
