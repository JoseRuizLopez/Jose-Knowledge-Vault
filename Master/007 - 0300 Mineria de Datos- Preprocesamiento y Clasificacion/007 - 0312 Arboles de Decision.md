links: [[007 - 0310 Teoria|Teoría]]


# Árboles de Decisión

Los **Árboles de Decisión** son algoritmos de aprendizaje automático versátiles capaces de realizar tareas tanto de clasificación como de regresión. Son algoritmos potentes, capaces de ajustar conjuntos de datos complejos, y son los componentes fundamentales de los _Random Forests_.

Los árboles de decisión son intuitivos y sus decisiones son fáciles de interpretar, por lo que se consideran modelos de **caja blanca** (_white box_).

## 📌 Cómo Funcionan

### Estructura y Predicciones

Un árbol se compone de varios tipos de nodos:

1. **Nodo Raíz (Root node)**: El nodo superior por el que se empieza.
    
2. **Nodo Partido (Split node)**: Un nodo que hace una pregunta (p.ej., "¿longitud del pétalo <= 2.45cm?") y se divide en nodos hijos.
    
3. **Nodo Hoja (Leaf node)**: Un nodo terminal que no tiene hijos y que contiene la predicción final (la clase o el valor).
    

Para hacer una predicción, se recorre el árbol desde la raíz, respondiendo a las preguntas de cada nodo partido, hasta llegar a un nodo hoja.

## Algoritmo de Entrenamiento (CART)

Scikit-Learn utiliza el algoritmo **CART (Classification and Regression Tree)** para entrenar árboles de decisión.

- El algoritmo CART solo produce **árboles binarios** (nodos con exactamente dos hijos).
    
- Funciona dividiendo el conjunto de entrenamiento en dos subconjuntos usando una única característica (k) y un umbral ($t_k$).
    
- Es un **algoritmo _greedy_** (codicioso): busca el par ($k$, $t_k$) que produce los subconjuntos más puros (ponderados por su tamaño) en el paso actual. Al ser _greedy_, suele producir una buena solución, pero no garantiza que sea la óptima global.
    

### Medidas de Impureza (Clasificación)

El algoritmo mide la "pureza" de un nodo. Un nodo es "puro" ($gini=0$) si todas las instancias de entrenamiento a las que se aplica pertenecen a la misma clase.

- **Impureza de Gini** (default en Scikit-Learn):
- $$G_{i}=1-\sum_{k=1}^{n}{p_{i,k}}^{2}$$

    - Donde $p_{i,k}$ es la proporción de instancias de clase _k_ en el nodo _i_.
        
    - Es ligeramente más rápida de calcular que la entropía.
        
    - Tiende a aislar la clase más frecuente en su propia rama.
        
- **Entropía**:
    
    - Mide el desorden o la impureza.
  $$H_{i}=-\sum_{p_{i,k}=1}^{n}p_{i,k}\log_{2}(p_{i,k})$$
	- Tiende a producir árboles ligeramente más equilibrados.
        

## Árboles de Regresión

Los árboles de decisión también son capaces de realizar tareas de regresión.

- En lugar de predecir una clase en cada nodo hoja, predice un valor.
    
- El valor predicho es el **valor objetivo medio** de las instancias de entrenamiento asociadas a ese nodo hoja.
    
- El algoritmo CART funciona de manera similar, pero en lugar de minimizar la impureza (Gini o entropía), divide el conjunto de entrenamiento para minimizar el **Error Cuadrático Medio (MSE)**.
    

## Regularización y [[Overfitting]]

Los árboles de decisión son **modelos no paramétricos**; la estructura del modelo no se determina antes del entrenamiento, sino que se adapta libremente a los datos.

- Si no se imponen restricciones, el árbol se ajustará mucho a los datos de entrenamiento, lo que muy probablemente resulte en **[[Overfitting|sobreajuste]]**.
    
- Para evitar el sobreajuste, es necesario restringir la libertad del árbol durante el entrenamiento. Esto se logra mediante **hiperparámetros de regularización**:
    
    - `max_depth`: Restringe la profundidad máxima del árbol (el valor por defecto `None` es ilimitado).
        
    - `min_samples_split`: Número mínimo de muestras que debe tener un nodo para poder dividirse.
        
    - `min_samples_leaf`: Número mínimo de muestras que debe tener un nodo hoja.
        
    - `max_features`: Número máximo de características a evaluar en cada división.
        
    - `max_leaf_nodes`: Número máximo de nodos hoja.
        

## 📌 Limitaciones

Aunque potentes, los árboles de decisión tienen algunas limitaciones:

- **Sensibilidad a la orientación del eje**: A los árboles de decisión les gustan los límites de decisión ortogonales (paralelos a los ejes). Si los datos están rotados, el límite de decisión puede volverse innecesariamente complejo y no generalizar bien.
    
- **Alta Varianza**: Pequeños cambios en los datos o en los hiperparámetros pueden producir modelos muy diferentes. El algoritmo de entrenamiento también puede ser estocástico (aleatorio), lo que significa que reentrenar con los mismos datos puede dar un modelo diferente.
    

---

tags:

#Concept #MachineLearning #ÁrbolesDeDecisión