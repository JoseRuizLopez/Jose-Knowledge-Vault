links: [[103 - TFM]] | [[motaz_2026_reformulating_influence_maximization]]


# CBC Solver

El _CBC (Coin-or Branch and Cut)_ es un solver de código abierto para problemas de [[ProgramacionLinealEntera]] (MIP, _Mixed Integer Programming_) y [[ProgramacionLineal]] (LP). Forma parte del proyecto [[COIN-OR]] (_Computational Infrastructure for Operations Research_) y es uno de los solvers open-source más utilizados en optimización combinatoria.

## Contexto: tipos de problemas que resuelve

CBC está diseñado para resolver dos grandes familias de problemas:

- **LP (_Linear Programming_)**: optimización de una función objetivo lineal sujeta a restricciones lineales, con variables continuas.
- **MIP (_Mixed Integer Programming_)**: igual que LP pero con algunas o todas las variables restringidas a valores enteros. Es NP-hard en el caso general.

## Arquitectura y algoritmos

CBC combina dos técnicas principales:

### Branch and Bound

El árbol de búsqueda se construye ramificando sobre variables enteras cuya relajación LP devuelve valores fraccionarios. En cada nodo se resuelve una relajación LP; si el valor de la relajación no mejora la mejor solución entera conocida (_incumbent_), la rama se poda.

### Cut Generation

CBC incorpora generación de [[CuttingPlanes]] para reforzar la relajación LP antes de ramificar, reduciendo el gap entre la cota continua y la solución entera óptima. Algunos de los planos de corte que utiliza:

- _Gomory cuts_
- _Clique cuts_
- _Flow cover cuts_
- _Knapsack cuts_

La combinación de ambas estrategias da nombre al algoritmo: _Branch and Cut_.

## Instalación y acceso

CBC puede usarse de varias formas:

**Via PuLP (Python)**

```python
pip install pulp
# CBC viene incluido con PuLP por defecto
```

**Via OR-Tools (Python)**

```python
pip install ortools
# CBC disponible como uno de los solvers del backend
```

**Binario standalone**

```bash
# En Ubuntu/Debian
sudo apt install coinor-cbc
```

## Uso con PuLP

```python
import pulp

# Definición del problema
prob = pulp.LpProblem("ejemplo", pulp.LpMinimize)

x = pulp.LpVariable("x", lowBound=0, cat="Integer")
y = pulp.LpVariable("y", lowBound=0, cat="Continuous")

# Función objetivo
prob += 3 * x + 2 * y

# Restricciones
prob += x + y >= 4
prob += 2 * x + y >= 6

# Resolver con CBC
solver = pulp.PULP_CBC_CMD(msg=True)
prob.solve(solver)

print(f"Status: {pulp.LpStatus[prob.status]}")
print(f"x = {pulp.value(x)}, y = {pulp.value(y)}")
```

## Parámetros relevantes

|Parámetro|Descripción|Valor por defecto|
|---|---|---|
|`maxSeconds`|Tiempo máximo de resolución (s)|sin límite|
|`maxNodes`|Máximo de nodos del árbol B&B|sin límite|
|`gapRel`|Gap relativo aceptable para terminar|1e-4|
|`gapAbs`|Gap absoluto aceptable|1e-10|
|`threads`|Número de hilos (soporte limitado)|1|
|`msg`|Mostrar log del solver|True|

Ejemplo con límite de tiempo:

```python
solver = pulp.PULP_CBC_CMD(msg=False, timeLimit=60, gapRel=0.01)
```

## Comparativa con otros solvers

|Solver|Licencia|Velocidad MIP|Uso típico|
|---|---|---|---|
|**CBC**|Open-source|Buena|Proyectos académicos, open-source|
|[[Gurobi]]|Comercial|Excelente|Producción, alta escala|
|[[CPLEX]]|Comercial|Excelente|Producción, alta escala|
|[[GLPK]]|Open-source|Moderada|Problemas pequeños/medianos|
|[[HiGHS]]|Open-source|Muy buena|Alternativa moderna a CBC|

Para problemas de gran escala en producción, solvers comerciales como Gurobi suelen superar a CBC significativamente. Para proyectos sin presupuesto de licencias, [[HiGHS]] es actualmente la alternativa open-source más competitiva.

## Ventajas y limitaciones

|Ventajas|Limitaciones|
|---|---|
|Gratuito y open-source|Inferior a solvers comerciales en problemas grandes|
|Integrado en PuLP y OR-Tools|Soporte de paralelismo limitado|
|Activo y bien documentado|Puede ser lento en instancias MIP densas|
|Sin límite de variables/restricciones|Configuración avanzada menos accesible|

## Casos de uso habituales

- Problemas de [[SchedulingOptimization]]: asignación de tareas, turnos de trabajo.
- [[VehicleRoutingProblem]] (VRP) en instancias medianas.
- Problemas de _bin packing_ y corte de materiales.
- Prototipado y validación de modelos antes de mover a un solver comercial.
- Entornos académicos y proyectos de investigación operativa.

---

tags: #Optimizacion #ProgramacionLineal #OperationsResearch #Python