# Mapas de Color en Visión por Computador
*Prof. Jose Francisco Ruiz Muñoz*<br>
*Visión por Computador 2026-I*<br>
*Universidad Nacional de Colombia - Sede de La Paz*<br>
---


## 1. Introducción

En visión por computador, el color no es únicamente un atributo visual, sino una representación matemática de la información capturada por un sensor. Un **mapa de color (colormap)** es una función que asigna valores numéricos a colores.

Formalmente:

[
f: \mathbb{R} \rightarrow \mathbb{R}^3
]

Es decir, un valor escalar puede mapearse a un vector de tres componentes que representan un color.

En procesamiento de imágenes, los mapas de color permiten:

* Visualización científica
* Representación de intensidades
* Segmentación basada en color
* Extracción de características
* Preparación de datos para modelos de aprendizaje profundo

---

## 2. Escala de grises

### 2.1 Definición

Una imagen en escala de grises puede modelarse como una función:

[
I(x,y) \in [0,255]
]

donde:

* 0 representa negro
* 255 representa blanco
* Los valores intermedios representan niveles de intensidad

Desde el punto de vista matricial, una imagen de tamaño ( M \times N ) es:

[
I \in \mathbb{R}^{M \times N}
]

Cada píxel es un escalar que representa intensidad luminosa.

---

### 2.2 Interpretación como tensor

Una imagen en escala de grises puede entenderse como un **tensor de dos dimensiones espaciales**:

[
I \in \mathbb{R}^{M \times N}
]

No existe dimensión adicional asociada al color.

---

### 2.3 Conversión desde RGB

Una imagen RGB puede convertirse a escala de grises mediante una combinación lineal ponderada de los canales:

[
I = 0.299R + 0.587G + 0.114B
]

Estos coeficientes reflejan la sensibilidad del sistema visual humano.

---

### 2.4 Ventajas

* Menor costo computacional
* Menor uso de memoria
* Adecuada para:

  * Detección de bordes
  * Filtrado
  * Operaciones morfológicas
  * Segmentación clásica

---

## 3. Modelo RGB

### 3.1 Definición

El modelo RGB es un modelo aditivo basado en tres canales:

* R (Red)
* G (Green)
* B (Blue)

Un píxel se representa como:

[
I(x,y) = (R,G,B)
]

Cada componente pertenece al intervalo ([0,255]) en imágenes de 8 bits.

---

### 3.2 Representación como tensor

Una imagen RGB puede representarse como:

[
I \in \mathbb{R}^{M \times N \times 3}
]

Es decir:

* Dos dimensiones espaciales
* Una dimensión adicional correspondiente a los canales de color

---

### 3.3 Interpretación geométrica

El espacio RGB puede interpretarse como un cubo en (\mathbb{R}^3):

* (0,0,0) → negro
* (255,255,255) → blanco
* (255,0,0) → rojo puro

Cada color es un punto dentro de ese cubo.

---

### 3.4 Separación de canales

Frecuentemente se trabaja con cada canal por separado:

[
I_R, \quad I_G, \quad I_B
]

Esto permite realizar análisis específicos sobre cada componente.

---

## 4. Comparación: Escala de grises vs RGB

| Característica           | Escala de grises                                                       | RGB                                                                                                                                              |
| ------------------------ | ---------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| Representación tensorial | Tensor de dos dimensiones espaciales ( I \in \mathbb{R}^{M \times N} ) | Tensor de dos dimensiones espaciales y una dimensión adicional correspondiente a los canales de color ( I \in \mathbb{R}^{M \times N \times 3} ) |
| Información              | Intensidad luminosa                                                    | Intensidad luminosa y composición cromática                                                                                                      |
| Costo computacional      | Menor                                                                  | Mayor                                                                                                                                            |
| Uso típico               | Procesamiento clásico de imágenes                                      | Visión por computador moderna y aprendizaje profundo                                                                                             |

---

## 5. Visualización con Matplotlib

### 5.1 ¿Qué es Matplotlib?

