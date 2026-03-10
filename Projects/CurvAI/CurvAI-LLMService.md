links: [[Projects/CurvAI/CurvAI]]


# CurvAI LLM Service

Servicio de generación de waypoints de [[CurvAI]]. Recibe la descripción del usuario y devuelve una lista de coordenadas geográficas junto con el título y descripción de la ruta.

## Estrategia multi-proveedor

El servicio soporta varios proveedores de LLM con fallback automático, seleccionados en función de las variables de entorno disponibles al arrancar. Todos los proveedores se consumen a través de la misma interfaz, lo que facilita añadir o sustituir proveedores sin cambios estructurales en el código.

## Generación de waypoints

El modelo recibe el contexto de la solicitud y genera una respuesta estructurada en JSON con el título de la ruta, una descripción y la lista de waypoints. El servicio usa [[structured output]] para garantizar que la respuesta sea siempre JSON válido y parseable sin lógica adicional de extracción.

Las restricciones del modelo están definidas en el system prompt para asegurar que los waypoints generados sean geográficamente coherentes y enrutables en la medida de lo posible.

## Tolerancia a fallos

El LLM puede generar coordenadas en ubicaciones sin carretera accesible. Este problema se resuelve aguas abajo en [[CurvAI-RoutingService]], que detecta y elimina waypoints que el servicio de enrutamiento no puede procesar. La estrategia de defensa en profundidad — restricciones en el prompt + tolerancia a fallos en routing — es más robusta que intentar validar coordenadas en un único punto.


---
tags:
	#Project #CurvAI #LLM #PromptEngineering
