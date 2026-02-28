links: [[001 - 020 Inteligencia Artificial|Inteligencia Artificial]]


# Quantization
La cuantización en Deep Learning se refiere al proceso de aproximar los **parámetros de un modelo** (pesos y activaciones) utilizando tipos de datos de **menor precisión**, como los enteros de 8 bits (INT8), en lugar de los números de coma flotante de 32 bits (FP32) que se utilizan habitualmente. El objetivo principal es mejorar la eficiencia de los modelos de aprendizaje profundo reduciendo el número de bits necesarios para los cálculos.

En un modelo típico de aprendizaje profundo, cada peso y activación se almacena como un número de coma flotante de 32 bits, lo que resulta caro desde el punto de vista computacional, especialmente en dispositivos con una potencia de procesamiento limitada. La cuantización reduce la precisión de estos pesos y activaciones, lo que comprime el modelo y acelera la inferencia.


---
tags:
	#Concept #MachineLearning #Quantization