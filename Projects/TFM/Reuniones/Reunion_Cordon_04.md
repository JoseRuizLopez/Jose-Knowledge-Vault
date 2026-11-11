links: [[103 - TFM]]


*Jose Ruiz (alumno), Oscar Cordon (tutor) e Isaac (cotutor) — 9 de noviembre de 2026*
# Reunión Cordón 04

Reunión anterior: [[Reunion_Cordon_03]] · Preparación: [[Pre-Reunion_Cordon_04]]

## Resumen

Se presentaron los resultados de los bloques D, DL y E sobre [[motaz_2026_reformulating_influence_maximization|DCIIM]]. El hallazgo clave: el cuello de botella **no es el solver ILP ni Monte Carlo**, sino la **construcción del modelo en Python** (`coverage_count` + generación de restricciones), que consume el 97–99 % del tiempo en redes con N ≥ 3000. El crecimiento es **polinómico (~O(N²))**, no exponencial como se pensaba en la reunión 03.

La línea de trabajo pasa a ser: **separar la construcción del modelo, guardarlo en disco y resolver las redes grandes** con presupuestos de tiempo independientes.

## Decisiones y comentarios de la reunión

### Rendimiento
- Usar **PyPy** para acelerar la parte en Python.
- Precalcular `coverage_count` y `cover()` **fuera del límite de 1800 s**, registrando cuánto tardan. Presupuesto orientativo: 1–2 h (Isaac llegó a dejarlo 6 h).
- Confirmar si los métodos originales usaban **paralelismo**. En nuestra propuesta también se puede paralelizar el Monte Carlo.

### Medición de tiempos
- En todos los métodos el Monte Carlo forma parte del cálculo, así que podría **excluirse de los tiempos**. Solo se ejecuta sobre las semillas candidatas.

