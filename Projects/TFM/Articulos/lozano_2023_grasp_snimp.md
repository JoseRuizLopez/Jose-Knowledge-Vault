links: [[103 - TFM]]


# Lozano-Osorio et al. (2023) — A Quick GRASP-based Method for Influence Maximization in Social Networks

**Referencia completa:**
Lozano-Osorio, I., Sánchez-Oro, J., Duarte, A., Cordón, Ó. (2023). *A quick GRASP-based method for influence maximization in social networks*. Journal of Ambient Intelligence and Humanized Computing, 14, 3767–3779. https://doi.org/10.1007/s12652-021-03510-4

Paper de Óscar Cordón (tutor del TFM) que propone una metaheurística [[GRASP]] (*Greedy Randomized Adaptive Search Procedure*) para resolver el **Social Network Influence Maximization Problem** ([[SNIMP]]). El objetivo principal es minimizar el uso de simulaciones [[Monte Carlo]] en la evaluación del modelo de difusión, logrando soluciones de alta calidad en tiempo reducido. Supera a PSO — considerado el estado del arte según [[banerjee_2020_survey_influence_maximization]] — en todas las instancias del benchmark.

---

## Contribuciones principales

1. **Procedimiento constructivo greedy aleatorizado** con dos funciones heurísticas novedosas ($g_{ne}$ y $g_{cc}$) que no requieren simulación Monte Carlo en la fase de construcción.
2. **Estrategia inteligente de exploración de vecindad** (parámetro $\delta$) en la búsqueda local, que limita el número de evaluaciones costosas del ICM.
3. **Validación estadística rigurosa** con test de Friedman y test de Wilcoxon sobre 35 instancias de benchmark.
4. **Superioridad demostrada** frente a CELF, CELF++ y PSO en calidad de solución y tiempo de cómputo.

---

## Definición del problema

Dada una red social $G(V, E)$ con $|V| = n$ nodos, el SNIMP consiste en seleccionar un conjunto semilla $S \subseteq V$ de tamaño $|S| = k$ que maximice el número de usuarios influenciados:

$$S^* \leftarrow \arg\max_{S \in \mathcal{S}} \; ICM(G, S, p, ev)$$

El SNIMP es **NP-hard**. La evaluación de la función objetivo requiere una simulación [[Monte Carlo]] del [[Independent Cascade Model]] (ICM) — el cuello de botella computacional del problema.

**Parámetros del ICM usados:** $p = 0.01$ (probabilidad de influencia), $ev = 100$ iteraciones Monte Carlo.

---

## Metodología: algoritmo GRASP

[[GRASP]] es un framework *multi-start* que itera dos fases:

### Fase de construcción (Algorithm 2)

Genera una solución inicial sin necesidad de simulación Monte Carlo:

1. Se selecciona aleatoriamente un nodo inicial $v \in V$.
2. Se crea la *Candidate List* (CL) con todos los nodos restantes.
3. En cada iteración se calcula el umbral $\mu = g_{max} - \alpha \cdot (g_{max} - g_{min})$.
4. Se forma la *Restricted Candidate List* (RCL): nodos con $g(v) \geq \mu$.
5. Se selecciona un nodo al azar de la RCL y se añade a $S$.
6. Se repite hasta $|S| = k$.

El parámetro $\alpha \in [0,1]$ controla el balance entre greedy puro ($\alpha = 0$) y aleatorio puro ($\alpha = 1$). La mejor configuración es $\alpha = RND$ (valor aleatorio en cada iteración).

**Dos funciones greedy propuestas:**

- **$g_{ne}$ — two-step neighborhood heuristic**: captura la influencia potencial de un nodo sumando su out-degree más el de sus vecinos directos:
$$g_{ne}(u) = d_u^+ + \sum_{v \in N_u^+} d_v^+$$

- **$g_{cc}$ — clustering coefficient heuristic**: estima la probabilidad de que los vecinos del nodo formen clusters:
$$g_{cc}(u) = \frac{|\{(v,w) \in E : v, w \in N_u^+\}|}{d_u^+ \cdot (d_u^+ - 1)}$$

