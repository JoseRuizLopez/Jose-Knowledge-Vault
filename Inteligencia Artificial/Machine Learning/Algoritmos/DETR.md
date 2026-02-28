links: [[001 - 020 Inteligencia Artificial|Inteligencia Artificial]]


# DETR — Detection Transformer

Modelo de detección de objetos end-to-end (Carion et al., Facebook AI, 2020) que elimina NMS y anchor boxes reformulando la detección como predicción de conjuntos con matching bipartito (algoritmo húngaro).

## Arquitectura

1. Backbone CNN — mapa de características.
2. Encoder Transformer — atención global sobre el mapa.
3. Decoder Transformer — N object queries → N predicciones (bbox + clase).


---
tags:
	#ComputerVision #ObjectDetection #Transformers