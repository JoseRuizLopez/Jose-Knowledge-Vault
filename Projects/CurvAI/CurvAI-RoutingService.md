links: [[Projects/CurvAI/CurvAI]]


# CurvAI Routing Service

Servicio de calculo de rutas de [[CurvAI]] (`services/routing.py`). Toma los waypoints generados por el [[CurvAI-LLMService]] y devuelve la ruta real por carretera en formato [[GeoJSON]] con distancia y duracion estimada.

## Integracion con OpenRouteService

Usa la API de [[OpenRouteService]] (ORS) con el perfil `driving-car` ya que actualmente no existe perfil especifico para motos. Los waypoints se pasan como coordenadas `[lon, lat]` (orden GeoJSON, inverso al habitual lat/lon).

Parametros de la llamada:

```json
{
  "coordinates": [[lon, lat], ...],
  "preference": "recommended",
  "units": "km",
  "language": "es",
  "radiuses": [350, 350, ...]
}
```

El radio de 350 metros por waypoint le indica a ORS que acepte el punto mas cercano con carretera dentro de ese radio, reduciendo rechazos por coordenadas ligeramente desplazadas.

## Mecanismo de tolerancia a fallos

El LLM puede generar waypoints en ubicaciones sin carretera accesible. ORS devuelve un error HTTP con el indice del waypoint problematico en el cuerpo de la respuesta. El servicio implementa un bucle de reintento que:

1. Llama a ORS con la lista completa de coordenadas
2. Si falla, extrae el indice del waypoint inroutable con una expresion regular
3. Elimina ese waypoint de la lista (solo intermedios, nunca origen ni destino)
4. Reintenta con la lista reducida
5. Se detiene cuando la ruta se calcula con exito o cuando quedan menos de 2 puntos

```python
for attempt in range(len(waypoints) - 1):
    try:
        data = await _call_ors(coords)
        break
    except httpx.HTTPStatusError as e:
        bad_idx = _parse_bad_index(str(e))
        if bad_idx is not None and 0 < bad_idx < len(coords) - 1:
            coords.pop(bad_idx)
```

Este patron garantiza que siempre se preservan el punto de inicio y el punto de llegada indicados por el usuario.

## Respuesta

La funcion principal devuelve una tupla `(geojson, distance_km, duration_min)`:

- `geojson`: estructura completa devuelta por ORS, lista para pasar a Mapbox
- `distance_km`: distancia total redondeada a 1 decimal
- `duration_min`: duracion estimada en minutos (convertida desde segundos)

## Consideraciones

- Timeout configurado a 30 segundos para tolerar latencias de ORS en rutas largas
- El cliente `httpx.AsyncClient` se crea y destruye por llamada; en produccion con alta concurrencia conviene usar un cliente compartido con connection pooling
- ORS free tier tiene limites de peticiones por dia; relevante para planificar el escalado


---
tags:
	#ProyectoPersonal #CurvAI #Routing #OpenRouteService #GeoJSON #API
