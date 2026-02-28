links: [[001 - 020 Inteligencia Artificial|Inteligencia Artificial]]


# Normalización

Transformación de las características de entrada para que tengan escala comparable, mejorando el entrenamiento y la convergencia.

## Técnicas

- **Min-Max scaling**: rango [0,1]. Sensible a outliers.
- **Standardization (Z-score)**: media 0, std 1. Robusta y habitual.
- **Normalización L2**: norma unitaria. Útil para embeddings.

Esencial en [[KNN]], donde las variables con rangos grandes dominarían el cálculo de distancias.


---
tags:
	#MachineLearning #Preprocesamiento #DataEngineering