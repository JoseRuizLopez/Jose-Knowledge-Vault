links: [[Projects/CurvAI/CurvAI]]


# CurvAI Backend

Backend de [[CurvAI]] construido con [[FastAPI]] y Python 3.12. Expone una API REST que orquesta la generacion de rutas moteras combinando un [[LLM]] con la API de enrutamiento [[OpenRouteService]].

## Endpoint principal

### POST /api/routes/generate

Recibe la descripcion del usuario y devuelve una ruta completa lista para renderizar y exportar.

**Request (`RouteRequest`)**

| Campo | Tipo | Requerido | Descripcion |
|-------|------|-----------|-------------|
| `description` | str | Si | Descripcion en texto libre de la experiencia buscada |
| `start_location` | str | Si | Punto de partida (ciudad o coordenadas) |
| `distance_km` | int (10-2000) | No | Distancia aproximada en km |
| `road_type` | str | No | `mountain`, `coastal`, `rural`, `mixed` |
| `region` | str | No | Region o pais |

**Response (`RouteResponse`)**

```json
{
  "title": "Nombre de la ruta",
  "description": "Descripcion evocadora",
  "waypoints": [{"lat": ..., "lon": ..., "name": ..., "description": ...}],
  "geojson": {...},
  "distance_km": 145.3,
  "duration_min": 187,
  "export": {
    "gpx_url": "/api/routes/export/gpx",
    "google_maps_url": "https://www.google.com/maps/dir/?...",
    "apple_maps_url": "maps://?..."
  }
}
```

## Orquestacion de servicios

El router (`routers/routes.py`) actua como orquestador sin logica de negocio propia:

```python
title, description, waypoints = await generate_waypoints(request)   # llm.py
geojson, distance_km, duration_min = await calculate_route(waypoints)  # routing.py
export = build_export_links(title, waypoints)                        # export.py
```

Los errores de cada servicio se capturan por separado y se devuelven con HTTP 502 con mensajes descriptivos.

## Modelos Pydantic

Definidos en `models/route.py` con [[Pydantic]] v2:

- `RouteRequest` — entrada del usuario con validaciones de rango
- `Waypoint` — coordenadas lat/lon con nombre y descripcion opcionales
- `RouteResponse` — respuesta completa con forward reference a `ExportLinks`
- `ExportLinks` — URLs de exportacion (GPX, Google Maps, Apple Maps)

## Configuracion

`core/config.py` usa `pydantic-settings` para leer variables de entorno desde `.env`. Soporta multiples proveedores LLM con fallback automatico (ver [[CurvAI-LLMService]]).

## Export service

`services/export.py` genera tres formatos de exportacion:

- **GPX**: XML estandar con `<wpt>` por cada waypoint, listo para importar en Garmin, Wahoo, etc.
- **Google Maps URL**: deep link con `origin`, `destination` y `waypoints` intermedios
- **Apple Maps URL**: deep link con `saddr` y `daddr` (solo origen y destino; limitacion de la API)

> Pendiente: en produccion el GPX deberia guardarse en Supabase Storage y devolver una URL firmada en lugar del placeholder actual.

## Tests

Suite minima en `tests/test_export.py` con [[pytest]]. La cobertura objetivo son los endpoints criticos y las funciones de exportacion.


---
tags:
	#ProyectoPersonal #CurvAI #FastAPI #Python #Backend #API