Matplotlib es una biblioteca de Python para visualización de datos. Permite:

* Visualizar imágenes
* Dibujar gráficos científicos
* Aplicar mapas de color a datos escalares
* Controlar explícitamente los rangos de intensidad

En visión por computador se usa frecuentemente junto con NumPy y OpenCV para visualizar resultados intermedios.

---

### 5.2 Visualización básica de imágenes

```python
import cv2
import matplotlib.pyplot as plt

img = cv2.imread("imagen.jpg")
img_rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
img_gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

plt.figure(figsize=(10,4))

plt.subplot(1,2,1)
plt.imshow(img_rgb)
plt.title("RGB")
plt.axis("off")

plt.subplot(1,2,2)
plt.imshow(img_gray, cmap="gray")
plt.title("Escala de grises")
plt.axis("off")

plt.show()
```

---

### 5.3 Uso del parámetro `cmap`

Cuando la imagen es un arreglo bidimensional (escala de grises), `imshow()` necesita un mapa de color para interpretar los valores escalares.

Ejemplo:

```python
plt.imshow(img_gray, cmap="gray")
```

Otros ejemplos de colormaps:

```python
plt.imshow(img_gray, cmap="viridis")
plt.imshow(img_gray, cmap="plasma")
plt.imshow(img_gray, cmap="inferno")
```

Aquí:

* `cmap` define la función ( f: \mathbb{R} \rightarrow \mathbb{R}^3 )
* Los valores escalares se transforman en colores según el mapa seleccionado

---

### 5.4 Intervalos de intensidad: `vmin` y `vmax`

Matplotlib permite controlar explícitamente el rango de intensidades que se mapean al colormap mediante:

* `vmin`
* `vmax`

Ejemplo:

```python
plt.imshow(img_gray, cmap="gray", vmin=0, vmax=255)
```

Interpretación:

* Valores ≤ `vmin` se mapean al color mínimo del colormap
* Valores ≥ `vmax` se mapean al color máximo

Esto es fundamental cuando:

* Se visualizan imágenes normalizadas en ([0,1])
* Se comparan varias imágenes con distintos rangos dinámicos
* Se visualizan gradientes o mapas de activación en redes neuronales

---

### 5.5 Normalización automática

Si no se especifican `vmin` y `vmax`, Matplotlib usa:

[
vmin = \min(I), \quad vmax = \max(I)
]

Esto puede producir visualizaciones engañosas cuando se comparan imágenes distintas.

Para análisis científico riguroso es recomendable fijar explícitamente el rango.

---

### 5.6 Barra de color

Para interpretar cuantitativamente un colormap se puede añadir una barra:

```python
plt.imshow(img_gray, cmap="viridis")
plt.colorbar()
plt.show()
```

La barra indica la correspondencia entre valores escalares e intensidades cromáticas.

---

## 6. Otras representaciones de color

Aunque RGB es el modelo más común en adquisición digital, no siempre es el más adecuado para análisis.

### 6.1 HSV (Hue, Saturation, Value)

Descompone el color en:

* Hue: tono
* Saturation: pureza
* Value: brillo

Es útil para segmentación basada en rangos de tono.

---

### 6.2 CIELAB (LAB)

Separación en:

* L: luminosidad
* a: eje verde–rojo
* b: eje azul–amarillo

Es aproximadamente perceptualmente uniforme.

---

### 6.3 Mapas de color científicos

En visualización científica se prefieren colormaps perceptualmente uniformes como:

* viridis
* plasma
* magma
* inferno

Algunos mapas como `jet` no son recomendados debido a discontinuidades perceptuales que pueden inducir interpretaciones erróneas.

---

## 7. Cierre

El color en visión por computador es una representación matemática dependiente:

* Del modelo de color
* Del sensor
* Del espacio vectorial de procesamiento
* Del mapa de color utilizado en la visualización


* Añadir un laboratorio guiado con comparación de distintos `cmap`
* Integrar esto en un notebook estructurado para tu curso de Visión por Computador
