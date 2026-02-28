links: [[001 - 020 Inteligencia Artificial|Inteligencia Artificial]]

# Retrieval-Augmented Generation (RAG)

## 🧩 ¿Qué es RAG?
**Retrieval-Augmented Generation (RAG)** es una técnica avanzada en el campo de la inteligencia artificial y procesamiento del lenguaje natural (NLP) que permite a los modelos de lenguaje acceder a información externa antes de generar una respuesta. A diferencia de los modelos generativos tradicionales que responden únicamente con base en lo que aprendieron durante su entrenamiento, **RAG combina generación de texto con búsqueda de información**, lo que lo hace más preciso, flexible y verificable.

  

### ✨ Motivación
Los modelos de lenguaje grandes (LLMs) como GPT, BERT, T5 o BART tienen un conocimiento fijo: una vez entrenados, no pueden aprender nueva información a menos que se reentrenen. Esto representa una limitación en contextos donde:

- Se necesita información actualizada.
- Se requiere conocimiento específico o privado.
- Es importante la trazabilidad de la respuesta.

RAG soluciona este problema permitiendo que el modelo **busque activamente información relevante en una base externa** (por ejemplo, una colección de documentos, bases de datos, APIs o Wikipedia) antes de generar la respuesta.

## ⚙️ ¿Cómo funciona RAG?
Un sistema RAG consta de dos módulos principales:

### 1. **Retriever (recuperador)**
- Toma la pregunta del usuario y la convierte en una consulta.
- Busca documentos relevantes en una base de conocimiento.
- Técnicas comunes: búsqueda semántica con embeddings, BM25, búsqueda híbrida.

### 2. **Generator (generador)**
- Recibe la pregunta original + documentos recuperados.
- Genera una respuesta en lenguaje natural, fundamentada en esos documentos.

### 🔄 Flujo completo
![[rag.png]]

## ✅ Ventajas clave

| Ventaja                          | Descripción |
|----------------------------------|-------------|
| **Actualización sin reentrenar** | Basta con actualizar los documentos, no el modelo. |
| **Conocimiento personalizado** | Puede trabajar con datos de una empresa, proyecto o persona. |
| **Mayor precisión factual**   | Reduce alucinaciones, se basa en hechos reales. |
| **Verificabilidad**           | Permite mostrar las fuentes utilizadas. |
| **Más eficiente**             | No necesitas entrenar modelos gigantes si tienes buenos datos. |

## ⚠️ Limitaciones y desafíos
- **Dependencia del recuperador**: si los documentos recuperados no son relevantes, la respuesta tampoco lo será.
- **Sin razonamiento multihop**: no encadena múltiples búsquedas por sí solo.
- **Infraestructura compleja**: requiere componentes adicionales (vector stores, embeddings, pipelines).
- **Calidad de la base de datos**: necesita una base bien estructurada, fragmentada y mantenida.
- **Privacidad y seguridad**: si se trabaja con datos sensibles, se debe evitar enviar documentos a APIs externas.

## 💼 Casos de uso

### 🧑‍💼 Empresas
- Chatbots de soporte que responden con base en FAQs o manuales internos.
- Asistentes legales o financieros que consultan bases documentales internas.
- Generación de informes ejecutivos con datos actualizados y específicos.

### 🎓 Educación e investigación
- Tutores inteligentes que responden usando libros de texto o papers.
- Asistentes para redactar trabajos académicos con referencias reales.

### 👨‍💻 Desarrollo y documentación técnica
- Respuestas basadas en documentación de APIs.
- Generación de ejemplos o código usando la base documental del proyecto.

### 🧠 Productividad personal
- Búsqueda de información en notas propias, emails o archivos PDF.
- Resumen de múltiples documentos en lenguaje natural.

## 🛠️ Tecnologías y frameworks

### Frameworks para construir sistemas RAG
- **[[Haystack]]**: Orientado a búsqueda y QA; muy usado en entornos empresariales.
- **[[LangChain]]**: Composición de cadenas y agentes con LLMs + recuperación.
- **[[LlamaIndex]]**: Ideal para conectar documentos con un modelo LLM.

