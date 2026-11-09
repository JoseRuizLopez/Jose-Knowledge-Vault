# Preparación de la reunión con los tutores — 9 de noviembre de 2026

TFM: estudio de escalabilidad de DCIIM y propuesta de un método escalable.

**Objetivo de la reunión:** presentar los resultados del estudio de escalabilidad y **decidir la línea de la propuesta propia (CMSA o GRASP)**.

---

## 0. Mensaje central (para abrir la reunión)

> DCIIM obtiene igual o mejor calidad que Greedy, HCIM y α-HCIM en redes pequeñas y medianas, y con un tiempo de CPU varios órdenes de magnitud menor. Sin embargo, escala de forma aproximadamente **cuadrática en N**, y el motivo **no es el ILP ni la simulación Monte Carlo, sino la construcción del modelo en Python** (`coverage_count` y la generación de restricciones). Por tanto, la propuesta escalable debe **reducir el tamaño de la instancia o el coste de construir el modelo**.

---

## Glosario de notación y términos

### Problema y modelo de difusión

| Símbolo / término | Significado |
|---|---|
| **IM** (Influence Maximization) | Problema de elegir los k nodos de una red que, activados al inicio, maximizan la difusión esperada. Formulado por Kempe, Kleinberg y Tardos (2003); es NP-difícil. |
| **G = (V, E)** | Grafo de la red social. V es el conjunto de nodos y E el de aristas. |
| **N** = \|V\| | Número de nodos. Es la medida principal del tamaño de la red. |
| **E** = \|E\| | Número de aristas. |
| **k** | Número de semillas a seleccionar, es decir, el tamaño del conjunto S. Valores usados: 3, 4, 5, 10, 15 y 20. |
| **S** | Conjunto semilla: los k nodos elegidos para iniciar la difusión. |
| **σ(S)** | Influencia esperada de S: número medio de nodos que acaban activados cuando la difusión parte de S. Es la **métrica de calidad**; cuanto mayor, mejor. |
| **IC** (Independent Cascade) | Modelo de difusión de Cascada Independiente. Cada nodo recién activado tiene una única oportunidad de activar a cada vecino, con probabilidad p. |
| **p** | Probabilidad de propagación por arista en el modelo IC. Fijada en 0,1. |
| **mc** | Número de simulaciones Monte Carlo para estimar σ(S). Como IC es aleatorio, se simula mc veces y se promedia. Fijado en 100. |
| **Monte Carlo** | Técnica de estimación por repetición de simulaciones aleatorias. Aquí se usa para calcular σ(S). |

### Formulación de DCIIM

| Símbolo / término | Significado |
|---|---|
| **DCIIM** | *Descriptive Clustering-based Influence Maximization*: el método de Motaz que reformula IM como clustering descriptivo resuelto con un ILP. |
| **Clustering descriptivo** | Agrupamiento en el que cada grupo se describe con un patrón interpretable. En DCIIM, cada semilla «describe» el grupo de nodos que cubre. |
| **Transacción (T)** | Representación de un nodo como el conjunto formado por él mismo y sus vecinos. La red completa se convierte en una base de N transacciones. |
| **Patrón** | Conjunto de nodos candidato a describir transacciones. En la versión usada, cada patrón es un único nodo u (patrones de un nodo). |
| **cover(T, P)** | Función que comprueba si el patrón P cubre la transacción T, es decir, si P ⊆ T. Con patrones de un nodo equivale a comprobar u ∈ T. |
| **coverage_count** | Número de transacciones que cubre cada patrón. Se calcula llamando a `cover()` sobre todas las transacciones. Es la fase más costosa. |
| **Σ\|T\|** | Suma de los tamaños de todas las transacciones (≈ 2E + N). Mide el volumen de datos que recorre `cover()`. |
| **ILP** (Integer Linear Programming) | Programa lineal entero: optimización con variables enteras (aquí binarias: elegir o no un patrón) y restricciones lineales. DCIIM elige los k patrones resolviendo un ILP. |
| **φ / overlapallowance** | Máximo número de patrones elegidos que pueden cubrir una misma transacción. Controla cuánto pueden solaparse las zonas de influencia de las semillas. Si es demasiado pequeño, el ILP no tiene solución. |
| **Infactible** | El ILP no tiene ninguna solución que cumpla todas las restricciones. En ese caso DCIIM incrementa φ y vuelve a intentarlo. |
| **n_iter_dciim** | Número de veces que DCIIM tuvo que incrementar φ hasta que el ILP fue factible. |
| **--max-iter** | Límite de incrementos de φ que he añadido (por defecto 10). Con 0, φ queda fijo. |
| **Solver** | Programa que resuelve el ILP. **CBC** es el solver libre que trae PuLP; **Gurobi** es comercial (licencia académica). |
| **PuLP** | Librería de Python con la que se escribe el ILP y se llama al solver. |

