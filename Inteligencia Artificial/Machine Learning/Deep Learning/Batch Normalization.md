links: [[001 - 020 Inteligencia Artificial|Inteligencia Artificial]]

# Batch Normalization (BN)
**Batch Normalization (BN)** es una técnica utilizada en redes neuronales profundas para acelerar el entrenamiento y mejorar la estabilidad del modelo. 

## 📌 ¿Cómo funciona?

Batch Normalization normaliza las activaciones de una capa antes de pasarlas a la siguiente, reduciendo la **varianza interna del covariado** (Internal Covariate Shift). Esto se hace en cuatro pasos:

1. **Cálculo de la media y varianza** de las activaciones dentro de un mini-lote (_batch_).
    $\mu_B = \frac{1}{m} \sum_{i=1}^{m} x_i$
    
    $σ^2_B = \frac{1}{m} \sum_{i=1}^{m} (x_i - \mu_B)^2$
2. **Normalización**: Se reescalan los valores para que tengan media 0 y varianza 1.
	$\hat{x}_I = \frac{x_i - \mu_B}{\sqrt{\sigma_B^2 + \epsilon}}​​$
    
    $\epsilon$ es un valor pequeño para evitar divisiones por cero.
    
3. **Reescalado y desplazamiento** con parámetros aprendibles (γ\gammaγ y β\betaβ). Esto permite que la red pueda ajustar la distribución si es necesario.
    $y_i = \gamma \hat{x}_i + \beta$
4. **Se usa la salida normalizada en la siguiente capa de la red**.


## 📌 ¿Por qué es útil?

✅ **Acelera el entrenamiento**: Reduce la dependencia del ajuste de la tasa de aprendizaje.  
✅ **Evita problemas de saturación** en funciones de activación como Sigmoide o Tanh.  
✅ **Reduce la sensibilidad a la inicialización de los pesos**.  
✅ **Actúa como una regularización** adicional, reduciendo la necesidad de _Dropout_.

## 📌 ¿Dónde se usa Batch Normalization?

- En capas ocultas de redes neuronales profundas.
- En modelos como CNNs y Transformers.
- En entrenamiento con mini-batches.




---
tags:
	#Concept  #MachineLearning