### Bases vectoriales
- **[[FAISS]]**: Rápido, ideal para desarrollo local.
- **[[Pinecone]]**, **Weaviate**, **Qdrant**: Escalables, con APIs modernas y funcionalidades avanzadas.
- **[[ElasticSearch]] (con plugin vectorial)**: Combinación de búsqueda lexical y semántica.

### Generadores y embeddings
- **GPT-4**, **GPT-3.5**, **BART**, **T5**.
- **OpenAI Embeddings**, **Cohere**, **SentenceTransformers**.

## 📚 Ejemplo práctico (simplificado)
Supongamos que un usuario pregunta:

> “¿Qué es el metaverso y qué empresas están trabajando en ello?”  

### RAG hará lo siguiente:
1. El **retriever** busca en una colección de artículos recientes o documentación empresarial.
2. Encuentra 5 fragmentos donde se habla de Meta, Microsoft y NVIDIA trabajando en el metaverso.
3. El **generador** recibe esos textos junto con la pregunta.
4. Produce una respuesta como:

> “El metaverso es un entorno virtual inmersivo. Empresas como Meta, Microsoft y NVIDIA están desarrollando tecnologías para soportarlo, desde realidad virtual hasta plataformas 3D colaborativas.”


---
tags:
	#Concept  #MachineLearning #RAG  

---

**🔗 References**
- 📄 [Paper original – Lewis et al. (2020)](https://arxiv.org/abs/2005.11401)
- 🧠 [Haystack](https://docs.haystack.deepset.ai/)
- 🔗 [LangChain](https://docs.langchain.com/)
- 📂 [LlamaIndex](https://docs.llamaindex.ai/)
- 🧾 [Azure + OpenAI + RAG](https://learn.microsoft.com/en-us/azure/architecture/example-scenario/ai/retrieval-augmented-generation)
- 🧪 [Blog IBM sobre RAG](https://research.ibm.com/blog/retrieval-augmented-generation-rag)


[^1]: https://research.ibm.com/blog/retrieval-augmented-generation-RAG#:~:text=RAG%20is%20an%20AI%20framework,insight%20into%20LLMs%27%20generative%20process
	

[^2]: https://js.langchain.com/docs/concepts/rag/#:~:text=Retrieval%20Augmented%20Generation%20,powerful%20technique%20for%20building%20more
	

[^3]: https://learn.microsoft.com/en-us/azure/search/retrieval-augmented-generation-overview#:~:text=Retrieval%20Augmented%20Generation%20,embedding%20models%20for%20that%20content
	

[^4]: https://qdrant.tech/articles/what-is-rag-in-ai/
	

[^5]: https://qdrant.tech/articles/what-is-rag-in-ai/#:~:text=RAG%20architecture
	

[^6]: https://qdrant.tech/articles/what-is-rag-in-ai/#:~:text=With%20the%20top%20relevant%20passages,that%20information%20in%20natural%20language
	

[^7]: https://qdrant.tech/articles/what-is-rag-in-ai/#:~:text=RAG%20architecture
	

[^8]: https://qdrant.tech/articles/what-is-rag-in-ai/#:~:text=The%20Retriever
	

[^9]: https://qdrant.tech/articles/what-is-rag-in-ai/#:~:text=This%20approach%20uses%20large%20language,distance%20metrics%20like%20cosine%20similarity
	

[^10]: https://qdrant.tech/articles/what-is-rag-in-ai/#:~:text=
	

[^11]: https://qdrant.tech/articles/what-is-rag-in-ai/#:~:text=With%20the%20top%20relevant%20passages,that%20information%20in%20natural%20language
	

[^12]: https://huggingface.co/docs/transformers/model_doc/rag#:~:text=Retrieval,to%20adapt%20to%20downstream%20tasks
	

[^13]: https://arxiv.org/abs/2005.11401#:~:text=pre,only%20seq2seq
	