### Tiempos medidos y estados de ejecución

| Símbolo / término | Significado |
|---|---|
| **t_coverage** | Tiempo de calcular `coverage_count`. |
| **t_build** | Tiempo de construir las restricciones del ILP. Se calcula como residuo: t_total − t_coverage − t_ilp − t_ic. |
| **t_ilp** | Tiempo que tarda el solver en resolver el ILP (`prob.solutionTime`). |
| **t_ic** | Tiempo de la simulación Monte Carlo para evaluar σ(S). |
| **t_total** | Tiempo de pared total de la ejecución para un k. |
| **CPU** (en las tablas) | Tiempo de ejecución del método en segundos. |
| **optimal** | El ILP se resolvió de forma óptima dentro del tiempo. |
| **timeout / tout** | La ejecución superó el tiempo límite y no dio resultado. |
| **global_timeout** | Se superó el `--global-timeout`, que cubre todas las fases (cobertura, ILP y reintentos), no solo el solver. |
| **Timeout 300 s / 1800 s** | Presupuestos de tiempo usados: 300 s (5 min) en el bloque D y 1800 s (30 min) en los bloques DL y E. |
| **ram_peak_mb** | Memoria máxima usada por el proceso, en MB. |

### Escalabilidad y análisis

| Símbolo / término                      | Significado                                                                                                                                         |
| -------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Escala log-log**                     | Gráfica con ambos ejes en logaritmo. Si t ∝ N^a, aparece como una recta de pendiente a.                                                             |
| **Pendiente (slope)**                  | Exponente a del ajuste t ∝ N^a. Una pendiente ≈ 2 indica que duplicar N multiplica el tiempo por ~4.                                                |
| **O(N²)**                              | Notación de complejidad: el coste crece con el cuadrado de N.                                                                                       |
| **O(E)**                               | El coste crece linealmente con el número de aristas. Es el objetivo si se vectoriza la construcción del modelo.                                     |
| **W = N·(2E + N)**                     | Medida de carga del modelo de coste: N patrones por Σ\|T\| ≈ 2E + N elementos recorridos.                                                           |
| **E**                                  | Es el número de aristas del grafo.                                                                                                                  |
| **N**                                  | Es el número de nodos del grafo.                                                                                                                    |
| **t ∝ W^0,98**                         | El tiempo de construcción es prácticamente proporcional a W (exponente ≈ 1).                                                                        |
| **Cuello de botella**                  | Fase que concentra la mayor parte del tiempo y limita la escalabilidad.                                                                             |
| **Vectorizar**                         | Sustituir bucles de Python por operaciones sobre matrices (NumPy / `scipy.sparse`), mucho más rápidas.                                              |
| **Matriz de adyacencia + I**           | Matriz N×N con 1 donde hay arista, más la identidad (cada nodo se cubre a sí mismo). Equivale a las filas de restricciones con patrones de un nodo. |
| **Réplica / semilla aleatoria (seed)** | Repetición del experimento con otra semilla del generador aleatorio, para medir la variabilidad.                                                    |

### Datos y grafos

| Símbolo / término | Significado |
|---|---|
| **LFR** | Generador de grafos sintéticos de Lancichinetti–Fortunato–Radicchi, con estructura comunitaria controlada. Permite variar N de forma limpia. |
| **μ (mu)** | Parámetro de mezcla de LFR: fracción de aristas de cada nodo que salen de su comunidad. μ bajo da comunidades muy definidas; μ alto, comunidades difusas. |
| **ad5, mc50** (en nombres LFR) | `average_degree = 5` (grado medio) y `min_community = 50` (tamaño mínimo de comunidad). Otros parámetros: τ1 = 3 (exponente de la distribución de grados) y τ2 = 1,5 (exponente de tamaños de comunidad). Ver `generate_lfr.py`. |
| **SNAP** | Stanford Network Analysis Project: repositorio de redes reales grandes (CA-AstroPh, Cit-HepPh, Email-Enron, etc.). |
| **Bloques A–E, DL** | Grupos de experimentos del diseño factorial fraccionado (ver sección 2). |

