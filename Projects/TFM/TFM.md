
[[Parte Simbolica]]
# 🧠 Guía para la Reunión con Siham Tabik (TFM Neuro-Simbólico)

**Autor:** José Ruiz López  
**Máster:** DATCOM UGR (modalidad parcial)  
**Propuesta de TFM:**  
> *Modelos neurosimbólicos para el reconocimiento semántico de escenas mediante integración de grafos de conocimiento y redes neuronales profundas.*

---
## 🎯 Objetivo de la reunión

1. Confirmar si Siham Tabik puede ser tutora del TFM.  
2. Validar el **alcance técnico** y **dominio** del proyecto.  
3. Alinear la propuesta con el enfoque de su grupo **[[X-NeSyL]]**.  
4. Obtener **orientación bibliográfica y metodológica**.  

---
## 🧭 Estructura sugerida (20–30 min)

### 1. Introducción breve (2–3 min)
> “Gracias por su tiempo.  
> Me interesa trabajar en la intersección entre visión por computador, conocimiento simbólico y explicabilidad.  
> Creo que su enfoque en [[X-NeSyL]] es ideal para aprender a integrar conocimiento estructurado con aprendizaje profundo.”

---
### 2. Presentación de la propuesta (5 min)

#### 🧩 Idea general
Ir más allá de la detección de objetos, buscando **interpretar escenas completas** mediante la integración:
- *Deep Learning (DL)* → percepción visual.  
- *Knowledge Graphs (KG)* → razonamiento simbólico.  

#### ⚙️ Enfoque técnico
1. **Extracción de scene graphs** desde la imagen.  
2. **Alineación con un KG** de dominio.  
3. **Razonamiento simbólico** (verificación, inferencia, explicabilidad).  

#### 💡 Valor añadido
- Explicaciones estructuradas (“por qué ve lo que ve”).  
- Evaluación de consistencia lógica.  
- IA más confiable y transparente.

---
### 3. Preguntas clave para discutir (10–15 min)
1. ¿Qué dominio recomienda usar (tráfico, urbano, patrimonio)?  
2. ¿Conviene centrarse en toda la integración DL+KG+razonamiento o empezar con una parte (p. ej. alineación)?  
3. ¿Qué métricas considera más adecuadas para medir explicabilidad simbólica?  
4. ¿Cree posible adaptar la metodología **[[X-NeSyL]]** al reconocimiento de relaciones espaciales o funcionales?

---
### 4. Cuestiones prácticas (5 min)
- Acceso a datasets o frameworks de **MonuMAI / [[X-NeSyL]]**.  
- Bibliografía o papers recomendados.  
- Calendario sugerido si empiezo este año.  
- Posibilidad de asistir como oyente a alguna clase/seminario.

---
## 🧠 Conocimientos que debo mostrar

| Área                      | Qué mostrar                                                                                                                   |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| **Deep Learning**         | Conozco arquitecturas para detección/segmentación ([[001 - 0212 Redes Neuronales Convolucionales\|CNN]], [[DETR]], [[CLIP]]). |
| **Scene Graphs**          | Entiendo cómo representar objetos y relaciones en forma de grafo.                                                             |
| **Knowledge Graphs (KG)** | Sé cómo modelar conocimiento explícito (conceptos, relaciones, ontologías).                                                   |
| **Integración NeSy**      | Sé que puede ser superficial (post-hoc) o profunda (en el entrenamiento).                                                     |
| **Explicabilidad**        | Comprendo diferencia entre explicabilidad visual y simbólica.                                                                 |
| **Evaluación**            | Mido precisión y también coherencia o consistencia lógica.                                                                    |

---
## 🧩 Qué es [[X-NeSyL]]

**[[X-NeSyL]] (eXplainable Neuro-Symbolic Learning)**  
Metodología desarrollada por Siham Tabik et al.  
Combina **DL + conocimiento simbólico** para generar modelos **explicables y verificables**.
### Enfoque
1. **Integrar conocimiento experto (KG)** dentro o junto al modelo neuronal.  
2. **Explicar predicciones** con reglas y relaciones del KG.  
3. **Evaluar alineación explicativa** entre lo que “piensa” el modelo y lo que “sabe” el experto.
### Aplicación: MonuMAI
- Clasificación de fachadas monumentales.  
- Integración de reglas arquitectónicas (simbólicas) con redes visuales.  
- Usa **SHAP-Backprop** para alinear explicaciones visuales con el conocimiento.

---
## 🧩 La parte simbólica de mi TFM

### 1. Representación
Definir un **grafo de conocimiento (KG)** con:
- Entidades (`Persona`, `Coche`, `Edificio`, `Semáforo`).  
- Relaciones válidas (`conduce`, `entra_en`, `parte_de`, `junto_a`).  
- Reglas lógicas sobre coherencia:

> Regla: Si A es Persona y B es Coche, la relación válida es 'conduce' o 'entra_en'.
### 2. Alineación semántica
Traducir las **predicciones del modelo neuronal** a **conceptos simbólicos**.

| Label Neuronal | Nodo Simbólico |
|----------------|----------------|
| car | Automóvil |
| person | Persona |
| traffic light | Semáforo |

> Ejemplo: “(person, next_to, car)” → “(Persona, junto_a, Automóvil)”
### 3. Razonamiento y Explicación
- **Verificar** coherencia (“Una Persona puede estar junto a un Automóvil”).  
- **Inferir** nuevas relaciones (“Si hay semáforo rojo, los coches deben estar detenidos”).  
- **Explicar** decisiones (“Es una calle porque hay coches, peatones y semáforos”).  
### 4. Implementación práctica
| Nivel | Herramienta | Dificultad | Aporte |
|-------|--------------|-------------|---------|
| Básico | Python + diccionarios | 🟢 | Verificación |
| Medio | Neo4j / RDF + SPARQL | 🟠 | Consultas semánticas |
| Avanzado | OWL / Prolog / Datalog | 🔴 | Razonamiento completo |

