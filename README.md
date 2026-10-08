<div align="center">

# 🚍 Comparador de Rutas de Transporte Urbano

**Herramienta interactiva para medir cuánto se superponen las rutas de transporte urbano entre sí y frente a los grandes proyectos de infraestructura de la ciudad.**

<br>

<a href="https://comparador-rutas-vj8yq4mjrew7ujxoykq7nd.streamlit.app/">
  <img src="https://img.shields.io/badge/▶%20PROBAR%20LA%20APLICACIÓN-Abrir%20en%20Streamlit-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white" alt="Probar la aplicación" height="45">
</a>

<br><br>

🔗 **https://comparador-rutas-vj8yq4mjrew7ujxoykq7nd.streamlit.app/**

<br>

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white)
![GeoPandas](https://img.shields.io/badge/GeoPandas-139C5A?style=for-the-badge&logo=pandas&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![Shapely](https://img.shields.io/badge/Shapely-6A1B9A?style=for-the-badge&logo=python&logoColor=white)
![Folium](https://img.shields.io/badge/Folium-77B829?style=for-the-badge&logo=leaflet&logoColor=white)
![Plotly](https://img.shields.io/badge/Plotly-3F4F75?style=for-the-badge&logo=plotly&logoColor=white)
![Apache Arrow](https://img.shields.io/badge/PyArrow-D22128?style=for-the-badge&logo=apache&logoColor=white)
![OpenStreetMap](https://img.shields.io/badge/OpenStreetMap-7EBC6F?style=for-the-badge&logo=openstreetmap&logoColor=white)
![GeoJSON](https://img.shields.io/badge/GeoJSON-F7931E?style=for-the-badge&logo=json&logoColor=white)

</div>

---

## 📌 ¿Qué es?

El **Comparador de Rutas** es una aplicación web de análisis geoespacial pensada para planeación de transporte. Permite elegir una ruta de transporte urbano y averiguar rápidamente:

- 🔀 **Qué otras rutas recorren las mismas vías**, y en qué porcentaje.
- 🏗️ **Cuánto de su recorrido coincide con proyectos de infraestructura de la ciudad**: metro, tren regional y nuevas troncales.
- 🧍 **En qué parte del recorrido suben los pasajeros**: al inicio, en la mitad o al final de la ruta.

Con esto se puede detectar **competencia entre rutas**, anticipar el **impacto de las obras futuras** sobre el servicio actual y entender **cómo se comporta la demanda** a lo largo de cada trayecto.

---

## ✨ Funcionalidades

| | Módulo | Descripción |
|:-:|---|---|
| 🗺️ | **Mapa interactivo** | Muestra la ruta de estudio, hasta 5 rutas o proyectos para comparar y la **zona compartida resaltada en rojo**. Tiene zoom libre y control de capas. |
| 📈 | **Distribución por ruta y sentido** | Gráfico de barras apiladas con el % de abordajes en cada tramo del recorrido: **Origen (0–30 %)**, **Intermedio (30–70 %)** y **Destino (70–100 %)**. |
| 📊 | **Tabla comparativa** | Detalle de las rutas seleccionadas con su porcentaje de solapamiento y su operador. |
| 🏗️ | **Competencia con proyectos** | Matriz que cruza las rutas de referencia con los proyectos de la ciudad y resalta en amarillo las coincidencias **mayores al 10 %**. |
| 🏢 | **Competencia por operador** | Para cada ruta de referencia, encuentra la ruta del operador elegido que más se le superpone y resalta los casos **mayores al 50 %**. |
| 📏 | **Métricas clave** | Elementos seleccionados, mayor solapamiento, kilómetros compartidos y longitud total de la ruta de estudio. |

---

## 🧠 ¿Cómo funciona?

### 1. Preparación de geometrías
Los trazados se reproyectan a un sistema de coordenadas **métrico** (`EPSG:3116`) para medir distancias reales en metros y kilómetros, y a **WGS84** (`EPSG:4326`) para dibujarlos en el mapa.

### 2. Cálculo del solapamiento
Alrededor de la ruta de estudio se crea una **franja de influencia de 30 metros** (buffer) que abarca carriles paralelos y paraderos. Después se mide cuántos kilómetros de la otra ruta o del proyecto caen dentro de esa franja:

$$
\text{Solapamiento (\%)} = \frac{\text{km compartidos dentro del buffer}}{\text{longitud total de la ruta de estudio (km)}} \times 100
$$

En el selector solo aparecen las rutas con **más del 5 %** de coincidencia, para que la lista sea manejable.

### 3. Distribución de la demanda
Cada ruta se divide en **tres tramos** según el porcentaje de recorrido. Las validaciones de los pasajeros se asignan a su paradero y luego al tramo que le corresponde:

```
 Origen          Intermedio            Destino
 0% ──────── 30% ──────────────── 70% ──────── 100%
 🟦 captación     🔷 rotación / conexión   🟧 descenso
```

---

## 🗂️ Estructura del proyecto

```
📦 comparador-rutas
 ┣ 📜 app.py                                    → Aplicación Streamlit (archivo principal)
 ┣ 🗺️ Servicios_(Rutas_Troncales_y_Zonales).geojson → Trazados de las rutas de transporte
 ┣ 📂 proyectos_bogota/                         → Trazados de proyectos de infraestructura
 ┃ ┣ Metro_Linea_1.geojson
 ┃ ┣ Metro_Linea_2.geojson
 ┃ ┣ Regiotram_Occidente.geojson
 ┃ ┣ Troncal_Av68.geojson
 ┃ ┗ Troncal_Calle13.geojson
 ┣ 📊 resumen_rutas_sentido.csv                 → Distribución de abordajes ya procesada
 ┣ 📄 CONTEXTO_TECNICO.md                       → Documentación técnica
 ┗ 📋 requirements.txt                          → Dependencias
```

---

## 🚀 Ejecutar en local

```bash
# 1. Clonar el repositorio
git clone https://github.com/anderson-sarmiento-briceno/comparador-rutas.git
cd comparador-rutas

# 2. Crear y activar un entorno virtual
python -m venv .venv
.venv\Scripts\activate        # Windows
# source .venv/bin/activate   # Linux / macOS

# 3. Instalar dependencias
pip install -r requirements.txt

# 4. Iniciar la aplicación
streamlit run app.py
```

La aplicación se abrirá en `http://localhost:8501`.

---

## 💾 Datos pesados (opcional)

El resumen de abordajes (`resumen_rutas_sentido.csv`) **ya viene incluido**, así que la aplicación funciona sin pasos extra.

Si quieres volver a calcularlo con datos nuevos de validaciones, deja el archivo `validaciones_rutas_consolidado.parquet` **fuera del repositorio** por su tamaño y apunta a él con una variable de entorno:

| Variable | Ejemplo |
|---|---|
| `VALIDACIONES_PATH` | `C:/datos/validaciones_rutas_consolidado.parquet` |
| `VALIDACIONES_DIR` | `C:/datos` |
| `TABLA_RESUMEN_PATH` | `C:/datos/resumen_rutas_sentido.csv` |

Orden de búsqueda: variable de entorno → raíz del proyecto → `VALIDACIONES_DIR` → carpeta `data/`.

---

<div align="center">

### 👤 Autor

**Anderson Sarmiento Briceño**

[![GitHub](https://img.shields.io/badge/GitHub-anderson--sarmiento--briceno-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/anderson-sarmiento-briceno)

<br>

⭐ Si este proyecto te resulta útil, ¡dale una estrella al repositorio!

</div>
