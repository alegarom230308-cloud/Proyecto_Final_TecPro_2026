# Proyecto_Final_TecPro_2026
En esta sección aparecen todos los proyectos de Tecnologicas Pro he estado haciendo a lo largo del curso "Introducción a Ciencia de datos". Al ultimo se presentara mi aplicación personal la cual es nombrada como "ElectroApp."
# ElectroApp

*(Proyecto MIT App Inventor identificado internamente como `proyectofinal26` / "El volumen y el magnetismo")*

## ¿De qué se trata?

ElectroApp es una aplicación móvil educativa (STEM) construida en **MIT App Inventor** que investiga si el volumen del sonido reproducido por una laptop influye en la intensidad del campo magnético que esta genera. La app permite comparar datos experimentales tomados de **tres laptops distintas** (Lenovo, Gateway y ASUS), visualizarlos, limpiarlos, ajustarles una línea de regresión y pedirle a una IA generativa que interprete los resultados.

## Palabras clave

`Android` · `MIT App Inventor` · `Ciencia de datos` · `Regresión lineal` · `Campo magnético` · `Electromagnetismo` · `Volumen de audio / decibelios` · `Google Sheets API` · `Detección de anomalías` · `Inteligencia artificial generativa` · `Chatbot` · `Gemini` · `Ollama` · `STEM` · `Visualización de datos`

## Descripción avanzada

La hipótesis del proyecto es que, al aumentar el volumen (decibelios) de una canción reproducida en una laptop, cambia la intensidad del campo electromagnético medible cerca del equipo. Para probarlo, se registraron mediciones en tres laptops de generaciones distintas a distintos porcentajes de volumen, guardadas en una hoja de cálculo de Google Sheets (una pestaña por laptop: `Lenovo`, `Gateway`, `ASUS`).

La app se organiza en un menú principal (`Screen1`) con tres flujos, cada uno correspondiente a una etapa del análisis de datos:

1. **Draw Line of Best Fit** (`drawLOBFscreen`) — importa los datos de la laptop elegida y traza dos gráficas de dispersión (campo magnético promedio, e incertidumbre de la medición, ambas contra el % de volumen), cada una con su línea de mejor ajuste.
2. **Clean Data** (`cleanDataScreen`) — el mismo flujo de gráficas, pero añade un paso de limpieza: detecta automáticamente valores atípicos (anomalías) en los datos y permite eliminarlos tocando el punto directamente en la gráfica.
3. **Make Predictions** (`makePredictionsScreen`) — combina limpieza de datos con un chatbot de IA que analiza la pendiente, el intercepto y el coeficiente de correlación de la regresión, y devuelve una interpretación en lenguaje natural (incluyendo una predicción del comportamiento a otros niveles de volumen).

En las tres pantallas, el usuario elige la laptop con tres botones (Lenovo / Gateway / ASUS), lo que dispara una lectura dinámica de la pestaña correspondiente en Google Sheets y recalcula todo (gráficas, regresión, y en su caso el análisis de IA) para esa laptop específica.

## Características

- **Selector de laptop por botones** (Lenovo, Gateway, ASUS) presente en las tres pantallas, que cambia en tiempo real la fuente de datos leída desde Google Sheets.
- **Visualización de datos** mediante gráficas de dispersión (`ChartData2D`) para campo magnético promedio e incertidumbre de la medición, en función del % de volumen.
- **Línea de mejor ajuste (regresión lineal)** calculada automáticamente, mostrando pendiente (M), intercepto en Y (B) y coeficiente de correlación (R).
- **Limpieza y detección de anomalías**: resalta visualmente los puntos atípicos y permite eliminarlos con un toque, recalculando la gráfica limpia al instante.
- **Análisis con Inteligencia Artificial**: botón que envía los resultados numéricos de la regresión a un chatbot de IA, que devuelve un análisis e interpretación de hasta 120 palabras, incluyendo una predicción a un volumen no medido.
- **Navegación centralizada** con botón "Home" y menú principal con descripción del proyecto.
- **Fuente de datos en vivo**: toda la información se lee de una hoja de cálculo de Google Sheets pública, por lo que se puede actualizar sin volver a compilar la app.

## Modelos implementados

| Modelo | Componente en App Inventor | Función |
|---|---|---|
| Regresión lineal simple (mínimos cuadrados) | `Trendline` | Ajusta una recta `y = Mx + B` a los puntos de cada gráfica y calcula el coeficiente de correlación (R). |
| Detección de anomalías / valores atípicos | `AnomalyDetection` | Identifica puntos que se desvían significativamente del resto de los datos de una gráfica. |
| Modelo de lenguaje generativo (LLM) | `ChatBot` | Genera la interpretación en lenguaje natural de los resultados de la regresión (soporta proveedores Gemini u Ollama). |

## Parámetros de los modelos

**Trendline (regresión lineal)**
- `ChartData`: referencia al `ChartData2D` con los puntos importados (uno distinto por gráfica/pantalla).
- Salidas usadas: `LinearCoefficient` (pendiente M), `YIntercept` (B), `CorrelationCoefficient` (R).

**AnomalyDetection**
- `chartData`: el `ChartData2D` sobre el que se buscan anomalías.
- `umbral` (threshold): `2` — controla la sensibilidad de detección (a mayor valor, más estricta la definición de "atípico").

**ChatBot (análisis con IA)**
- `Provider`: `ollama` (modelo tipo Llama, alojado por MIT App Inventor) — también compatible con `gemini` (Google) cambiando esta propiedad.
- `ApiKey`: clave asociada al proveedor elegido, configurada en el Designer.
- `pregunta` (prompt): construida dinámicamente incluyendo `CorrelationCoefficient`, `LinearCoefficient`, `YIntercept` y todas las entradas de la gráfica limpia, con instrucción explícita de límite de respuesta (120 palabras).

**Fuente de datos (Spreadsheet)**
- `SpreadsheetID`: liga a la hoja de Google Sheets del proyecto.
- `nombreHoja`: dinámico, según el botón de laptop presionado (`Lenovo`, `Gateway` o `ASUS`).
- `columnaX` / `columnaY`: texto exacto de los encabezados de columna (ej. "Porcentanje del volumen V/% ±0.01%", "Promedio electromagnetismo de cada segundo del video.").
- `usarEncabezados`: `verdadero` (las columnas se identifican por el texto del encabezado, no por letra).

## Requisitos previos para la instalación

- Cuenta activa en [MIT App Inventor](https://appinventor.mit.edu) (o instancia compatible) para importar el archivo `.aia`.
- Conexión a internet estable, necesaria para:
  - Leer/escribir en la hoja de Google Sheets vía `Spreadsheet` component.
  - Consultar al chatbot de IA (Gemini u Ollama) vía `ChatBot` component.
- Hoja de cálculo de Google Sheets con **acceso público de lectura** ("Cualquiera con el enlace"), con tres pestañas nombradas exactamente `Lenovo`, `Gateway` y `ASUS`, cada una con:
  - Una sola fila de encabezados en la fila 1.
  - Datos numéricos sin celdas vacías ni texto suelto entre filas.
- `ApiKey` válida configurada en el componente `ChatBot1` para el proveedor elegido (Gemini u Ollama).
- Dispositivo Android con **SDK mínimo 12** (Android 3.1+), o la app **MIT AI2 Companion** instalada para pruebas en vivo desde el editor.
- Componentes/extensiones usados por el proyecto (incluidos de forma nativa en App Inventor, no requieren instalación aparte): `Spreadsheet`, `Chart`, `ChartData2D`, `Trendline`, `AnomalyDetection`, `ChatBot`.
