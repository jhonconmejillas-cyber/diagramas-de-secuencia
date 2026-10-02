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
| [`cancelacion-reserva.html`](cancelacion-reserva.html) | Cancelación de una reserva | Asíncrono (ack inmediato + notificación PUSH/PULL diferida) |

## Diagramas de arquitectura

Páginas con la imagen del diagrama (en `img/`) y zoom: rueda del mouse o doble clic
para acercar, arrastrar para moverse, botones Ajustar / 1:1 / − / +.

| Diagrama | Contenido |
|---|---|
| [`diagrama-componentes.html`](diagrama-componentes.html) | Capas de consumidores, servicios de negocio y almacenamiento |
| [`diagrama-despliegue.html`](diagrama-despliegue.html) | Distribución de los servicios en PC1 a PC4 con REQ/REP y PUSH/PULL |
| [`diagrama-clases.html`](diagrama-clases.html) | Servicios y modelo de datos (Evento, Ocurrencia, Reserva) |

## Arquitectura de referencia

Basado en la arquitectura de 3 capas del enunciado:

- **Capa de consumidores**: Clientes
- **Capa de servicios de negocio**: Gestor de Transacciones (GT), Servicio RMC
  (Reserva/Modificación/Consulta), Servicio de Cancelación y Notificaciones
- **Capa de servicios de almacenamiento**: Servicio de Persistencia y BD (PostgreSQL)

Las comunicaciones síncronas usan el patrón Req/Rep de ZeroMQ; la cancelación usa
PUSH/PULL: el cliente abre un PULL en su propio puerto y el Servicio de Cancelación le
envía la notificación directamente, sin pasar de nuevo por el Gestor de Transacciones.
El Servicio de Persistencia accede a la BD con operaciones SQL (SELECT, UPDATE, INSERT).
