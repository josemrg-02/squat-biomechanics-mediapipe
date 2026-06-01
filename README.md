# 🦵 Análisis Biomecánico de Sentadilla con Visión por Computador

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python&logoColor=white)](https://www.python.org/)
[![MediaPipe](https://img.shields.io/badge/MediaPipe-Pose%20Landmarker-orange?logo=google)](https://developers.google.com/mediapipe)
[![OpenCV](https://img.shields.io/badge/OpenCV-4.x-green?logo=opencv)](https://opencv.org/)
[![Colab](https://img.shields.io/badge/Google%20Colab-ready-F9AB00?logo=googlecolab&logoColor=white)](https://colab.research.google.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Pipeline de análisis biomecánico que detecta automáticamente puntos anatómicos en vídeo lateral de sentadilla y calcula el ángulo de flexión de rodilla a lo largo del ejercicio.

---

## ¿Qué hace?

A partir de un vídeo en vista sagital (lateral), el notebook:

1. **Detecta 33 landmarks corporales** por frame usando *MediaPipe Pose Landmarker* (API moderna, modelo `pose_landmarker_lite`)
2. **Calcula el ángulo de flexión de rodilla** (cadera → rodilla → tobillo) en cada instante
3. **Detecta automáticamente el inicio y fin del movimiento** usando la velocidad angular (derivada del ángulo suavizado), evitando umbrales subjetivos
4. **Clasifica la profundidad** de la sentadilla:
   - Parcial (flexión < 60°)
   - Media profundidad (60–90°)
   - Profunda (90–120°)
   - Muy profunda (> 120°)
5. **Exporta dos outputs:**
   - Gráfica temporal del ángulo con marcadores de inicio, fin y máxima flexión
   - Vídeo anotado con esqueleto superpuesto y ángulo en tiempo real

---

## Outputs de ejemplo

| Gráfica temporal | Vídeo anotado |
|:---:|:---:|
| ![Gráfica ángulo rodilla](assets/resultado_sentadilla.png) | ![Frame del vídeo anotado](assets/frame_anotado.png) |

> *Coloca tus propias capturas en la carpeta `assets/` para personalizar el README.*

---

## Instalación

```bash
pip install mediapipe opencv-python matplotlib numpy
```

O en **Google Colab** (recomendado):

```python
!pip install -q mediapipe
```

El modelo de MediaPipe se descarga automáticamente en la celda 4 del notebook.

---

## Uso

1. Sube tu vídeo a Google Drive
2. Abre el notebook en Colab: [![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)
3. Ajusta los parámetros en la **celda de configuración**:

```python
VIDEO_PATH      = '/content/drive/MyDrive/sentadilla.mp4'  # ruta a tu vídeo
LADO_VISIBLE    = 'derecho'   # 'derecho' o 'izquierdo'
UMBRAL_INICIO   = 5.0         # grados/s, sensibilidad para detectar inicio
SUAVIZADO_VENTANA = 5         # frames para el filtro de media móvil
```

4. Ejecuta todas las celdas (`Runtime → Run all`)

### Outputs generados

| Archivo | Descripción |
|---|---|
| `resultado_sentadilla.png` | Gráfica del ángulo de rodilla vs. tiempo |
| `sentadilla_anotada.mp4` | Vídeo original con esqueleto y ángulo superpuestos |

---

## Resultados en consola

```
=======================================================
RESULTADOS DEL ANÁLISIS DE LA SENTADILLA
=======================================================
Lado analizado:                 derecho
FPS del vídeo:                  30.00
Frames procesados:              312
Inicio del ejercicio:           t = 1.23 s
Fin del ejercicio:              t = 4.87 s
Duración total del ciclo:       3.64 s
Instante de máxima flexión:     t = 3.10 s
Ángulo mínimo de rodilla:       72.4°
Flexión máxima de rodilla:      107.6°
=======================================================
Clasificación: sentadilla profunda (>= 90°)
```

---

## Estructura del proyecto

```
squat-biomechanics-mediapipe/
│
├── analisis_sentadilla_v2.ipynb   # Notebook principal
├── assets/                        # Imágenes para el README
│   ├── resultado_sentadilla.png
│   └── frame_anotado.png
├── README.md
└── LICENSE
```

---

## Stack tecnológico

| Librería | Uso |
|---|---|
| [MediaPipe](https://developers.google.com/mediapipe) | Detección de pose (33 landmarks) |
| [OpenCV](https://opencv.org/) | Lectura/escritura de vídeo, dibujado |
| [NumPy](https://numpy.org/) | Cálculo vectorial, derivadas, suavizado |
| [Matplotlib](https://matplotlib.org/) | Visualización de la señal angular |

---

## Aplicaciones

- **Entrenamiento de fuerza**: feedback objetivo sobre profundidad y técnica
- **Fisioterapia y rehabilitación**: seguimiento de rango de movimiento
- **Investigación biomecánica**: análisis cuantitativo sin equipos especializados
- **Docencia**: demostración de principios de análisis de movimiento humano

---

## Contexto académico

Desarrollado como proyecto práctico en el marco de **Biomecánica II** — Grado en Ingeniería Biomédica, Universidad de Málaga / Università degli Studi di Padova (Erasmus).

---

## Licencia

MIT — libre para uso académico y personal. Ver [LICENSE](LICENSE).
