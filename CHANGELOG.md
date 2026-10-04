# CHANGELOG

[Ejercicio 07]
- Conclusión del Sprint 1 en `port_log/reports/conclusion.md` con valores computados del dataset limpio.
- Evaluación de la calidad del dataset, patrones de infracción, reflexión de impacto y propuesta de mejora de captura.

[Ejercicio 06]
- Porcentaje de infracciones con fecha inválida (flag de normalización).
- Porcentaje de infracciones con hora inválida (flags de ingreso y egreso).
- Tipo de carga y origen más frecuentes entre infractores.
- Duración promedio de estadía excluyendo valores `pd.NA`.

[Ejercicio 05]
- Pipeline completo de preprocesamiento (gris, CLAHE, bilateral, Canny, morfología)
- Detección de regiones de matrícula via contornos con filtros geométricos
- OCR con EasyOCR + validación regex (patrones ES, AR, genérico)
- Consolidado CSV matriculas_sprint2.csv y resumen_sprint2.csv
- Conclusiones escritas en port_log/reports/conclusion.md

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