### Métodos comparados

| Método | Significado |
|---|---|
| **Greedy** | Algoritmo voraz de Kempe et al.: añade en cada paso el nodo que más aumenta σ(S). Buena calidad (garantía 1 − 1/e), pero muy lento porque evalúa σ con Monte Carlo para cada candidato. |
| **HCIM** | Método basado en comunidades usado como baseline en el paper de Motaz: (1) pondera cada arista por la similitud de vecindarios de sus extremos (vecinos comunes normalizados), (2) detecta comunidades y (3) reparte las k semillas entre ellas. |
| **α-HCIM** | Variante de HCIM en la que el peso de arista combina dos similitudes: (1 − α)·vecinos comunes + α·densidad de aristas entre los vecinos comunes. Se usa α = 1, es decir, solo el segundo término. |
| **CMSA** | *Construct, Merge, Solve & Adapt* (Blum): metaheurística híbrida que construye soluciones, fusiona sus componentes en una subinstancia reducida y la resuelve con un solver exacto. |
| **GRASP** | *Greedy Randomized Adaptive Search Procedure*: metaheurística de construcción greedy aleatorizada seguida de búsqueda local. |

---

## 1. Contexto

### 1.1. Qué es DCIIM
DCIIM es el método de Motaz (repositorio público `2x254/IPMU_RegularPaper`, artículo *Reformulating Influence Maximization through Descriptive Clustering*). Reformula la maximización de influencia (IM) como un problema de **clustering descriptivo** y lo resuelve con un **programa lineal entero (ILP)**:

1. A partir del grafo se construye una representación transaccional: cada nodo genera una transacción con su vecindario.
2. Se calcula la cobertura de cada patrón candidato (`coverage_count`, que usa la función `cover()`).
3. Se construyen las restricciones del ILP. `overlapallowance` (φ) limita cuántos patrones pueden cubrir una misma transacción.
4. El solver (CBC o Gurobi) elige los k patrones o semillas.
5. Si el ILP es infactible, se incrementa `overlapallowance` y se repite.
6. La calidad del conjunto semilla S se evalúa con σ(S) bajo el modelo de Cascada Independiente (IC) mediante Monte Carlo.

### 1.2. Qué he aportado hasta ahora
El código algorítmico de DCIIM y de los baselines **no se ha modificado**: los tiempos medidos corresponden al código original. Lo que he añadido es el **arnés experimental**:

- **CLI con argparse** en todos los métodos: dataset, k, solver, semilla, p, mc, timeouts y salida CSV.
- **Medición separada por fases:** `t_coverage`, `t_build` (construcción de restricciones), `t_ilp` y `t_ic`.
- `--max-iter N`: límite de incrementos de `overlapallowance` (por defecto 10; con 0, φ queda fijo, que es lo que usa el estudio de φ).
- `--global-timeout S`: límite de tiempo de pared por k que cubre cobertura, ILP y reintentos. Las ejecuciones que lo superan se registran con `status=global_timeout`.
- `im_utils.py`: carga común de datasets, **un evaluador σ(S) común** (todos los métodos se comparan con el mismo IC), salida CSV y timeout global.
- Scripts de soporte: `run_experiments.sh` (bloques A–E), `download_instances.py` (redes SNAP), `generate_lfr.py` (grafos sintéticos LFR) y `analyze_results.py` (tablas y figuras).
- Corrección del uso de `np.random.seed(i)` dentro del bucle interno, que afectaba a la varianza de σ(S) entre réplicas.

---

## 2. Diseño experimental

Parámetros controlados en todos los bloques: probabilidad de propagación IC **p = 0,1** y **mc = 100** réplicas Monte Carlo.

