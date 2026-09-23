# Taller 2 · De 16 canales a la figura

Curso *De la neurona a la figura: análisis neuronal en insectos asistido por IA*.
Autores: valentina Salamanca & Leidy Ruiz.

## Qué hace este notebook

Pipeline completo de spike sorting sobre una sonda lineal de 16 electrodos, aplicado a
dos registros:

- `taller2_limpia.h5` (90 s, sin estímulos): para calibrar el método.
- `taller2_ruidosa.h5` (200 s, tres condiciones de estímulo A/B/C): el análisis real.

Etapas: exploración del `.h5` y filtrado pasa-banda → detección de disparos en los 16
canales → extracción de forma de onda y tres características por evento (amplitud,
media anchura del valle, centroide de profundidad) → agrupamiento con K-means →
métricas de calidad tipo *bombcell* → autocorrelograma/correlograma cruzado para
fusionar fragmentos de una misma neurona → curación en cuatro etiquetas
(`noise`/`mua`/`non-somatic`/`good`) → verificación contra la verdad oculta →
raster, PSTH, Z-score, t-test pareado y bootstrap por condición → figura de
resultados final con las unidades `good`.

## Cómo correrlo

Abrir el notebook en Google Colab y ejecutar de arriba a abajo. La primera celda baja
automáticamente `taller2_limpia.h5`, `taller2_ruidosa.h5` y `verificacion.py` desde una
carpeta de Google Drive (no están en este repositorio: pesan ~105 MB en total y un
binario grande infla el historial de git de forma permanente). No requiere subir nada
a mano.

## Resultado principal

De 19.077 eventos detectados y 16 clusters de K-means (fusionados a 15 grupos), 2
grupos se etiquetaron `good`, 6 `mua`, 7 `noise` y 0 `non-somatic`. La curación subió
la fracción de disparos identificables como unidad individual de 0,645 a 0,934. Una de
las dos unidades `good` (cluster 3) responde selectivamente a la condición de
estímulo B; la otra (cluster 5) responde a las tres condiciones.


