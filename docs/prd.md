# PRD · HuecoCero

> Fuente: `docs/ficha-idea.md`. Los nombres y cifras de los perfiles son ilustrativos.

## 1. Visión

Que ningún cupo cancelado a última hora se quede vacío: HuecoCero lo ofrece automáticamente a quien está en lista de espera.

## 2. Problema

Los centros con cupos por horario pierden ingresos por cancelaciones de último momento y no-shows, y gestionan a mano las reprogramaciones y las listas de espera.

- **Hoy:** reservas en Excel, chat o cuaderno, anticipos por transferencia manual y, cuando alguien cancela, mensajes uno por uno por WhatsApp.
- **Dolor:**
  - Si alguien cancela 30 minutos antes, el cupo no se alcanza a reasignar y ese dinero se pierde.
  - Cobrar penalizaciones por chat genera fricción.
  - Hay dobles reservas entre administradores.

## 3. Usuarios

### Perfil 1 · Carlos, dueño de un complejo de 3 canchas sintéticas (ilustrativo)

- **Contexto:** unas 40 reservas diarias; sus clientes le reservan por WhatsApp (muchos por audio). En el MVP, **Carlos es quien opera la app**: registra las reservas, gestiona la lista de espera, avisa las ofertas por WhatsApp y las acepta en nombre del cliente.
- **Objetivo:** llenar cada cupo cancelado sin escribirle a nadie uno por uno.
- **Dolor:** las cancelaciones de última hora le dejan canchas vacías y pierde ese dinero; cobrar multas por chat es incómodo y le espanta clientes.

### Perfil 2 · Andrés, cliente que juega fútbol con amigos (ilustrativo)

- **Contexto:** está en la lista de espera de la cancha. En el MVP no usa la app: se comunica con Carlos por WhatsApp.
- **Objetivo:** conseguir cancha en el horario que quiere (por ejemplo, entre semana por la noche).
- **Dolor:** los horarios buenos están llenos y se entera tarde, o nunca, de que alguien canceló.

## 4. Métricas de éxito

- **Tasa de recuperación de turnos cancelados:** porcentaje de cupos cancelados con menos de 24 horas de anticipación que se reasignan a alguien de la lista de espera antes de la hora de inicio.
- **Objetivo:** por definir, tras medir la línea base del primer mes de operación.

## 5. Alcance del MVP

Clasificación MoSCoW de las funcionalidades candidatas:

| # | Funcionalidad | Usuario | MoSCoW |
|---|---|---|---|
| 1 | Registrar una reserva a nombre de un cliente (no se permite en un cupo ocupado ni en uno con oferta en curso) | Dueño | Must |
| 2 | Anotar a un cliente en la lista de espera de un cupo ocupado | Dueño | Must |
| 3 | Cancelar la reserva de un cliente | Dueño | Must |
| 4 | Ofrecer automáticamente el cupo cancelado al primero de la lista de espera (FIFO), con plazo para que el dueño acepte o rechace en nombre del cliente | Sistema / Dueño | Must |
| 5 | Ver la agenda del día con el estado de cada cupo (libre, reservado, ofertado con su plazo, reasignado) | Dueño | Must |
| 6 | El cliente reserva directamente desde la app | Cliente | Should |
| 7 | Configurar canchas y bloques horarios desde la app | Dueño | Should |
| 8 | Ver la tasa de recuperación de turnos cancelados | Dueño | Should |
| 9 | Registrar pagos, señas, no-shows y saldo de penalización | Dueño | Should |
| 10 | Priorizar la lista de espera con reglas (frecuencia, historial) | Dueño | Could |
| 11 | Notificaciones por WhatsApp | Cliente | Won't |
| 12 | Cobro en línea (pasarela de pagos) o facturación electrónica | Dueño | Won't |
| 13 | Marketplace público para descubrir locales | Cliente | Won't |
| 14 | Historia clínica o datos médicos | — | Won't |
| 15 | Varios locales o sedes por cuenta | Dueño | Won't |

**MVP (solo los Must):** 1 a 5. Cubre el flujo principal de punta a punta: Carlos registra la reserva de Felipe y anota a Andrés en la lista de espera; Felipe cancela por WhatsApp y Carlos lo registra; el sistema ofrece el cupo a Andrés; Carlos le avisa por WhatsApp y acepta en su nombre; la agenda muestra el cupo reasignado.

**Historias:** el Must 4 se parte en dos historias (HU-004 oferta y respuesta; HU-006 vencimiento del plazo).

**Orden de construcción:** HU-001 → HU-005 → HU-003 → HU-002 → HU-004 → HU-006. Con HU-001 y HU-005 ya hay algo visible de punta a punta (registrar una reserva y verla en la agenda).

## 6. Fuera del MVP

**Siguientes (Should / Could):**

