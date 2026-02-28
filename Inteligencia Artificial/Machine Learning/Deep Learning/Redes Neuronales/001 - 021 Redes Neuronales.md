links: [[001 - 020 Inteligencia Artificial|Inteligencia Artificial]]

# Redes Neuronales
Las **redes neuronales artificiales** (RNA) son modelos computacionales inspirados en la estructura y funcionamiento del cerebro humano. Están diseñadas para procesar información de manera distribuida y adaptativa, permitiendo el aprendizaje a partir de datos.

## Tipos de Redes Neuronales
- [[001 - 0211 Perceptrón y Perceptrón Multicapa|Perceptrón y Perceptrón Multicapa (MLP)]]
- [[001 - 0212 Redes Neuronales Convolucionales|Redes Neuronales Convolucionales (CNN)]]
- [[001 - 0213 Redes Neuronales Recurrentes|Redes Neuronales Recurrentes (RNN)]]
- [[001 - 0214 Redes LSTM y GRU|Redes LSTM y GRU]]
- [[001 - 0215 Redes Generativas|Redes Generativas (GANs)]]
- [[001 - 0216 Redes Autoencoder|Redes Autoencoder]]
- [[001 - 0217 Redes Transformers|Redes Transformers]]

## Estructura de una Red Neuronal

Una red neuronal está compuesta por **neuronas artificiales** organizadas en capas. Estas capas pueden clasificarse en:

- **Capa de entrada**: Recibe los datos de entrada.
- **Capas ocultas**: Procesan la información mediante funciones de activación.
- **Capa de salida**: Genera el resultado final del modelo.

Cada neurona está conectada con otras mediante **pesos sinápticos**, que determinan la influencia de una neurona sobre otra.
## Funcionamiento de una Neurona Artificial
Cada neurona realiza tres pasos principales:

1. **Suma ponderada**: Se multiplica cada entrada por su peso y se suman los valores.

2. **Aplicación de función de activación**: Se usa una función matemática para decidir si la neurona se activa o no.

3. **Generación de salida**: La neurona envía su resultado a la siguiente capa de la red.


## Funciones de Activación

Las funciones de activación determinan cómo se propaga la información a través de la red. Algunas de las más utilizadas son:

- **ReLU (Rectified Linear Unit)**: $f(x) = max(0, x)$
    
- **Sigmoide**: $f(x) = \frac{1}{1 + e^{-x}}$
    
- **Tangente hiperbólica (Tanh)**: $f(x) = \frac{e^x - e^{-x}}{e^x + e^{-x}}$
    

## Proceso de Entrenamiento

El entrenamiento de una red neuronal implica ajustar los pesos de las conexiones para minimizar el error en las predicciones. Se utilizan algoritmos como:

- **Forward Propagation**: Calcula la salida de la red.
    
- **Cálculo del error**: Se mide la diferencia entre la salida obtenida y la esperada.
    
- **[[Backpropagation]]**: Ajusta los pesos utilizando técnicas de optimización como **[[Descenso de Gradiente]]**.
    

## Evaluación del Modelo

Para medir el rendimiento de una red neuronal, se usan métricas como la **precisión (accuracy)**, el **error cuadrático medio (MSE)** y la **función de pérdida**, que indica qué tan buenas son las predicciones del modelo. [[Métricas de Evaluación|Para más detalles sobre estas métricas]].

## Desafíos y Limitaciones

A pesar de su éxito, las redes neuronales presentan desafíos como:

- **[[Overfitting]]**: Cuando el modelo memoriza los datos en lugar de aprender patrones generales.
    
- **Alto costo computacional**: Requiere gran capacidad de procesamiento y almacenamiento.
    
- **Interpretabilidad**: Puede ser difícil entender cómo toma decisiones un modelo neuronal.
    




---
tags:
	#Concept #RedesNeuronales 