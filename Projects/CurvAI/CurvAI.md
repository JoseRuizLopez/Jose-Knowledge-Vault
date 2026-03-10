links: [[100  Projects]]


# CurvAI

CurvAI es un generador de rutas moteras impulsado por IA. A diferencia de apps como Rever o Calimoto, el usuario no necesita saber a dónde quiere ir: describe el tipo de experiencia que busca y el sistema construye la ruta de forma automática.

## Vision y propuesta de valor

El flujo central del producto es el siguiente:

1. El usuario describe sus preferencias en texto libre (tipo de carretera, paisaje, distancia, región)
2. Un [[LLM]] interpreta la descripción y genera una serie de waypoints geográficos
3. [[OpenRouteService]] calcula la ruta real por carretera entre esos waypoints
4. El resultado se renderiza sobre un mapa [[Mapbox]]
5. El usuario exporta la ruta en formato [[GPX]] o mediante deep links a Google Maps / Apple Maps

## Stack tecnologico

| Capa | Tecnología |
|------|-----------|
| Frontend | [[NextJS]] 14 + [[MapboxGL]] |
| Backend | [[FastAPI]] (Python 3.12) |
| LLM | Multi-proveedor (ver [[CurvAI-LLMService]]) |
| Routing | [[OpenRouteService]] API |
| Auth + DB | [[Supabase]] (PostgreSQL + Auth) |
| Deploy | Vercel (frontend) + Railway (backend) |

## Arquitectura del sistema

El proyecto se divide en dos capas principales:

- **Frontend** (Next.js App Router): gestiona el formulario de entrada, el mapa interactivo y la visualización de resultados.
- **Backend** (FastAPI): expone la API REST que orquesta la generación de rutas combinando el LLM con el servicio de enrutamiento.

Dentro del backend, la lógica está separada en servicios independientes por responsabilidad: generación de waypoints, cálculo de rutas y exportación.

Notas detalladas por capa:
- [[CurvAI-Backend]] — servicios, endpoints y lógica de negocio
- [[CurvAI-LLMService]] — integración multi-proveedor con LLM
- [[CurvAI-RoutingService]] — cálculo de rutas y tolerancia a fallos

## Flujo principal

El endpoint de generación recibe la descripción del usuario y la transforma en una ruta completa en tres pasos encadenados: generación de waypoints mediante LLM, cálculo de la ruta real por carretera, y construcción de los enlaces de exportación. El endpoint es tolerante a fallos de routing: si algún waypoint intermedio no tiene carretera accesible, el sistema lo gestiona automáticamente para mantener al menos el origen y el destino indicados por el usuario.

## Convenciones de desarrollo

- **Commits**: conventional commits (`feat:`, `fix:`, `docs:`, `chore:`, `refactor:`, `test:`)
- **Branches**: nunca push directo a `main`; branch + PR siempre
- **Python**: PEP8, type hints obligatorios, [[Pydantic]] para todos los schemas
- **TypeScript**: strict mode, no `any` sin justificacion, componentes en PascalCase

## Estado actual y proximos pasos

El MVP cubre el flujo completo de generación y exportación de rutas. Pendiente:
- Integración de [[Supabase]] Auth para persistencia de rutas por usuario
- Almacenamiento real de archivos GPX
- Features comunitarios (rutas compartidas, valoraciones)
- CI/CD con GitHub Actions


---
tags:
	#Project #AI #Motociclismo #FullStack #FastAPI #NextJS
