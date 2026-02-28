links: [[001 - 020 Inteligencia Artificial|Inteligencia Artificial]]


# Linear Threshold Model

El *Linear Threshold Model* (LTM) es un modelo de difusión en redes sociales en el que un nodo se activa cuando la suma ponderada de sus vecinos activos supera un umbral θ_v.

## Mecanismo

1. Cada nodo v recibe un umbral θ_v ∼ U(0,1).
2. Cada arista (u, v) tiene peso b_{u,v} tal que Σ_u b_{u,v} ≤ 1.
3. v se activa si Σ_{u activo} b_{u,v} ≥ θ_v.

## Comparación con [[Independent Cascade Model]]

| Aspecto | LTM | ICM |
|---|---|---|
| Decisión de activación | Umbral acumulado | Probabilidad por arista |
| Semántica | Presión social acumulada | Contagio estocástico |


---
tags:
	#InfluenceMaximization #SocialNetworkAnalysis