links: [[103 - TFM]]


*Jose Ruiz (alumno), Oscar Cordon (tutor) e Isaac (Cotutor)*
# Reunión Cordón 03

### Resumen
Durante la reunión, se han confirmado algunas de las hipótesis que planteaban dudas sobre la escalabilidad de la solución presentada por [[motaz_2026_reformulating_influence_maximization|motaz]]. Entre ellas:
- Se ve claramente que es exponencial y se demuestra claramente que es sufre mucho la metodología con estancias grandes.
- Es interesante 


Nos interesa mucho mas el estudio del overlapallowance (phi). El mu no nos interesa porque solo se usa para generar las redes generadas.

### Siguientes Tareas
- [x] Añadir también k 10 y 15 para las tablas reales. → Bloque D (12 redes, k=3,4,5,10,15, timeout 300 s), tabla `results/table_methods_comparison.csv`.
- [x] Debemos de reproducir un estudio de los phi, pero con 3/4 redes grandes reales (nuestras o de la literatura.) Phi de 1 a 10. Puedo cogerlas de los papers que me ha mandado Isaac. Es probable que para esas instancias, el modelo no sea capaz de que encuentre valor factible. (10k, 20k, 50k, 100k) → Bloque E (6 redes SNAP, φ=1..10, k=3, timeout global 1800 s), `results/table_phi.csv` y `results/fig_phi.png`.
- [ ] Seguir escribiendo literatura. → Se redacta en [[tfm.tex]] (ver Pendiente).
- [x] Debo de añadir un limite de overlapallowance o poner un timeout general. → `--max-iter` (por defecto 10; 0 = φ fijo) y `--global-timeout` en `DCIIM.py`. Falta justificar los valores en la memoria.

---

## Trabajo realizado tras la reunión

- Bloque D: DCIIM frente a Greedy, HCIM y α-HCIM en 12 redes reales (6 pequeñas + 6 SNAP).
- Bloque DL: DCIIM en las 6 redes SNAP con timeout ampliado a 1800 s.
- Bloque E: estudio de φ en CA-AstroPh, CA-CondMat, Cit-HepPh, Email-Enron, p2p-Gnutella31 y twitter_combined.
- Análisis completo con `analyze_results.py`: log-log t vs N, barras apiladas por fase, modelo de coste, tabla de μ y tabla de φ.

## Resultados y análisis

**Escalabilidad**
- Pendiente log-log ≈ 1,9–2,0 en LFR → crecimiento aproximadamente cuadrático en N (`table_loglog_slopes.csv`).
- Modelo de coste: `t_coverage + t_build ∝ W^0,98`, con `W = N·(2E+N)`; predice bien los tiempos observados (`table_cost_model.csv`).

**Cuello de botella**
- No es el ILP ni Monte Carlo: es construir el modelo en Python. Con N ≥ 3000, `coverage_count` (52–70 %) + construcción de restricciones (28–43 %) ≈ 97–99 % de `t_total`; el solver ILP < 4 % y la IC < 2 % (`table_time_breakdown.csv`).
- Ese código (`cover()`, `coverage_count`, bucle de restricciones) viene del repositorio original de [[motaz_2026_reformulating_influence_maximization|Motaz]], sin vectorizar. Lo añadido por mí es el arnés de experimentos (CLI, medición por fases, `--max-iter`, `--global-timeout`).
- Corrige lo anotado en [[Reunion_Cordon_02]] y en el esqueleto de la introducción, que apuntaba a Monte Carlo.

**Redes grandes sin resultado**
- Cit-HepPh, p2p-Gnutella31 y twitter_combined dan `global_timeout` con cualquier φ. El modelo de coste prevé ≈ 4300 s, ≈ 3200 s y ≈ 31 000 s respectivamente, frente a 1800 s: el solver ni llega a ejecutarse.
- Se reportan como límite de la construcción del modelo, no del ILP.
- Email-Enron (≈ 1720 s) está justo en el límite: la alternancia optimal/timeout entre valores de φ es ruido, no efecto de φ. Con 300 s (bloque D) no termina.

**Estudio de φ**
- φ apenas influye: en CA-AstroPh y CA-CondMat σ(S) es idéntica de φ=3 a φ=10 (solo φ=1–2 varían ligeramente) y el tiempo es plano (~910 s y ~655 s).
- La hipótesis de que el modelo no encontraría solución factible no se confirma tal cual: no llega a construirse el modelo. φ no limita la escalabilidad.
- `overlapallowance` solo se incrementó en karate (k=10: +2, k=15: +4); `--max-iter 10` nunca se agota.

**Efecto de μ**
- El tiempo no cambia (21,2–21,7 s), pero σ(S) sí varía (86 → 104, sin tendencia monótona). En la memoria, una frase: μ no afecta al coste.

**Hardware**
- Comparando con la ejecución antigua en Ubuntu: la máquina actual es ~1,3× más rápida, con la misma pendiente. El hardware cambia el factor constante, no la forma de la curva.

## Decisiones tomadas

1. Las redes sin resultado se reportan como límite de la construcción del modelo; no se sube el timeout ni se añaden redes.
2. La tabla de redes reales con k=10 y 15 sale del bloque D (una semilla).
3. μ queda fuera de las figuras; solo una frase en la memoria.
4. Escalabilidad: log-log y barras apiladas. Se descartan el heatmap (μ, k), la tabla CBC vs Gurobi y el punto de cruce N\*.

## Para la próxima reunión

- **Decidir la línea de solución:** CMSA (conserva el ILP sobre subinstancias reducidas, [[blum_2024_cmsa_libro]]) o metaheurística tipo GRASP ([[lozano_2023_grasp_snimp]]). Bloquea Objetivos, Estado del arte, Descripción algorítmica y la comparación experimental.
- Valorar vectorizar `coverage_count` (con patrones de un nodo se puede construir en O(E)) como primer paso de la propuesta.
