links: [[001 - 020 Inteligencia Artificial|Inteligencia Artificial]]


# RoPE — Rotary Position Embedding

Esquema de codificación posicional relativa que rota los vectores Query y Key por un ángulo proporcional a la posición, de modo que q^T k depende solo de la distancia relativa (Su et al., 2021).

## Ventajas

- Información posicional relativa implícita.
- Compatible con Flash Attention.
- Estándar en LLaMA, Mistral, Qwen.


---
tags:
	#LLM #Transformers #PositionalEncoding