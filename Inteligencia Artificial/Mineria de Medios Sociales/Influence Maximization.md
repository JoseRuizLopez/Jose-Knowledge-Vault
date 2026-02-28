links: [[001 - 020 Inteligencia Artificial|Inteligencia Artificial]]


# Influence Maximization

El problema de *influence maximization* consiste en seleccionar un conjunto semilla de k nodos en una red social de forma que la difusión esperada de información sea máxima bajo un modelo de propagación dado.

## Formulación

Dado un grafo G = (V, E) y un modelo de difusión ([[Independent Cascade Model]] o [[Linear Threshold Model]]), se busca:

$$S^* = \arg\max_{S \subseteq V,\, |S|=k} \sigma(S)$$

donde σ(S) es la esperanza del número de nodos activados partiendo de S.

## Complejidad

El problema es [[NP-Hard]] y la función σ es [[Submodular Function|submodular]] y monótona, lo que garantiza que [[Greedy Hill-Climbing]] ofrece una aproximación de (1 − 1/e).

## Algoritmos principales

- [[Greedy Hill-Climbing]] — aproximación con garantía teórica.
- [[CELF]] — optimización greedy mediante lazy evaluation.
- [[CELF++]] — mejora adicional de CELF.
- [[Degree Centrality Heuristic]] y [[Distance Centrality Heuristic]] — heurísticas rápidas.


---
tags:
	#InfluenceMaximization #GraphTheory #SocialNetworkAnalysis