| Bloque | Objetivo | Configuración | Estado |
|---|---|---|---|
| **A** | Escalabilidad en N | LFR, N ∈ {1k, 5k, 10k, 20k, 50k}, μ=0,3, k ∈ {3, 5, 10, 20}, CBC y Gurobi, 3 réplicas, sin timeout global | Completo |
| **B** | Efecto de la estructura comunitaria | LFR N=5000, k=5, μ ∈ {0,1, 0,3, 0,5, 0,7, 0,9}, Gurobi, 3 réplicas | Completo |
| **C** | Redes reales pequeñas | karate, dolphins, books, football, emailEuCore, citeseer; k ∈ {3, 5, 10}; 3 réplicas | Completo |
| **D** | Comparación con baselines | DCIIM vs Greedy, HCIM y α-HCIM; 12 redes (6 pequeñas + 6 SNAP); k ∈ {3, 4, 5, 10, 15}; timeout 300 s | Completo |
| **DL** | Redes grandes con más presupuesto | Redes SNAP, timeout 1800 s | Completo |
| **E** | Estudio de φ | φ = 1…10 fijo, k=3, 6 redes SNAP, timeout global 1800 s | Completo |

Redes SNAP utilizadas: CA-AstroPh (N=18 771), CA-CondMat (23 133), Cit-HepPh (34 546), Email-Enron (36 692), p2p-Gnutella31 (62 586) y twitter_combined (81 306).

---

## 3. Resultados

### 3.1. El cuello de botella es la construcción del modelo, no el ILP

Fuente: `results/table_time_breakdown.csv` y `results/fig_bottleneck_bars.png`. Solo se incluyen ejecuciones `optimal`, porque en los timeouts `t_coverage` queda vacío.

`t_build` se calcula como residuo: `t_total − t_coverage − t_ilp − t_ic`.

| Instancia | t_total (s) | Cobertura | Restricciones | ILP | IC |
|---|---|---|---|---|---|
| LFR N=1000 (Gurobi) | 1,2 | 50,2 % | 36,6 % | 4,8 % | 8,3 % |
| LFR N=5000 (Gurobi) | 20,8 | 54,7 % | 42,2 % | 2,4 % | 0,7 % |
| LFR N=10000 (Gurobi) | 83,1 | 55,2 % | 42,5 % | 2,1 % | 0,1 % |
| LFR N=20000 (Gurobi) | 334,2 | 57,7 % | 42,0 % | 0,3 % | 0,1 % |
| LFR N=50000 (Gurobi) | 2361,9 | 61,5 % | 38,4 % | 0,1 % | 0,0 % |
| citeseer | 8,0 | 52,0 % | 43,5 % | 3,5 % | 1,0 % |
| CA-AstroPh | 935,1 | 69,2 % | 27,6 % | 1,4 % | 1,9 % |
| CA-CondMat | 698,0 | 63,3 % | 35,2 % | 0,4 % | 1,2 % |
| Email-Enron | 1718,7 | 64,1 % | 34,5 % | 0,6 % | 0,8 % |
| emailEuCore | 6,9 | 28,3 % | 13,4 % | 41,4 % | 17,0 % |

**Interpretación:**
- En todas las ejecuciones con N ≥ 3000, cobertura + restricciones suponen **≈ 97–99 % del tiempo total**. El ILP queda **por debajo del 4 %** y la simulación IC **por debajo del 2 %**.
- El peso relativo de la cobertura crece con N.
- **Contradice la hipótesis inicial** del plan de pruebas, que esperaba que dominara el IC (O(|V|·|E|·mc)) o el ILP.
- **Excepción:** emailEuCore es una red pequeña y muy densa (N=1005, E=16 706), por lo que el ILP pesa más. Aun así, el tiempo total es de solo 6,9 s.
- **Origen:** `cover()`, `coverage_count` y el bucle de restricciones son código original de Motaz, sin vectorizar. Recorren las transacciones en Python puro, con coste O(N·Σ|T|).

### 3.2. Complejidad empírica aproximadamente cuadrática

Fuente: `results/table_loglog_slopes.csv` y `results/fig_loglog_Nvst.png` (bloque A).

| Solver | k=3 | k=5 | k=10 | k=20 |
|---|---|---|---|---|
| CBC | 1,93 | 1,96 | 1,97 | 1,97 |
| Gurobi | 1,95 | 1,97 | 1,94 | 1,90 |

