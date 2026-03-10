links: [[Projects/CurvAI/CurvAI]]


# CurvAI Backend

Backend de [[CurvAI]] construido con [[FastAPI]] y Python 3.12. Expone una API REST que orquesta la generación de rutas moteras combinando un [[LLM]] con la API de enrutamiento [[OpenRouteService]].

## Endpoint principal

El endpoint de generación acepta una descripción en texto libre junto con parámetros opcionales como la ubicación de inicio, distancia aproximada, tipo de carretera y región. Devuelve una ruta completa lista para renderizar y exportar, incluyendo los waypoints, el GeoJSON de la traza y los enlaces de exportación.

## Orquestación de servicios

El router actúa como orquestador sin lógica de negocio propia: delega en servicios especializados para la generación de waypoints (LLM), el cálculo de la ruta real (ORS) y la construcción de los enlaces de exportación. Los errores de cada servicio se capturan por separado y se devuelven con mensajes descriptivos.

## Modelos de datos

Los schemas de entrada y salida están definidos con [[Pydantic]] v2, con validaciones de rango donde corresponde. La respuesta incluye la traza GeoJSON, distancia, duración estimada y los enlaces de exportación.

## Configuración

La configuración se gestiona mediante variables de entorno, con soporte para múltiples proveedores LLM con fallback automático (ver [[CurvAI-LLMService]]).

## Export service

El servicio de exportación genera tres formatos a partir de los waypoints de la ruta:

- **GPX**: estándar compatible con dispositivos Garmin, Wahoo, etc.
- **Google Maps URL**: deep link con origen, destino y waypoints intermedios
- **Apple Maps URL**: deep link con origen y destino

## Tests

Suite de tests con [[pytest]] centrada en los endpoints críticos y las funciones de exportación.


---
tags:
	#Project #CurvAI #FastAPI #Python #Backend #API
