
# Conclusión del Sprint 1

## Evaluación de la calidad del dataset heredado

El dataset `port_movements.csv` heredado presentaba una baja calidad inicial, lo que requirió un proceso exhaustivo de limpieza y normalización. Se descartaron un total de 1088 filas (aproximadamente un 72.53% del dataset original de 1500 filas) debido a:
-   **Valores nulos en columnas críticas:** Columnas como `matricula`, `tonelaje_declarado`, `velocidad_ingreso`, `radar_id`, `estado_despacho` y `duracion_horas` contenían un número significativo de nulos, haciendo inviable su análisis sin una imputación o eliminación adecuada. Para asegurar la fiabilidad de los análisis subsiguientes, se optó por la eliminación de las filas con nulos en estas columnas, resultando en la pérdida de 331 filas.
-   **Outliers extremos:** Se identificaron y eliminaron 80 filas adicionales con valores atípicos en `tonelaje_declarado` y `velocidad_ingreso` utilizando el método IQR, el cual fue preferido sobre Z-score por su robustez ante distribuciones no normales. Estos outliers habrían distorsionado gravemente los cálculos de promedios y la detección de infracciones.
-   **Registros sin infracción:** Un total de 677 filas fueron eliminadas al no presentar ningún exceso de velocidad, lo cual es coherente con el objetivo de analizar específicamente los buques infractores.

Los tipos de error más frecuentes fueron la presencia de valores nulos, fechas/horas en formatos inconsistentes (normalizados a '1900-01-01' o '00:00' cuando eran inválidos), y valores numéricos fuera de rangos esperados (outliers).

## Patrones de infracción detectados

El análisis de los buques infractores reveló varios patrones clave:
-   **Turnos:** Las infracciones se concentran principalmente en la **Madrugada** (27.9%) y la **Tarde** (27.7%), seguido de la Noche (23.5%) y la Mañana (20.9%). Esto sugiere que los turnos con menor supervisión o mayor congestión podrían ser puntos críticos.
-   **Muelles:** Algunos muelles muestran un exceso de velocidad promedio considerablemente más alto, como **MUELLEA** (5.50 km/h) y **MUELLED** (5.13 km/h). Esto podría indicar problemas específicos de infraestructura, señalización o supervisión en esos muelles.
-   **Tipos de carga:** **CONTENEDORES** es el tipo de carga más frecuente entre los infractores (15.78%), seguido de TRIGO (14.56%) y HARINA (13.11%). Aunque esto podría deberse a que son los tipos de carga más comunes, podría también señalar la necesidad de un monitoreo más estricto para estos buques.
-   **Orígenes:** **VALPARAISO** es el origen más frecuente de buques infractores (62 registros), lo que podría indicar la necesidad de comunicar las regulaciones de velocidad a los operadores de buques que provienen de ese puerto.
-   **Duración de estadía:** La duración promedio de estadía en muelle para buques infractores (excluyendo fechas inválidas) es de 37.17 horas, un dato importante para evaluar la eficiencia portuaria en relación con los infractores.

## Reflexión sobre el impacto de incorporar estos datos sin limpieza previa

Incorporar el dataset heredado sin una limpieza previa al nuevo sistema habría tenido consecuencias severas y negativas:
-   **Análisis erróneos:** Los valores nulos y outliers habrían distorsionado cualquier análisis estadístico, llevando a conclusiones incorrectas sobre patrones de tráfico, eficiencias operativas y cumplimiento de normativas. Por ejemplo, el cálculo inicial de la duración promedio de estadía era de 2587 horas (más de 100 días), un valor completamente irreal debido a fechas inválidas. Sin la limpieza, no se habrían podido identificar los valores reales de 37.17 horas.
-   **Decisiones operativas y estratégicas deficientes:** Basarse en datos erróneos podría haber llevado a la toma de decisiones ineficaces o contraproducentes, como la asignación inadecuada de recursos, la implementación de políticas de seguridad incorrectas o la penalización injusta de operadores.
-   **Problemas de integridad del nuevo sistema:** La migración de datos inconsistentes podría haber corrompido la base de datos del nuevo sistema, dificultando su operatividad y la confianza en la información almacenada.
-   **Pérdida de credibilidad:** La baja calidad de los datos y los análisis resultantes erosionarían la credibilidad del sistema de gestión portuaria y de los analistas responsables.

## Propuesta concreta de mejora para el proceso de captura de datos en el puerto

**Propuesta: Implementación de un Sistema de Validación de Entrada de Datos en Tiempo Real (SVDT).**

Este sistema se integraría directamente en la interfaz de entrada de datos del nuevo sistema portuario. Sus características clave serían:

1.  **Validación de Formato y Tipo de Datos:** Al ingresar una fecha, hora, matrícula o valor numérico, el sistema validaría automáticamente que el formato y el tipo de dato sean correctos. Por ejemplo, las fechas deben estar en `YYYY-MM-DD` y las horas en `HH:MM` (24h). Los valores numéricos (`tonelaje_declarado`, `velocidad_ingreso`) serían validados para asegurar que son numéricos y están dentro de rangos esperados (e.g., velocidad no negativa, tonelaje dentro de un rango físico razonable).
2.  **Validación de Completitud:** El sistema no permitiría guardar un registro si campos críticos (`matricula`, `fecha_ingreso`, `hora_ingreso`, `muelle`, `tipo_carga`, `estado_despacho`) están vacíos. Esto obligaría a los operadores a registrar la información completa en el momento.
3.  **Alertas y Retroalimentación Inmediata:** En caso de un dato inválido o incompleto, el sistema mostraría una alerta clara y específica al operador, indicando el error y la corrección necesaria. Esto permitiría rectificar los datos en la fuente, en el momento de la captura.
4.  **Listas Desplegables y Autocompletado:** Para campos como `muelle`, `tipo_carga` y `origen`, se utilizarían listas desplegables (dropdowns) con valores predefinidos y autocompletado para `matricula` (si ya existe en el sistema), reduciendo errores tipográficos y asegurando la consistencia de los datos.
5.  **Registro de Cambios (Audit Trail):** Cualquier modificación posterior a un registro validado inicial sería auditada, registrando quién, cuándo y qué se modificó. Esto ayudaría a mantener la trazabilidad y la responsabilidad.

**Impacto esperado:** El SVDT reduciría drásticamente la cantidad de datos inconsistentes y nulos desde el origen, minimizando la necesidad de procesos de limpieza manual costosos y propensos a errores. Esto garantizaría que el nuevo sistema opere con datos de alta calidad desde el primer día, facilitando análisis precisos y la toma de decisiones informadas para la gestión portuaria.
