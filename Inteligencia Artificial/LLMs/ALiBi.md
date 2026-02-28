links: [[001 - 020 Inteligencia Artificial|Inteligencia Artificial]]


# ALiBi — Attention with Linear Biases

Esquema de codificación posicional que, en lugar de añadir embeddings posicionales, resta un sesgo lineal proporcional a la distancia entre tokens en las puntuaciones de atención.

## Ventajas

- Buena generalización a secuencias más largas que las vistas en entrenamiento.
- Sin parámetros adicionales.

## Comparación con [[RoPE]]

| | ALiBi | RoPE |
|---|---|---|
| Tipo | Sesgo aditivo en atención | Rotación de Q/K |
| Uso notable | BLOOM, MPT | LLaMA, Mistral |


---
tags:
	#LLM #Transformers #PositionalEncoding