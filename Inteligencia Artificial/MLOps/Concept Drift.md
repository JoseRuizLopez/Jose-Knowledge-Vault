links: [[001 - 020 Inteligencia Artificial|Inteligencia Artificial]]

🔗 References: [Page Reference](https://www.evidentlyai.com/ml-in-production/data-drift#:~:text=Data%20drift%20is%20a%20change%20in%20the%20input%20data.,between%20input%20and%20target%20variables)

# Concept Drift

> 📌 **Nota:** Para entender mejor las diferencias entre **Data Drift** y **Concept Drift**, revisar [[Data Drift vs Concept Drift]].

**Concept Drift** se refiere a los cambios en la distribución de los datos a lo largo del tiempo, lo que impacta la capacidad de un modelo para hacer predicciones precisas.

Cuando un modelo de aprendizaje se entrena sobre un conjunto de datos, **se asume** que la **relación entre las variables de entrada y salida se mantiene estable**. Sin embargo, en **entornos dinámicos**, esta relación **puede cambiar**, lo que lleva a una disminución en el rendimiento del modelo.

## Tipos de Concept Drift

Existen diferentes tipos de **Concept Drift**, que se pueden clasificar de la siguiente manera:

### Sudden Drift (Cambio Brusco)

Este tipo de deriva ocurre cuando el concepto **cambia abruptamente** de un estado a otro. Un ejemplo común es una crisis económica que afecta los patrones de gasto de los consumidores de un día para otro.

### Gradual Drift (Cambio Gradual)

En este caso, la transición de un concepto a otro ocurre de **manera progresiva**. Por ejemplo, la preferencia del usuario por un producto puede cambiar lentamente debido a tendencias en el mercado.

### Incremental Drift (Cambio Incremental)

Se produce cuando el concepto cambia de manera **progresiva a través de múltiples etapas intermedias**. Por ejemplo, un cambio en los hábitos de conducción a medida que se introducen nuevas regulaciones de tráfico.

### Recurrent Drift (Cambio Recurrente)

Ocurre cuando los **conceptos cambian** y luego **regresan a estados anteriores** de forma **cíclica**. Un ejemplo clásico es la demanda de productos estacionales, como ropa de invierno y verano.

## Métodos para Detectar Concept Drift

Detectar la deriva del concepto es fundamental para mantener modelos precisos. Algunos enfoques incluyen:

### Métodos Basados en Estadísticas

Estos métodos analizan cambios en la distribución de los datos de entrada y salida. Ejemplos incluyen:

- **[[Test de Kolmogorov-Smirnov]]**: Detecta diferencias en distribuciones de datos antes y después de un período determinado.
    
- **Kullback-Leibler Divergence**: Mide la diferencia entre dos distribuciones de probabilidad.
    

### Métodos Basados en Error de Predicción

Comparan el error del modelo a lo largo del tiempo para detectar cambios significativos.

- **Page-Hinkley Test**: Monitoriza la media de los errores para detectar cambios abruptos.
    
- **Desviación de Precisión**: Una caída significativa en la precisión del modelo puede indicar Concept Drift.
    

### Métodos Basados en Ventanas de Tiempo

Estos enfoques utilizan ventanas deslizantes para comparar los datos recientes con los históricos.

- **ADWIN (Adaptive Windowing)**: Ajusta dinámicamente el tamaño de la ventana para detectar cambios.
    
- **DDM (Drift Detection Method)**: Basado en la tasa de error, detecta Concept Drift y recomienda actualización del modelo.
    

## 🔍 Cómo solucionarlo

Una vez detectado, existen diversas estrategias para manejar el Concept Drift:

### Reentrenamiento Periódico del Modelo

**Actualizar el modelo** con datos recientes ayuda a adaptarlo a los nuevos patrones.

### Uso de Modelos Adaptativos

Implementar modelos que se **ajusten dinámicamente** a los cambios, como:

- **Online Learning Algorithms** (aprendizaje en línea).
    
- **Ensembles Dinámicos**, que combinan modelos antiguos y nuevos.
    

### Aplicación de Métodos de Pesado

Asignar **mayor peso** a los **datos más recientes** para priorizar la información actualizada.


---
tags:
	#Concept  #MachineLearning #Drift #ConceptDrift