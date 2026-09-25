# Diagramas de secuencia - Sistema de Reserva de Boletos (SRB)

Generados con [Archify](https://github.com/tt-a1i/archify) a partir del enunciado
"Sistema Distribuido Basado en Servicios para la Reserva de Boletos" (Introducción a
Sistemas Distribuidos, Pontificia Universidad Javeriana).

Cada `.html` es un archivo autocontenido: descárgalo y ábrelo en el navegador para
explorar el diagrama (zoom, tema claro/oscuro, búsqueda). Cada `.sequence.json` es la
especificación fuente que generó ese HTML.

## Diagramas incluidos

| Diagrama | Operación | Patrón |
|---|---|---|
| [`consulta-eventos.html`](consulta-eventos.html) | Consulta de eventos del mes | Síncrono (Req/Rep) |
| [`reserva-asientos.html`](reserva-asientos.html) | Reserva de puestos para un evento | Síncrono (Req/Rep) |
| [`modificacion-reserva.html`](modificacion-reserva.html) | Modificación de una reserva existente | Síncrono (Req/Rep) |
| [`cancelacion-reserva.html`](cancelacion-reserva.html) | Cancelación de una reserva | Asíncrono (ack inmediato + notificación diferida) |

## Arquitectura de referencia

Basado en la arquitectura de 3 capas del enunciado:

- **Capa de consumidores**: Clientes
- **Capa de servicios de negocio**: Gestor de Transacciones (GT), Servicio RMC
  (Reserva/Modificación/Consulta), Servicio de Cancelación y Notificaciones
- **Capa de servicios de almacenamiento**: Servicio de Persistencia

Las comunicaciones síncronas usan el patrón Req/Rep de ZeroMQ; la cancelación usa un
patrón asíncrono (pub/sub o push/pull) y notifica al cliente directamente, sin pasar de
nuevo por el Gestor de Transacciones.
