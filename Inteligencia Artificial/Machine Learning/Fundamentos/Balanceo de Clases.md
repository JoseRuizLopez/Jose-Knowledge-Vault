links: [[001 - 020 Inteligencia Artificial|Inteligencia Artificial]]


# Balanceo de Clases
El **balanceo de clases** es una técnica fundamental en *Machine Learning* que busca **contrarrestar la desproporción entre clases** en un conjunto de datos. Este fenómeno, conocido como **desbalanceo de clases**, ocurre cuando una **clase** (mayoritaria) **supera** significativamente en **cantidad** a **otra** (minoritaria), afectando el rendimiento de los modelos predictivos[^1]. Por ejemplo, en contextos médicos como el diagnóstico de enfermedades raras, los datos pueden estar dominados por casos de pacientes sanos frente a los enfermos, sesgando las predicciones hacia la clase mayoritaria[^1].

El **objetivo principal** del balanceo de clases es **equilibrar la distribución de instancias** entre clases, mejorando la capacidad de los algoritmos para generalizar correctamente. Esta técnica es especialmente crítica en aplicaciones donde la precisión en la clase minoritaria es crucial, como en fraud detection o diagnóstico médico[^1].


## Técnicas para Manejar el Desbalanceo de Clases
### Técnicas a Nivel de Datos
Las técnicas a nivel de datos son las más comunes y actúan directamente sobre el conjunto de datos antes del entrenamiento del modelo. Se dividen en tres categorías principales:

#### Upsampling
Este método **agrega instancias a la clase minoritaria** para equilibrarla con la mayoritaria. Existen múltiples enfoques:

- **ROS (Random Oversampling)**: Duplica aleatoriamente instancias de la clase minoritaria, pero puede [[Overfitting|sobreajustarse]] a los datos existentes[^1].
- **SMOTE (Synthetic Minority Oversampling Technique)**: Genera instancias sintéticas en el espacio vecinal de las existentes, evitando la duplicación exacta[^1].
- **ADASYN (Adaptive Synthetic Sampling)**: Combina elementos de SMOTE y ROS, adaptándose dinámicamente a la densidad de datos[^1].


#### [[Subsampling]]
**Elimina instancias de la clase mayoritaria** para lograr equilibrio. Sin embargo, puede perder información relevante y reducir la capacidad de generalización[^1].

#### Métodos Híbridos
Combinan **upsambling** y **subsampling** para optimizar la distribución. Estos métodos buscan **equilibrar sin sacrificar** datos críticos[^1].

### Técnicas a Nivel de Algoritmo
Modifican los algoritmos de clasificación para priorizar la clase minoritaria. Por ejemplo:

- **Clasificadores Costo-Sensitivos**: Asignan pesos distintos a errores de clasificación según la clase[^1].
- **Ensembles**: Combina múltiples modelos para mejorar la robustez ante el desbalanceo[^1].

### Técnicas de Costo Sensitivo
Introducen **matrices de costos que penalizan errores específicos**. Por ejemplo, clasificar erróneamente un paciente enfermo como sano puede tener un costo mayor que el inverso[^1].

## Evaluación del Rendimiento en Modelos Balanceados
El rendimiento de los modelos se evalúa comúnmente mediante métricas como la **área bajo la curva ROC (AUC)**, que mide la capacidad discriminatoria del clasificador. En estudios experimentales, como el realizado para clasificar subtipos de **síndrome de Guillain-Barré (SGB)**, se observó que balancear el dataset mediante SMOTE mejoró significativamente el AUC, alcanzando hasta un 90% de precisión en algunos casos[^1].

### Comparación de Métodos de Balanceo
Un estudio comparativo entre ROS, SMOTE y ADASYN mostró que:

| Método | Ventajas | Limitaciones |
| :-- | :-- | :-- |
| **SMOTE** | Genera instancias sintéticas | Requiere ajuste de parámetros |
| **ROS** | Simple de implementar | [[Overfitting]] a datos duplicados |
| **ADASYN** | Adaptativo a la densidad de datos | Complejidad computacional |

## Consideraciones Prácticas y Limitaciones
### 1. Selección del Método de Balanceo

La elección depende del contexto:

- **SMOTE** es ideal para evitar sobreajuste y mejorar separabilidad[^1].
- **ROS** es rápido pero menos efectivo en datos complejos[^1].
- **ADASYN** se recomienda para datasets de alta dimensionalidad[^1].


### 2. Evaluación de Métricas

Además de AUC, se sugiere usar **precisión**, **recuerdo** y **F1-score** para clases minoritarias, ya que la precisión global puede engañar en datasets desbalanceados[^1].

### 3. Limitaciones de las Técnicas

- Los métodos de sobremuestreo pueden introducir ruido si no se controlan parámetros como el porcentaje de balanceo[^1].
- El submuestreo puede eliminar patrones importantes de la clase mayoritaria[^1].



---
tags:
	#Concept  #MachineLearning #Balanceo-de-clases

[^1]: https://pdfs.semanticscholar.org/c17c/8fdfa1a41a91aee24d4e50af17d4456b0af9.pdf

[^2]: https://pdfs.semanticscholar.org/7717/e95bead815f804c24fb66667ec9b5b3b431c.pdf

[^3]: https://pdfs.semanticscholar.org/fc01/351a8d0a8995a7beb090ca4b15781c07fbee.pdf

[^4]: https://pdfs.semanticscholar.org/9f4e/58d606b0441bda9cface5e9cef7902a756f8.pdf

[^5]: https://pdfs.semanticscholar.org/b487/28026bef6984d8a1b906dd5e50af2884ca3e.pdf

[^6]: https://pdfs.semanticscholar.org/f5f8/9aa81d02cd786db96b6df78dbbede14709ae.pdf

[^7]: https://pdfs.semanticscholar.org/4261/3b020b8a462a30fa14cbe3f614ed9db39495.pdf