---
## 🧱 Esquema conceptual del sistema

Imagen → Red Neuronal (DETR/[[CLIP]])  → Scene Graph  → Mapeo labels ↔ KG  → Razonamiento simbólico  → Explicación + Evaluación (coherencia, consistencia)

---
## 🧩 Papers Clave

### 🧱 [[X-NeSyL]] y MonuMAI

| Paper | Año | Qué aporta |
|--------|-----|-------------|
| Díaz-Rodríguez et al., “[[X-NeSyL]] methodology” | 2021 | Fusión DL + KG + SHAP-Backprop |
| Lamas et al., “MonuMAI Dataset and Pipeline” | 2020 | Caso práctico: patrimonio |
| Díaz-Rodríguez et al., “[[X-NeSyL]] (Information Fusion)” | 2022 | Formalización del método |
| Lamb et al., “GNNs Meet NeSy” | 2020 | Contexto amplio de integración GNN + simbólico |

---
### 🔍 Referencias recientes (2022–2025)

| Paper | Año | Idea Principal |
|--------|-----|----------------|
| Khan et al., *Survey of Neurosymbolic Visual Reasoning* | 2025 | Estado del arte con scene graphs |
| DeLong et al., *Neurosymbolic AI for Reasoning over KG* | 2023 | Taxonomía de enfoques híbridos |
| Khan et al., *MuRelSGG* | 2025 | SGG neurosimbólico multimodal |
| Belmecheri et al., *Explainable Scene Understanding* | 2025 | Escenas explicables con GNN |
| Cotovio et al., *KG Alignment + NeSy* | 2023 | Alineación simbólica entre grafos |
| Li et al., *SGTR+* | 2024 | SGG end-to-end con Transformer |
| Theory-Driven Regularization (Scene Graphs) | 2023 | Restricciones simbólicas en entrenamiento |

---
## 🧠 Frases útiles para la reunión

- “Mi idea es que la **parte simbólica** actúe como capa de razonamiento y explicabilidad sobre las predicciones neuronales.”  
- “He leído el paper base de [[X-NeSyL]], donde integran explicaciones SHAP con conocimiento experto. Me gustaría **investigar si un enfoque similar** podría emplearse para representar relaciones espaciales y funcionales en escenas.”  
- “Creo que la métrica de **alineación explicativa** podría adaptarse para medir la coherencia entre el scene graph y el KG.”  
- “Me interesa su opinión sobre qué **dominio** sería más adecuado para un primer prototipo (urbano, patrimonio, tráfico…).”  
- “Me gustaría conocer **si existe alguna bibliografía o recursos** de _[[X-NeSyL]]_ o _MonuMAI_ que puedan servirme de referencia para orientar el marco metodológico del trabajo.”

---
## 📩 Seguimiento tras la reunión

1. Agradecer su tiempo.  
2. Resumir acuerdos (tutorización, lecturas, próximos pasos).  
3. Organizar un plan inicial (lectura, dataset, dominio).  

> ✉️ “Gracias por su tiempo. Resumo brevemente los puntos tratados: …”

---
## ✅ Checklist final

- [x] Tener PDF del proyecto abierto.  
- [x] Conocer 2–3 papers de [[X-NeSyL]] y MonuMAI.  
- [x] Saber explicar qué es la parte simbólica.  
- [x] Mostrar claridad conceptual en la integración DL + KG.  
- [ ] Preparar 2–3 preguntas técnicas abiertas.  
- [x] Mostrar disposición a aprender de su enfoque. 
- [ ] Preguntar si puedo asistir a alguna clase suya.
---


# Apuntes de la Reunión
Mis tutoras les interesa publicar.
Pero debemos de buscar para usar algo mas actualizado como transformer o GPTs.

Como idea podríamos usar LlaVa.
Como idea que explorar: 
- So funcionan bien el ChatGPT para clarificarlo con las descripciones de las imágenes (se puede buscar una posible solución)
- Se podría centrar en las interacciones de los humanos con el entorno, como en el caso de Sierra Nevada viendo que tipo de turismo hacen (gastronómico, de actividad, de paisaje, etc).
- Como posible idea se puede probar que cuando falla, con la explicación del fallo aprenda a clarificarlo bien, aunque eso parece que si que no habría que probar ya que si rectifica si se lo dices.

Otra idea sería usar varios Llama que uno aprendido la gastronomía española y otro no, si podemos detectar si olvida conocimiento. Para detectar también si podemos detectas si se ha aprendido conocimiento. Pero esto se desvía demasiado de la idea original, mejor ajustar primero la idea original.

# Tareas
- revisar bases de datos actuales/estándares para modelos simbólicos.
- hacer revisión bibliográfica del campo, modelos, evolución de los modelos, y por donde van,
- determinar por donde tiramos, una solución concreta y las mejoramos o cogemos varios.
- que modelos coger
- mejorar un modelo o que?
- Modelos Nesy o Lenguaje-visión.

- Hay benchmark dentro de Nesy: Para ver que evalúan o que podemos echar en falta, y ver algún estado del arte y poder mejorarlo.
- Busca forma de explicabilidad en esos modelos.
- Buscar sobre [CUBIC](https://arxiv.org/pdf/2505.11060) de Natalia Diaz Rodriguez, sobre los sesgos usando embeddings.
https://ieeexplore.ieee.org/abstract/document/9991965
https://openaccess.thecvf.com/content/WACV2024/papers/Koch_SGRec3D_Self-Supervised_3D_Scene_Graph_Learning_via_Object-Level_Scene_Reconstruction_WACV_2024_paper.pdf

