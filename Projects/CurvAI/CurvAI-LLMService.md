links: [[Projects/CurvAI/CurvAI]]


# CurvAI LLM Service

Servicio de generacion de waypoints de [[CurvAI]] (`services/llm.py`). Recibe la descripcion del usuario y devuelve una lista de coordenadas geograficas junto con el titulo y descripcion de la ruta.

## Estrategia multi-proveedor

El servicio prioriza proveedores gratuitos o de bajo coste antes de caer a OpenAI. La seleccion se hace en tiempo de arranque leyendo las variables de entorno:

```
GEMINI_API_KEY  →  Gemini 1.5 Flash  (via OpenAI-compatible endpoint de Google)
GROQ_API_KEY    →  Llama 3.3 70B     (via Groq, gratuito con limites)
OPENAI_API_KEY  →  GPT-4o            (fallback de calidad maxima)
```

Los tres proveedores se consumen con el mismo cliente `AsyncOpenAI` cambiando `base_url` y `api_key`, lo que simplifica el codigo y facilita anadir nuevos proveedores en el futuro.

## System prompt

El [[prompt]] de sistema define al modelo como experto en rutas de moto y establece restricciones criticas sobre los waypoints:

- Entre 4 y 8 waypoints por ruta
- Coordenadas exclusivamente sobre carreteras reales y accesibles
- Prohibido: mar, lagos, campos sin carretera, zonas privadas
- Preferencia por carreteras de montana, curvas y paisajes
- Sin autopistas ni vias rapidas (salvo acceso al inicio)
- Orden logico desde inicio hasta fin

## Structured output

La llamada usa `response_format={"type": "json_object"}` para forzar al modelo a devolver JSON valido. El schema esperado es:

```json
{
  "title": "string",
  "description": "string",
  "waypoints": [
    {"lat": float, "lon": float, "name": "string", "description": "string"}
  ]
}
```

Temperatura configurada a 0.7 para equilibrar creatividad y coherencia geografica.

## Funcion principal

```python
async def generate_waypoints(request: RouteRequest) -> tuple[str, str, list[Waypoint]]
```

Devuelve `(title, description, waypoints)`. El router envuelve la llamada en try/except y devuelve HTTP 502 si el LLM falla o el JSON es invalido.

## Consideraciones de calidad

El LLM puede generar coordenadas inaccesibles (campo abierto, zona sin carretera). Este problema se resuelve aguas abajo en [[CurvAI-RoutingService]], que detecta y elimina waypoints que ORS no puede enrutar. La estrategia de defensa en profundidad — restricciones en el prompt + tolerancia a fallos en routing — es mas robusta que intentar validar coordenadas solo en un punto.


---
tags:
	#ProyectoPersonal #CurvAI #LLM #OpenAI #Groq #Gemini #PromptEngineering
