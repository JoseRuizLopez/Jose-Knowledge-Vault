links: [[001 - 020 Machine Learning|Machine Learning]]

# Subsampling
El **subsampling** (submuestreo) es una técnica utilizada para reducir la cantidad de datos en un conjunto de información sin perder información esencial. Se emplea en diversas áreas como el procesamiento de señales, la estadística, el aprendizaje automático y la compresión de datos.

El objetivo principal del subsampling es disminuir el costo computacional y evitar el sobreajuste en modelos de aprendizaje automático, asegurando que los datos representen adecuadamente la información original.

## Métodos de Subsampling

Existen diferentes enfoques de subsampling dependiendo del contexto y los datos que se están procesando:

### Subsampling en Estadística y Ciencia de Datos

Utilizado para trabajar con grandes volúmenes de datos reduciendo su tamaño sin perder representatividad.

- **Muestreo aleatorio simple**: Se selecciona un subconjunto de datos de manera aleatoria.
    
- **Muestreo estratificado**: Se seleccionan datos manteniendo la proporción de clases o categorías.
    

### Subsampling en Aprendizaje Automático

Ayuda a reducir la complejidad del modelo y mejorar su rendimiento.

##### [[Balanceo de Clases]]

Se aplica en problemas de clasificación con clases desbalanceadas. Se reduce la cantidad de muestras de la clase mayoritaria para equilibrar el conjunto de datos.

- **Random Undersampling (RUS)**: Se selecciona aleatoriamente un subconjunto de la clase mayoritaria.
    
- **Cluster-Based Undersampling**: Se agrupan instancias similares y se elige una muestra representativa.
    
- **Tomek Links**: Se eliminan las instancias cercanas en clases opuestas para mejorar la separación de clases.

##### Subsampling en Redes Neuronales Convolucionales (CNNs)

Se implementa a través del **pooling**, donde se reducen dimensiones en mapas de características:

- **Max Pooling**: Se elige el valor máximo de una región.
    
- **Average Pooling**: Se calcula el valor promedio de una región.
    
- **Global Pooling**: Se reduce toda la característica a un solo valor.
    

##### Subsampling en Conjuntos de Datos Masivos

Cuando los datos son demasiado grandes para ser procesados eficientemente, se usa subsampling para reducir su tamaño sin perder representatividad.

- **Muestreo aleatorio simple**: Selección de datos al azar.
    
- **Muestreo basado en densidad**: Se seleccionan datos en función de su distribución en el espacio de características.
    
- **Muestreo en streaming**: Se toman muestras dinámicamente en flujos de datos en tiempo real.

### Subsampling en Compresión de Imágenes

Utilizado en formatos como JPEG para reducir el tamaño de la imagen sin afectar la percepción visual.

- **Submuestreo de crominancia**: Reduce la resolución del color manteniendo la información de luminancia.

## 3. Ventajas y Desventajas del Subsampling

### ✅ Ventajas:

- Disminuye el costo computacional.

- Reduce el riesgo de sobreajuste en modelos de aprendizaje.

- Mejora la eficiencia en el almacenamiento y transmisión de datos.


### ❌ Desventajas:

- Puede perderse información importante si no se aplica correctamente.

- Puede generar sesgos si la muestra no es representativa.

- En algunos casos, puede degradar la calidad de la señal o imagen procesada.



## Aplicaciones del Subsampling

El subsampling se aplica en múltiples áreas, entre ellas:

- **Procesamiento de imágenes y video** (compresión, reducción de ruido, mejora de eficiencia en CNNs).

- **Procesamiento de audio y señales** (reducción de frecuencia de muestreo en sistemas de comunicación).

- **Aprendizaje automático** (reducción de datos para modelos más eficientes y generalizables).

- **Estadística y análisis de datos** (trabajo con grandes volúmenes de datos sin sacrificar representatividad).





---
tags:
	#Concept #MachineLearning #Subsampling #Pooling