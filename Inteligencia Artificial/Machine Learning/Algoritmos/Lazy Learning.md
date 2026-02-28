links: [[001 - 020 Inteligencia Artificial|Inteligencia Artificial]]


# Lazy Learning

**Lazy Learning** (Aprendizaje Perezoso), también conocido como **aprendizaje basado en instancias**, es un paradigma de [[001 - 020 Inteligencia Artificial|Inteligencia Artificial]] en el que el algoritmo **pospone la mayor parte del cómputo** hasta el momento de la predicción.

A diferencia de otros métodos, un algoritmo _lazy_ **no construye un modelo generalizado** durante la fase de entrenamiento. En su lugar, simplemente **almacena todas las instancias** del conjunto de datos de entrenamiento.

El "aprendizaje" real ocurre cuando se presenta una nueva consulta (un nuevo punto de datos a predecir). En ese momento, el algoritmo utiliza los datos almacenados para realizar la predicción.

## 📌 Eager Learning vs. Lazy Learning

La forma más fácil de entender el _Lazy Learning_ es compararlo con su opuesto, _Eager Learning_ (Aprendizaje Ansioso):

|**Característica**|**Lazy Learning (Perezoso)**|**Eager Learning (Ansioso)**|
|---|---|---|
|**Fase de Entrenamiento**|Rápida. Solo almacena los datos.|Lenta. Construye un modelo (ej. [[Backpropagation]]).|
|**Fase de Predicción**|Lenta. Realiza todo el cálculo al recibir una consulta.|Rápida. Solo aplica la función del modelo aprendido.|
|**Modelo**|No hay un modelo explícito; se basa en instancias.|Se genera un modelo o función generalizada.|
|**Ejemplos**|**[[K-Nearest Neighbors (KNN)|KNN]]**|

---

## ✅ Ventajas y ❌ Desventajas

### Ventajas

- **Adaptabilidad**: Es muy fácil añadir nuevos datos al "modelo", ya que solo implica añadirlos al conjunto de datos almacenado.
    
- **Captura de complejidad local**: Puede modelar fronteras de decisión muy complejas y específicas, ya que se basa en los vecinos locales.
    
- **Sin tiempo de entrenamiento**: La fase de "entrenamiento" es prácticamente instantánea.
    

### Desventajas

- **Predicción lenta**: Es computacionalmente costoso en el momento de la predicción, ya que debe comparar la nueva consulta con todos los puntos de entrenamiento.
    
- **Alto coste de almacenamiento**: Requiere almacenar todo el conjunto de datos de entrenamiento en memoria.
    
- **Sensibilidad al escalado**: Al igual que [[K-Nearest Neighbors (KNN)|KNN]], es muy sensible a la escala de las características y a la "maldición de la dimensionalidad".
    

---

## Ejemplo principal: [[K-Nearest Neighbors (KNN)|KNN]]

El ejemplo por excelencia de _Lazy Learning_ es el algoritmo **[[K-Nearest Neighbors (KNN)|K-Nearest Neighbors (KNN)]]**.

KNN no "aprende" nada durante el entrenamiento; simplemente guarda todos los puntos. Cuando llega un nuevo punto, KNN calcula la distancia a todos los puntos guardados, encuentra los K más cercanos y realiza una votación (en clasificación) o un promedio (en regresión) para generar la predicción. Todo el trabajo se hace "perezosamente" en el momento de la consulta.

---

tags:

#Concept #MachineLearning #Algoritmo