$g_{ne}$ con $\alpha = RND$ supera consistentemente a $g_{cc}$ y se selecciona como función constructiva final ($C_{ne}$).

### Fase de búsqueda local

Mejora la solución construida explorando la vecindad de *swap moves*:

$$Swap(S, u, v) = S \setminus \{u\} \cup \{v\}, \quad u \in S, \; v \notin S$$

La vecindad completa $N_s(S)$ tiene tamaño $k \cdot (n-k)$, lo que hace inviable la exploración exhaustiva. La **estrategia inteligente de exploración** limita la búsqueda:

- Nodos candidatos a añadir: ordenados en descendente por out-degree.
- Nodos candidatos a eliminar: ordenados en ascendente por out-degree.
- Solo se exploran los $\delta$ mejores candidatos por iteración.
- Estrategia *First Improvement*: se acepta la primera mejora encontrada.

La mejor configuración es $\delta = 20$: máxima eficiencia con 0.56% de desviación media respecto al óptimo encontrado.

**Configuración final del algoritmo GRASP:** función greedy $g_{ne}$, $\alpha = RND$, $\delta = 20$, 100 soluciones construidas.

---

## Experimentos computacionales

### Instancias (SNAP — Stanford Network Analysis Project)

| Instancia       | Nodos   | Aristas |
|----------------|---------|---------|
| WikiVote        | 7,115   | 103,689 |
| CA-AstroPh      | 18,772  | 198,110 |
| CA-CondMat      | 23,133  | 93,497  |
| Cit-HepPh       | 34,546  | 421,578 |
| Email-Enron     | 36,692  | 183,831 |
| p2p-Gnutella31  | 62,586  | 147,892 |
| Email-EuAll     | 265,214 | 420,045 |

Tamaños de seed set: $k \in \{10, 20, 30, 40, 50\}$ → **35 instancias en total**.

### Algoritmos comparados

- **[[CELF]]** (Leskovec et al., 2007): greedy hill-climbing con lazy forward.
- **[[CELF++]]** (Goyal et al., 2011): versión optimizada de CELF.
- **PSO** (Gong et al., 2016): *Particle Swarm Optimization*, estado del arte según [[banerjee_2020_survey_influence_maximization]].

### Resultados principales

| Métrica | GRASP | CELF++ | CELF | PSO |
|---|---|---|---|---|
| Ranking Friedman | **1.00** | 2.44 | 2.79 | 3.77 |
| Tiempo medio (s) | **39.04** | 60.09 | — | — |
| Instancias con mejor resultado | **35 / 35** | — | — | — |

- GRASP obtiene la mejor solución en **todas las 35 instancias**.
- El test de Wilcoxon confirma diferencias estadísticamente significativas ($p < 0.0005$) entre GRASP y cada competidor.
- PSO pierde calidad en instancias grandes (diseñado para redes de 410–15,233 nodos).
- CELF y CELF++ no escalan bien con el tamaño del seed set (tiempo cuasi-constante).

---

## Relevancia para el TFM

Este paper es obra del tutor del TFM (Óscar Cordón) y es la referencia metodológica directa:

- Proporciona el **algoritmo de referencia** ([[GRASP]]) que el TFM puede extender o comparar.
- Demuestra que las heurísticas basadas en propiedades estructurales del grafo ($g_{ne}$, $g_{cc}$) son competitivas sin requerir simulación en la fase de construcción — principio aplicable a métodos basados en [[Community Detection]].
- Los **7 datasets de SNAP** usados aquí son los datasets que se usarán en el TFM para evaluación.
- El benchmark CELF / CELF++ / PSO establece la línea de comparación del TFM.

---

## Ver también

- [[banerjee_2020_survey_influence_maximization]] — survey que este paper cita como referencia de estado del arte
- [[kempe_kleinberg_tardos_2003]] — paper fundacional del área
- [[CELF]] — uno de los algoritmos comparados
- [[CELF++]] — uno de los algoritmos comparados
- [[Independent Cascade Model]] — modelo de difusión usado


---
tags:
	#TFM #InfluenceMaximization #GRASP #Metaheuristica #SNIMP #NetworkScience
