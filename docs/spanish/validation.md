# Validación — Evidencia, exclusiones y estado actual

[Caso de estudio](../../README.es-MX.md) · [English](../english/validation.md) · [Español (México)](validation.md)

Esta es una descripción depurada de información confidencial de la auditoría privada de evidencia del **18 de octubre de 2026**. Los resultados que aparecen a continuación son resultados históricos de esa auditoría; preparar este caso de estudio no implicó volver a ejecutar la aplicación ni establecer un nuevo resultado de pruebas. Posteriormente, Lucas confirmó el origen del proyecto, el desarrollo colaborativo, la decisión tecnológica y el despliegue organizacional pendiente.

## Categorías de evidencia

| Categoría | Cómo se usa aquí |
| --- | --- |
| Hecho verificado | Inspección del código fuente, pruebas ejecutadas o cálculos registrados en la auditoría. |
| Declaración del desarrollador | El relato de Lucas sobre el proyecto y las declaraciones históricas de las notas de desarrollo. No sustituyen los resultados aceptados por el ERP. |
| Inferencia razonable | Una interpretación del diseño o de sus implicaciones operativas, identificada explícitamente como tal. |
| Desconocido | Un resultado o condición no establecido por la evidencia disponible. |
| Mejora propuesta | Una posible medida futura, no una capacidad implementada ni un resultado actual. |

## Un solo conjunto de datos de referencia conservado

La muestra de referencia conservada permitió revisar las claves de pedido y partida y sus relaciones entre encabezados y partidas. Los tamaños de muestra operativos y las mediciones derivadas se excluyen de esta descripción pública.

Los libros de Excel conservados **ya estaban enriquecidos**. Las hojas de reporte correspondientes contienen valores iguales, por lo que representan **un solo conjunto de datos distinto**, no varios lotes independientes. Las notas del desarrollador describen los registros como datos operativos; su uso histórico y su aceptación por el ERP no se confirmaron de manera independiente. Se tratan como confidenciales independientemente de esa limitación sobre su procedencia.

La muestra conservada no incluía un archivo de prueba separado con el reporte original sin modificaciones. La regresión de enriquecimiento construye una entrada similar a la original al seleccionar columnas originales del reporte de un libro enriquecido y restaurar información histórica de entrega y confirmación a partir de esa misma referencia. Esto respalda validaciones definidas de reconstrucción, no una prueba completamente independiente desde la descarga original hasta la aceptación por el ERP.

## Pruebas ejecutadas y limitación del entorno

La versión auditada definía **154 métodos de prueba**. Un subconjunto seleccionado deliberadamente de **106 pruebas se ejecutó con éxito**, con **cero fallas o errores**. Los otros **48 métodos no se ejecutaron porque PyYAML no estaba disponible** en el entorno de auditoría. Se excluyeron del ejecutor de pruebas; no se reportaron como omitidos ni aprobados.

| Área | Ejecutados con éxito | No ejecutados |
| --- | ---: | ---: |
| Lectura de entrada e integración de esquemas | 0 | 13 |
| Enriquecimiento | 45 | 15 |
| Transformación | 25 | 0 |
| Regresión de transformación con la muestra conservada | 0 | 13 |
| Validaciones definidas | 27 | 7 |
| Estructura del mapeo de carga y de las plantillas de conciliación | 9 | 0 |
| **Total** | **106** | **48** |

El entorno utilizó Python 3.12.14, pandas 2.2.3, openpyxl 3.1.5 y el ejecutor unittest de la biblioteca estándar en macOS. pytest tampoco estaba disponible. No se instalaron dependencias durante la auditoría.

Las pruebas ejecutadas cubrieron transformaciones específicas y casos positivos y negativos, incluidos la agrupación, la selección de cantidades, el cruce de condiciones de pago, el uso de fechas alternativas, los códigos no reconocidos por el catálogo, validaciones definidas de consistencia y tipos, las advertencias de metadatos faltantes y las relaciones estáticas de las plantillas de carga. Las pruebas de plantillas no ejecutaron la herramienta de carga ni SAP. Se inspeccionaron las definiciones de pruebas de entrada y esquemas, pero sus 13 métodos no se ejecutaron.

Este es un conteo de validaciones ejecutadas, no una medición de cobertura de código, un porcentaje de exactitud ni una prueba de que los 154 métodos se hayan aprobado. Las notas históricas mencionan 135 pruebas aprobadas; esa cifra coincide con el número de métodos de una rama principal local anterior, no con una ejecución completa verificada de la versión ampliada auditada.

## Diagnósticos separados de comparación con referencias

Otros diagnósticos de la auditoría llamaron a las funciones reales de enriquecimiento, transformación y validación en memoria. Como faltaba la dependencia YAML habitual, se leyó la configuración mediante un analizador alternativo instalado y se reconstruyeron de manera independiente los diccionarios de consulta del catálogo a partir del libro maestro conservado.

Esos diagnósticos **no ejecutaron la ruta de carga de configuración y datos maestros de la aplicación, el generador actual de libros, el exportador de texto, la interfaz de escritorio ni el componente de carga al ERP**. Fueron independientes de las 106 pruebas ejecutadas y no deben contarse como aprobación de los métodos de regresión que no se ejecutaron.

