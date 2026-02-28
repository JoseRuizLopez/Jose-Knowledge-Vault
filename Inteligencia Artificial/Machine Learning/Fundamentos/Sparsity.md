links: [[001 - 020 Inteligencia Artificial|Inteligencia Artificial]]


# Sparsity

Proporción de pesos que son cero en una red neuronal. Alta sparsity permite comprimir el modelo y acelerar la inferencia.

## Tipos

- **No estructurada**: pesos individuales a cero. Alta compresión, difícil de acelerar en hardware estándar.
- **Estructurada**: bloques o cabezas completas podadas. Más fácil de acelerar.

## Relación con [[Prunning]]

El prunning es la técnica principal para inducir sparsity.


---
tags:
	#ModelCompression #DeepLearning #Optimizacion