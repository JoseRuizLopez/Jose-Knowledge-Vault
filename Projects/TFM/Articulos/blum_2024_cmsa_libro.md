links: [[103 - TFM]]


# Blum (2024): Construct, Merge, Solve & Adapt. A Hybrid Metaheuristic for Combinatorial Optimization

## Resumen

Monografía de Christian Blum (IIIA-CSIC) que recoge el estado de la investigación sobre CMSA, un algoritmo híbrido para problemas de optimización combinatoria difíciles. La idea central es aplicar de forma iterada un método exacto, normalmente un *ILP solver*, a subinstancias reducidas del problema original. Las subinstancias se construyen con los componentes de soluciones generadas de forma probabilística. El libro tiene 6 capítulos, un apéndice con el código C++ de CMSA para el problema MDS y unas 200 páginas.

**Referencia completa:**
Blum, C. (2024). *Construct, Merge, Solve & Adapt: A Hybrid Metaheuristic for Combinatorial Optimization*. Computational Intelligence Methods and Applications. Springer, Cham. https://doi.org/10.1007/978-3-031-60103-3

ISBN 978-3-031-60102-6 (impreso), 978-3-031-60103-3 (eBook)

Clave BibTeX en la memoria: `blum2024cmsa`

El capítulo 1 tiene su propia nota: [[CMSA-Capitulo1]].

---

## Origen de CMSA

El trabajo empezó en 2015 en la Universidad del País Vasco. Blum observó que la [[Large Neighborhood Search]] (LNS) basada en destruir parcialmente la solución actual se estanca en óptimos locales cuando las soluciones tienen pocos componentes, como en instancias muy restringidas del *multi-dimensional knapsack problem*. CMSA nació como alternativa para esos casos. El artículo original se publicó en 2016 en *Computers and Operations Research* y, según el prefacio, en febrero de 2024 CMSA se había aplicado a 20 problemas distintos.

---

## CMSA estándar (capítulo 1)

### Requisitos

Para aplicar CMSA a un problema hacen falta dos cosas:

1. Una forma probabilística de generar soluciones válidas.
2. Un modelo ILP del problema.

Además hay que definir el conjunto C de componentes de solución, de modo que toda solución válida S sea un subconjunto de C.

### Pseudocódigo

```
S_bsf := ∅ ; C' := ∅ ; age[c] := 0 para todo c ∈ C
mientras no se alcance el límite de tiempo:
    para i = 1..n_a:
        S := ProbabilisticSolutionConstruction(C)
        si f(S) < f(S_bsf): S_bsf := S
        para cada c ∈ S que no esté en C': age[c] := 0 ; C' := C' ∪ {c}
    S_ILP := SolveSubinstance(C', t_ILP)
    si f(S_ILP) < f(S_bsf): S_bsf := S_ILP
    Adapt(C', S_ILP, age_max)
devolver S_bsf
```

### Los cuatro pasos

- **Construct**: se generan n_a soluciones válidas de forma probabilística, normalmente con una heurística voraz aleatorizada.
- **Merge**: los componentes de esas soluciones que no estaban en la subinstancia C' se añaden con edad 0.
- **Solve**: un *ILP solver* busca la mejor solución que solo usa componentes de C', con un límite de t_ILP segundos.
- **Adapt**: la edad de los componentes de C' que no están en S_ILP aumenta en 1, la de los que sí están vuelve a 0, y se eliminan los que alcanzan age_max. Así la subinstancia no crece hasta ralentizar al *solver*.

*Construct* y *Solve* dependen del problema. *Merge* y *Adapt* son genéricos.

### Parámetros

n_a (soluciones por iteración), age_max (edad máxima) y t_ILP (tiempo del *solver* por iteración).

### Aplicación al Minimum Dominating Set (MDS)

Se comparan dos formas de definir los componentes: una intuitiva basada en los vértices (CMSA_INT) y otra genérica válida para cualquier modelo binario, con dos componentes por variable, uno para x_i = 0 y otro para x_i = 1 (CMSA_GEN). Se evalúan frente a CPLEX y a un *greedy* en grafos Erdős-Rényi, Watts-Strogatz y Barabási-Albert.

### Herramientas de evaluación

- **irace**: ajuste automático de parámetros.
- **STNWeb**: comparación visual de algoritmos mediante *Search Trajectory Networks*.
- **scmamp**: paquete de R para comparaciones estadísticas con gráficos de diferencia crítica (CD plots).

---

## Variantes y temas del resto del libro

### Capítulo 2: Self-adaptive CMSA (ADAPT_CMSA)

CMSA estándar es sensible a sus parámetros, y los valores buenos cambian entre grupos de instancias. ADAPT_CMSA elimina las edades: tras cada iteración la subinstancia vuelve a ser la mejor solución encontrada. El tamaño de la subinstancia se regula con dos mecanismos:

- Un parámetro α_bsf que sesga la construcción hacia la mejor solución. Si el ILP se resuelve muy rápido (la subinstancia es pequeña), α_bsf baja para generar soluciones más diversas.
- El número de construcciones por iteración n_a, que crece mientras el ILP no mejora la mejor solución pero la iguala.

Se aplica al **Minimum Positive Influence Dominating Set (MPIDS)**, un problema sobre redes sociales: encontrar el menor conjunto de nodos tal que cada vértice tenga al menos la mitad de sus vecinos en el conjunto. También se aplica al *Far From Most String* (FFMS). Con un único ajuste de parámetros, ADAPT_CMSA funciona en instancias de tamaños distintos y supera a CMSA estándar en las instancias grandes.

### Capítulo 3: Adding Learning to CMSA (LEARN_CMSA)

En CMSA estándar la distribución de construcción no cambia entre iteraciones, así que la calidad media de las soluciones generadas no mejora. LEARN_CMSA introduce aprendizaje integrando un algoritmo bacteriano (un evolutivo) que genera las soluciones que se fusionan. Se aplica a MDS y FFMS. El autor considera que casi cualquier metaheurística poblacional serviría y que el éxito se debe a cómo se combina con CMSA.

### Capítulo 4: Set covering en lugar de modelos de asignación

Sustituye los ILP de tipo asignación por formulaciones de *set covering* para resolver las subinstancias. Se aplica a *Variable-Sized Bin Packing* (VSBP), donde encuentra nuevas mejores soluciones conocidas en 68 de 150 instancias, y a un problema de rutas de vehículos eléctricos (EVRP-TW-SPD) con la variante ADAPT_CMSA_SETCOV.

### Capítulo 5: Variables no binarias

Si el modelo tiene variables enteras no binarias, CMSA puede producir dominios no contiguos (por ejemplo {1, 3}) que los *ILP solvers* no admiten. La solución propuesta es binarizar el ILP. Se ilustra con el *Bounded Knapsack Problem with Conflicts*.

### Capítulo 6: Líneas de investigación adicionales

- **CMSA_GEN**: CMSA independiente del problema para cualquier programa binario, con construcción por redondeo aleatorizado sobre la relajación LP y apoyo opcional de *constraint propagation*.
- **Metaheurística como solver**: usar un *Population-Based Iterated Greedy* (PBIG) en lugar del ILP para resolver las subinstancias, aplicado a *Weighted Independent Domination*.
- **CMSA frente a LNS**: en el *multi-dimensional knapsack problem*, CMSA funciona mejor que LNS cuando las soluciones tienen pocos componentes, y LNS cuando tienen muchos. La explicación propuesta es que LNS conserva siempre parte de la solución y no puede dar saltos grandes, mientras que CMSA genera soluciones en cualquier zona del espacio y las fusiona.
- **Trabajo futuro**: un CMSA genérico que compita con CPLEX o Gurobi, usar *solvers* específicos del problema en lugar de ILP genéricos y añadir *machine learning* (por ejemplo *reinforcement learning* con *multi-armed bandits*) a la construcción.

---

## Relevancia para el TFM

- DCIIM ([[motaz_2026_reformulating_influence_maximization]]) cumple los dos requisitos de CMSA: tiene un modelo ILP y admite una construcción voraz aleatorizada de conjuntos de semillas. CMSA es una de las líneas candidatas para hacerlo escalable.
- Resolver el ILP solo sobre una subinstancia C' de patrones candidatos reduciría el tamaño de la estructura `coverage_count`, que es el cuello de botella identificado en el estudio de escalabilidad ([[Reunion_Cordon_02]]).
- El problema de selección de semillas tiene soluciones pequeñas (k nodos), el caso en el que CMSA supera a LNS según el capítulo 6.
- El capítulo 4 es relevante porque DCIIM ya es un modelo de cobertura de transacciones.
- ADAPT_CMSA y su aplicación a MPIDS son el antecedente más cercano dentro del libro: un problema de influencia en redes sociales resuelto con CMSA.
- irace, scmamp y STNWeb pueden usarse en la parte experimental del TFM.
- Fue recomendado como bibliografía base en [[Reunion_Cordon_02]], con el capítulo 1 como lectura fundamental.

---

## Conceptos clave

- [[CMSA]]
- [[ADAPT_CMSA]]
- [[Large Neighborhood Search]]
- [[Integer Linear Programming]]
- [[Minimum Dominating Set]]
- [[Minimum Positive Influence Dominating Set]]
- [[Set Covering]]
- [[irace]]


---
tags:
	#TFM #CMSA #Metaheuristicas #OptimizacionCombinatoria #Libro