| Diagnóstico | Resultado observado | Alcance |
| --- | --- | --- |
| Transformar la entrada ya enriquecida | Tablas preparadas de encabezados y partidas con advertencias por uso de fechas alternativas ante fechas faltantes. | Estaban presentes los valores ya enriquecidos y la información histórica de entrega. |
| Comparar con las tablas de referencia | Se reprodujeron las tablas normalizadas de referencia de encabezados y partidas. | Comparación en memoria con filas alineadas, no igualdad de bytes de archivo. |
| Ejecutar validaciones definidas posteriores a la transformación sobre ese resultado | El resultado seguía sujeto a revisión. | Las advertencias de transformación seguían sin resolverse. |

La comparación normalizó los valores vacíos, dio un formato uniforme a las fechas y normalizó los números mediante redondeo a dos decimales. No demostró igualdad exacta de números de punto flotante, archivos exportados idénticos byte por byte, corrección independiente de la referencia ni aceptación por SAP. El resultado más sólido es la reproducibilidad de la representación normalizada de referencia a partir de la muestra ya enriquecida.

## Volver a enriquecer la muestra reveló brechas relevantes

| Hallazgo | Qué observó la auditoría | Qué no demuestra |
| --- | --- | --- |
| Metadatos de catálogo incompletos | Al volver a enriquecer la muestra, se encontraron los códigos de producto, pero siguieron faltando metadatos requeridos de unidad, clave fiscal del producto y capacidad de empaque. | Un producto reconocido no está necesariamente lo suficientemente completo para su uso posterior. Las brechas no se corrigieron automáticamente. |
| Ajuste manual histórico de cantidad | Volver a enriquecer la muestra no reprodujo un ajuste manual histórico de cantidad. | Se desconoce la justificación de negocio del ajuste; la auditoría no decidió si debía prevalecer este o el valor automatizado. |
| Exclusiones en comparaciones específicas | La regresión de cantidades existente omite el ajuste. Las comparaciones de metadatos usan resultados con datos y omiten los resultados faltantes en los campos correspondientes. | Obtener resultados satisfactorios en esas comparaciones específicas no demuestra equivalencia completa en todas las partidas y todos los campos. |

Con las citas históricas restauradas, volver a enriquecer la muestra y después transformarla y revisarla produjo advertencias por uso de fechas alternativas y por metadatos faltantes. Sin citas, reproduciendo el comportamiento de la preparación inicial en escritorio, las tablas preparadas quedaron acompañadas de errores por fechas faltantes y advertencias de metadatos. Estos resultados ponen de manifiesto información que requiere revisión; no muestran resolución automática ni aprobación operativa.

La auditoría también encontró una inconsistencia entre las cantidades de las partidas del libro histórico y la referencia de texto esperada. Esa discrepancia respalda una revisión cuidadosa de los archivos de preparación, pero no prueba que hayan entrado cantidades incorrectas al ERP. No se deriva de ella una tasa de error operativo.

## Límites de revisión y exportación

La aplicación muestra excepciones específicas, pero **no impone un control de aprobación obligatorio**. Los registros con nivel de error pueden acompañar a la salida. Un recordatorio para guardar Excel no demuestra que se hayan aprobado las decisiones de negocio.

La exportación lee las hojas guardadas y actualiza ciertos campos de encabezado. **No vuelve a ejecutar toda la validación**, no reconstruye todas las tablas preparadas ni sincroniza todos los cambios relacionados. La lista de excepciones puede quedar desactualizada después de los cambios. Las modificaciones de cantidades, productos y entregas pueden necesitar actualizaciones coordinadas entre las hojas del libro.

La auditoría no encontró una prueba directa del generador actual de libros, una regresión del texto final a nivel de bytes, una prueba de interacción con el escritorio ni una prueba de ejecución del componente de carga en Windows. Esas etapas se inspeccionaron en el código en lugar de ejecutarse en el flujo completo de la aplicación. La comparación con la referencia no puede ampliarse para afirmar la corrección de todo el recorrido desde la entrada original hasta el ERP.

## Madurez y resultados no verificados

La evidencia respalda una implementación local desarrollada, con pruebas específicas de componentes y diagnósticos basados en referencias. Lucas confirma que el despliegue organizacional y la adopción operativa en el ERP siguen pendientes. El caso no afirma que la aplicación esté lista para producción.

Las notas de desarrollo describen una carga de prueba en un entorno de desarrollo, pero no había resultados conservados de aceptación o rechazo ni conciliación posterior a la carga. El componente de carga que lo reemplazó estaba documentado explícitamente como aún no ejecutado en Windows. Estas declaraciones no demuestran importaciones exitosas al ERP ni uso habitual por parte de la persona responsable.

No hay una medición de referencia del trabajo manual, ahorro de trabajo, reducción de errores, retorno de inversión, capacidad de procesamiento en producción ni métricas de adopción. La auditoría no verificó la actualización de la rama remota, la ejecución de integración continua ni los resultados de publicación de versiones. Los resultados se aplican a la versión local inspeccionada y a la muestra conservada, no a todas las entradas o catálogos futuros.

## Qué se necesitaría para hacer afirmaciones más amplias

Para afirmar que hubo importaciones aceptadas, se necesitarían resultados conservados del ERP vinculados a los archivos preparados específicos. Para afirmar adopción, se necesitaría evidencia de uso real en la organización. Para afirmar mejoras de eficiencia, se necesitarían una referencia manual definida y mediciones comparables. Estos son requisitos de evidencia para afirmaciones futuras, no validación ya completada ni un nuevo plan de implementación.

El caso actual puede describir con precisión el rediseño del proceso, el trabajo colaborativo de requisitos, la preparación determinista, las pruebas específicas aprobadas y los límites encontrados durante la validación.

[Arquitectura](architecture.md) · [Decisiones de diseño](design-decisions.md) · [Volver al caso de estudio](../../README.es-MX.md)