### Parámetro φ (overlapallowance)
- Oscar no lo interpreta como que con k = 3 φ quede "limitado" a φ < 3: con ese φ las redes **ya alcanzan el máximo**. Matizar la redacción de la sección [[#Estudio de φ en redes grandes]].
- No parece necesario subir `--max-iter` por encima de 10 (influencia baja).
- Práctica habitual: primero un **estudio de parámetros** y después el estudio final con los parámetros más prometedores.

### Trabajo futuro
- **CSNA** usa modelos matemáticos pero construye un **grafo reducido** con solo las aristas más importantes, lo que lo hace mucho más rápido y viable. Candidato para reducir el tamaño del modelo.

## Siguientes tareas

- [ ] **(Prioritaria)** Implementar el Monte Carlo en paralelo.
- [ ] Construir el modelo aparte y **guardarlo en disco**, sin límite de tiempo (o uno laxo).
- [ ] Ejecutar los experimentos con las redes grandes usando el modelo precalculado, con timeout para el ILP de 3–4 h.
- [ ] Probar PyPy y medir la mejora.
- [ ] Confirmar si los baselines usaban paralelismo.
- [ ] Seguir con el apartado de literatura (hay poca). La revisión se hará cuando estén los 3 primeros apartados.
- [ ] Preparar **título y párrafo** de definición del TFM.
- [ ] Redactar objetivos: principal (análisis de la escalabilidad y cómo abordarla) y específicos (Python, solvers, modelado matemático, etc.).
- [ ] *(Futuro)* Estudiar cómo reducir el modelo y hacerlo más escalable (CSNA).

---

## Trabajo presentado

### Estado de las tareas de la reunión 03

| Tarea | Estado | Dónde |
| --- | --- | --- |
| Añadir k = 10 y 15 en las tablas de redes reales | Hecho | Bloque D → `datos/table_methods_comparison.csv` |
| Estudio de φ (1–10) con redes grandes reales | Hecho | Bloque E → `datos/table_phi.csv`, `figuras/fig_phi.png` |
| Límite de overlapallowance / timeout general | Hecho | `--max-iter` (defecto 10) y `--global-timeout` en `DCIIM.py` |
| Seguir escribiendo literatura | En curso | `tfm.tex` |

### Experimentos lanzados

- **Bloque D**: DCIIM frente a [[Greedy Hill-Climbing|Greedy]], HCIM y α-HCIM en 12 redes (6 pequeñas + 6 SNAP), k = 3, 4, 5, 10, 15, timeout 300 s.
- **Bloque DL**: DCIIM en las 6 redes SNAP con timeout 1800 s.
- **Bloque E**: φ fijo de 1 a 10, k = 3, en las 6 redes SNAP, timeout global 1800 s.

Redes SNAP utilizadas (ver [[Datasets de Redes]]):

| Red | N | E |
| --- | ---: | ---: |
| CA-AstroPh | 18 771 | 198 050 |
| CA-CondMat | 23 133 | 93 439 |
| Cit-HepPh | 34 546 | 420 877 |
| Email-Enron | 36 692 | 183 831 |
| p2p-Gnutella31 | 62 586 | 147 892 |
| twitter_combined | 81 306 | 1 342 296 |

> [!note] Cambios sobre el código original
> No se ha modificado el algoritmo de DCIIM ni los baselines; los tiempos son los del código original de Motaz. Solo se ha añadido: CLI, medición de tiempo por fases, `--max-iter`, `--global-timeout` y un evaluador σ(S) común para comparar todos los métodos con el mismo modelo IC.

### Escalabilidad: tiempo frente a N

![[fig_loglog_Nvst.png]]

Pendientes del ajuste log-log en grafos [[lancichinetti_2008_lfr_benchmark|LFR]] (μ = 0,3, N de 1000 a 50 000) — `datos/table_loglog_slopes.csv`:

| Solver | k = 3 | k = 5 | k = 10 | k = 20 |
| --- | ---: | ---: | ---: | ---: |
| [[CBC Solver\|CBC]] | 1,93 | 1,96 | 1,97 | 1,97 |
| Gurobi | 1,95 | 1,97 | 1,94 | 1,90 |

> [!important] Crecimiento cuadrático, no exponencial
> La recta log-log tiene **pendiente ≈ 2**: el crecimiento es **~O(N²)** (duplicar N multiplica el tiempo por ~4). En la práctica, el resultado es el mismo: a partir de ~50 000 nodos no es viable.
>
> La pendiente **no depende de k ni del solver**. CBC y Gurobi se solapan, primera pista de que el ILP no es lo que limita.

### Cuello de botella

![[fig_bottleneck_bars.png]]

Porcentaje de `t_total` por fase, solo ejecuciones que terminan (`datos/table_time_breakdown.csv`):

| Instancia | t_total | coverage_count | Restricciones | Solver ILP | Simulación IC |
| --- | ---: | ---: | ---: | ---: | ---: |
| LFR N=1000 | 1 s | 50 % | 37 % | 5 % | 8 % |
| LFR N=10000 | 83 s | 55 % | 43 % | 2 % | 0,1 % |
| LFR N=50000 | 2362 s | **62 %** | **38 %** | 0,1 % | 0,0 % |
| CA-AstroPh | 935 s | **69 %** | **28 %** | 1,4 % | 1,9 % |
| CA-CondMat | 698 s | **63 %** | **35 %** | 0,4 % | 1,2 % |
| Email-Enron | 1719 s | **64 %** | **35 %** | 0,6 % | 0,8 % |

> [!important] Hallazgo principal
> El coste está en **construir el modelo en Python**: `coverage_count` + restricciones suponen el **97–99 %** del tiempo en todas las redes con N ≥ 3000. El solver queda por debajo del 4 % y la simulación IC por debajo del 2 %. Esto contradice la hipótesis de la [[Reunion_Cordon_02|reunión 02]], que situaba el coste en Monte Carlo.

**Causa**: `cover()`, `coverage_count` y el bucle de restricciones vienen del repositorio original y usan bucles de Python sin vectorizar. Para cada patrón recorren todas las transacciones: coste **O(N·Σ|T|)**.

**Excepción**: emailEuCore (N = 1005, E = 16 706) es pequeña pero muy densa, y el ILP pesa un 41 %. Aun así tarda solo 7 s.

### Modelo de coste y redes sin resultado

![[fig_cost_model.png]]

Ajuste del tiempo de construcción frente a la carga `W = N·(2E + N)` (≈ número de comprobaciones de `cover()`): **t ∝ W^0,98**, prácticamente lineal en W.

| Red | Previsto (s) | Observado (s) | ¿Cabe en 1800 s? |
| --- | ---: | ---: | --- |
| CA-CondMat | 716 | 661 | Sí |
| CA-AstroPh | 1138 | 894 | Sí |
| Email-Enron | 2145 | 1736 | En el límite |
| p2p-Gnutella31 | **3220** | — | No |
| Cit-HepPh | **4324** | — | No |
| twitter_combined | **31 046** | — | No |

> [!note]
> Cit-HepPh, Gnutella y Twitter necesitarían entre **1,8× y 17×** el presupuesto de 1800 s **solo para construir el modelo**. El solver ni siquiera llega a ejecutarse.

### Estudio de φ en redes grandes

![[fig_phi.png]]

σ(S) y tiempo con φ fijo de 1 a 10, k = 3, timeout 1800 s (`datos/table_phi.csv`):

| Red | φ=1 | φ=2 | φ=3…10 | Tiempo |
| --- | ---: | ---: | --- | --- |
| CA-AstroPh | 9618 | 9598 | 9616 (todos iguales) | ~910 s, plano |
| CA-CondMat | 5041 | 5053 | 5046 (todos iguales) | ~655 s, plano |
| Email-Enron | 9572 | timeout | 9563 con φ = 3 y 5; timeout el resto | ~1790–1800 s |
| Cit-HepPh | timeout | timeout | timeout | 1800 s |
| p2p-Gnutella31 | timeout | timeout | timeout | 1800 s |
| twitter_combined | timeout | timeout | timeout | 1800 s |

Restricciones del ILP:

$$
\sum_j y_j \cdot \text{cover}(T_i, P_j) \le \varphi \quad \forall\, T_i \qquad (1)
$$
$$
\sum_j y_j = k \qquad (2)
$$

> [!warning] Limitación del diseño
> Con **k = 3**, una transacción puede estar cubierta como mucho por 3 semillas, así que con **φ ≥ 3 la restricción (1) es redundante** y el ILP es el mismo problema. Que σ(S) sea idéntica de φ = 3 a 10 es consecuencia de usar k = 3, no un resultado experimental. El bloque solo estudia realmente **φ = 1 y φ = 2** (variación < 0,3 %).
>
> *Matiz de Oscar*: más que una limitación, indica que con ese φ ya se obtiene el máximo.

**Lo que sí muestra el bloque E**:
- El **tiempo no depende de φ**, y las redes que fallan lo hacen con cualquier φ. Coherente con que el coste está en construir el modelo, igual para todo φ.
- La hipótesis de la reunión 03 (no habría solución factible en redes grandes) **queda sin comprobar**: el modelo no llega a construirse dentro del tiempo.
- En Email-Enron la alternancia óptimo/timeout es **ruido de ejecución** en el límite (~1790–1800 s), no efecto de φ.

> [!tip] Propuesta
> φ no limita la escalabilidad en tiempo; la limita el tamaño de la red. Para estudiar su efecto sobre calidad y factibilidad, repetir el bloque E con **k = 10 o 15**, de forma que φ = 1…10 sea siempre una restricción efectiva.

### Comparación con Greedy, HCIM y α-HCIM

Bloque D: una semilla, timeout 300 s, σ(S) evaluada con el mismo IC para todos (`datos/table_methods_comparison.csv`).

#### Redes pequeñas

| Red | k | DCIIM σ (CPU) | Greedy σ (CPU) | HCIM σ (CPU) | α-HCIM σ (CPU) |
| --- | ---: | --- | --- | --- | --- |
| karate | 10 | 15 (0,03 s) | 15 (4,5 s) | 15 (0,4 s) | 15 (0,6 s) |
| karate | 15 | 19 (0,04 s) | **20** (7,4 s) | **20** (0,7 s) | 19 (0,8 s) |
| dolphins | 10 | 19 (0,06 s) | 19 (11,4 s) | 18 (1,1 s) | 18 (0,7 s) |
| dolphins | 15 | **25** (0,06 s) | 24 (20,1 s) | 24 (1,4 s) | 24 (1,0 s) |
| Books | 10 | **37** (0,07 s) | 35 (42,9 s) | 32 (2,7 s) | 34 (3,9 s) |
| Books | 15 | **42** (0,09 s) | 41 (71,1 s) | **42** (4,1 s) | **42** (4,3 s) |
| football | 10 | 41 (0,08 s) | 41 (52,7 s) | **42** (3,6 s) | **42** (3,8 s) |
| football | 15 | **52** (0,11 s) | 48 (86,9 s) | **52** (4,8 s) | **52** (5,4 s) |

#### Redes medianas

| Red | k | DCIIM σ (CPU) | Greedy | HCIM σ (CPU) | α-HCIM σ (CPU) |
| --- | ---: | --- | --- | --- | --- |
| emailEuCore | 10 | 709 (6,9 s) | timeout | 709 (239 s) | timeout |
| emailEuCore | 15 | **714** (13,3 s) | timeout | timeout | timeout |
| citeseer | 10 | **75** (7,9 s) | timeout | 62 (17,8 s) | 63 (54,8 s) |
| citeseer | 15 | **90** (7,9 s) | timeout | 75 (20,8 s) | 76 (102,6 s) |

> [!note] Lectura de resultados
> - **Redes pequeñas**: calidad equivalente (±1 nodo, dentro del ruido: una semilla, 100 simulaciones MC, desviación típica de 0,6–3,5 nodos en el bloque B). La diferencia está en el tiempo: DCIIM es **2–3 órdenes de magnitud más rápido** que Greedy (football k = 15: 0,11 s vs 86,9 s).
> - **citeseer**: DCIIM logra **+20 % de σ** frente a HCIM y α-HCIM (+13–15 nodos), en menos tiempo; Greedy no termina.
> - **emailEuCore**: misma σ que HCIM, pero 6,9 s frente a 239 s; con k = 15 es el único que termina.
> - **Redes SNAP (N ≥ 18 000)**: con 300 s **no termina ningún método**, tampoco los baselines. Con 1800 s DCIIM resuelve CA-AstroPh y CA-CondMat. Email-Enron solo difiere por el presupuesto de tiempo.

### Otros resultados

#### Efecto de μ (`datos/table_mu.csv`)

| μ | 0,1 | 0,3 | 0,5 | 0,7 | 0,9 |
| --- | ---: | ---: | ---: | ---: | ---: |
| t_total (s) | 21,2 | 21,7 | 21,2 | 21,7 | 21,2 |
| σ(S) | 91 | 86 | 100 | 104 | 99 |

Como se acordó, μ queda fuera de las figuras. El tiempo no varía (2 %); σ cambia porque cada μ genera un grafo distinto, así que no es comparable.

#### Incrementos de overlapallowance (`datos/report_overlap_increments.txt`)

- Solo fue necesario en **karate**: +2 con k = 10 (φ: 2 → 4) y +4 con k = 15 (φ: 2 → 6).
- **Motivo**: karate tiene 34 nodos con un par de nodos centrales conectados a casi todos. Elegir 15 de 34 (44 % de la red) fuerza a que su vecindario contenga más de 2 semillas, y con φ = 2 el ILP es infactible. En redes grandes y dispersas siempre hay semillas lo bastante separadas.
- **`--max-iter 10`**: con k ≤ 20 nunca se superaron 4 incrementos, así que el límite no corta ninguna ejecución.


---
tags:
	#TFM #Reunion #InfluenceMaximization #Escalabilidad
