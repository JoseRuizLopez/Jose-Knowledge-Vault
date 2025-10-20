links: [[001 - 020 Machine Learning|Machine Learning]]


# K-Nearest Neighbors (KNN)

**K-Nearest Neighbors (KNN)**, o K-Vecinos más Cercanos, es un algoritmo fundamental de [[001 - 020 Machine Learning|Machine Learning]] supervisado. Es un método **no paramétrico** (no asume una distribución específica de los datos) y de [[lazy learning]].

Su funcionamiento se basa en la premisa de "similitud de características": asume que los puntos de datos que son similares (vecinos) existen en proximidad. Por lo tanto, asigna una etiqueta a un nuevo punto de datos basándose en las etiquetas de sus vecinos más cercanos.

## 📌 Cómo funciona

A diferencia de muchos otros algoritmos, KNN **no tiene una fase de "entrenamiento"** explícita donde aprende una función. Simplemente almacena todo el conjunto de datos. El cálculo real ocurre en el momento de la predicción (o "búsqueda").

El proceso de predicción sigue estos pasos:

1. **Almacenamiento de Datos**: El modelo guarda todas las instancias del conjunto de entrenamiento.
    
2. **Cálculo de Distancia**: Cuando se introduce un nuevo punto (query) para predecir, el algoritmo calcula la distancia entre este punto y _todos_ los puntos de entrenamiento. La elección de la métrica de distancia es crucial (ver sección siguiente).
    
3. **Identificación de Vecinos**: Ordena todas las distancias de menor a mayor e identifica los **'K'** puntos de entrenamiento más cercanos (los "K vecinos").
    
4. **Predicción (Votación o Promedio)**:
    
    - **Para Clasificación**: El algoritmo realiza una "votación mayoritaria". El nuevo punto se asigna a la clase que más se repite entre sus K vecinos.
        
    - **Para Regresión**: El algoritmo calcula el promedio (o a veces la mediana) de los valores de sus K vecinos y asigna ese resultado al nuevo punto.
        

---

## 🔑 Componentes Clave

### El valor 'K'

Es el número de vecinos que se considerarán para la predicción. Es un [[Hiperparámetros|Hiperparámetro]] crítico que debe elegirse con cuidado:

- **K pequeño** (ej. K=1): El modelo es muy flexible y se ajusta mucho al ruido de los datos, lo que puede llevar al [[Overfitting]].
    
- **K grande** (ej. K=N): El modelo se vuelve muy simple (siempre predice la clase/valor mayoritario de todo el dataset), lo que puede llevar al [[Underfitting]].
    

El valor de K suele elegirse usando técnicas como [[Cross Validation]].

### Métrica de Distancia

Define qué tan "cercanos" son dos puntos. La elección depende de la naturaleza de los datos. Las más comunes están definidas en [[Métricas de Similitud]], como:

- **Distancia Euclidiana**: La distancia "recta" entre dos puntos. Es la más común para datos numéricos de baja dimensión.
    
    $$ d(p, q) = \sqrt{\sum_{i=1}^n (p_i - q_i)^2}$$
    
- **Distancia de Manhattan**: La suma de las diferencias absolutas de las coordenadas. Es más robusta ante _outliers_ que la Euclidiana.
    
    $$ d(p, q) = \sum_{i=1}^n |p_i - q_i|$$
    
- **Similitud de Coseno**: Mide el ángulo entre dos vectores. Es muy utilizada en datos de alta dimensión, como en el procesamiento de texto (NLP).
    

---

## ✅ Ventajas y ❌ Desventajas

|**Ventajas**|**Desventajas**|
|---|---|
|✅ **Simple e intuitivo**: Fácil de entender y explicar.|❌ **Coste computacional**: Lento en la predicción, ya que debe comparar el nuevo punto con _todos_ los puntos de entrenamiento.|
|✅ **Sin fase de entrenamiento**: Es un algoritmo _lazy_, los nuevos datos se pueden agregar fácilmente.|❌ **"Maldición de la Dimensionalidad"**: Pierde efectividad a medida que aumenta el número de características (dimensiones).|
|✅ **Versátil**: Funciona tanto para clasificación como para regresión.|❌ **Sensible al escalado**: Requiere [[normalización]] o estandarización de características, ya que las variables con rangos más grandes dominarán las distancias.|
|✅ **No paramétrico**: No hace suposiciones sobre la distribución de los datos.|❌ **Sensible a datos desbalanceados**: En clasificación, la clase mayoritaria tiende a dominar la votación. Requiere técnicas de [[Balanceo de Clases]].|

---

## 💡 Aplicaciones

- **Sistemas de Recomendación**: Sugerir productos basándose en lo que compraron "usuarios similares" (vecinos).
    
- **Búsqueda Semántica**: Encontrar los _embeddings_ (vectores) más cercanos a una consulta de texto o imagen.
    
- **Clasificación de Documentos**: Asignar un tópico a un documento basándose en los tópicos de documentos similares.
    
- **Detección de Anomalías**: Identificar puntos cuyos vecinos más cercanos están muy lejos, sugiriendo que son _outliers_.
    

---

tags:

#Concept #MachineLearning #Algoritmo #Clasificacion #Regresion