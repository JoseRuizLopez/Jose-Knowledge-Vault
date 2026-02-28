links: [[001 - 020 Inteligencia Artificial|Inteligencia Artificial]]


# Influence Maximization with Community Detection

Variante del problema de [[Influence Maximization]] que incorpora la estructura de comunidades de la red para mejorar la selección de nodos semilla.

## Motivación

Seleccionar semillas que cubran comunidades distintas evita solapamiento de influencia y maximiza el alcance global.

## Estrategia general

1. Detectar comunidades con [[Community Detection]] ([[Louvain Method]] o [[Leiden Algorithm]]).
2. Aplicar [[CELF]] o [[Greedy Hill-Climbing]] dentro o entre comunidades.
3. Combinar con [[Community-Based Seed Selection]].


---
tags:
	#InfluenceMaximization #CommunityDetection #GraphTheory