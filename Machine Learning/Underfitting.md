links: [[001 - 020 Machine Learning|Machine Learning]]

# Underfitting
El subajuste (underfitting) ocurre cuando el modelo es demasiado simple y no logra capturar patrones importantes.

## 🔍 Cómo prevenirlo
Dado que el underfitting se puede detectar a partir del conjunto de entrenamiento, es más útil establecer la relación dominante entre las variables de entrada y salida al principio. Si se logra mantener una complejidad de modelo adecuada, se puede evitar el subajuste y se pueden realizar predicciones más precisas. A continuación, se describen algunas técnicas que pueden servir para reducir el subajuste:

### Disminuir la regularización
La regularización se suele utilizar para reducir la [[Varianza]] con un modelo aplicando una penalización a los parámetros de entrada con los coeficientes más grandes. Existen varios métodos de regularización, como la regularización L1, la regularización de Lasso, el descarte, etc. que ayudan a reducir el ruido y los valores atípicos dentro de un modelo. Sin embargo, si las funciones de datos se vuelven demasiado uniformes, el modelo no puede identificar la tendencia dominante, y se origina el subajuste. Al disminuir la cantidad de regularización, se introduce más complejidad y variación en el modelo, lo que permite un correcto entrenamiento del mismo.

### Aumentar la duración del entrenamiento
Como se ha mencionado previamente, detener el entrenamiento demasiado pronto también puede originar un modelo underfitted. Por lo tanto, podemos evitarlo ampliando la duración del entrenamiento. Sin embargo, es importante reconocer el sobreentrenamiento y, por consiguiente, el sobreajuste. La clave está en encontrar el equilibrio adecuado entre los dos casos.

### Selección de funciones
Con cualquier modelo, se utilizan funciones específicas para determinar un resultado determinado. Si no hay presencia suficiente de funciones predictivas, se deben introducir más funciones o funciones con mayor importancia. Por ejemplo, en una red neuronal se añadirían más neuronas ocultas, y en un bosque aleatorio se añadirían más árboles. Este proceso inyecta más complejidad en el modelo y permite obtener mejores resultados de entrenamiento.


---
tags:
	#Concept #MachineLearning 