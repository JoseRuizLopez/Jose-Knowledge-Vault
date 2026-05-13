links: [[InvestigacionOperativa/Metaheuristicas/RPP]]


# TFG: Metaheurísticas para el Problema de Fijación de Precios Basado en Preferencias (RPP)

Trabajo de Fin de Grado centrado en la resolución del *Rank Pricing Problem* (RPP) mediante [[Metaheuristicas|metaheurísticas]]. El RPP es un problema [[NP-Completo]] de [[OptimizacionCombinatoria|optimización combinatoria]] cuyo objetivo es maximizar el beneficio de una empresa fijando los precios de sus productos en función de las preferencias y presupuestos de sus clientes.

El problema fue definido por primera vez en el contexto de General Motors, que lo utilizó para optimizar la recomendación de vehículos en su web corporativa a partir de las preferencias declaradas por los usuarios.

## Definición formal del problema

Dado un conjunto de productos $I = \{i_1, \ldots, i_n\}$ y un conjunto de clientes $K = \{k_1, \ldots, k_t\}$, cada cliente $k \in K$ tiene un subconjunto de productos preferidos $I^k \subseteq I$, un valor de preferencia $s^k_i$ por cada producto (a mayor valor, mayor preferencia), y un presupuesto fijo $b^{\sigma(k)}$. Cada cliente compra como mucho un producto: el más preferido que pueda permitirse. La función objetivo es:

$$\max_S \sum_{k \in K} \sum_{i \in I^k} p_i x^k_i$$

sujeto a que $p_i \geq 0$ y a que $x^k$ es la solución óptima del subproblema de cada cliente, que maximiza su satisfacción sujeto a su presupuesto. Un resultado clave demostrado en la literatura es que los precios óptimos siempre pertenecen al conjunto de presupuestos $B = \{b^1, \ldots, b^w\}$, lo que reduce considerablemente el espacio de búsqueda.

### Modelos matemáticos

Se analizan dos formulaciones uni-nivel (derivadas de la linealización del modelo bi-nivel):

**Modelo 1** (de Jiménez-Cordero et al., 2024): presenta un fallo en su formulación publicada ya que no restringe las compras al subconjunto de preferencias $I^k$ de cada cliente, produciendo soluciones incorrectas. En la implementación con Gurobi se verificó que el valor que reportan para la instancia `30c_5p` (807) coincide con el modelo correcto, lo que sugiere que su implementación real es correcta pero su formulación escrita tiene una errata.

**Modelo 2** (de Domínguez Sánchez, 2021): modelo correcto, implementado finalmente en [[Gurobi]]. Introduce la variable binaria $v^m_i = 1$ si el producto $i$ tiene precio $b^m$, y la variable $z^k_i$ para el beneficio del cliente $k$ asociado al producto $i$, permitiendo linealizar el modelo completo.

## Metodología

### Gestión del proyecto

Desarrollo mediante [[SCRUM]] con 12 sprints semanales. Stack: Java 21, IntelliJ IDEA, Python (scripts auxiliares), Git/GitHub, VisualVM para profiling y [[Gurobi]] para soluciones exactas. Documentación en Overleaf con LaTeX.

### Instancias utilizadas

Solo existían 3 instancias en la literatura (tamaño máximo 60×50, clientes×productos). Se generaron nuevas instancias mediante un script de Python con presupuestos en el rango [1, 1000], parametrizadas por número de clientes, productos y semilla. El conjunto final abarca instancias de hasta 200×150 (resumen en tabla: mínimo 665 B, máximo ~100 KB, hasta 200 clientes y 150 productos).

### Optimizaciones de rendimiento

VisualVM identificó que `asociarClientes` consumía la mayor parte del tiempo. Las mejoras aplicadas fueron: sustituir la *priority queue* de preferencias por un `ArrayList` ordenado en carga (consultas pasan a $O(1)$), y sustituir un `HashSet` por un `ArrayList` para almacenar qué clientes compran cada producto (método `add` más rápido).

## Algoritmos implementados

**Aleatorio**
Genera un vector de precios $S = (p_1, \ldots, p_n)$ muestreando uniformemente en $[0, M_{\max}]$ y repite $N$ iteraciones guardando el mejor resultado. Sirve de línea de base.

**Aleatorio inteligente**
Igual que el anterior pero muestreando únicamente valores del conjunto de presupuestos $B$, aprovechando el resultado teórico que garantiza que el óptimo pertenece a $B$.

**Voraz**
Para cada producto $i \in I$, asigna como precio el mínimo de los presupuestos de los clientes que tienen $i$ como producto preferido. Criterio simple que maximiza la satisfacción de clientes pero no la función objetivo.

**Voraz OF (*Greedy based on Objective Function*)**
Construye la solución de forma iterativa: en cada paso evalúa todos los pares $(i, b)$ con $b \in B$ para los productos sin precio fijado, calcula el valor de la función objetivo para cada par, y asigna al producto con mejor evaluación el presupuesto correspondiente. Complejidad $O(|I|^2 \cdot |B| \cdot |K|)$. Empates resueltos aleatoriamente en GRASP.

