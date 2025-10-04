links: [[003 - 020 Summaries|Summaries]] 

Reference: [[2023-10-EB-Big-Book-of-MLOps-2nd-Edition.pdf|Big Book of MLOps]]

# Resumen del libro "The Big Book of MLOps - 2nd Edition"

### 1. Introducción al MLOps

MLOps es la combinación de **DataOps, DevOps y ModelOps** para mejorar la estabilidad y eficiencia en la producción de modelos de ML. Su objetivo es facilitar la administración de datos, código y modelos en un flujo automatizado.

### 2. Principios fundamentales

- **Automatización y reproducibilidad**: Facilita la integración y el despliegue continuo.
- **Seguridad y gobernanza**: Control de acceso a datos y modelos.
- **Monitorización continua**: Para detectar problemas de rendimiento o sesgos en los modelos.

### 3. Etapas en el ciclo de vida del MLOps

1. **Desarrollo**: Exploración de datos, preprocesamiento, entrenamiento de modelos y experimentación.
2. **Staging (pruebas e integración)**: Validación de modelos, pruebas automatizadas y ajustes.
3. **Producción**: Implementación de modelos, inferencia en tiempo real o en lotes, monitoreo y mantenimiento.

### 4. Estrategias de Despliegue

- **Deploy Code (Recomendado)**: Se actualiza el código sin necesidad de cambiar el modelo.
- **Deploy Models**: Se despliega un modelo nuevo sin modificar el código base.

### 5. Novedades en la Segunda Edición

- **Unity Catalog**: Gestión centralizada de datos y modelos con gobernanza unificada.
- **Model Serving**: Despliegue eficiente de modelos en producción con API en tiempo real.
- **Lakehouse Monitoring**: Herramientas avanzadas de monitoreo para detectar degradaciones en el rendimiento de los modelos.

### 6. Implementación en Databricks

- **Registro de Modelos en Unity Catalog** para facilitar el control de versiones y la auditoría.
- **Model Serving en Databricks** para crear endpoints escalables y seguros.
- **Monitoreo con Lakehouse Monitoring** para detectar sesgos, [[Data Drift]] y problemas en producción.

### 7. LLMOps (Machine Learning para Modelos de Lenguaje)

- **[[RAG|RAG (Retrieval Augmented Generation)]]**: Uso de bases de datos vectoriales para mejorar respuestas de modelos de lenguaje.
- **Fine-tuning vs Pre-training**: Cuándo ajustar un modelo existente y cuándo entrenarlo desde cero.
- **Estrategias de inferencia y reducción de costos**.

### 8. Arquitectura de Referencia

El libro recomienda el uso de tres entornos separados (**desarrollo, staging y producción**) y flujos de trabajo con CI/CD para garantizar calidad y seguridad.

## Puntos Clave para las entrevistas
- **Su importancia**: Acelera la entrega de modelos a producción, reduciendo riesgos y costos operativos.
- **Automatización**: Uso de pipelines para integración y despliegue continuo.
- **Monitoreo**: Identificar y corregir problemas antes de que afecten a los usuarios.
- **Databricks y Unity Catalog**: Herramientas clave para gobernanza y escalabilidad.

---
tags:
	#Writing #Summary