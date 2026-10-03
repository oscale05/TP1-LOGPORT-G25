# Conclusión del Sprint 1

## Evaluación de la calidad del dataset heredado

El dataset heredado (`port_movements.csv`, 1500 registros) presentó problemas de
calidad importantes: 90 fechas con formatos inconsistentes o imposibles
(p. ej. `32/13/2021`), 228 horas inválidas (placeholders como `AB:CD`,
`sin dato`, `N/A`, valores fuera de rango o formato 12 hs sin normalizar),
91 matrículas ausentes o sin caracteres válidos, 364 valores
nulos totales y valores numéricos fuera de rango (tonelaje negativo, velocidades de
hasta ~200 km/h).

Del total de registros se descartó un **68.40%**
(1026 registros): 254 por problemas
de calidad (nulos en columnas críticas y outliers por IQR) y 772
por no constituir infracción (filtro propio del análisis). Los errores más frecuentes fueron los formatos
inconsistentes de hora y fecha, seguidos por los nulos y los outliers numéricos.

## Patrones de infracción detectados

- **Turnos:** las infracciones se concentran en la **Madrugada**
  (134) y la **Tarde** (131),
  por encima de la Noche (106) y la Mañana
  (103).
- **Muelles:** el muelle con más infracciones es **MUELLED**
  (87) y el de mayor exceso promedio es
  **MUELLED** (3.28 km/h).
- **Tipo de carga:** el más frecuente entre infractores es **CONTENEDORES**
  (14.98%).
- **Origen:** el más frecuente es **VALPARAISO**
  (71 registros).
- **Estadía:** la duración promedio en muelle de los infractores es de
  **40.63 horas**.

## Reflexión sobre el impacto de incorporar estos datos sin limpieza previa

Cargar el dataset heredado al nuevo sistema sin depuración habría producido
estadísticas absurdas (duraciones de millones de horas por la fecha ficticia
`1900-01-01`, duraciones negativas como la de MOV-00005), rankings duplicados por
matrículas con distinto formato (`MSC-GENOVA` vs `msc genova!!`) y muelles
fragmentados (`MUELLE-B` vs `muelle  b!!`), además de porcentajes imposibles de
calcular por los nulos. Las decisiones operativas basadas en esos datos (asignación
de muelles, sanciones por exceso de velocidad, planificación de turnos) serían
erróneas y difíciles de detectar, comprometiendo la integridad y la credibilidad del
nuevo sistema.

## Propuesta de mejora para la captura de datos

Implementar **validación en el punto de captura**: campos de fecha/hora con selector
de calendario y reloj (imposibilitando formatos libres como `32/13/2021` o `AB:CD`),
listas desplegables para `muelle`, `tipo_carga` y `origen`, validación de rango para
`tonelaje_declarado` y `velocidad_ingreso`, y bloqueo de guardado cuando una
matrícula no cumpla el patrón alfanumérico. Esto elimina los errores en el origen,
en lugar de depurarlos después.