- La pendiente de log(t_total) frente a log(N) está entre **1,90 y 1,97**, lo que indica un comportamiento **≈ O(N²)** en LFR.
- La pendiente **no depende ni de k ni del solver**, coherente con que el ILP no sea el cuello de botella.
- **Hardware:** en la máquina anterior (`results/old_ubuntu_native/`) la pendiente era la misma. Solo cambia el factor constante (~1,3×). *Pendiente: anotar CPU y RAM en el capítulo de Descripción informática.*

### 3.3. Modelo de coste: explica los timeouts

Fuente: `results/table_cost_model.csv` y `results/fig_cost_model.png`.

Se define la carga `W = N·(2E + N)`, que aproxima el número de comprobaciones de `cover()`. El ajuste da **t_coverage + t_build ∝ W^0,98**, es decir, prácticamente lineal en W.

| Instancia | N | E | t previsto (s) | t observado (s) | > 1800 s |
|---|---|---|---|---|---|
| LFR N=10000 | 10 000 | 20 911 | 79 | 81 | No |
| LFR N=20000 | 20 000 | 42 417 | 313 | 331 | No |
| CA-CondMat | 23 133 | 93 439 | 716 | 661 | No |
| CA-AstroPh | 18 771 | 198 050 | 1138 | 894 | No |
| LFR N=50000 | 50 000 | 107 258 | 1916 | 2362 | Sí |
| Email-Enron | 36 692 | 183 831 | 2145 | 1736 | Sí (en el límite) |
| p2p-Gnutella31 | 62 586 | 147 892 | 3220 | — | Sí |
| Cit-HepPh | 34 546 | 420 877 | 4324 | — | Sí |
| twitter_combined | 81 306 | 1 342 296 | 31 046 | — | Sí |

**Interpretación:**
- El modelo predice bien los tiempos observados, con errores del orden del 5–25 %.
- **Las tres redes sin resultado (Cit-HepPh, Gnutella, Twitter) se explican por el coste de construcción.** Su tiempo previsto está entre 1,8× y 17× por encima del presupuesto de 1800 s. En esas redes el solver ni siquiera llega a ejecutarse.
- Enron (≈ 1720 s observados) está justo en el límite, lo que explica su comportamiento intermitente (ver 3.5).
- LFR N=50000 termina en ~2365 s porque el bloque A no usa `--global-timeout`.

### 3.4. Calidad y tiempo frente a los baselines (bloque D, timeout 300 s)

Fuente: `results/table_methods_comparison.csv`. σ es la influencia esperada; CPU es el tiempo en segundos; *tout* indica timeout.

**Redes pequeñas (selección):**

| Red | k | DCIIM σ / CPU | Greedy σ / CPU | HCIM σ / CPU | α-HCIM σ / CPU |
|---|---|---|---|---|---|
| karate | 5 | 10 / 0,02 | 11 / 1,71 | 10 / 0,36 | 11 / 0,36 |
| karate | 15 | 19 / 0,04 | 20 / 7,37 | 20 / 0,72 | 19 / 0,78 |
| dolphins | 15 | 25 / 0,06 | 24 / 20,13 | 24 / 1,39 | 24 / 1,02 |
| Books | 10 | 37 / 0,07 | 35 / 42,89 | 32 / 2,72 | 34 / 3,92 |
| football | 15 | 52 / 0,11 | 48 / 86,85 | 52 / 4,80 | 52 / 5,40 |

- DCIIM obtiene una **σ igual o superior** en casi todos los casos. Greedy solo le supera por 1 nodo en karate con k=4, 5 y 15.
- El tiempo de CPU es **2–3 órdenes de magnitud menor** que el de Greedy.

**Redes medianas:**

| Red | k | DCIIM σ / CPU | Greedy | HCIM σ / CPU | α-HCIM σ / CPU |
|---|---|---|---|---|---|
| emailEuCore | 3 | 712 / 4,8 | tout | 709 / 171,2 | tout |
| emailEuCore | 15 | 714 / 13,3 | tout | tout | tout |
| citeseer | 3 | 44 / 8,3 | tout | 40 / 13,4 | 38 / 62,4 |
| citeseer | 15 | 90 / 7,9 | tout | 75 / 20,8 | 76 / 102,6 |

- En **citeseer**, DCIIM supera a HCIM y α-HCIM en σ en torno a un **10–20 %**, con un tiempo menor. Greedy no termina.
- En **emailEuCore**, HCIM consigue una σ similar, pero tarda **171–239 s** frente a 5–13 s de DCIIM. Greedy y α-HCIM no terminan.

