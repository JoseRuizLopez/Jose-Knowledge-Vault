links: [[001 - 010 Estadística|Estadística]]


# Índice de Estabilidad de la Población (PSI)

Métrica que cuantifica el cambio en la distribución de una variable entre dos periodos.

$$\text{PSI} = \sum_{i=1}^{n} (A_i - E_i) \cdot \ln\left(\frac{A_i}{E_i}\right)$$

## Interpretación

| PSI | Interpretación |
|---|---|
| < 0.1 | Sin cambio significativo |
| 0.1 – 0.2 | Cambio moderado, revisar |
| > 0.2 | Cambio importante, reentrenar |

Métrica estándar para detectar [[Data Drift]] tras [[Bucketización]].


---
tags:
	#Estadistica #DataDrift #MLOps