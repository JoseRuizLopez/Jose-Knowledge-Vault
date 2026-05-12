links: [[103 - TFM]] | [[Reunion_Cordon_02]]

# PuLP

PuLP es una librería de Python para formular y resolver problemas de [[ProgramacionLineal]] (LP) y [[ProgramacionLinealEnteraMixta]] (MILP). Actúa como una interfaz de alto nivel que traduce el problema a un formato estándar y lo delega a un _solver_ externo, separando el modelado de la resolución.

## Instalación

```bash
pip install pulp
```

Por defecto incluye el _solver_ [[CBC Solver|CBBC]] (COIN-OR Branch and Cut). Para _solvers_ comerciales como [[Gurobi]] o [[CPLEX]], se necesita instalación y licencia aparte.

## Componentes de un problema

Todo problema de optimización en PuLP tiene tres elementos obligatorios:

- **Variables de decisión**: las incógnitas que el _solver_ debe determinar.
- **Función objetivo**: la expresión a maximizar o minimizar.
- **Restricciones**: condiciones lineales que debe satisfacer la solución.

## Estructura básica

```python
from pulp import *

# 1. Crear el problema
prob = LpProblem("nombre_problema", LpMaximize)  # o LpMinimize

# 2. Definir variables
x = LpVariable("x", lowBound=0)
y = LpVariable("y", lowBound=0)

# 3. Añadir función objetivo
prob += 5*x + 4*y

# 4. Añadir restricciones
prob += 6*x + 4*y <= 24
prob += x + 2*y <= 6

# 5. Resolver
prob.solve()

# 6. Leer resultados
print(f"Status: {LpStatus[prob.status]}")
print(f"x = {value(x)}, y = {value(y)}")
print(f"Objetivo = {value(prob.objective)}")
```

## Tipos de variables

```python
# Continua (por defecto)
x = LpVariable("x", lowBound=0)

# Con límite superior
x = LpVariable("x", lowBound=0, upBound=10)

# Entera
x = LpVariable("x", cat="Integer", lowBound=0)

# Binaria (0 o 1)
x = LpVariable("x", cat="Binary")

# Múltiples variables a la vez
variables = LpVariable.dicts("x", range(5), lowBound=0)
```

## Solvers disponibles

PuLP soporta varios _solvers_ que se pueden especificar explícitamente:

```python
# CBC (incluido por defecto)
prob.solve(PULP_CBC_CMD(msg=0))

# GLPK
prob.solve(GLPK(msg=0))

# Gurobi (requiere licencia)
prob.solve(GUROBI(msg=0))

# HiGHS (muy recomendado para MILP)
prob.solve(HiGHS_CMD(msg=0))
```

Para silenciar el output del _solver_ usar `msg=0`.

## Leer el estado de la solución

```python
LpStatus[prob.status]
# "Optimal"    -> solución óptima encontrada
# "Infeasible" -> restricciones contradictorias
# "Unbounded"  -> objetivo puede crecer indefinidamente
# "Not Solved" -> no se ha resuelto
```

## Ejemplo: Problema de asignación binaria

Asignar tareas a trabajadores minimizando coste total:

```python
from pulp import *

costes = {
    (0, 0): 9, (0, 1): 2, (0, 2): 7,
    (1, 0): 3, (1, 1): 6, (1, 2): 4,
    (2, 0): 1, (2, 1): 8, (2, 2): 5,
}

trabajadores = range(3)
tareas = range(3)

prob = LpProblem("asignacion", LpMinimize)

x = LpVariable.dicts("x", costes.keys(), cat="Binary")

# Objetivo
prob += lpSum(costes[i, j] * x[i, j] for i in trabajadores for j in tareas)

# Cada trabajador tiene exactamente una tarea
for i in trabajadores:
    prob += lpSum(x[i, j] for j in tareas) == 1

# Cada tarea es asignada a exactamente un trabajador
for j in tareas:
    prob += lpSum(x[i, j] for i in trabajadores) == 1

prob.solve(PULP_CBC_CMD(msg=0))

for (i, j), var in x.items():
    if value(var) == 1:
        print(f"Trabajador {i} -> Tarea {j}")
```

## Ejemplo: Problema de la mochila (Knapsack)

```python
from pulp import *

items = {"A": (10, 6), "B": (6, 4), "C": (4, 3), "D": (3, 2)}
# formato: nombre -> (valor, peso)
capacidad = 8

prob = LpProblem("mochila", LpMaximize)
x = LpVariable.dicts("x", items.keys(), cat="Binary")

prob += lpSum(items[i][0] * x[i] for i in items)         # maximizar valor
prob += lpSum(items[i][1] * x[i] for i in items) <= capacidad  # respetar peso

prob.solve(PULP_CBC_CMD(msg=0))
seleccion = [i for i in items if value(x[i]) == 1]
print(f"Items seleccionados: {seleccion}")
```

## lpSum vs sum

Usar siempre `lpSum` en lugar de `sum` nativo de Python al construir expresiones lineales grandes, ya que es significativamente más eficiente con muchas variables:

```python
# Correcto y eficiente
prob += lpSum(x[i] for i in range(1000))

# Funciona pero lento con muchas variables
prob += sum(x[i] for i in range(1000))
```

## Ventajas y limitaciones

|Ventajas|Limitaciones|
|---|---|
|API simple e intuitiva|Solo soporta modelos lineales|
|Solver CBC incluido sin configuración|No apto para optimización no lineal|
|Interfaz uniforme para múltiples _solvers_|Rendimiento limitado vs APIs nativas de Gurobi/CPLEX|
|Bien integrado con pandas y NumPy|Modelado de problemas grandes puede ser verboso|
|Open source y gratuito|Menos funcionalidades avanzadas que [[Pyomo]]|

## Alternativas

- [[Pyomo]]: más potente y flexible, orientado a modelos complejos.
- [[OR-Tools]]: librería de Google, especialmente fuerte en _constraint programming_.
- [[SciPy]] `linprog`: para LP sencillos sin necesidad de librería dedicada.
- [[Gurobi]] API Python: máximo rendimiento, pero comercial.

---

tags: #Optimizacion #ProgramacionLineal #Python #OperationsResearch