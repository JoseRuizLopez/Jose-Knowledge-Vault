links: [[001 - 020 Machine Learning|Machine Learning]]`


# Decision Trees

Los **Árboles de Decisión** (Decision Trees) son uno de los modelos más fundamentales y ampliamente utilizados en el _Machine Learning_. Pertenecen a la categoría de **aprendizaje supervisado** y son capaces de realizar tareas tanto de **clasificación** (predecir una categoría) como de **regresión** (predecir un valor numérico).

Su popularidad radica en su **inteligencia y facilidad de interpretación**. Se asemejan a un diagrama de flujo, donde cada nodo interno representa una "pregunta" o prueba sobre una característica (feature), cada rama es el resultado de esa prueba, y cada nodo hoja (terminal) representa la predicción final.

## ¿Cómo Funcionan?

El objetivo de un árbol de decisión es **aprender un conjunto de reglas** a partir de los datos de entrenamiento que le permitan hacer predicciones. Esto lo logra mediante un proceso de **particionamiento recursivo**.

El algoritmo "construye" el árbol haciendo la mejor pregunta posible en cada paso para dividir los datos en subconjuntos que sean lo más **"puros"** posibles con respecto a la variable objetivo.

### 1. El Proceso de "Splitting" (División)

Para decidir qué característica usar para dividir los datos en cada nodo, el algoritmo evalúa la "calidad" de la división. Busca la pregunta que genere los grupos más homogéneos. Para medir esto, se utilizan métricas de **impureza**:

- **Índice de Gini (Gini Impurity):** Se usa comúnmente en tareas de clasificación. Mide la probabilidad de que un elemento, elegido al azar del conjunto, sea clasificado incorrectamente. Un valor de Gini de **0** significa pureza total (todos los elementos pertenecen a la misma clase).
    
- **Entropía (Entropy):** Mide el nivel de "desorden" o incertidumbre en un conjunto de datos. El algoritmo busca maximizar la **Ganancia de Información** (Information Gain), que es la reducción de la entropía después de realizar una división.
    

### 2. Condiciones de Parada

El árbol deja de crecer (de dividirse) cuando se cumple una de estas condiciones:

- Todos los elementos en un nodo pertenecen a la misma clase (pureza total).
    
- Se alcanza la **profundidad máxima** definida (un hiperparámetro para evitar el sobreajuste).
    
- El número de muestras en un nodo es demasiado pequeño para seguir dividiendo.
    

---

## Componentes de un Árbol

La estructura de un árbol de decisión se compone de tres elementos principales:

- **Nodo Raíz (Root Node):** Es el nodo inicial que representa todo el conjunto de datos de entrenamiento.
    
- **Nodos Internos (Internal Nodes):** Son los nodos donde se evalúa una característica (se hace una "pregunta"). De ellos nacen las ramas.
    
- **Nodos Hoja (Leaf Nodes):** Son los nodos terminales del árbol. No tienen más divisiones y contienen la predicción final (la clase más probable o el valor de regresión).
    
- **Ramas (Branches):** Representan el resultado de la evaluación en un nodo interno (ej. "Edad > 30" o "Edad <= 30").
    

---

## Ventajas y Desventajas

Comprender sus pros y contras es clave para saber cuándo utilizarlos.

### ✅ Ventajas

- **Alta Interpretabilidad (Modelo de Caja Blanca):** Son fáciles de visualizar y entender. Las reglas de decisión que genera son explícitas y pueden ser explicadas a personas no técnicas.
    
- **Poco Preprocesamiento de Datos:** No requieren escalado o normalización de características. Pueden manejar datos numéricos y categóricos de forma nativa.
    
- **Selección Implícita de Características:** El propio árbol selecciona las características más relevantes (las que están más cerca de la raíz son, generalmente, las más importantes).
    
- **Rendimiento Eficiente:** Una vez entrenados, la predicción (inferencia) es muy rápida, ya que solo implica seguir un camino de preguntas.
    

### ❌ Desventajas

- **Sobreajuste ([[Overfitting]]):** Es su principal debilidad. Los árboles pueden crecer demasiado y volverse muy complejos, "memorizando" los datos de entrenamiento (incluido el ruido) y perdiendo capacidad de generalización con datos nuevos.
    
- **Inestabilidad:** Pequeños cambios o variaciones en los datos de entrenamiento pueden llevar a la creación de un árbol de decisión completamente diferente.
    
- **Sesgo hacia Clases Dominantes:** En problemas de clasificación desbalanceados, el árbol puede tender a favorecer a las clases mayoritarias. Esto puede mitigarse usando técnicas de [[Balanceo de Clases]].
    
- **Divisiones No Óptimas:** El algoritmo toma decisiones "codiciosas" (greedy) en cada nodo, buscando la mejor división local en ese momento. Esto no garantiza que el árbol resultante sea el mejor a nivel global.
    

---

## Prevención del Sobreajuste: La Poda (Pruning)

Para combatir la tendencia al sobreajuste, se utiliza una técnica fundamental llamada **Poda** (relacionada con el concepto de [[Prunning]]).

1. **Pre-poda (Pre-pruning):** Consiste en detener el crecimiento del árbol _antes_ de que se ajuste perfectamente a los datos. Esto se logra estableciendo hiperparámetros como la *profundidad máxima* del árbol, el *número mínimo de muestras* en una hoja, o el *número mínimo de muestras* necesarias para realizar una división.
    
2. **Post-poda (Post-pruning):** Se deja que el árbol crezca completamente (hasta sobreajustarse) y, posteriormente, se "cortan" las ramas (nodos hoja y sus padres) que no aportan una ganancia de información significativa, basándose en un conjunto de datos de validación.
    

## Evolución: El Poder de los Bosques

Aunque un árbol de decisión individual puede ser débil, su verdadero poder se desata cuando se combinan muchos de ellos en **modelos de ensamblaje (Ensemble Models)**. Los dos métodos más famosos basados en árboles son:

- **Random Forest (Bosque Aleatorio):** Construye múltiples árboles de decisión independientes (cada uno entrenado con un subconjunto aleatorio de datos y características) y promedia sus resultados (o usa una votación) para obtener una predicción mucho más robusta y resistente al overfitting.
    
- **Gradient Boosting (ej. XGBoost, LightGBM):** Construye árboles de forma secuencial, donde cada nuevo árbol se entrena para corregir los errores cometidos por los árboles anteriores.
    

---

tags:

#Concept #MachineLearning #SupervisedLearning