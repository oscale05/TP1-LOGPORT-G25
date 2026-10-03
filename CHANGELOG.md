# CHANGELOG

[Ejercicio 07]
- Redacción y guardado de la conclusión del Sprint 1 en `port_log/reports/conclusion.md`.
- Evaluación de la calidad del dataset heredado.
- Identificación de patrones de infracción detectados.
- Reflexión sobre el impacto de incorporar datos sin limpieza.
- Propuesta de mejora para el proceso de captura de datos.

[Ejercicio 06]
- Respuesta: ¿Qué porcentaje de infracciones provienen de registros con fecha inválida?
- Respuesta: ¿Qué porcentaje de infracciones provienen de registros con hora inválida?
- Respuesta: ¿Cuál es el tipo de carga más frecuente en infracciones y qué porcentaje representa?
- Respuesta: ¿Cuál es el origen más frecuente entre los buques infractores?
- Respuesta: ¿Cuál es la duración promedio de estadía en muelle de los buques infractores? (Valor corregido)

[Ejercicio 05]
- Gráfico 'Top 10 matrículas más reincidentes' → `plots/top_infractores.jpg`.
- Gráfico 'Total de infracciones por turno del día' → `plots/turnos.jpg`.
- Gráfico 'Total de infracciones por mes' sin la fecha ficticia `1900-01-01` y ordenado de mayor a menor → `plots/meses.jpg`.
- Histograma del exceso de velocidad real con KDE → `plots/distribucion_exceso.jpg`.
- Gráfico 'Exceso de velocidad promedio por muelle' → `plots/exceso_por_muelle.jpg`.
- Gráfico 'Fecha válida vs inválida' → `plots/fechas_invalidas.jpg`.

[Ejercicio 04]
- Clase `PortAnalyzer` con datos encapsulados y type hints completos.
- Métodos: `top_infractores`, `infracciones_por_turno`, `exceso_promedio`, `exceso_promedio_tolerancia`, `infracciones_por_muelle`, `infractores_por_tipo_carga`.
- Instanciación e invocación de cada método en celdas separadas.

[Ejercicio 03]
- Normalización de fechas (`YYYY-MM-DD`, `dd/mm/YYYY`, `dd-mm-YYYY`); inválidas → `1900-01-01` con flag de validez.
- Normalización de horas a 24 hs con detalle de inválidas originales; `00:00` se trata como hora válida.
- `duracion_horas` con `pd.NA` ante fechas/horas inválidas o duraciones negativas.
- Matrículas y muelles sin separadores (solo alfanumérico, mayúsculas); matrícula inválida → `pd.NA`.
- Eliminación de nulos solo en columnas críticas justificadas.
- Outliers por IQR (comparado con Z-score) en `tonelaje_declarado` y `velocidad_ingreso`.
- Columnas `exceso_velocidad_real` y `exceso_velocidad`; filtro de infracciones; dataset limpio en `data/interim/` y resumen en `reports/summary_sprint1.csv`.

[Ejercicio 02]
- Descarga del dataset con `curl` a `port_log/data/raw/port_movements.csv` (sin pandas).
- Exploración: primeras/últimas filas, tipos de datos y nulos por columna.
- Completitud por columna con validación de campos (no solo valores presentes).

[Ejercicio 01]
- Clonado idempotente del repositorio y creación/verificación de la rama `Sprint_1`.
- Estructura de carpetas `port_log/` (data/raw, data/interim/plots, data/processed, reports).
- Limpieza del repositorio: eliminación de `.config/` y archivos ajenos; `.gitignore`.
- Creación/actualización de `README.md` y `CHANGELOG.md`.
