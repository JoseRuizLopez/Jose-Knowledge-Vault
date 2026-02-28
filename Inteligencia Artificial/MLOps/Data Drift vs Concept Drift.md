links: [[001 - 020 Inteligencia Artificial|Inteligencia Artificial]]

# Data Drift vs Concept Drift

La diferencia clave entre **[[Data Drift]]** y **[[Concept Drift]]** radica en **qué aspecto de los datos cambia** y **cómo afecta el rendimiento del modelo**.

## Principales Diferencias
|**Característica**|**Data Drift**|**Concept Drift**|
|:---|:---|:---|
|**Definición**|Cambio en la distribución de las variables de entrada (features) a lo largo del tiempo.|Cambio en la relación entre las variables de entrada (features) y la variable objetivo (target).|
|**Causa principal**|Cambios en la fuente de datos, cambios en la población de usuarios, cambios en sensores, etc.|Cambios en el comportamiento del fenómeno que se está modelando.|
|**Efecto en el modelo**|Puede afectar la calidad de las predicciones si los datos con los que se entrenó el modelo ya no representan la realidad actual.|El modelo deja de ser válido porque la relación entre entrada y salida ha cambiado.|
|**Ejemplo**|Un modelo de predicción de fraudes bancarios entrenado con datos históricos, pero los patrones de transacciones cambian debido a nuevas normativas financieras.|Un modelo de detección de spam donde los correos considerados como spam cambian con el tiempo (nuevas estrategias de phishing).|
|**Métodos de detección**|Pruebas estadísticas como Kolmogorov-Smirnov, divergencia de Kullback-Leibler, análisis de distribuciones.|Seguimiento del error del modelo en el tiempo, métodos como DDM (Drift Detection Method) o ADWIN (Adaptive Windowing).|
|**Solución**|Reentrenamiento con datos actualizados para reflejar los nuevos patrones.|Ajustar el modelo, reentrenarlo o desarrollar modelos adaptativos que respondan a los cambios dinámicos.|

---
tags:
	#Concept  #MachineLearning #Drift #DataDrift

[^1]: https://www.evidentlyai.com/ml-in-production/data-drift

[^2]: https://www.iic.uam.es/procesamiento-del-lenguaje-natural/data-drifting-y-concept-drifting/
	
