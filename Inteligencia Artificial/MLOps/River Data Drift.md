links: [[001 - 020 Machine Learning|Machine Learning]]


# River Data Drift
El **River Data Drift** se refiere a un tipo específico de [[Data Drift]] en el que los cambios en la distribución de los datos ocurren de manera **continua y progresiva**, en lugar de ser abruptos o discretos. Este término se asocia a entornos de **aprendizaje en flujo ([[Online Machine Learning]])**, donde los modelos de machine learning deben adaptarse a datos que llegan en tiempo real.

## 📌 Características Claves del River Data Drift

1. **Cambio progresivo**: La distribución de los datos cambia poco a poco, como el flujo de un río, lo que puede hacer que el modelo se degrade lentamente con el tiempo.
2. **Afecta modelos en tiempo real**: Es especialmente problemático en sistemas de **aprendizaje en línea**, donde los datos se procesan de manera incremental.
3. **Difícil de detectar**: Al ser un cambio gradual, puede no ser evidente de inmediato y requerir técnicas avanzadas de monitoreo.

## Ejemplo

Un sistema de detección de fraudes en pagos con tarjeta puede experimentar **River Data Drift** si los patrones de compra cambian lentamente con el tiempo debido a nuevas tendencias o hábitos de consumo.

## 🔍 Cómo manejarlo

- **Monitoreo continuo** con herramientas como [[Evidently AI]] o [[River (Python)]] para detectar cambios sutiles.
- **Modelos adaptativos** que ajustan sus pesos a medida que llegan nuevos datos ([[Online Machine Learning]]).
- **Reentrenamiento incremental** en lugar de esperar grandes acumulaciones de datos.

Este concepto está estrechamente relacionado con [[Concept Drift]], ya que puede ocurrir cuando la relación entre las variables de entrada y salida cambia progresivamente.

---

###### 🔗 Más Información
- 📌 [Documentación de RiverML](https://riverml.xyz/0.8.0/examples/concept-drift-detection/)


---
tags:
	#Concept  #MachineLearning #DataDrift 