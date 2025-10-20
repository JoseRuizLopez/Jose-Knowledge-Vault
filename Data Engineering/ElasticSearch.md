links: [[001 - 040 Data Engineering|Data Engineering]]


# Elasticsearch

**Elasticsearch** es un motor de búsqueda y analítica distribuido y de código abierto, construido sobre **Apache Lucene**. Está diseñado para escalabilidad horizontal, fiabilidad y búsqueda en tiempo casi real.

En su núcleo, Elasticsearch almacena datos como documentos **JSON** sin un esquema predefinido (_schema-free_) y utiliza una estructura de datos llamada **índice invertido** (_inverted index_), que le permite realizar búsquedas de texto completo extremadamente rápidas.

Es el componente central del **Elastic Stack** (anteriormente conocido como ELK Stack), que también incluye:

- **Logstash**: Un _pipeline_ de procesamiento de datos del lado del servidor para ingerir datos de múltiples fuentes.
    
- **Kibana**: Una herramienta de visualización de datos y _dashboarding_ que funciona sobre Elasticsearch.
    
- **Beats**: Agentes ligeros y de propósito único para enviar datos.
    

---

## 📌 Características Clave

- **Distribuido por Defecto**: Diseñado para ejecutarse en un clúster, proporcionando alta disponibilidad y escalabilidad horizontal al distribuir datos (en "shards") entre múltiples nodos.
    
- **Búsqueda de Texto Completo**: Ofrece potentes capacidades de búsqueda (tokenización, _ranking_, búsqueda difusa) basadas en Lucene.
    
- **API RESTful**: Todas las operaciones (indexación, búsqueda, gestión) se realizan mediante una API REST simple basada en JSON.
    
- **Tiempo Casi Real (NRT)**: Los documentos suelen estar disponibles para búsqueda menos de un segundo después de ser indexados.
    
- **Motor de Agregación**: Proporciona un potente motor para realizar analíticas (ej. SUM, AVG, histogramas) sobre los datos.
    
- **Schema-Free**: Aunque _se puede_ definir un esquema estricto (un "mapping"), Elasticsearch también puede inferir tipos de datos al vuelo cuando se indexa un documento JSON.
    

---

## Casos de Uso Comunes

- **Análisis de Logs**: El caso de uso más popular (a través del ELK Stack) para ingerir, almacenar y analizar volúmenes masivos de datos de logs para monitorización y _troubleshooting_.
    
- **Búsqueda de Texto Completo**: Alimentar la barra de búsqueda en sitios web, plataformas de comercio electrónico y aplicaciones.
    
- **Inteligencia de Negocio (BI)**: Usado como un _backend_ rápido para _dashboards_ analíticos (ej. en Kibana) para explorar métricas de negocio.
    
- **Análisis de Seguridad (SIEM)**: Analizar eventos de seguridad y logs en tiempo real para detectar amenazas.
    
- **Monitorización de Métricas**: Almacenar y analizar datos de [[Series Temporales]], como métricas de servidores o monitorización del rendimiento de aplicaciones (APM).
    

---

tags:

#Concept #DataEngineering #SearchEngine #Database #NoSQL