links: [[001 - 020 Inteligencia Artificial|Inteligencia Artificial]]


# Monte Carlo Simulation

En [[Influence Maximization]], las simulaciones de Monte Carlo estiman la función de difusión esperada σ(S), intractable de calcular exactamente.

## Procedimiento

1. Para un conjunto semilla S, simular la propagación (según [[Independent Cascade Model]]) R veces.
2. Promediar los nodos activados: σ̂(S) = (1/R) Σ_r |activados_r|.
3. Usar esta estimación en [[Greedy Hill-Climbing]].

[[CELF]] y [[CELF++]] reducen el número de evaluaciones necesarias.


---
tags:
	#InfluenceMaximization #Simulacion #GraphTheory