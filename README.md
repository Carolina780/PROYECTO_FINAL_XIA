# ¿Lee la señal o solo ve el borde?
## Auditoría explicable de CNNs para señales de tráfico

---

## ¿Qué hace este proyecto?

En este trabajo entrenamos tres redes neuronales convolucionales sobre señales de tráfico del dataset GTSRB y analizamos qué partes de cada imagen utiliza cada modelo para tomar sus decisiones mediante técnicas de IA explicable (XAI).

La idea principal no es solo medir el accuracy, sino comprobar si los modelos están usando la información correcta de la señal.

Por ejemplo, en una señal de límite de velocidad:
- ¿el modelo realmente lee el número?
- ¿o simplemente detecta un círculo rojo y responde “señal de velocidad”?

Para responder estas preguntas utilizamos cuatro métodos XAI:
- Grad-CAM
- Integrated Gradients
- Oclusión
- LIME

Además, comparamos modelos con distinta complejidad para ver cómo cambia la explicación según la capacidad de la red.

---

## Estructura del repositorio

```text
G6_Alejandra_Carolina/
├── capitulo.ipynb
├── archive.zip
├── readme.md
└── requirements.txt
```

---

## Cómo ejecutar

### 1. Instalar dependencias

```bash
pip install -r requirements.txt
```

### 2. Abrir el notebook

```bash
jupyter notebook capitulo.ipynb
```

También puede abrirse desde JupyterLab, VS Code o cualquier entorno compatible con notebooks Jupyter.

### 3. Ejecutar las celdas

El notebook carga los datos directamente desde `archive.zip`, que está en la carpeta principal del proyecto.

No hace falta descargar nada adicional si el archivo `archive.zip` ya está incluido.

---

## Dataset

- **Dataset:** GTSRB (German Traffic Sign Recognition Benchmark)
- **Número total de clases del dataset:** 43
- **Clases utilizadas en este trabajo:** 12

Las clases seleccionadas permiten estudiar distintos tipos de diferencias entre señales:

| Tipo de diferencia | Clases |
|---|---|
| Diferencia por número | Límite 20, 30, 50, 80 |
| Diferencia por forma | Stop, Ceda el paso |
| Diferencia por símbolo | Obras, Peatones, Precaución |
| Diferencia por dirección | Girar izquierda, Girar derecha |
| Diferencia por restricción | Prohibido paso |

---

## Modelos utilizados

Entrenamos tres CNNs con distinta complejidad para comparar no solo el rendimiento, sino también el tipo de información que aprende cada una.

| Modelo | Características |
|---|---|
| CNN Supersencilla | 1 bloque convolucional y MaxPooling grande |
| CNN Simple | 2 bloques convolucionales + Dropout |
| CNN Compleja | 3 bloques convolucionales + BatchNorm + Dropout + Data Augmentation |

---

## Métodos XAI utilizados

| Método | Qué muestra |
|---|---|
| Grad-CAM | Mapa de calor general sobre las regiones importantes |
| Integrated Gradients | Importancia detallada píxel a píxel |
| Oclusión | Caída de probabilidad al tapar zonas |
| LIME | Regiones o superpíxeles importantes para la predicción |

---

## Casos analizados

1. **Límite 30** — ¿número o borde rojo?
2. **Límite 50 vs Límite 30** — oclusión manual sobre número y borde
3. **Stop** — ¿palabra STOP, forma octogonal o color rojo?
4. **Señales triangulares** — ¿símbolo interno o forma triangular?
5. **Perturbaciones diseñadas** — tapar, desenfocar, oscurecer, escala de grises, ruido y recorte

---

## Resultados principales

Los resultados muestran que los modelos simples tienden a apoyarse más en rasgos generales como el borde o la forma, mientras que los modelos más complejos utilizan mejor los números y símbolos interiores.

También observamos que un modelo puede tener un accuracy alto y aun así usar atajos visuales. Por eso, la explicabilidad permite detectar comportamientos que no se ven solo mirando el porcentaje de acierto.

---

## Requisitos

- Python 3.9+
- TensorFlow 2.12+
- NumPy
- Matplotlib
- scikit-learn
- LIME
- SciPy
- Pillow

Todas las dependencias están incluidas en `requirements.txt`.

---

## Autoras

- Alejandra Llord
- Carolina Romero
