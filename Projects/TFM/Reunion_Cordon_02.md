links: [[103 - TFM]]


*Jose Ruiz (alumno), Oscar Cordon (tutor) e Isaac (Cotutor)*
# Reunión Cordón 02

### 1. Objetivos Inmediatos (Próximos Pasos)
La prioridad actual es la toma de contacto con el entorno técnico y la replicación de resultados previos.

- **Entorno de desarrollo:** Descargar el repositorio del [[Reformulating_Infuence_Maximization_throughDescriptive_Clustering.pdf|paper principal]], verificar su funcionamiento y realizar ingeniería inversa para comprender la lógica interna.
    
- **Gestión de repositorios:** Una vez validado el primer repositorio, Isaac proporcionará acceso a recursos adicionales.
    
- **Configuración de Solvers:** Investigar la herramienta **[[PuLP]]**, y como se podría introducir débilmente en el paper de [[Reformulating_Infuence_Maximization_throughDescriptive_Clustering.pdf|Motaz Ben]]. Para la resolución de modelos, se plantean tres alternativas de acceso a solvers potentes:
    
    1. **Gurobi:** Utilizar la licencia académica gratuita.
        
    2. **Servidor dedicado:** Uso de un servidor proporcionado por el cotutor.
        
    3. **CPLEX:** Alternativa de optimización disponible.
        

### 2. Estructura y Metodología del Trabajo

Se ha definido una estrategia para la narrativa del TFM (o de un posible artículo derivado), dividida en dos fases principales:

**Fase 1: Justificación y Limitaciones (Estudio de Escalabilidad)**

- **Objetivo:** Demostrar el punto de ruptura de los métodos actuales.
    
- **Acción:** Realizar pruebas de rendimiento utilizando instancias de tamaño creciente.
    
- **Resultado esperado:** Probar que, incluso utilizando los mejores solvers comerciales, el problema se vuelve inabordable (no termina en un tiempo razonable) ante grandes volúmenes de datos. Esto servirá como justificación crítica para la necesidad de la Fase 2.
    

**Fase 2: Propuesta de Soluciones**

- Desarrollo y exposición de las soluciones propuestas que superen las limitaciones de escalabilidad identificadas en la fase anterior.
    

---

### 3. Recursos y Referencias

Se han facilitado los siguientes materiales de apoyo:

- **Escritura colaborativa:** Invitación al proyecto en **Overleaf** (revisar correo/enlace compartido).
    
- **Bibliografía base:** * _Libro de referencia:_ [[CMSA - Libro.pdf|Springer - Optimización/Modelado]]. El **Capítulo 1** es fundamental para el marco teórico. Archivo disponible en el Google Drive compartido.
    
