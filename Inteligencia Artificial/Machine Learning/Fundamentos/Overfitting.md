links: [[001 - 020 Machine Learning|Machine Learning]]

# Overfitting
El sobreajuste (overfitting) es el efecto de sobreentrenar un algoritmo de aprendizaje con unos ciertos datos para los que se conoce el resultado deseado, de esta forma el modelo aprende demasiado bien los detalles y ruido del conjunto de entrenamiento, perdiendo capacidad de generalización en datos nuevos.

## 🔍 Cómo prevenirlo
Métodos adicionales para manejar overfitting incluyen dropout, regularización L1/L2 y reducción de la complejidad del modelo.

- **Cantidad mínima de muestras** tanto para entrenar el modelo como para validarlo.
- **Clases variadas y equilibradas en cantidad**: En caso de aprendizaje supervisado y suponiendo que tenemos que clasificar diversas clases o categorías, es importante que los datos de entrenamiento estén balanceados. Supongamos que tenemos que diferenciar entre manzanas, peras y bananas, debemos tener muchas fotos de las 3 frutas y en cantidades similares.  Si tenemos muy pocas fotos de peras, esto afectará en el aprendizaje de nuestro algoritmo para identificar esa fruta.
- **Conjunto de Test de datos**. Siempre subdividir nuestro conjunto de datos y mantener una porción del mismo “oculto” a nuestra máquina entrenada. Esto nos permitirá obtener una valoración de aciertos/fallos real del modelo y también nos permitirá detectar fácilmente efectos del [[Overfitting]] /[[Underfitting]].
- **Parameter Tunning o Ajuste de Parámetros**: deberemos experimentar sobre todo dando más/menos “tiempo/iteraciones” al entrenamiento y su aprendizaje hasta encontrar el equilibrio.
- **Cantidad excesiva de Dimensiones** (features), con muchas variantes distintas, sin suficientes muestras. A veces conviene eliminar o reducir la cantidad de características que utilizaremos para entrenar el modelo. Una herramienta útil para hacerlo es *PCA*
- Quiero notar que si nuestro modelo es una red neuronal artificial–deep learning-, podemos caer en overfitting **si usamos capas ocultas en exceso**, ya que haríamos que el modelo memorice las posibles salidas, en vez de ser flexible y adecuar las activaciones a las entradas nuevas.

---
tags:
	#Concept  #MachineLearning 