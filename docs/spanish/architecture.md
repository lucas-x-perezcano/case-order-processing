# Arquitectura y alcance del flujo de trabajo

[Caso de estudio](../../README.es-MX.md) · [English](../english/architecture.md) · [Español (México)](architecture.md)

## El alcance del proceso de negocio

La aplicación prepara órdenes de compra de clientes para un proceso de importación al ERP. Su alcance comienza cuando una persona selecciona un reporte estructurado descargado y termina con archivos preparados para una etapa separada de carga. La obtención de pedidos, la aprobación operativa, la aceptación del ERP, el surtido, la facturación y la cobranza quedan fuera de este alcance de preparación.

Esta descripción refleja la implementación inspeccionada. No significa que la secuencia completa de escritorio y ERP se haya ejecutado durante la auditoría. La evidencia y sus límites se detallan en [Validación](validation.md).

![Arquitectura conceptual de la solución con una etapa posterior separada de carga al ERP.](../../assets/solution-architecture.es-MX.svg)

## Entradas y etapas de procesamiento

| Etapa | Función en el negocio | Comportamiento implementado y límites |
| --- | --- | --- |
| Reporte estructurado de pedidos | Proporcionar las partidas de las órdenes de compra del cliente. | Una persona obtiene el archivo de Excel. Las validaciones por nombre de columna identifican columnas faltantes o inesperadas; esto no constituye una validación completa del negocio. |
| Catálogo y configuración | Proporcionar referencias de datos maestros y parámetros de las reglas. | Las tablas de consulta privadas aportan atributos de productos, de clientes, de pago y de entrega. La configuración aporta esquemas y algunas correspondencias. |
| Preparación determinista | Aplicar procedimientos repetibles. | Normalizar identificadores específicos, realizar el cruce con el catálogo, enriquecer partidas, aplicar transformaciones y agrupar referencias de pedidos. |
| Construcción de encabezados y partidas | Representar los pedidos con una relación adecuada para importación. | Crear encabezados de pedidos y partidas de detalle vinculados mediante claves internas. Estas claves no son números de pedido asignados por el ERP. La agrupación no elimina partidas duplicadas de origen ni demuestra unicidad entre ejecuciones. |
| Validaciones definidas | Hacer visibles inconsistencias definidas. | Revisar códigos, tipos, cantidades, consistencia de encabezados y metadatos obligatorios específicos. Los registros señalados pueden permanecer en la salida. |

El cruce con el catálogo depende de los datos maestros conservados. Encontrar un producto no demuestra que su información de unidad, fiscal o de empaque esté completa. La selección entre entradas duplicadas del catálogo tampoco demuestra de manera independiente que el registro elegido sea comercialmente correcto.

## El libro de revisión es una interfaz operativa

El generador implementado define un reporte enriquecido, encabezados de pedidos, partidas de pedidos, un índice de pedidos y una hoja de excepciones. Estas vistas conectan la información de origen con la representación prevista para importación y ofrecen espacios para revisión y edición. El generador actual se inspeccionó, pero no se ejecutó durante la auditoría; los libros históricos utilizados como datos de prueba no demuestran por sí solos su salida actual.

Las fechas de entrega y las confirmaciones siguen siendo entradas humanas. La interfaz de escritorio solicita guardar el libro antes de exportarlo, porque la exportación lee el archivo guardado. No obtiene información de entrega de otro sistema, no verifica todas las decisiones de revisión ni exige una aprobación antes de continuar.

La lista de excepciones se genera durante la preparación. Es una instantánea, no una evaluación que se actualice continuamente con los cambios posteriores en Excel.

## La exportación es un paso separado

El exportador lee las hojas guardadas de encabezados y partidas, actualiza ciertos textos de encabezado y campos relacionados con la entrega a partir del reporte enriquecido, da formato a los valores y escribe un archivo de encabezados y otro de partidas, delimitados por tabulaciones. Ambos usan la estructura de encabezados y la representación de fechas previstas para DTW.

La exportación no reconstruye todos los campos a partir del reporte enriquecido ni vuelve a ejecutar toda la validación. Cambiar una cantidad en el reporte no regenera automáticamente la hoja de partidas. Editar una fecha de entrega en un lugar no sincroniza todos los campos relacionados. La revisión coordinada sigue siendo necesaria.

Los dos archivos se escriben de forma secuencial, no como una sola operación atómica, y repetir nombres de salida puede reemplazar archivos anteriores. La aplicación de preparación carece de un registro estructurado persistente de ejecuciones que vincule las versiones de entrada y catálogo con los resultados finales aceptados. Estos límites son relevantes para la recuperación y la trazabilidad; la auditoría no probó los bytes de los archivos finales ni la ruta de exportación.

## SAP y DTW siguen siendo una integración separada

La aplicación de escritorio prepara archivos destinados a Data Transfer Workbench de SAP Business One. La implementación privada incluye un componente separado de carga para Windows. Define revisiones de estructura y relaciones entre archivos, un control contra el reprocesamiento de archivos idénticos, separación de credenciales y preparación de consultas de conciliación.

Esos mecanismos no demuestran importaciones exitosas. La auditoría no ejecutó el componente de carga ni se conectó a SAP. Una descripción interna de una carga en un entorno de desarrollo no aporta resultados de aceptación o rechazo, y el componente de carga que lo reemplazó estaba documentado como aún no ejecutado en Windows.

El control de reprocesamiento se refiere a contenidos de archivo idénticos, no a todos los posibles duplicados del negocio. Las consultas de conciliación requieren que una persona las ejecute e interprete. La terminación exitosa de un proceso no prueba por sí sola que todos los registros hayan sido aceptados por el ERP. El despliegue organizacional y la adopción operativa en el ERP siguen pendientes.

## Componentes técnicos en contexto

Python y pandas realizan la preparación de tablas, openpyxl gestiona el acceso a Excel y PyYAML carga la configuración. Tkinter proporciona la interfaz de escritorio. El empaquetado con PyInstaller y los flujos de compilación y pruebas están definidos para la distribución en Windows. PowerShell, XML y SQL apoyan la integración separada con DTW.

El catálogo sigue siendo una entrada operativa externa. En la ejecución empaquetada, la configuración se incluye con la aplicación; actualizar un archivo externo de configuración no equivale necesariamente a cambiar el comportamiento de la versión empaquetada. Las definiciones de compilación demuestran la intención de empaquetar, no una versión publicada o una instalación verificadas.

Este caso contiene únicamente una explicación arquitectónica depurada de información confidencial. No incluye archivos de la implementación privada, configuración operativa, credenciales de carga ni reglas específicas del negocio.

[Decisiones de diseño](design-decisions.md) · [Validación](validation.md) · [Volver al caso de estudio](../../README.es-MX.md)