**Redes SNAP (N ≥ 18 771):** con 300 s **ningún método termina**, tampoco los baselines. No es una desventaja exclusiva de DCIIM.

**Nota sobre Enron:** da `global_timeout` en D (300 s), pero termina en DL/E (1800 s). La diferencia se debe únicamente al timeout distinto. *Pendiente de explicar en el paper.*

### 3.5. Estudio de φ (overlapallowance), bloque E

Fuente: `results/table_phi.csv` y `results/fig_phi.png`. k=3, φ fijo de 1 a 10, timeout 1800 s.

| Red | σ (φ=1 / 2 / 3–10) | Tiempo |
|---|---|---|
| CA-AstroPh | 9618 / 9598 / 9616 | ~893–919 s, plano |
| CA-CondMat | 5041 / 5053 / 5046 | ~642–673 s, plano |
| Email-Enron | resuelve con φ=1, 3 y 5 (σ ≈ 9563–9572); timeout con el resto | ~1790–1805 s en todos |
| Cit-HepPh, Gnutella, Twitter | timeout con cualquier φ | 1800 s |

**Interpretación:**
- **φ apenas influye:** a partir de φ=3, σ es idéntica, y el tiempo no depende de φ.
- La alternancia de Enron **no se debe a φ**: todos los tiempos rondan los 1800 s, así que es ruido de ejecución en torno al límite.
- Las redes que fallan lo hacen con cualquier φ, por el coste de construcción. **No se debe afirmar que φ limita la escalabilidad.**

### 3.6. Efecto de μ, bloque B

Fuente: `results/table_mu.csv`. LFR N=5000, k=5.

| μ | t_total medio (s) | σ media |
|---|---|---|
| 0,1 | 21,22 | 91,0 |
| 0,3 | 21,70 | 86,0 |
| 0,5 | 21,21 | 99,7 |
| 0,7 | 21,65 | 104,0 |
| 0,9 | 21,23 | 98,7 |

- **El tiempo no cambia** (variación del 2 %): la estructura comunitaria no afecta al coste.
- σ sí varía (86 → 104, un 21 %), pero **sin tendencia monótona**.
- Decisión: va en una sola frase del paper, sin figura. La frase debe decir solo que μ no afecta al coste.

### 3.7. Incrementos de overlapallowance

Fuente: `results/report_overlap_increments.txt`.

- Solo se incrementó en **karate**: k=10 (+2) y k=15 (+4).
- En ningún otro bloque o red hizo falta.
- `--max-iter 10` **nunca se agota**, lo que justifica ese valor por defecto.

---

## 4. Decisiones ya tomadas (31 de octubre de 2026)

1. **Redes grandes sin resultado** (Cit-HepPh, Gnutella, Twitter): se reportan como **límite de escalabilidad de la construcción del modelo**, no como un fallo del ILP. No se sube el timeout ni se añaden redes.
2. **Tabla de redes reales (k=10, 15):** se usa el bloque D (12 redes, una semilla, 300 s). No se relanza el bloque C.
3. **Bloque B (μ):** sin figura; una sola frase en el paper.
4. **Escalabilidad:** curvas log-log, barras apiladas y modelo de coste. Se descartan la tabla CBC vs Gurobi, el punto de cruce N\* y el heatmap (μ, k).

---

## 5. Puntos a decidir en la reunión

### 5.1. Decisión principal: ¿qué método escalable se propone?

| | CMSA (Construct, Merge, Solve & Adapt) | GRASP sobre la representación transaccional |
|---|---|---|
| Idea | Construir soluciones heurísticas, fusionar sus componentes en una subinstancia reducida y resolverla con el ILP de DCIIM | Construcción greedy aleatorizada más búsqueda local directamente sobre las transacciones |
| Relación con DCIIM | **Conserva la formulación ILP** de Motaz | Se aleja de la formulación exacta |
| Material o precedente | Blum (2024), cap. 1, ya en el vault | Lozano-Osorio y Cordón (2023), precedente cercano |
| Ataque al cuello de botella | Reduce N en la subinstancia, y el coste es ~O(N²) | Evita construir el modelo completo |
| Riesgo | Si la subinstancia se sigue construyendo con el `cover()` original, el cuello persiste dentro de cada iteración | Pierde la garantía de optimalidad del ILP y la conexión con el paper original |

