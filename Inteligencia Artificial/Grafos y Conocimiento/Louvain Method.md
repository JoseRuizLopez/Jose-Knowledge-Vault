links: [[001 - 020 Inteligencia Artificial|Inteligencia Artificial]]


# Louvain Method

Algoritmo greedy de [[Community Detection]] que optimiza la modularidad Q del grafo de forma jerárquica (Blondel et al., 2008).

## Fases

1. **Fase local**: asignar cada nodo a la comunidad vecina que maximice el incremento de Q.
2. **Fase de agregación**: contraer comunidades en supernodos y repetir.

Muy eficiente: O(n log n). No garantiza comunidades internamente conexas (corregido por [[Leiden Algorithm]]).


---
tags:
	#CommunityDetection #GraphTheory