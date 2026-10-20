# Decisiones de diseño — Simplificar después de entender el trabajo

[Caso de estudio](../../README.es-MX.md) · [English](../english/design-decisions.md) · [Español (México)](design-decisions.md)

## De dónde viene el razonamiento

El relato de Lucas establece la identificación de la oportunidad, la colaboración con la persona responsable, el mapeo del proceso, el uso de Claude Code y la decisión de alejarse de un enfoque inicialmente orientado a inteligencia artificial. La auditoría privada de evidencia establece qué hace la implementación inspeccionada y qué demuestra la validación conservada.

Las siguientes distinciones son relevantes: el razonamiento personal se atribuye a Lucas; las consecuencias operativas inferidas del diseño se identifican como análisis. Ninguna se presenta como un beneficio medido para el negocio.

## Aclarar el procedimiento con la persona que lo realiza

**Relato confirmado de Lucas.** Después de la automatización de facturas, continué entrevistando a empleados sobre tareas repetitivas. Trabajé directamente con la persona responsable de preparar los pedidos, revisé los procedimientos existentes en Excel, mapeé el proceso más amplio y el flujo de preparación rediseñado, y traduje esos procedimientos en requisitos. Iteramos con la retroalimentación de la persona responsable.

La persona responsable aportó conocimiento de la operación existente. Mi contribución fue conectar ese conocimiento con un flujo de preparación repetible y desarrollar la aplicación con Claude Code. La colaboración no demuestra la aprobación formal de todas las reglas de negocio; la auditoría aún identifica supuestos de las reglas que requieren confirmación de la persona responsable del proceso.

Los diagramas internos se mantienen privados. Las ilustraciones públicas explican el alcance más amplio sin reproducir los mapas originales.

## Pasar de una idea orientada a inteligencia artificial al procesamiento determinista

**Razonamiento confirmado de Lucas.** Al principio exploré un enfoque similar al del flujo de facturas asistido por inteligencia artificial. A través de las iteraciones, reconocí que estas entradas estaban estructuradas y que las transformaciones recurrentes seguían, en gran medida, reglas explícitas. Simplifiqué la solución mediante código convencional.

**Implementación verificada.** El flujo de preparación usa campos de entrada con nombres definidos, cruce con el catálogo, cálculos, agrupación y estructuras de salida fijas. La inferencia recurrente de un modelo de lenguaje no forma parte del procesamiento de un pedido. La asistencia de inteligencia artificial durante el desarrollo de la aplicación es una actividad separada.

**Interpretación de ingeniería.** Las transformaciones deterministas proporcionan una forma reproducible de expresar las reglas conocidas y comparar resultados con referencias. Esa adecuación no implica que todas las entradas, los catálogos o los supuestos del negocio sean correctos. Eliminar la inferencia recurrente tampoco hace gratuitos el mantenimiento, la infraestructura o el trabajo operativo.

## Mantener Excel como interfaz de trabajo para la revisión

**Implementación verificada.** La aplicación crea un libro de revisión y exporta a partir de su estado guardado. Las personas proporcionan allí la información de entrega y confirmación. Esto mantiene el punto de intervención cerca del procedimiento basado en Excel que se revisó con la persona responsable.

**Interpretación de ingeniería.** Una interfaz de revisión familiar puede apoyar la transferencia entre los datos generados y el conocimiento operativo. No se han medido la usabilidad real, la adopción ni los beneficios de capacitación. Las hojas editables también crean la responsabilidad de coordinar cambios: la exportación actualiza únicamente ciertos campos y no reconstruye todas las tablas preparadas.

## Hacer visibles las excepciones mientras continúa la preparación

**Política de implementación documentada.** Una estructura de reporte incompatible detiene la preparación. Otros problemas definidos generalmente generan registros de excepción mientras el lote continúa. Esto incluye información faltante y ciertos problemas de catálogo o consistencia.

**Interpretación de ingeniería.** Continuar la preparación mantiene disponible el resto de la información para su revisión, pero las advertencias visibles no garantizan su resolución. La gravedad de un error no impone un control de aprobación del negocio. La aplicación puede preparar una salida que todavía necesita una decisión operativa, y la exportación no vuelve a ejecutar todas las validaciones.

Algunas fechas faltantes pueden recibir una estimación señalada como tal a partir de la información disponible en el lote. Una estimación no es una cita de entrega confirmada. En el paso inicial habitual de preparación, la persona todavía no ha proporcionado citas.

## Tratar los datos maestros como parte del proceso

**Evidencia verificada.** Volver a enriquecer la muestra conservada puso de manifiesto metadatos obligatorios de catálogo incompletos. Un ajuste manual histórico de cantidad también difirió de la regla automatizada. Algunas comparaciones de regresión existentes excluyen esas brechas.

**Interpretación de ingeniería.** Las transformaciones confiables necesitan información de referencia utilizable y una forma de conservar las excepciones legítimas. Una consulta exitosa puede devolver un registro incompleto, y una fórmula reproducible puede diferir de un ajuste manual justificado. El caso público no infiere que el ajuste haya sido incorrecto ni que los campos faltantes se hayan resuelto automáticamente.

Las correspondencias y reglas de conversión específicas del negocio se mantienen privadas. [Validación](validation.md) explica el alcance de las discrepancias observadas y las exclusiones de las comparaciones.

## Distinguir la preparación de archivos de la adopción en la organización

**Alcance verificado.** La aplicación genera la representación prevista para importación, y existe un componente separado de carga. La auditoría no demuestra importaciones exitosas a SAP, uso habitual ni preparación para producción.

**Estado confirmado por Lucas.** El despliegue organizacional y la adopción operativa en el ERP siguen pendientes. El caso usa lenguaje neutral para describir su estado y no revela circunstancias internas de la organización.

**Interpretación de ingeniería.** Implementar transformaciones, validar entradas conocidas e integrar una herramienta a la operación diaria son logros separados. La responsabilidad de aprobación, la conciliación, la responsabilidad sobre los datos maestros y los resultados aceptados por el ERP siguen siendo límites relevantes de la evidencia.

## Qué demuestra este diseño

El proyecto demuestra análisis de procesos, colaboración con la persona que realiza el trabajo, traducción de procedimientos en reglas explícitas y disposición para simplificar la tecnología. No demuestra mejoras de eficiencia medidas. Cualquier afirmación más amplia sobre confiabilidad o adopción debe sustentarse en evidencia adicional a las pruebas locales de componentes y a la muestra de referencia conservada.

[Arquitectura](architecture.md) · [Validación](validation.md) · [Volver al caso de estudio](../../README.es-MX.md)
