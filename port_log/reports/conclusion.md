# Conclusión del Sprint 1

## Evaluación de la calidad del dataset heredado

El dataset `port_movements.csv` heredado presentaba una baja calidad inicial, lo que requirió un proceso exhaustivo de limpieza y normalización. Se descartaron un total de 1088 filas (aproximadamente un 72.53% del dataset original de 1500 filas) debido a:
-   Valores nulos en columnas críticas (`matricula`, `tonelaje_declarado`, `velocidad_ingreso`, `radar_id`, `estado_despacho`, `duracion_horas`).
-   Outliers significativos en `tonelaje_declarado` y `velocidad_ingreso`.
-   Registros que, tras la limpieza, no presentaban ninguna infracción.

Los tipos de error más frecuentes incluyeron:
-   **Inconsistencias de formato:** Fechas y horas con formatos variados e inválidos, requiriendo normalización a `YYYY-MM-DD` y `HH:MM` respectivamente.
-   **Caracteres especiales:** En campos como `matricula` y `muelle`, que fueron eliminados para estandarización.
-   **Valores atípicos:** En variables numéricas como `tonelaje_declarado` y `velocidad_ingreso`, que distorsionaban los promedios y análisis.
-   **Valores nulos:** Que afectaban la integridad de registros clave para la identificación de infracciones y el cálculo de duraciones.

Afortunadamente, después de la limpieza y filtrado de las filas sin infracción, no se encontraron infracciones provenientes de registros con fecha o hora inválida (0.00% en ambos casos), lo que indica que el proceso de limpieza fue efectivo para los datos relevantes.

## Patrones de infracción detectados

El análisis de los 412 registros de infracciones restantes reveló los siguientes patrones:

-   **Por Turno:** La mayoría de las infracciones se concentran en los turnos de **Madrugada (115)** y **Tarde (114)**, seguidos de Noche (97) y Mañana (86). Esto podría indicar momentos de menor supervisión o mayor congestión en ciertos horarios.
-   **Por Muelle:** Las infracciones están distribuidas de manera relativamente uniforme entre los muelles principales (MUELLE-A a MUELLE-F), con MUELLE-B (70) y MUELLE-C (70) ligeramente por encima de los demás. Sin embargo, se detectaron algunas variaciones debido a errores de tipeo en los nombres de los muelles (e.g., "MUELLEA" vs "MUELLE-A").
-   **Por Tipo de Carga:** Los **Contenedores (65)** y el **Trigo (60)** son los tipos de carga con mayor número de infracciones, seguidos de Harina (54) y Granos (53). Esto podría sugerir que buques que transportan estos tipos de carga son más propensos a cometer infracciones de velocidad.
-   **Exceso de Velocidad Promedio:** El exceso de velocidad promedio real fue de 3.02 km/h, y con una tolerancia del 5%, fue de 2.46 km/h, indicando que las infracciones no son marginales.
-   **Origen Más Frecuente:** El origen más frecuente entre los buques infractores es "ASIA" con un número significativo de registros.
-   **Duración Promedio de Estadía:** La duración promedio de estadía en muelle para los buques infractores es de aproximadamente 28.52 horas.

## Reflexión sobre el impacto de incorporar datos sin limpieza previa

Incorporar los datos del sistema heredado sin una limpieza y normalización previas al nuevo sistema tendría graves consecuencias:

-   **Análisis distorsionados:** Los informes y análisis generados a partir de datos inconsistentes serían inexactos, llevando a conclusiones erróneas sobre el rendimiento portuario, la seguridad y la eficiencia.
-   **Ineficiencia operativa:** Los operadores del nuevo sistema tendrían que lidiar con entradas de datos incorrectas, lo que podría causar retrasos, errores en la planificación y una gestión deficiente de los recursos.
-   **Pérdida de confianza:** La fiabilidad del nuevo sistema se vería comprometida, erosionando la confianza en la información y en las decisiones basadas en ella.
-   **Costos ocultos:** La corrección manual de datos, la investigación de discrepancias y la repetición de tareas generarían costos operativos adicionales significativos.
-   **Dificultad en la identificación de patrones:** Los errores y la falta de estandarización en campos clave impedirían la detección de patrones relevantes, como los top infractores o los horarios/lugares con más problemas, dificultando la toma de medidas correctivas.

## Propuesta de mejora para el proceso de captura de datos

Una propuesta concreta de mejora para el proceso de captura de datos en el puerto sería la implementación de un sistema de **validación y estandarización en el punto de entrada de datos**. Esto incluiría:

1.  **Validación de formatos en tiempo real:** Para fechas (AAAA-MM-DD), horas (HH:MM), y campos numéricos (rangos de tonelaje y velocidad).
2.  **Campos con selección predefinida:** Utilizar listas desplegables o catálogos para `muelle`, `tipo_carga` y `estado_despacho` para evitar errores de tipeo y garantizar la consistencia.
3.  **Normalización automática:** Implementar rutinas que, al ingresar una `matricula`, eliminen automáticamente caracteres especiales y la conviertan a mayúsculas, o incluso validen su formato contra un registro de matrículas conocidas.
4.  **Retroalimentación instantánea:** Proporcionar mensajes claros al usuario si los datos ingresados no cumplen con los estándares, permitiendo la corrección inmediata.

Esta estrategia proactiva reduciría drásticamente la cantidad de datos inconsistentes, mejorando la calidad de la información en el nuevo sistema desde su origen.
