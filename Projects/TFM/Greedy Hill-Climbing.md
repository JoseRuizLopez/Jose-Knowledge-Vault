links: [[103 - TFM]]


# Greedy Hill-Climbing

Algoritmo de aproximación para [[Influence Maximization]] con garantía (1 − 1/e) basado en la [[Submodular Function|submodularidad]] de σ.

## Procedimiento

1. S = ∅.
2. En cada iteración, añadir v* que maximiza la ganancia marginal σ(S ∪ {v*}) − σ(S).
3. Estimar σ con [[Monte Carlo Simulation]].
4. Repetir k veces.

Optimizado por [[CELF]] y [[CELF++]].


---
tags:
	#InfluenceMaximization #Optimizacion #TFM