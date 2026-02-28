links: [[001 - 020 Machine Learning|Machine Learning]]

# 🧠 Knowledge-Augmented Generation (KAG)
## ¿Qué es KAG?
**Knowledge-Augmented Generation (KAG)** es un enfoque de generación de texto que incorpora conocimiento estructurado (como grafos de conocimiento, ontologías o bases simbólicas) en el proceso de generación. A diferencia de [[RAG]], que trabaja con documentos textuales no estructurados, KAG se basa en **hechos organizados como entidades y relaciones explícitas**.

## ✨ Motivación
KAG responde a la necesidad de usar **conocimiento preciso, verificable y estructurado**, ideal para contextos donde:

- Se requiere exactitud en relaciones entre conceptos o entidades.
- Es importante el razonamiento lógico o inferencial.
- El dominio está formalizado (como biomedicina, legal, ingeniería).  

## ⚙️ ¿Cómo funciona?
1. **Entrada del usuario**: una consulta o prompt.
2. **Análisis semántico**: se extraen entidades clave.
3. **Consulta estructurada**: se busca en un grafo (ej. SPARQL, Cypher).
4. **Fusión de conocimiento**: se transforman triples en texto o embeddings.
5. **Generación**: el modelo genera texto apoyado en esos datos estructurados.

## 🏗️ Componentes principales
- **Grafo de conocimiento (KG)**: estructura de datos con entidades y relaciones.

- **Motor de consulta**: interpreta preguntas y extrae datos (SPARQL, Cypher).

- **Adaptador de conocimiento**: convierte los datos en formato utilizable por el LLM.

- **Modelo generativo**: produce la respuesta final, condicionado por el conocimiento.

  
  

## 💬 Ejemplo práctico

  

**Pregunta**: “¿Qué premios ha ganado Marie Curie?”

  

- **RAG**: busca fragmentos en Wikipedia y genera una respuesta con base textual.

- **KAG**: consulta un grafo que contiene:

  - `(Marie Curie, recibió, Premio Nobel de Física)`

  - `(Marie Curie, recibió, Premio Nobel de Química)`

  - Luego genera: “Marie Curie ganó dos premios Nobel: uno en Física y otro en Química.”

  
  

## ✅ Ventajas

  

- 📖 **Precisión factual**: los datos vienen de una fuente estructurada.

- 🔗 **Verificabilidad**: se puede rastrear la fuente del dato.

- 🔄 **Reutilización lógica**: útil para tareas inferenciales.

- 📊 **Ideal para dominios formales**.

  
  

## ⚠️ Limitaciones

  

- 🧱 **Dependencia de grafos existentes**: si no existe el conocimiento estructurado, no funciona.

- 🧹 **Mantenimiento del grafo**: requiere actualización constante.

- 🧠 **Curación semántica**: errores en las relaciones afectan las respuestas.

- ⚙️ **Complejidad de integración**: más difícil de implementar que RAG en algunos entornos.

  
  

## 💼 Casos de uso

  

- **Medicina**: con ontologías como SNOMED CT.

- **Legal y normativo**: estructuras de reglas y artículos legales.

- **Científico/técnico**: taxonomías especializadas.

- **XAI (IA explicable)**: donde se necesitan argumentos trazables.

  
  

## 🛠️ Tecnologías asociadas
- **Grafos**: Wikidata, DBpedia, ConceptNet, Neo4j.
- **Consultas**: SPARQL, Cypher.
- **Modelos híbridos**:
	- KG-BART
	- KGT5
	- LLMs + GNNs
- **Frameworks**: [[LangChain]] + Neo4j, [[LlamaIndex]] con bases estructuradas.

  
  

## 🔍 Comparativa KAG vs. RAG
| Criterio           | **[[RAG]]**                                      | **KAG**                                             |
|--------------------|-------------------------------------------|--------------------------------------------------|
| Fuente de datos    | Documentos en texto libre                 | Conocimiento estructurado (grafo, ontología)    |
| Ideal para         | Preguntas abiertas, contexto narrativo    | Preguntas precisas, relaciones semánticas       |
| Precisión factual  | Media (depende del texto)                 | Alta (relaciones explícitas)                    |
| Transparencia      | Parcial (se pueden citar fuentes)         | Alta (datos trazables por entidad-relación)     |
| Curación necesaria | Moderada (indexación de textos)           | Alta (estructuración semántica)                 |

## 🧭 ¿Cuándo usar KAG?
- ✅ Cuando necesitas exactitud, explicabilidad y relaciones claras.
- ✅ En dominios con grafos u ontologías existentes.

- ❌ No es práctico en dominios amplios sin estructuras previas.

  

En muchos sistemas avanzados, se combinan **KAG + RAG**: conocimiento estructurado para precisión, recuperación textual para contexto rico.

  
> KAG ofrece un puente entre los modelos generativos y el conocimiento simbólico: lo mejor de ambos mundos cuando la estructura y la precisión son críticas.


---
tags:
	#Concept  #MachineLearning #KAG  

---

**🔗 References**
- [📘 KG-BART (paper)](https://arxiv.org/abs/2010.12688)
- [📘 KGT5 (paper)](https://arxiv.org/abs/2012.14600)
- [🔗 Wikidata](https://www.wikidata.org)
- [🔧 LangChain con Neo4j](https://python.langchain.com/docs/integrations/graph/neo4j)
- [📚 LlamaIndex](https://docs.llamaindex.ai/)

