links: [[006 Career |Career]]


# Información General
**Fecha de Inicio**: 28/05/2025
**Sueldo Inicial**: 26.000€

# Proyectos
## Onboarding
***Desarrollo de Tareas sueltas***
- Documentación de Test de integración y Unitarios en **MEGARA**.
	- **Stack utilizado**: Desarrollo en **Django**.
- Investigación del uso de NotebookLM para preguntarle sobre los documentos de los proyectos.
- Iniciada la realización del curso de [Aprendizaje automático supervisado: Regresión y clasificación](https://www.coursera.org/learn/machine-learning/home/welcome)


## ENELX
***Ayuda en desarrollo y mantenimiento de un gran proyecto.***
- Añadir información en el informe del Job05.
- Modularización del Job 05 para el uso del Balanceador.
- Optimización de las consultas del Job05.
- **Stack utilizado**: Desarrollo en **Django** con base de datos **PostgreSQL**.
- **Infraestructura**: Modificación de los **Dockerfiles** para usar junto al balanceador.


## Balanceador
***Responsable del Proyecto***
- Desarrollo un proyecto desde 0 para la paralelización de las llamadas a los LLM:
    - Implementación de un sistema de **procesamiento asíncrono de lotes** con control de concurrencia.
    - Desarrollo de un **dispatcher con balanceo de carga** entre endpoints disponibles.
    - Creación de **workers asincrónicos** para ejecución paralela y secuencial según el caso.
    - Uso de una **cola prioritaria** que gestiona la ejecución según urgencia y orden de llegada.
    - Integración con **Azure OpenAI** con reintentos automáticos y manejo de errores.
    - Serialización estandarizada de consultas y resultados para trazabilidad y monitoreo.
- **Resultado**: reducción del tiempo de ejecución de **8 horas a 2 horas**, aumentando 4x la eficiencia en el procesamiento de lotes.
- **Stack utilizado**: Desarrollo en **Django** con base de datos **PostgreSQL**.
- **Infraestructura**: Creación de los **Dockerfiles** para el balanceador, facilitando despliegue y escalabilidad. 


## ALOE GENAI
***Desarrollo Cooperativo bajo un Responsable del Proyecto***
-  Desarrollo de un chatbot para preguntar sobre el estado actual de los procesos del cliente:
    - Creación del **servicio de agente (AgentService)** que orquesta la comunicación con **Vertex AI Gemini** o **Azure OpenAI**.
    - Implementación de un **plugin de acceso a datos (DataAccessPlugin)** que expone funciones del proyecto principal (procesos, casos, alarmas, robots, estadísticas, etc.) como herramientas invocables por el agente.
    - Definición de un **prompt de control estricto** con reglas para que el agente responda únicamente en base a los datos devueltos por las funciones, evitando especulaciones o información irrelevante.
    - Reutilización de funciones ya existentes en el **gran proyecto base** (APIs de procesos, casos, alarmas, máquinas) integradas como llamadas del agente conversacional.
    - Soporte de **desambiguación automática** (ej. selección de proceso, rango temporal, robot) y normalización de fechas/periodos en lenguaje natural.
    - Desarrollo completo sobre **.NET + Semantic Kernel**, integrado con la capa de **DataAccess** ya existente.