links: [[Projects/CurvAI/CurvAI]]


# CurvAI Routing Service

Servicio de cálculo de rutas de [[CurvAI]]. Toma los waypoints generados por el [[CurvAI-LLMService]] y devuelve la ruta real por carretera en formato [[GeoJSON]] con distancia y duración estimada.

## Integración con OpenRouteService

Usa la API de [[OpenRouteService]] (ORS) con un perfil de conducción por carretera. Los waypoints se pasan en el orden de coordenadas que espera la API, con un radio de tolerancia por punto para reducir rechazos por coordenadas ligeramente desplazadas respecto a la carretera más cercana.

## Tolerancia a fallos

El LLM puede generar waypoints en ubicaciones sin carretera accesible. El servicio implementa un mecanismo de reintento que, ante un error de ORS, identifica el waypoint problemático, lo elimina de la lista y reintenta el cálculo. Este proceso se repite hasta que la ruta se calcula con éxito, siempre preservando el punto de inicio y el punto de llegada indicados por el usuario.

## Respuesta

La función principal devuelve la traza GeoJSON lista para pasar al frontend, junto con la distancia total y la duración estimada del trayecto.

## Consideraciones técnicas

- Timeout configurado para tolerar latencias de ORS en rutas largas
- El cliente HTTP se crea por llamada; en producción con alta concurrencia convendría usar un cliente compartido con connection pooling
- ORS free tier tiene límites de peticiones por día; relevante para planificar el escalado


---
tags:
	#Project #CurvAI #Routing #OpenRouteService #GeoJSON #API
