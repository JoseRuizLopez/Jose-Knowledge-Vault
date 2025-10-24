links: [[001 - 020 Machine Learning|Machine Learning]]

# Underfitting

El **Underfitting** (o subajuste) es un problema común en [[001 - 020 Machine Learning|Machine Learning]] donde un modelo es **demasiado simple** para capturar la estructura subyacente o los patrones en los datos de entrenamiento.

A diferencia del [[Overfitting]], que memoriza el ruido, el _underfitting_ ni siquiera logra aprender la relación fundamental. Como resultado, el modelo tiene un **rendimiento deficiente** tanto en los datos de entrenamiento como en los datos de prueba (validación).

Generalmente, un modelo con _underfitting_ tiene un **sesgo alto** (_high bias_).

## Causas Comunes

El _underfitting_ suele ocurrir por las siguientes razones:

- **Modelo demasiado simple**: Elegir un modelo lineal (como la regresión lineal) para modelar datos que tienen una relación compleja y no lineal.
    
- **Insuficientes características (_features_)**: Los datos de entrada no contienen suficiente información o las características correctas para que el modelo pueda hacer una predicción precisa.
    
- **Entrenamiento insuficiente**: Detener el proceso de entrenamiento demasiado pronto, antes de que el modelo haya tenido la oportunidad de aprender los patrones.
    
- **Regularización excesiva**: Aplicar técnicas de regularización (como L1 o L2) con demasiada fuerza, penalizando en exceso la complejidad del modelo y "simplificándolo" demasiado.
    

## Cómo Detectarlo

Detectar el _underfitting_ es relativamente sencillo:

- **Error de entrenamiento alto**: El modelo no es preciso ni siquiera con los datos que "vio" durante el entrenamiento.
    
- **Error de validación/prueba alto**: El error en datos nuevos es también alto, y es muy similar al error de entrenamiento.
    
- **Curvas de aprendizaje**: Las curvas de pérdida (error) tanto de entrenamiento como de validación se estancan en un valor alto y no mejoran.
    

## Cómo Solucionarlo

Para combatir el _underfitting_, el objetivo es aumentar la complejidad del modelo:

1. **Usar un modelo más complejo**:
    
    - En [[001 - 021 Redes Neuronales|Redes Neuronales]], esto significa añadir más capas o más neuronas por capa.
        
    - En regresión, se puede pasar de un modelo lineal a uno polinómico.
        
2. **Ingeniería de Características (_Feature Engineering_)**: Añadir nuevas características o transformar las existentes para que representen mejor el problema.
    
3. **Reducir la regularización**: Disminuir el valor del hiperparámetro de regularización (ej. _alpha_) para permitir que el modelo se ajuste más a los datos.
    
4. **Aumentar el tiempo de entrenamiento**: Entrenar el modelo durante más _epochs_ para darle más tiempo a converger.
    

---

tags:

#Concept #MachineLearning #Underfitting #Bias