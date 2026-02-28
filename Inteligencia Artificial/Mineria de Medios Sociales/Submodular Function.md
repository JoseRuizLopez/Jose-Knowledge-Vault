links: [[001 - 020 Inteligencia Artificial|Inteligencia Artificial]]


# Submodular Function

Una función f: 2^V → ℝ es *submodular* si satisface rendimientos marginales decrecientes: para todo A ⊆ B ⊆ V y v ∉ B:

$$f(A \cup \{v\}) - f(A) \geq f(B \cup \{v\}) - f(B)$$

Si además es monótona y f(∅) = 0, el algoritmo greedy ofrece garantía de (1 − 1/e) ≈ 0.63.

## Relación con [[Influence Maximization]]

La función σ(S) en los modelos [[Independent Cascade Model]] y [[Linear Threshold Model]] es submodular, lo que justifica el uso de [[Greedy Hill-Climbing]].


---
tags:
	#Optimizacion #InfluenceMaximization #GraphTheory