**Argumento con datos:** como el ILP supone menos del 4 % del tiempo, **acelerar el solver no sirve de nada**. La propuesta tiene que **reducir el tamaño del modelo que se construye** (CMSA) o **no construirlo entero** (GRASP).

**Qué bloquea esta decisión:** los Objetivos, el Estado del arte, la Descripción algorítmica y la comparación experimental.

### 5.2. Decisión secundaria: ¿vectorizar la construcción del modelo?

- Con patrones de un nodo, `cover(T, [u])` equivale a `u ∈ T`. Por tanto, la cobertura es grado + 1 y las filas de restricciones son la **matriz de adyacencia + I**. Se pueden construir en **O(E)** con `scipy.sparse`.
- **Ventaja:** eliminaría el ~97 % del tiempo y sería una mejora propia y medible sobre el original.
- **Coste:** habría que relanzar los bloques para que los tiempos sean comparables.
- **Pregunta para los tutores:** ¿se presenta como contribución, como baseline mejorado, o como paso previo a CMSA?

### 5.3. Otras preguntas para los tutores
- ¿Es suficiente justificar `--max-iter 10` (nunca se agota) y el timeout de 1800 s como presupuesto práctico?
- ¿Cómo se cita a Motaz en la bibliografía (`@unpublished` o el artículo de IPMU)?

---

## 6. Estado de la memoria

Se redacta en `tfm.tex` (vault de Obsidian) y se copia a Overleaf.

| Capítulo | Estado |
|---|---|
| Introducción, bloque 1 (difusión en redes sociales) | Hecho |
| Introducción, bloque 2 (problema IM) | Falta la formulación de Kempe y el repaso de enfoques |
| Introducción, motivación | Solo 3 líneas; falta adelantar hallazgos y el puente a Objetivos |
| Objetivos | Provisional; depende de la decisión 5.1 |
| Estructura del documento | Provisional |
| Literatura / Estado del arte | Vacío |
| Formulación del problema | Vacío |
| Descripción informática (herramientas, datos, hardware, presupuesto) | Vacío |
| Descripción algorítmica | Vacío; depende de la decisión 5.1 |
| Resultados | Vacío; aquí entra todo este estudio |
| Conclusiones y trabajos futuros | Vacío |
| Bibliografía | Completar la entrada `motaz` |

**Tareas pendientes de redacción derivadas del estudio:**
- Reportar las redes grandes como límite de la construcción del modelo.
- Explicar la diferencia de Enron entre D (300 s) y DL/E (1800 s).
- Justificar `--max-iter 10` y 1800 s.
- Redactar la frase sobre μ.

---

## 7. Figuras y ficheros para mostrar

| Fichero | Qué muestra |
|---|---|
| `results/fig_loglog_Nvst.png` | t_total frente a N en escala log-log; pendiente ≈ 2 |
| `results/fig_bottleneck_bars.png` | Reparto porcentual del tiempo por fase |
| `results/fig_cost_model.png` | Ajuste t ∝ W^0,98 y predicción de los timeouts |
| `results/fig_phi.png` | σ y tiempo frente a φ en redes SNAP |
| `results/table_methods_comparison.csv` | Comparación completa del bloque D |

---

## 8. Resumen en una línea por resultado

1. **Cuello de botella:** cobertura + restricciones ≈ 97–99 %; ILP < 4 %; IC < 2 %.
2. **Complejidad:** ≈ O(N²), con pendiente 1,90–1,97 independiente de k y del solver.
3. **Modelo de coste:** t ∝ W^0,98, con W = N·(2E+N); explica todos los timeouts.
4. **Calidad:** DCIIM ≥ baselines en σ y 2–3 órdenes de magnitud más rápido en redes pequeñas y medianas.
5. **Redes SNAP con 300 s:** ningún método termina.
6. **φ:** no influye ni en σ (a partir de φ=3) ni en el tiempo.
7. **μ:** no influye en el tiempo.
8. **overlapallowance:** solo se incrementa en karate; `--max-iter 10` nunca se agota.
9. **Pendiente de decidir:** CMSA o GRASP, y si se vectoriza la construcción del modelo.