**[[BusquedaLocal|Búsqueda local (LS)]]**
Movimiento: intercambio de precios entre dos productos $i, j \in I$. Dos estrategias evaluadas:
- *Best Improvement* (BI): explora todo el vecindario y acepta el mejor movimiento.
- *First Improvement* (FI): acepta el primer movimiento que mejore la solución actual.

**[[GRASP]] (*Greedy Randomized Adaptive Search Procedure*)**
Metaheurística multi-arranque propuesta por Feo y Resende (1989/1995). Cada iteración consta de:
1. Fase constructiva: genera la *Candidate List* (CL) con el criterio Voraz OF, calcula el umbral $umbral = v_{max} - \alpha \cdot (v_{max} - v_{min})$, filtra la *Restricted Candidate List* (RCL) con candidatos $\geq umbral$, y selecciona aleatoriamente uno de la RCL.
2. Fase de mejora: aplica búsqueda local *First Improvement* sobre la solución construida.
El parámetro $\alpha \in [0,1]$ controla el balance entre voracidad y aleatoriedad: $\alpha = 0$ es prácticamente voraz (con desempate aleatorio), $\alpha = 1$ es puramente aleatorio.

## Resultados

### Soluciones exactas (Gurobi)

Gurobi resuelve instancias $\leq 100 \times 75$ (tiempo medio ~3,5 horas). Instancias de mayor tamaño quedan fuera de alcance por limitaciones de memoria.

### Experimentos preliminares (subconjunto de entrenamiento: 22% de instancias)

| Algoritmo | OF medio | Tiempo (s) | GAP (%) |
|---|---|---|---|
| Aleatorio | 19471,25 | ~0 | 41,93 |
| N-Aleatorio (100 it.) | 22003,75 | ~0 | 33,16 |
| Aleatorio inteligente | 16110,00 | ~0 | 49,97 |
| N-Aleatorio inteligente | 22282,00 | ~0 | 32,96 |
| Voraz | 17294,50 | ~0 | 44,43 |
| **Voraz OF** | **31646,25** | 1,55 | **6,29** |
| Voraz OF + BI | 32240,00 | 1,60 | 4,36 |
| **Voraz OF + FI** | **32322,00** | **1,62** | **4,00** |
| GRASP α=0, 100 it. | 32803,50 | 158,46 | 2,44 |
| **GRASP α=0, 250 it.** | **32919,25** | 401,00 | **2,01** |

El parámetro $\alpha = 0$ resulta ser el mejor (la aleatoriedad proviene del desempate en la RCL). La condición de parada de 250 iteraciones ofrece el mejor equilibrio calidad/tiempo: la mejora de 100 a 250 es de 0,43% de GAP, mientras que de 250 a 500 la mejora adicional (0,05%) no justifica doblar el tiempo.

### Experimento final: GRASP vs. estado del arte (VNS)

| Instancia | GRASP OF | GRASP GAP (%) | VNS OF | VNS GAP (%) |
|---|---|---|---|---|
| 30×5 | 807 | 0,00 | 807 | 0,00 |
| 30×25 | 1026 | 1,54 | 960 | 7,87 |
| 60×50 | 1973 | 2,18 | 1842 | 8,68 |
| **Promedio** | **1268,67** | **1,24** | 1203,00 | 5,52 |

GRASP supera al VNS del estado del arte tanto en calidad de solución como en tiempo de ejecución (23,80 s vs. 600 s de tiempo límite del VNS).

Sobre el conjunto completo de instancias con óptimo conocido, GRASP obtiene una desviación media del 2,5% y solo 1 solución óptima (la instancia más pequeña), pero en un tiempo medio de 346 s frente a las ~12.465 s de Gurobi. Además, GRASP es capaz de resolver instancias de hasta 200×150 que Gurobi no puede abordar por memoria.

## Conclusiones

Se desarrolló un sistema completo capaz de importar instancias RPP, resolverlas con múltiples algoritmos y exportar resultados, con arquitectura modular basada en una interfaz `Algoritmo`. Se implementaron satisfactoriamente todos los algoritmos propuestos y se generó un nuevo conjunto de instancias que cubre tamaños no presentes en la literatura. La metaheurística GRASP resultó ser competitiva frente al VNS del estado del arte, con mejor calidad y mucho menor tiempo de ejecución.

## Trabajo futuro

- Optimización adicional del GRASP para reducir tiempo en instancias grandes.
- Reimplementación del VNS de Jiménez-Cordero et al. para comparativa directa.
- Aplicación de los métodos desarrollados a variantes del problema: [[RPPT]] (clientes con empates en preferencias) y [[CRPP]] (stock finito de productos).
- Estudio de otras metaheurísticas para el RPP y comparativa sistemática.


---
tags:
	#Metaheuristicas #OptimizacionCombinatoria #GRASP #PricingProblems #TFG
