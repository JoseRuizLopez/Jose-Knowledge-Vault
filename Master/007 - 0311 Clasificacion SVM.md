links: [[001 - 020 Machine Learning|Machine Learning]]


# Máquinas de Vectores Soporte (SVM)

Las **Máquinas de Vectores Soporte (SVM)** (Support Vector Machines) son un conjunto de algoritmos de aprendizaje supervisado. Su objetivo principal es encontrar un hiperplano óptimo que separe las clases maximizando el margen (la "calle") entre ellas.

Son un caso particular de "Kernel Machines".

## 📌 Conceptos Clave

### Clasificación de Margen Máximo

El objetivo de un clasificador SVM es ajustarse a la "calle" más ancha posible entre las clases.

- **Vectores de Soporte**: Son las instancias de entrenamiento que se sitúan en el borde de la "calle". El límite de decisión está totalmente determinado por estos vectores.
    
- **Sensibilidad a la Escala**: Las SVM son sensibles a las escalas de las características , por lo que es conveniente normalizar los atributos (p.ej., a rangos [-1, 1] o [0, 1]).
    

### Margen Duro vs. Margen Blando

- **Clasificación de Margen Duro**: Impone estrictamente que todas las instancias deben estar fuera de la "calle" y en el lado correcto. Solo funciona si los datos son linealmente separables y es sensible a valores atípicos (_outliers_).
    
- **Clasificación de Margen Blando**: Es un modelo más flexible. Busca un equilibrio entre mantener la "calle" lo más grande posible y limitar las "violaciones del margen" (instancias dentro o al otro lado de la calle).
    
    - Este equilibrio se controla mediante el **hiperparámetro de regularización `C`**.
        
    - Un valor de `C` bajo permite más violaciones (más permisivo con outliers).
        
    - Un valor de `C` alto permite menos violaciones (menos permisivo), pero puede llevar al [[Overfitting|sobreajuste]].
        

## Clasificación No Lineal y el Truco del Kernel

Para manejar conjuntos de datos no lineales, un método consiste en añadir más características (como características polinómicas) para hacer el conjunto de datos linealmente separable.

### El Truco del Kernel (Kernel Trick)

El **truco del kernel** es una técnica matemática que permite obtener el mismo resultado que si se hubieran añadido muchas características polinómicas (incluso de grado muy alto), sin tener que añadirlas realmente.

Esto evita la "explosión combinatoria" del número de características. Un kernel es una función capaz de calcular el producto punto de los vectores transformados basándose únicamente en los vectores originales.

Tipos de Kernels Comunes 

- **Lineal**: $K(a,b)=a^{\top}b$ 
    
- **Polinómico**: $K(a,b)=(\gamma a^{\top}b+r)^{d}$ 
    
- **RBF Gaussiano**: $K(a,b)=\exp(-\gamma||a-b||^{2})$ 
    
    - Este kernel suele funcionar muy bien y es una buena opción por defecto.
        
    - El parámetro `gamma` ($\gamma$) también actúa como regularizador. Valores altos de gamma pueden conducir a [[Overfitting|sobreajuste]].
        
- **Sigmoide**: $K(a,b)=\tanh(\gamma a^{\top}b+r)$ 
    

## Regresión SVM (SVR)

Las SVM también pueden usarse para regresión.

- **Objetivo**: En lugar de separar clases, la regresión SVM intenta ajustar el mayor número posible de instancias _dentro_ de la "calle", limitando las violaciones de margen.
    
- **Hiperparámetro $\epsilon$ (epsilon)**: La anchura de la "calle" se controla mediante el hiperparámetro `epsilon`.
    
- **$\epsilon$-insensible**: El modelo se considera $\epsilon$-insensible porque añadir más instancias de entrenamiento _dentro_ del margen (la calle) no afecta a las predicciones del modelo.
    
- Para tareas de regresión no lineal, también se pueden utilizar SVM kernelizadas.
    

📌 Ventajas y Desventajas

✅ **Ventajas**:

- El entrenamiento es relativamente fácil.
    
- No tiene óptimos locales (a diferencia de las redes neuronales).
    
- Escala relativamente bien a datos de alta dimensionalidad.
    
- Permite usar kernels para datos no tradicionales (cadenas, árboles).
    

❌ **Desventajas**:

- Requiere la elección de una función "kernel" adecuada.
    

---

tags:

#Concept #MachineLearning #SVM

# Clasificación SVM Lineal
Es un plano que separa los datos del superespacio.

El parámetro C si o si hay que modificar lo al usar sckit-learn, siempre hay que ajustarlo. Sirve para ser mas permisivo con los puntos que crucen o estén dentro de la 'calle'. Cuanto mayor sea el valor de C, mas estrictos es.

Son procesos bastantes rápidos.
Se pueden añadir mas características para intentar mejorarlo.

Pero no suele funcionar.


Maximiar el margen equivale en minimizar el w.

# Clasificación SVM no Lineal
## Kernel Polinómico


Con SVC(kernel="linear") podemos elegir entre los 3 Kernels.


