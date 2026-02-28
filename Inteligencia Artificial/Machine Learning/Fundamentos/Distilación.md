links: [[001 - 020 Inteligencia Artificial|Inteligencia Artificial]]


# Distilación (Knowledge Distillation)

Técnica de compresión en la que un modelo pequeño ("estudiante") aprende a imitar las salidas de un modelo grande ("profesor"), combinando pérdida dura (cross-entropy con etiquetas reales) y pérdida blanda (KL-divergence con salidas del profesor).

## Ejemplo

[[DistilBERT]] es BERT destilado a 6 capas con un 97% del rendimiento original.


---
tags:
	#ModelCompression #MachineLearning #LLM