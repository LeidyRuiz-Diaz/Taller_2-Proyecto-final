# De la neurona a la figura — Análisis neuronal en LP asistido por IA

Proyecto final del curso · Análisis de registros Neuropixels del área **LP** (núcleo lateral posterior del
tálamo) del ratón, usando datos del **Allen Brain Observatory**.

**Integrantes:** Leidy Ruiz · Valentina Salamanca
**Área asignada:** LP (431 neuronas, 5 ratones)
**Comparación asignada:** rejillas estáticas vs. rejillas en movimiento

---

## Resumen del resultado

Contrario a la predicción inicial ("diferencia grande, a favor del movimiento"), en nuestro subconjunto de LP
una **mayor fracción de neuronas responde a rejillas estáticas (27.5%) que a rejillas en movimiento (11.4%)**,
con intervalos de confianza del 95% (bootstrap jerárquico) que no se traslapan, una prueba de permutación con
p < 0.0001, y un tamaño de efecto moderado (d de Cohen = -0.43, a favor de estáticas).

| Condición | Fracción respondedora | IC 95% (bootstrap jerárquico) |
|---|---|---|
| Rejillas en movimiento | 11.4% | [5.0%, 18.8%] |
| Rejillas estáticas | 27.5% | [22.1%, 32.7%] |

El intervalo de confianza jerárquico resultó considerablemente más ancho que el de un t-test que ignora la
identidad del ratón (0.138 vs. 0.056 de ancho), evidenciando la correlación entre neuronas del mismo animal.

---

## Contenido del repositorio

```
├── Proyecto_final_LP_Ruiz_Salamanca.ipynb   # Notebook completo: carga de datos, pipeline, figuras, estadística
├── README.md                                 # Este archivo
├── poster/
│   └── Poster_LP_Ruiz_Salamanca.pptx         # Póster A0 para la presentación
└── figuras/                                  # Figuras exportadas por el notebook (.png)
    ├── fig1C_onda.png
    ├── fig1D_raster_psth.png
    ├── fig2A.png
    ├── fig2B.png
    ├── fig2C.png
    ├── fig2D.png
    └── fig2E.png
```

---

## Datos

Los datos provienen del conjunto **Visual Coding Neuropixels** del Allen Brain Observatory: registros
extracelulares de corteza y tálamo visual de ratón despierto, obtenidos con sondas Neuropixels de 384 canales,
mientras el animal veía distintos estímulos visuales en una pantalla. Las unidades ya vienen ordenadas
(*spike-sorted*) con Kilosort2 y filtradas por criterios de calidad.

El archivo de esta área (`datos.npz`, ~42 MB) **no está incluido en el repositorio** por su tamaño; el
notebook lo descarga automáticamente desde Google Drive en la primera celda.

**Cita de los datos:**
> Siegle JH, Jia X, Durand S, et al. (2021). *Survey of spiking in the mouse visual system reveals functional
> hierarchy.* Nature 592, 86–92.

### Estructura del archivo `.npz`

| Prefijo | Variables | Una fila por |
|---|---|---|
| `u_` | id, ratón, canal, profundidad, snr, amplitude_cutoff, presence_ratio, isi_violations, firing_rate, onda | neurona |
| `e_` | clase, ratón, t0, dur, orientación, frec_temporal, frec_espacial, contraste, color, imagen | ensayo |
| `s_` | unidad, ensayo, t | espiga |

---

## Cómo correr el notebook

1. Abrir `Proyecto_final_LP_Ruiz_Salamanca.ipynb` en Google Colab.
2. Ejecutar **todas las celdas en orden** (`Entorno de ejecución → Ejecutar todas`). La primera celda instala
   `gdown` y descarga el archivo de datos automáticamente — no se necesita ninguna configuración adicional.
3. El notebook corre de principio a fin sin intervención manual y genera las 7 figuras del póster como
   archivos `.png`, además de imprimir el resumen final de resultados (fracciones, intervalos de confianza,
   valor p, d de Cohen).

**Tiempo estimado de ejecución completa:** unos 2–3 minutos en Colab (el paso más lento es el bootstrap
jerárquico de 10,000 réplicas).

### Dependencias

Todas vienen preinstaladas en Colab; si se corre localmente:

```
numpy
pandas
matplotlib
scipy
gdown
```

---

## Pipeline de análisis

El notebook sigue el pipeline de seis pasos del enunciado, de la espiga a la figura:

1. **Cargar** — separar el `.npz` en tres tablas: `neurona`, `ensayos`, `spikes`.
2. **Alinear** — espigas de cada neurona referidas al inicio de cada ensayo.
3. **Contar** — tasa de disparo evocada y basal por neurona y ensayo (`calcular_tasas`), respetando que cada
   neurona solo tiene ensayos de su propio ratón.
4. **Decidir** — Z-score de cada neurona contra su propia basal, usando su combinación de características
   preferida (dirección × frecuencia temporal × frecuencia espacial para movimiento; orientación × frecuencia
   espacial para estáticas).
5. **Población** — mapa de calor de Z-score por estímulo y enjambre de puntos por condición.
6. **Comparar** — fracción de neuronas respondedoras en las dos condiciones asignadas, con intervalo de
   confianza, valor p y tamaño de efecto.

### Métodos estadísticos

- **Umbral de respuesta:** Z ≥ 2.5, validado visualmente revisando rasters de neuronas cerca del corte.
- **Bootstrap jerárquico de tres niveles** (ratón → neurona → ensayo, 10,000 réplicas) para los intervalos de
  confianza del 95%, dado que las neuronas de un mismo ratón no son observaciones independientes.
- **Prueba de permutación** (10,000 permutaciones, pareada por neurona) para el valor p de la diferencia entre
  condiciones.
- **d de Cohen** sobre la diferencia pareada de Z-scores entre condiciones, como tamaño del efecto.

---

## Declaración de uso de IA

Este notebook fue construido con la asistencia de un modelo de lenguaje (Claude, Anthropic) para escribir y
depurar el código del pipeline. Los errores más relevantes que el modelo cometió durante el desarrollo, y que
se corrigieron explícitamente, fueron:

- Un primer intento de bootstrap que remuestreaba solo neuronas, sin el nivel de ratón, lo que habría dado un
  intervalo de confianza falsamente angosto. Se corrigió forzando el sorteo con reemplazo en los tres niveles.
- Cálculos que en un borrador inicial no verificaban que cada ensayo perteneciera al mismo ratón que la
  neurona analizada, lo que habría mezclado animales distintos.
- Una primera versión del PSTH poblacional que filtraba la tabla completa de espigas (>10 millones de filas)
  dentro de un ciclo anidado por neurona, impráctica de ejecutar; se optimizó agrupando las espigas por
  neurona una sola vez antes de los cálculos.

---

## Limitaciones

- Solo 5 ratones: un animal atípico puede desplazar bastante el resultado (la fracción respondedora a
  movimiento varió entre 3.4% y 17.7% según el ratón).
- El umbral de Z = 2.5, aunque validado visualmente, sigue siendo una elección de los autores, no un límite
  biológico exacto.
- No se controló la posición retinotópica del campo receptivo de cada neurona; parte de la diferencia
  observada podría reflejar heterogeneidad espacial de los estímulos y no selectividad al movimiento en sí.
