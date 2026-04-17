links: [[103 - TFM]]


# Banerjee, Jenamani & Pratihar (2020) — A Survey on Influence Maximization in Social Networks

**Referencia completa:**
Banerjee, S., Jenamani, M., Pratihar, D.K. (2020). *A survey on influence maximization in a social network*. Knowledge and Information Systems, 62, 3417–3455. https://doi.org/10.1007/s10115-020-01461-4

Survey de referencia del área del [[Social Influence Maximization Problem]] (SIM Problem). Cubre de forma sistemática las variantes del problema, los resultados de complejidad computacional y una taxonomía completa de metodologías de solución. Es el paper de estado del arte más citado en el TFM, ya que Lozano et al. (2023) lo usan como benchmark de comparación.

---

## Contribuciones principales

1. **Taxonomía de variantes del problema** TSS (Target Set Selection): 9 variantes formalizadas con su definición matemática.
2. **Revisión de resultados de complejidad** bajo perspectivas de complejidad tradicional y parametrizada.
3. **Identificación de los 5 retos de investigación** principales del área.
4. **Taxonomía de metodologías de solución** en 5 categorías con descripción de ~25 algoritmos.
5. **Comparación experimental** que concluye que PSO (Gong et al., 2016) obtiene los mejores resultados globales — referencia usada por Lozano et al. (2023) como benchmark.

---

## Definición del problema

Dado un grafo dirigido ponderado $G(V, E, \theta, \mathcal{P})$, el **Top k-node Problem** (variante más estudiada) consiste en seleccionar $\mathcal{S} \subseteq V(G)$ con $|\mathcal{S}| = k$ tal que la influencia social esperada $\sigma(\mathcal{S})$ sea máxima.

La función $\sigma(\cdot)$ es **submodular** y **monótona** bajo los modelos IC y LT, lo que permite garantías de aproximación mediante algoritmos greedy.

---

## Modelos de difusión

### [[Independent Cascade Model]] (IC Model)
En cada paso $t$, cada nodo activo $u_i$ intenta activar a sus vecinos inactivos $u_j \in \mathcal{N}^{out}(u_i)$ con probabilidad $p_{ij}$ (peso de la arista). El proceso continúa por cascada hasta que no hay más activaciones. Es el modelo usado en el TFM.

### [[Linear Threshold Model]] (LT Model)
Un nodo inactivo $u_j$ se activa si la suma de los pesos de sus vecinos activos supera su umbral $\theta_j$.

### Otros modelos
Shortest Path Model, Majority Threshold Model (MT), Constant Threshold Model (CT), Unanimous Threshold Model (UT), Weighted Cascade Model, Trivalency Model.

---

## Variantes del problema

| Variante | Objetivo |
|---|---|
| Basic SIM Problem | Encontrar $\mathcal{S}$ con $|\mathcal{S}| \leq k$ tal que $|\sigma(\mathcal{S})| \geq \lambda$ |
| Top k-node Problem | Maximizar $\sigma(\mathcal{S})$ con $|\mathcal{S}| = k$ — *la más estudiada* |
| Influence Spectrum Problem | Resolver Top k-node para todo $k \in [k_{lower}, k_{upper}]$ |
| Multi-Round IM | Seleccionar $T$ conjuntos semilla para $T$ rondas de difusión |
| Target Set Selection | Mínima cardinalidad que activa toda la red |
| $\lambda$ Coverage Problem | Mínima cardinalidad con $\sigma(\mathcal{S}) \geq \lambda$ |
| Budgeted IM (BIM) | Maximizar $\sigma(\mathcal{S})$ con coste de selección $\leq \mathcal{B}$ |
| WTSS Problem | Minimizar coste de selección garantizando influencia total |
| r-round min-TSS | Mínima cardinalidad que activa la red en $r$ rondas |

---

## Complejidad computacional

- El SIM Problem es **NP-hard** e inaproximable en factor $\mathcal{O}(n^{1-\epsilon})$ bajo IC y LT (Kempe et al.).
- El TSS Problem bajo MT Model no es aproximable en factor constante salvo $NP \subseteq DTIME(n^{\text{polylog}(n)})$.
- El TSS Problem es **W[1]-hard** con parámetro *cluster vertex deletion number*.
- El TSS Problem es *fixed-parameter tractable* respecto al parámetro *distance to clique*.

