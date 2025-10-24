links: [[001 - 020 Machine Learning|Machine Learning]] | [[River Data Drift]]


# River

**River** es una biblioteca de **Python** diseñada para **aprendizaje automático en flujo de datos** (**[[Online Machine Learning]]**). Se utiliza para entrenar modelos de machine learning de manera **incremental**, lo que significa que estos pueden aprender de datos en **tiempo real** sin necesidad de almacenar grandes volúmenes de información histórica.


## 📌 Características Principales de River

1. **[[Online Machine Learning|Online Learning]]**
    - No requiere cargar todos los datos de una vez, aprende muestra a muestra.
    - Permite entrenar modelos en entornos con flujo de datos constante (**streaming**).

2. **Bajo Consumo de Memoria**
    - Ideal para grandes volúmenes de datos porque no almacena datasets enteros.
    - Usa técnicas como **modelos incrementales** y **resúmenes estadísticos**.

3. **Detección de Data Drift y Concept Drift**
    - Implementa herramientas para detectar cambios en la distribución de los datos en tiempo real.
    - Compatible con algoritmos como **ADWIN** y **Page-Hinkley**, útiles para monitorear cambios en el rendimiento del modelo.

4. **Soporte para Modelos de Clasificación, Regresión y Clustering**
    - Incluye **[[Decision Trees|árboles de decisión]]**, **regresión logística**, **Naive Bayes**, **K-Means**, entre otros.
    - Compatible con **procesamiento de texto** y **[[series temporales]]**.

5. **Integración con Sci-kit Learn y Pandas**    
    - Permite convertir modelos tradicionales en modelos de aprendizaje en línea.
    - Se puede combinar con flujos de trabajo existentes en **Python**.

## Ejemplo de Uso de River

Aquí un ejemplo simple de cómo entrenar un modelo de regresión con datos en tiempo real:

```
from river import linear_model
from river import optim
from river import datasets

# Crear un modelo de regresión lineal
modelo = linear_model.LinearRegression(optimizer=optim.SGD(0.1))

# Obtener datos de ejemplo (dataset en streaming)
data = datasets.TrumpApproval()  # Datos de aprobación de Trump

# Entrenamiento incremental con datos en flujo
for x, y in data:
    modelo.learn_one(x, y)  # Aprende muestra a muestra
    prediccion = modelo.predict_one(x)
    print(f"Predicción: {prediccion}, Real: {y}")
```


## **¿Cuándo Usar River?**

- Cuando necesitas modelos que **aprendan en tiempo real** sin almacenar grandes datasets.
- En entornos con **[[Data Drift]]** o **[[Concept Drift]]**, como detección de fraudes, precios dinámicos, etc.
- Para trabajar con **[[series temporales]]**, analizando datos en streaming sin reentrenar desde cero.

---

###### 🔗 Más Información
- 📌 [Documentación Oficial](https://github.com/online-ml/river)


---
tags:
	#Concept  #MachineLearning #HL #Python