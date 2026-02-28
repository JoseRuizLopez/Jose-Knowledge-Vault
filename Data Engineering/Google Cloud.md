links: [[001 - 040 Data Engineering|Data Engineering]]


# Google Cloud Platform (GCP)

**Google Cloud Platform (GCP)** es la suite de servicios de computación en la nube ofrecida por Google. Utiliza la misma infraestructura global que Google usa internamente para sus productos de usuario final, como el Buscador de Google, Gmail y YouTube.

GCP ofrece un conjunto robusto de servicios modulares que incluyen **computación, almacenamiento de datos, análisis de datos (Big Data) y [[001 - 020 Inteligencia Artificial|Inteligencia Artificial]]**. Está diseñado para ofrecer alta escalabilidad, seguridad y rendimiento, permitiendo a las empresas construir, probar y desplegar aplicaciones en una infraestructura global.

---

## 📌 Servicios Principales

GCP organiza sus servicios en varias categorías principales:

### Computación (Compute)

- **Compute Engine**: Máquinas virtuales (VMs) escalables bajo demanda (IaaS).
    
- **Google Kubernetes Engine (GKE)**: Orquestación de contenedores (basado en Kubernetes) gestionada.
    
- **Cloud Functions**: Plataforma _serverless_ (sin servidor) para ejecutar código en respuesta a eventos (FaaS).
    
- **App Engine**: Plataforma gestionada para desplegar aplicaciones web y móviles (PaaS).
    

### Almacenamiento y Bases de Datos (Storage & Databases)

- **Cloud Storage**: Almacenamiento de objetos escalable y duradero (para _blobs_, backups, archivos estáticos).
    
- **Cloud SQL**: Bases de datos relacionales gestionadas (MySQL, PostgreSQL, SQL Server).
    
- **Cloud Spanner**: Base de datos relacional globalmente distribuida y consistente.
    
- **Firestore / Firebase Realtime Database**: Bases de datos NoSQL flexibles y escalables para aplicaciones web/móviles.
    

### Big Data y Analítica

- **BigQuery**: Almacén de datos (_data warehouse_) _serverless_, altamente escalable y con capacidades de analítica y ML integradas.
    
- **Dataflow**: Servicio de procesamiento de datos por lotes (batch) y en tiempo real (streaming) basado en Apache Beam.
    
- **Pub/Sub**: Servicio de mensajería asíncrona global para ingesta y entrega de eventos.
    

### IA y Machine Learning

- **AI Platform (Vertex AI)**: Plataforma unificada para construir, desplegar y gestionar modelos de [[001 - 020 Inteligencia Artificial|Inteligencia Artificial]].
    
- **APIs de IA**: Modelos pre-entrenados para Visión (Vision AI), Lenguaje (Natural Language AI), y Conversación ([[Dialogflow]]).
    

---

## 📌 Casos de Uso Comunes

- **Desarrollo de Aplicaciones Nativas de la Nube**: Uso de GKE, Cloud Functions y Firestore para crear microservicios y aplicaciones _serverless_.
    
- **Análisis de Big Data**: Ingesta de datos con Pub/Sub, procesamiento con Dataflow y análisis/almacenamiento en BigQuery.
    
- **Inteligencia Artificial**: Entrenamiento de modelos personalizados en [[Vertex AI]] o uso de APIs pre-entrenadas para añadir inteligencia a las aplicaciones.
    
- **Almacenamiento y Recuperación de Desastres**: Uso de Cloud Storage para _backups_ seguros y económicos.
    
- **Infraestructura de Alto Rendimiento**: Despliegue de aplicaciones globales en Compute Engine con balanceo de carga.
    

---

tags:

#Concept #CloudComputing #GCP #DataEngineering #Infraestructura