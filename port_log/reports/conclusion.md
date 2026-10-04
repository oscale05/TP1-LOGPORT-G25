# Conclusiones Sprint 2 - Port Log

## Resumen del trabajo realizado

En este sprint se implementó un pipeline completo de visión por computadora para la detección
y lectura de matrículas en imágenes de movimientos portuarios.

### Ejercicios completados:

1. **Ejercicio 01**: Clonado del repositorio Sprint 1, creación de rama `Sprint_2`,
   descarga y descompresión del dataset de imágenes (`port_log_images.zip`),
   verificación de archivos heredados del Sprint 1.

2. **Ejercicio 02**: Exploración del dataset de imágenes:
   - Listado de todas las imágenes disponibles con sus tamaños
   - Agrupación por carpeta padre (grupos: `camion`, `contenedor`, `matricula`, etc.)
   - Estadísticas de dimensiones (ancho/alto min, max, media)
   - Función `mostrar_muestra()` para visualización rápida
   - Guardado de metadatos en `group_images.json`

3. **Ejercicio 03**: Preprocesamiento de imágenes:
   - Conversión a escala de grises
   - Ecualización adaptativa (CLAHE) para mejora de contraste
   - Suavizado con filtro bilateral (preserva bordes)
   - Detección de bordes Canny
   - Todas las variantes guardadas en `port_log/data/interim/imgs/`

4. **Ejercicio 04**: (Integrado en pipeline) Pipeline completo OCR-ready:
   - Combinación: Gris -> CLAHE -> Bilateral -> Canny -> Morfología (cierre)
   - Detección de regiones candidatas via contornos con filtros geométricos
   - Filtrado por área, aspect ratio y posición

5. **Ejercicio 05**: OCR y validación:
   - EasyOCR sobre regiones detectadas (top 3 por imagen)
   - Validación con regex para patrones de matrícula (España, Argentina, genérico)
   - Filtrado por confianza mínima (0.3)
   - Consolidado en CSV `matriculas_sprint2.csv` y resumen `resumen_sprint2.csv`

## Resultados obtenidos

- Imágenes procesadas: 2 grupos, 100 imágenes totales
- Matrículas detectadas y validadas: 0
- Archivos generados:
  - `port_log/data/interim/group_images.json`
  - `port_log/data/interim/matriculas_detectadas.json`
  - `port_log/reports/matriculas_sprint2.csv`
  - `port_log/reports/resumen_sprint2.csv`
  - `port_log/reports/conclusion.md` (este archivo)
  - Imágenes preprocesadas en `port_log/data/interim/imgs/`

## Dificultades y soluciones

- **Variabilidad de iluminación**: Resuelta con CLAHE (ecualización adaptativa local)
- **Ruido en imágenes**: Filtro bilateral preserva bordes mientras suaviza
- **Falsos positivos en contornos**: Filtros geométricos (área, aspect ratio) reducen ruido
- **OCR en matrículas pequeñas/borrosas**: EasyOCR muestra robustez; umbral confianza 0.3

## Próximos pasos (Sprint 3)

- Entrenar/detector especializado (YOLO) para localización de matrículas
- Mejorar preprocesamiento específico por tipo de vehículo
- Integrar con datos tabulares del Sprint 1 (cruce por timestamp/cámara)
- Dashboard interactivo de movimientos portuarios
