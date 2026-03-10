links: [[100  Projects]]


# CurvAI

CurvAI es un generador de rutas moteras impulsado por IA. A diferencia de apps como Rever o Calimoto, el usuario no necesita saber a dónde quiere ir: describe el tipo de experiencia que busca y el sistema construye la ruta de forma automática.

## Vision y propuesta de valor

El flujo central del producto es el siguiente:

1. El usuario describe sus preferencias en texto libre (tipo de carretera, paisaje, distancia, región)
2. Un [[LLM]] interpreta la descripción y genera una serie de waypoints geográficos
3. [[OpenRouteService]] calcula la ruta real por carretera entre esos waypoints
4. El resultado se devuelve como [[GeoJSON]] y se renderiza sobre un mapa [[Mapbox]]
5. El usuario exporta la ruta en formato [[GPX]] o mediante deep links a Google Maps / Apple Maps

El modelo de negocio es freemium: rutas limitadas en el plan gratuito, ilimitadas con suscripción mensual.

## Stack tecnologico

| Capa | Tecnología |
|------|-----------|
| Frontend | [[NextJS]] 14 + [[MapboxGL]] |
| Backend | [[FastAPI]] (Python 3.12) |
| LLM | GPT-4o / Llama 3.3 (Groq) / Gemini 1.5 Flash |
| Routing | [[OpenRouteService]] API |
| Auth + DB | [[Supabase]] (PostgreSQL + Auth) |
| Deploy | Vercel (frontend) + Railway (backend) |

## Arquitectura del sistema

```
frontend/                   # Next.js App Router
├── app/                    # Paginas y layouts
├── components/             # RouteForm, RouteMap, RouteDetails
├── lib/                    # API clients
└── types/                  # TypeScript types

backend/
├── app/
│   ├── main.py             # Entry point FastAPI
│   ├── routers/            # Endpoints por dominio
│   ├── services/
│   │   ├── llm.py          # Integracion con LLM (multi-provider)
│   │   ├── routing.py      # OpenRouteService
│   │   └── export.py       # GPX / deep links
│   ├── models/             # Pydantic schemas
│   └── core/               # Config, settings
└── tests/
```

Notas detalladas por capa:
- [[CurvAI-Backend]] — servicios, endpoints y logica de negocio
- [[CurvAI-LLMService]] — integracion multi-proveedor con OpenAI SDK
- [[CurvAI-RoutingService]] — calculo de rutas y manejo de waypoints inroutable

## Flujo principal (POST /api/routes/generate)

```
RouteRequest (descripcion, start_location, distancia?, road_type?, region?)
  → generate_waypoints()     # LLM → JSON con title, description, waypoints[]
    → calculate_route()      # ORS → GeoJSON + distance_km + duration_min
      → build_export_links() # GPX URL + Google Maps URL + Apple Maps URL
        → RouteResponse
```

El endpoint es tolerante a fallos de routing: si ORS rechaza un waypoint intermedio (sin carretera cercana), el servicio lo elimina automaticamente y reintenta hasta conservar al menos origen y destino.

## Variables de entorno

```bash
# backend/.env
OPENAI_API_KEY=
GROQ_API_KEY=          # fallback gratuito
GEMINI_API_KEY=        # fallback alternativo
ORS_API_KEY=
SUPABASE_URL=
SUPABASE_SERVICE_KEY=

# frontend/.env.local
NEXT_PUBLIC_MAPBOX_TOKEN=
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
NEXT_PUBLIC_API_URL=
```

## Convenciones de desarrollo

- **Commits**: conventional commits (`feat:`, `fix:`, `docs:`, `chore:`, `refactor:`, `test:`)
- **Branches**: nunca push directo a `main`; branch + PR siempre
- **Python**: PEP8, type hints obligatorios, [[Pydantic]] para todos los schemas
- **TypeScript**: strict mode, no `any` sin justificacion, componentes en PascalCase

## Estado actual y proximos pasos

El MVP cubre el flujo completo de generacion y exportacion de rutas. Pendiente:
- Integracion de [[Supabase]] Auth para persistencia de rutas por usuario
- Almacenamiento real de archivos GPX (storage firmado)
- Features comunitarios (rutas compartidas, valoraciones)
- CI/CD con GitHub Actions


---
tags:
	#ProyectoPersonal #AI #Motociclismo #FullStack #FastAPI #NextJS
