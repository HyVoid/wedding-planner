[ 🌐 عربي ](README.ar.md) | [ 🇩🇪 Deutsch ](README.de.md) | [ 🇫🇷 Français ](README.fr.md) | [ 🇳🇱 Nederlands ](README.nl.md) | [ 🇪🇸 Español ](README.sp.md) | [ 🇬🇧 English ](README.md)

**Un panel de control ligero de planificación de bodas que mantiene proveedores, invitados, presupuesto, tareas y notas de planificación en un solo lugar.**

[Versión de navegador](https://hyvoid.github.io/wedding-planner/) · [Versión de Excel](https://github.com/HyVoid/wedding-planner/blob/main/Wedding_Control_Board.xlsx) 



## Para quién es esto

| Usuario | Situación típica | Decisión soportada | Por qué fallan los métodos existentes |
|---|---|---|---|
| Pareja que planifica sin un organizador de servicio completo | Las tareas, pagos a proveedores y respuestas de invitados están dispersos entre chats y notas | Lo que necesita atención esta semana | Las apps de mensajería conservan la conversación, no el estado de decisión |
| Organizador que gestiona una boda ligera | El cliente necesita una fuente de verdad simple sin la sobrecarga de la gestión de proyectos | Qué hito, pago o problema de RSVP debe escalarse | Los paneles genéricos muestran el estado pero rara vez conectan las tareas con los pagos y las suposiciones de invitados |
| Miembro de la familia que coordina la ejecución de invitados | Los recuentos de RSVP y las asignaciones de mesa cambian con frecuencia | Si las suposiciones de asistencia siguen siendo fiables | Las listas de invitados estáticas no revelan el impacto de planificación de las respuestas inciertas |
| Comprador consciente del presupuesto | Los contratos de proveedores se firman en momentos distintos con depósitos y saldos | Si los compromisos reales siguen dentro del presupuesto planificado | Una cifra de presupuesto total oculta el riesgo de exceso a nivel de categoría |



| Antes | Después |
|---|---|
| "Creo que la mayoría de los proveedores están pagados." | El depósito pagado, el saldo final y el estado de pago son visibles por proveedor. |
| "El número de invitados debería rondar los 80." | Los recuentos confirmados, provisionales, rechazados y no contactados están separados. |
| "Vamos más o menos según lo previsto." | Las tareas atrasadas se cuentan y se marcan contra la fecha actual. |

La implicación operativa es simple. La verificación reduce el número de decisiones tomadas desde la memoria. La memoria no es auditable. Un libro de trabajo vinculado sí lo es.

## Qué rastrea

✓ Reservas y pagos de proveedores

✓ Estado de RSVP de invitados

✓ Presupuesto frente a compromisos reales

✓ Tareas y plazos próximos

✓ Notas de planificación y decisiones

## Por qué lo construí
La mayoría de los problemas de planificación de bodas no se deben a malas decisiones.

Se deben a que la información está dispersa entre:

- chats
- correos electrónicos
- notas
- hojas de cálculo

El objetivo de este rastreador es simple:

mantener el estado de planificación actual visible en un solo lugar.

## Lo que el libro de trabajo realmente entrega (no cómo calcula)

- Un panel de control único para las decisiones de planificación de bodas.
- Un rastreador de tareas que separa el progreso del riesgo de atraso.
- Un rastreador de proveedores que expone depósitos, saldos finales, estado de pago y estado de archivo de contratos.
- Un rastreador de invitados que separa la asistencia confirmada de la demanda provisional.
- Un gestor de presupuesto que compara el presupuesto planificado con la exposición firmada a proveedores.
- Un registro de decisiones para llamadas, reuniones familiares, visitas al sitio y compromisos verbales.
- Una versión HTML independiente con guardado automático local, exportación de respaldo, importación de respaldo y reinicio.
- Un ritmo operativo reutilizable para la revisión semanal de planificación.


## Obtén el libro de trabajo

Descarga el libro de Excel o abre la app HTML independiente.

Usa el libro de Excel para flujos de trabajo nativos de hoja de cálculo. Usa la versión HTML como demo de un solo archivo en el navegador con guardado automático local y respaldos JSON portátiles.

## Acerca de

Construyo rastreadores ligeros para situaciones en las que hay demasiadas piezas en movimiento para tenerlas en la cabeza.

La mayoría de los proyectos no necesitan otra plataforma.

Necesitan un lugar simple donde las decisiones, tareas e información permanezcan conectadas.

La planificación de bodas es solo un ejemplo.
## Limitaciones

Esta herramienta no es un sistema de planificación empresarial, un libro de contabilidad, una plataforma de gestión de contratos ni un registro legal. No reemplaza el juicio profesional de planificación de bodas. No sincroniza datos entre dispositivos a menos que el usuario exporte e importe un respaldo. El almacenamiento del navegador puede ser borrado por el usuario o el navegador. Las categorías de presupuesto y las asignaciones de proveedores deben revisarse antes del uso real. Los recuentos de invitados, términos de proveedores, impuestos, depósitos, políticas de cancelación y restricciones del lugar deben validarse de forma independiente.

El libro de trabajo respalda mejores decisiones. No garantiza mejores resultados sin actualizaciones disciplinadas.

## Otras herramientas de decisión que podrías necesitar

- Tarjeta de puntuación de comparación de proveedores para la evaluación de cotizaciones.
- Optimizador de plan de asientos para restricciones de asignación de mesas.
- Previsión de flujo de caja para el calendario de pagos de la boda.
- Lista de verificación de obligaciones contractuales para la revisión de cancelación, depósito y entregables.
- Informe de variación de presupuesto posterior al evento.

## Licencia

Este proyecto está licenciado bajo la Apache License 2.0.

Consulta el archivo `LICENSE` para los términos completos de la Apache License 2.0. Si no se incluye un archivo de licencia en un paquete de distribución, consulta el texto oficial de la Apache License 2.0 publicado por la Apache Software Foundation.