- **Documentación Técnica:**
    
    - Librería de optimización: [Documentación de PuLP](https://coin-or.github.io/pulp/).
        
- **Código de referencia:**
    
    - [Repositorio GitHub - Alpha-HCIM.py](https://github.com/2x254/IPMU_RegularPaper/blob/main/Alpha-HCIM.py): Script específico para replicar el modelo _Alpha-HCIM_, del [[Reformulating_Infuence_Maximization_throughDescriptive_Clustering.pdf|Paper de Motaz Ben Hassine]]
        

---

### Tareas Pendientes (To-Do List)

- [x] Clonar el repo de GitHub y ejecutar el script `Alpha-HCIM.py`. Probar a tocatear el paper.
    
- [ ] Solicitar/Configurar licencia de estudiante en Gurobi o acceso al servidor de Isaac.
    
- [x] Leer el Capítulo 1 del libro en el Drive.
    
- [x] Acceder al Overleaf y revisar la estructura del documento.
    
- [x] Diseñar (y empezar a probar) el plan de pruebas para el estudio de escalabilidad.


## Tareas Realizadas
### Pruebas con el repositorio del Paper
#### Resumen de resultados

|Dataset|Solver|k=3 σ(S)|k=4 σ(S)|k=5 σ(S)|CPU total medio|
|---|---|---|---|---|---|
|N=10000 (LFR)|CBC|246|259|269|~113.7 s|
|N=10000 (LFR)|GUROBI|246|259|269|~110.5 s|
|emailEuCore (N=1005)|CBC|693|695|693|~8.7 s|
|emailEuCore (N=1005)|GUROBI|693|695|693|~5.6 s|

#### Observaciones clave

**1. Optimalidad confirmada.** CBC y GUROBI obtienen exactamente los mismos σ(S) en ambos datasets y para todo k. Eso valida que el ILP de ***DCIIM*** está resolviéndose a optimalidad en los dos solvers, la elección del solver no cambia las semillas ganadoras.

**2. La comparación de solvers se invierte según el tamaño:**

- **N=10000:** CBC es más rápido en el ILP puro (~0.58 s vs ~1.39 s de GUROBI). El problema es relativamente "fácil" (patrones unitarios, restricciones de cobertura dispersas) y GUROBI paga su overhead de inicialización.
- **emailEuCore (N=1005):** GUROBI domina claramente (~0.50 s vs ~3.0 s de CBC). El grafo es más denso y la matriz de cobertura más compleja, donde los heurísticos y presolve de GUROBI compensan el overhead.

**3. El cuello de botella NO es el ILP.** En N=10000, el ILP tarda ~0.6–1.6 s pero el CPU total es ~110 s. Más del 99% del tiempo se va en la simulación Monte Carlo del modelo IC (***DCIIM***, `mc=100`) y/o en construir `coverage_count` (***DCIIM***, que es O(|T|·|P|) sobre 10⁴ × 10⁴). Por eso el CPU total es casi idéntico entre solvers.

**4. Saturación de cobertura en emailEuCore.** σ(S) prácticamente no crece con k (693 → 695 → 693). El grafo es muy clusterizado: con k=3 ya se alcanzan los componentes principales, y añadir semillas adicionales recae en zonas ya cubiertas (limitado además por `overlapallowance=2`). Que k=5 dé exactamente igual que k=3 sugiere que las semillas extra están eligiéndose en componentes pequeños.

**5. Crecimiento monótono moderado en LFR-10000** (246 → 259 → 269): ganancia de ~5% por seed adicional, coherente con un grafo sintético menos denso donde cada nueva semilla aporta un componente independiente.

### Plan de pruebas. Estudio de escalabilidad de DCIIM

#### 1. Preguntas de investigación

El plan debe responder, en orden de prioridad:

1. **¿Cómo escala DCIIM con el tamaño del grafo |V|?** (curva tiempo vs N)
2. **¿Cómo escala con la densidad / estructura comunitaria?** (efecto de μ en LFR)
3. **¿Cómo escala con k y con `overlapallowance`?** (parámetros de la formulación)
4. **¿Es el ILP el cuello de botella, o lo son IC y `coverage_count`?** (perfilado por componente)
5. **¿GUROBI escala mejor que CBC al crecer el problema?** (cruce ya observado en N=1005 vs N=10000)

#### 2. Variables del experimento

| Tipo                    | Variable                 | Niveles propuestos                                       | Descripción                                                                                                                                         |
| ----------------------- | ------------------------ | -------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| Independientes (factor) | N (LFR)                  | 1000, 5000, 10000, 20000\*, 50000\*                      | Número de nodos del grafo sintético LFR, mide escalabilidad pura en tamaño                                                                          |
| Independientes          | μ (LFR)                  | 0.1, 0.3, 0.5, 0.7, 0.9                                  | Parámetro de mezcla LFR: a mayor μ, comunidades menos definidas y propagación IC menos predecible                                                   |
| Independientes          | k                        | 3, 5, 10, 20, 50                                         | Número de semillas a seleccionar, controla el tamaño del conjunto semilla S y la complejidad del ILP                                                |
| Independientes          | overlapallowance inicial | 1, 2, 3                                                  | Máximo solapamiento permitido entre patrones que cubren una misma transacción, afecta la factibilidad del ILP y el nº de iteraciones de ***DCIIM*** |
| Independientes          | Solver                   | CBC, GUROBI                                              | Motor de resolución del ILP, compara el solver open-source de PuLP frente al comercial Gurobi                                                       |
| Independientes          | Datasets reales          | karate, dolphins, books, football, emailEuCore, citeseer | Redes reales de distintos dominios y tamaños, miden generalización más allá de los grafos sintéticos LFR                                            |
| Controladas             | p (prob. propagación IC) | 0.1 (fijo)                                               | Probabilidad de activación por arista en el modelo IC, fijada para aislar el efecto de N y k                                                        |
| Controladas             | mc (réplicas IC)         | 100 (fijo)                                               | Número de simulaciones Monte Carlo para estimar σ(S), fijado como balance entre precisión y coste computacional                                     |
| Controladas             | semilla NumPy global     | fija por réplica                                         | Semilla del generador de números aleatorios, garantiza reproducibilidad y comparabilidad entre solvers                                              |
| Dependientes            | t_ILP                    | —                                                        | Tiempo de resolución del programa lineal entero (devuelto por `prob.solutionTime`)                                                                  |
| Dependientes            | t_total                  | —                                                        | Tiempo de pared completo por configuración: incluye cobertura + ILP + IC                                                                            |
| Dependientes            | t_coverage               | —                                                        | Tiempo de construir `coverage_count` en ***DCIIM***, operación O(                                                                                   |
| Dependientes            | t_IC                     | —                                                        | Tiempo de la simulación Monte Carlo en ***DCIIM***, dominante en grafos con N grande                                                                |
| Dependientes            | σ(S)                     | —                                                        | Influencia esperada del conjunto semilla S bajo el modelo IC, métrica de calidad de la solución                                                     |
| Dependientes            | n_iter_DCIIM             | —                                                        | Número de incrementos de `overlapallowance` hasta obtener factibilidad, indica robustez del parámetro inicial                                       |
| Dependientes            | RAM pico                 | MB                                                       | Memoria residente máxima del proceso, identifica limitaciones de escalabilidad por memoria antes que por tiempo                                     |

*Niveles 20k/50k condicionados a viabilidad: debemos añadir time-out global (p.ej. 1800 s) y abortar si se rebasa.

#### 3. Diseño experimental

Diseño **factorial fraccionado** en tres bloques para no explotar combinatoriamente:

**Bloque A. Escalabilidad en N** (eje principal del estudio)

- Fijo: μ=0.3, overlapallowance=2, p=0.1, mc=100
- Variable: N ∈ {1000, 5000, 10000, 20000, 50000}, k ∈ {3, 5, 10, 20}, solver ∈ {CBC, GUROBI}
- 5 × 4 × 2 = 40 runs × 3 réplicas = **120 ejecuciones**

**Bloque B. Sensibilidad estructural** (efecto de μ)

- Fijo: N=5000, k=5, overlapallowance=2, solver=GUROBI
- Variable: μ ∈ {0.1, 0.3, 0.5, 0.7, 0.9}
- 5 × 3 réplicas = **15 ejecuciones**

**Bloque C. Redes reales**

- Fijo: overlapallowance=2, solver=GUROBI
- Variable: dataset × k ∈ {3, 5, 10}
- 6 × 3 × 3 réplicas = **54 ejecuciones**

Total ≈ 190 ejecuciones.

#### 4. Protocolo por ejecución

Para cada run, registrar como CSV una fila con: `N, |E|, μ, k, overlapallowance, solver, replica, t_coverage, t_ILP, t_IC, t_total, sigma_S, n_iter_DCIIM, ram_peak, status`.

**Pasos:**

1. Reset de la semilla aleatoria al inicio (`np.random.seed(replica)`).
2. Cargar grafo, medir |V|, |E|, ⟨k⟩.
3. Cronometrar separadamente: construcción de `coverage_count`, ILP (ya se mide), simulación IC.
4. Imponer **time-out por solver** (`timeLimit=1800`) y **time-out global** (`SIGALRM` o equivalente), registrar status: optimal / time-out / infeasible.
5. Capturar RAM con `resource.getrusage(RUSAGE_SELF).ru_maxrss` al final.
6. Guardar también las semillas elegidas para poder auditar σ(S) a posteriori.

#### 5. Modificaciones mínimas a ***DCIIM***

- Parametrizar dataset, k, μ, overlapallowance, solver y semilla por **CLI/argparse**, en lugar de descomentar líneas, imprescindible para automatizar 190 runs.
- Separar las tres fases con `time.perf_counter()`: cobertura, ILP, IC.
- Corregir el bug de `np.random.seed(i)` dentro del bucle interno en ***DCIIM*** **antes** de empezar el estudio (afecta a la varianza de σ(S) entre réplicas).
- Volcar resultados a `results/scalability_<timestamp>.csv` por _append_.
- Un wrapper `run_experiments.sh` (o `Makefile`) que itere los bloques A/B/C.

#### 6. Análisis y entregables

1. **Curvas log-log de t_total vs N** por solver y por k → ajustar pendiente para estimar el orden de complejidad empírico (esperado: dominado por O(|V|·|E|·mc) de IC, no por el ILP).
2. **Gráficas de barras apiladas** (t_coverage / t_ILP / t_IC) para visualizar el cuello de botella por tamaño.
3. **Heat-map σ(S) en (μ, k)** para el bloque B.
4. **Tabla comparativa CBC vs GUROBI**: speedup y desviación estándar entre réplicas.
5. **Identificar el “punto de cruce”** N* donde GUROBI empieza a ser más rápido que CBC en t_ILP (entre 1k y 10k según los resultados que ya tienes).
6. Reportar combinaciones donde DCIIM tuvo que incrementar `overlapallowance` (señal de problemas mal escalados).

#### 7. Riesgos y mitigaciones

|Riesgo|Mitigación|
|---|---|
|Bloque A inviable a N=50k por memoria/tiempo|Time-out 1800 s + reportar como "no escalable a esa N"|
|`coverage_count` O(|T|
|Varianza alta entre réplicas por la semilla mal puesta de IC|Arreglar el bug y verificar coef. variación < 5% antes del run completo|
|GUROBI con licencia académica no disponible en alguna máquina|Tener perfil CBC-only como plan B para reproducibilidad externa|