- **Reserva directa del cliente desde la app:** los clientes de Carlos reservan por WhatsApp; en el MVP es Carlos quien registra.
- **Configurar canchas y horarios desde la app:** en el MVP vienen precargados en la base de datos (seed).
- **Ver la tasa de recuperación:** la métrica se puede calcular a partir de los datos del MVP; el panel puede esperar.
- **Pagos, señas, no-shows y penalizaciones:** según la ficha son un complemento; sin llenar el cupo, no evitan la pérdida.
- **Priorización con reglas:** el MVP usa FIFO estricto; las reglas se afinan con datos reales.

**No haremos (Won't):**

- **Notificaciones por WhatsApp:** es una integración externa con costo y depende de la aprobación de Meta. En el MVP las ofertas se ven en la app y Carlos avisa a mano.
- **Pasarela de pagos o facturación electrónica:** la ficha lo excluye; solo se registran estados.
- **Marketplace público:** la ficha lo excluye; es una herramienta para los clientes propios de cada negocio.
- **Historia clínica o datos médicos:** la ficha lo excluye; solo se gestiona la agenda.
- **Varios locales o sedes:** el usuario objetivo es un solo centro.

## 7. Supuestos y riesgos

**Decisiones del MVP:**

- **Operación:** el MVP lo opera el dueño. Registra reservas, lista de espera y cancelaciones a nombre de sus clientes.
- **Configuración:** las canchas y los bloques horarios vienen precargados en la base de datos (seed).
- **Orden de la lista de espera:** FIFO estricto por orden de llegada (`created_at` ascendente).
- **Reasignación:** el cupo se ofrece al primero de la lista, con **15 minutos** para aceptar o rechazar. Si falta menos de 1 hora para el turno, el plazo baja a **5 minutos**. Si se rechaza o vence el plazo, la oferta pasa automáticamente al siguiente.
- **Aviso y respuesta:** Carlos avisa la oferta al cliente por WhatsApp a mano y acepta o rechaza en su nombre en la app.
- **Cupo con oferta en curso:** queda en estado "ofertado" y **no se puede reservar directamente**, para no saltarse la lista de espera.

**Estados oficiales:**

| Entidad | Estados | Nota |
|---|---|---|
| Cupo | libre · reservado · ofertado · reasignado | "Reasignado" es un cupo reservado cuya reserva viene de una oferta aceptada |
| Reserva | activa · cancelada | "Tardía" no es un estado: es una marca de la cancelación hecha con menos de 24 h |
| Oferta | pendiente · aceptada · rechazada · vencida | — |

**Catálogo de reglas de negocio** (las historias citan estos IDs):

| ID | Regla |
|---|---|
| BR-RES-01 | El dueño registra la reserva, o anota al cliente en la lista de espera, con su **nombre y teléfono**. No hay cuentas de cliente en el MVP. |
| BR-RES-02 | No se puede reservar un cupo "reservado" (evita la doble reserva). |
| BR-RES-03 | No se puede reservar un cupo "ofertado" (no se salta la lista de espera). |
| BR-RES-04 | Una cancelación hecha con menos de 24 h de anticipación queda marcada como "tardía" y cuenta para la métrica. |
| BR-RES-05 | No se puede cancelar una reserva ya "cancelada". |
| BR-LE-01 | Solo un cupo "reservado" admite lista de espera. |
| BR-LE-02 | La lista de espera sigue FIFO estricto (`created_at` ascendente). |
| BR-OF-01 | Al cancelar una reserva, el cupo se ofrece al primero de la lista y pasa a "ofertado". |
| BR-OF-02 | El plazo de la oferta es de 15 min, o de 5 min si falta menos de 1 h para el turno, y nunca pasa de la hora de inicio: si termina después, se recorta a esa hora. |
| BR-OF-03 | El dueño solo puede aceptar o rechazar en nombre del titular de la oferta. |
| BR-OF-04 | Si la oferta se rechaza o vence, pasa automáticamente al siguiente de la lista. |
| BR-OF-05 | Si la lista se agota o está vacía, el cupo vuelve a "libre". |
| BR-OF-06 | Una oferta "vencida" no se puede aceptar. |
| BR-OF-07 | Si la oferta se acepta, el cupo queda "reasignado" a nombre del titular. |

**Riesgos (propuestos, por validar):**

- El aviso depende de que Carlos vea la oferta y escriba por WhatsApp. Si está atendiendo la cancha, el plazo puede vencer, y esto afecta directamente la tasa de recuperación.
- El plazo de 5 minutos cerca del turno puede no ser realista cuando el aviso es manual. **Se mantiene y se valida** con la línea base del primer mes de operación.
- Se guardan datos personales del cliente (nombre y teléfono). Hay que definir su protección en el diseño (clase 4).
- El MVP tiene un solo usuario dueño. La doble reserva **entre varios administradores**, mencionada en el problema, no se resuelve con varios usuarios; solo la evita la regla de no reservar un cupo ocupado.
