links: [[001 - 020 Inteligencia Artificial|Inteligencia Artificial]]


# Girvan-Newman

Algoritmo de [[Community Detection]] basado en la eliminación iterativa de aristas con alta *betweenness centrality*.

## Procedimiento

1. Calcular betweenness centrality de todas las aristas.
2. Eliminar la arista con mayor betweenness.
3. Recalcular y repetir.

## Limitaciones

- Coste O(m² n), poco escalable.
- Superado por [[Louvain Method]] y [[Leiden Algorithm]] en redes grandes.


---
tags:
	#CommunityDetection #GraphTheory