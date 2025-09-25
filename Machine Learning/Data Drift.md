links: [[001 - 020 Machine Learning|Machine Learning]]

# Data Drift
> 📌 **Nota:** Para entender mejor las diferencias entre **Data Drift** y **Concept Drift**, revisar [[Data Drift vs Concept Drift]].

El **Data Drift** es el cambio en la **distribución de los datos de entrada** que un modelo de machine learning recibe en producción. Esto significa que, con el tiempo, **las características** o **propiedades estadísticas de los datos** pueden **variar** respecto a los **datos con los que se entrenó el modelo**, lo que puede deteriorar su precisión y rendimiento. Detectar y abordar este fenómeno es crucial para mantener la eficacia del modelo en entornos dinámicos.

## 🔍 Cómo solucionarlo
Para solucionar o evitar el **data drift** se pueden adoptar varias estrategias que ayuden a mantener la fiabilidad del modelo a lo largo del tiempo. Adoptar las estrategias de manera conjunta permite no solo detectar el data drift a tiempo, sino también **mitigar sus efectos** y **mantener la efectividad** del modelo en entornos cambiantes:

### Monitoreo continuo y alertas
Implementar sistemas de monitoreo que comparen periódicamente la **distribución de los datos actuales** con los **datos de entrenamiento**. Esto puede incluir el uso de dashboards y herramientas específicas (como [[Evidently AI]]) que alerten ante cambios significativos en las características de entrada. [^1]
    
### Detección temprana mediante pruebas estadísticas
Utilizar pruebas como el [[test de Kolmogorov-Smirnov]], el [[Índice de Estabilidad de la Población]] (PSI) o **[[Métricas de Similitud|métricas de distancia]]** para detectar diferencias en las **distribuciones de datos**. Estas pruebas permiten identificar cambios incluso antes de que afecten notablemente el rendimiento del modelo. [^2]
    
### Reentrenamiento periódico del modelo
Una vez detectado un drift, actualizar el modelo mediante el **reentrenamiento con datos recientes**. Esto puede implicar el uso de técnicas de aprendizaje continuo o el rediseño del modelo si el cambio en los datos es muy sustancial.
    
### Ingeniería de características robusta
**Seleccionar y transformar** las **características** de modo que sean **menos sensibles** a las variaciones en la distribución. Esto puede incluir técnicas de [[normalización]], [[bucketización]] o la **eliminación de variables** que muestren alta volatilidad.
    
### Modelos adaptativos
Emplear algoritmos que puedan adaptarse **de forma dinámica** a nuevos patrones de datos, como los [[Online Machine Learning|modelos de aprendizaje en línea]] o aquellos que integran **técnicas de actualización continua**.

### Mejora de la calidad del pipeline de datos

Garantizar que el **proceso de adquisición** y **procesamiento de datos** sea **consistente** y esté bien **documentado**. Esto ayuda a minimizar cambios indeseados por errores en la recopilación o transformación de datos.
    


---
tags:
	#Concept  #MachineLearning #Drift #DataDrift

[^1]: https://www.evidentlyai.com/ml-in-production/data-drift

[^2]: https://www.iic.uam.es/procesamiento-del-lenguaje-natural/data-drifting-y-concept-drifting/
	
