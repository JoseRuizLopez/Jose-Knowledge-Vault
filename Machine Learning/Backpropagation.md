links: [[001 - 020 Machine Learning|Machine Learning]]


# Backpropagation
El algoritmo de **backpropagation** ajusta los pesos de la red neuronal para minimizar el error entre la salida predicha y la salida real. 

Se realiza en tres pasos:
- Propagación hacia adelante: La entrada pasa por las capas de la red hasta la salida.
- Cálculo del error: Se mide la diferencia entre la predicción y el valor real usando una función de pérdida (ej. cross-entropy o MSE).
- Propagación hacia atrás (backpropagation): Se calculan los gradientes de la función de pérdida respecto a los pesos usando la regla de la cadena.

Se ajustan los pesos utilizando el [[descenso de gradiente]] (ej. [[Optimizador Adam|Adam]], SGD).




---
tags:
	#Concept #MachineLearning #Backpropagration
	