---

## Retos de investigación

- **Trade-off precisión / tiempo**: calcular $\sigma(\mathcal{S})$ exactamente es #P-Complete; se estima con [[Monte Carlo Simulation]].
- **Ruptura de submodularidad**: en *opinion-specific influence maximization*, $\sigma(\cdot)$ puede no ser submodular.
- **Practicidad**: las asunciones del modelo (nodos igualmente importantes, difusión perfecta) no siempre son realistas.
- **Escalabilidad**: las redes reales tienen millones de nodos y miles de millones de aristas.
- **Retos teóricos**: diseño de algoritmos con buenas cotas asintóticas.

---

## Taxonomía de metodologías de solución

### Algoritmos de aproximación (con garantía demostrable)

| Algoritmo | Complejidad | Ventaja principal |
|---|---|---|
| Basic Greedy (Kempe et al.) | $\mathcal{O}(kmnR)$ | Garantía $(1-1/e)$, base teórica |
| [[CELF]] (Leskovec et al.) | $\mathcal{O}(kmnR)$ | Hasta 700× más rápido que Basic Greedy |
| [[CELF++]] (Goyal et al.) | $\mathcal{O}(kmnR)$ | 35–55% más rápido que CELF |
| MIA / PMIA (Chen & Wang) | $\mathcal{O}(\mathcal{R}n + kn\mathcal{R}m)$ | Alta escalabilidad |
| Static Greedy (Cheng et al.) | — | 2 órdenes de magnitud más rápido que Basic Greedy |
| SKIM (Cohen et al.) | $\mathcal{O}(nl + \sum |E^i| + me^{-2}\log^2 n)$ | Alta escalabilidad |
| TIM / TIM+ (Tang et al.) | $\mathcal{O}((k+l)(n+m)\log n/\epsilon^2)$ | 2× más rápido que CELF++ |
| IMM (Tang et al.) | $\mathcal{O}((k+l)(n+m)\log n/\epsilon^2)$ | El más rápido teóricamente |
| Stop-and-Stare (Nguyen et al.) | — | 1200× más rápido que IMM |
| BCT (Nguyen et al.) | $\mathcal{O}((k+l)n\log n/\epsilon^2)$ | Más rápido disponible |

### Soluciones heurísticas

Mejor tiempo y escalabilidad, sin garantía de aproximación: Random Heuristic, Centrality-based (MDH, HCH, Page Rank), Degree Discount Heuristic (DDH), SIMPATH, SPIN, LDAG, IRIE, ASIM, EaSyIm, método de Cordasco et al.

### Soluciones metaheurísticas

GA, GNA, DPSO aplicados al SIM Problem.

### Soluciones basadas en comunidades

CIM, ComPath, INCIM, C-FIM — usan detección de comunidades como paso intermedio para reducir la escala del problema.

---

## Relevancia para el TFM

Este survey es el **punto de referencia bibliográfico central** del TFM:

- Proporciona el estado del arte completo sobre el que se posiciona el trabajo.
- Identifica que PSO (Gong et al., 2016) es el mejor algoritmo en su análisis comparativo, lo que Lozano et al. (2023) usan como motivación para proponer GRASP como alternativa superadora.
- Los modelos IC y LT definidos aquí son los modelos de difusión del TFM.
- La taxonomía de soluciones sirve como mapa conceptual para entender dónde se ubica el enfoque del TFM (métodos basados en comunidades).

---

## Ver también

- [[lozano_2023_grasp_snimp]] — paper que supera el PSO referenciado aquí
- [[kempe_kleinberg_tardos_2003]] — paper fundacional referenciado extensamente
- [[Independent Cascade Model]] — modelo de difusión del TFM
- [[CELF]] — uno de los algoritmos clave comparados


---
tags:
	#TFM #InfluenceMaximization #Survey #NetworkScience #SocialNetworks
