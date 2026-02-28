links: [[001 - 020 Inteligencia Artificial|Inteligencia Artificial]]


# Métricas de Evaluación
Las métricas de evaluación son esenciales para medir el rendimiento de un modelo de aprendizaje automático. Permiten determinar si un modelo es preciso, eficiente y generalizable a nuevos datos.

## Métricas para Modelos de Clasificación

Los modelos de clasificación predicen etiquetas categóricas y se evalúan con métricas como:

- **Precisión (Accuracy)**: Proporción de predicciones correctas sobre el total de muestras.
    
- **Precisión (Precision)**: Mide cuántas de las predicciones positivas fueron realmente correctas.
    
- **Recall**: Mide cuántos de los casos positivos reales fueron detectados por el modelo.
    
- **Puntuación F1 (F1-score)**: Promedio armónico entre precisión y recall.
    
- **Matriz de confusión**: Tabla que muestra la cantidad de verdaderos positivos (TP), falsos positivos (FP), verdaderos negativos (TN) y falsos negativos (FN).
    
- **AUC-ROC (Área bajo la curva ROC)**: Indica la capacidad del modelo para distinguir entre clases.
    

## Métricas para Modelos de Regresión

Los modelos de regresión predicen valores numéricos y se evalúan con métricas como:

- **Error cuadrático medio (MSE - Mean Squared Error)**:
    
- **Raíz del error cuadrático medio (RMSE - Root Mean Squared Error)**:
    
- **Error absoluto medio (MAE - Mean Absolute Error)**:
    
- **Coeficiente de determinación (R² - R squared)**: Indica qué tan bien el modelo explica la variabilidad de los datos.

## Métricas para Modelos de Procesamiento de Lenguaje Natural (NLP)

En tareas de NLP, las métricas ayudan a evaluar la calidad de las predicciones textuales. Algunas de las más utilizadas son:

- **[[BLEU]]:** Mide la similitud entre un texto generado y una referencia humana, utilizando la coincidencia de n-gramas. 
    
- **[[ROUGE]]:** Evalúa la superposición de palabras y frases entre textos generados y referencias humanas, especialmente en resúmenes automáticos.

## Función de Pérdida

La función de pérdida es una medida de qué tan mal está prediciendo un modelo. Se usa para ajustar los parámetros durante el entrenamiento y minimizar el error. Algunas funciones de pérdida comunes incluyen:

- **Para clasificación**:
    
    - **Entropía cruzada (Cross-Entropy Loss)**: Se usa en clasificación multiclase.
        
    - **Hinge Loss**: Se usa en máquinas de soporte vectorial (SVM).
        
- **Para regresión**:
    
    - **Error cuadrático medio (MSE)**: Penaliza los errores grandes de manera más fuerte.
        
    - **Error absoluto medio (MAE)**: Menos sensible a valores atípicos.
        

## Selección de Métricas Adecuadas

- Para **clasificación desbalanceada**, usar **F1-score y AUC-ROC** en lugar de solo precisión.
    
- Para **regresión con valores extremos**, **MAE** es más robusto que **MSE**.
    
- Para **modelos con múltiples clases**, considerar métricas ponderadas como **macro-F1 y micro-F1**


---
tags:
	#Concept  #MachineLearning #Metricas 