links: [[103 - TFM]]


# CELF — Cost-Effective Lazy Forward

Optimización del [[Greedy Hill-Climbing]] para [[Influence Maximization]] (Leskovec et al., 2007). Hasta 700× más rápido con la misma garantía (1 − 1/e), explotando la [[Submodular Function|submodularidad]] de σ.

## Idea clave

La ganancia marginal de un nodo solo puede decrecer. Si tras actualizar al nodo en cabeza de la cola sigue siendo el mayor, se añade a S sin reevaluar el resto.

## Véase también

[[CELF++]] — mejora adicional | [[leskovec_2007_celf]]


---
tags:
	#InfluenceMaximization #Optimizacion #TFM