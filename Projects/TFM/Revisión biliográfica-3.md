# Revisión de bases de datos, modelos y benchmarks para un TFM neurosimbólico

## 1. Fundamentos teóricos

El **aprendizaje neurosimbólico (NeSy)** combina redes neuronales con técnicas simbólicas para conseguir modelos capaces de aprender de datos, razonar con reglas y ofrecer explicaciones formales [arxiv.org](https://arxiv.org/pdf/2402.01889.pdf#:~:text=paper%2C%20we%20leverage%20the%20implicit,we%20highlight%20the%20effective%20use). La mayor dificultad es la _asimilación_ entre los componentes: los modelos basados en datos requieren grandes cantidades de etiquetas, mientras que los enfoques end‑to‑end pueden escalar mal por la explosión combinatoria en la asignación de símbolos [arxiv.org](https://arxiv.org/pdf/2402.01889.pdf#:~:text=raw%20data%20remains%20an%20open,symbolic%20tasks%20that). Los trabajos recientes intentan resolver este problema mediante técnicas que extraen características simbólicas de modelos fundacionales, como BLIP, y que posteriormente aprenden reglas lógicas con métodos como Answer Set Programming [arxiv.org](https://arxiv.org/pdf/2402.01889.pdf#:~:text=foundation%20models%20to%20enhance%20the,significantly%20reducing%20the%20amount%20of).

La **generación de grafos de escena (Scene Graph Generation, SGG)** detecta objetos y predice relaciones para representar una escena como un grafo con nodos (entidades) y aristas (relaciones). Esta representación facilita tareas de razonamiento visual como pregunta–respuesta, captioning y recuperación de información [neurosymbolic-ai-journal.com](https://neurosymbolic-ai-journal.com/system/files/nai-paper-719.pdf). Las métricas habituales para evaluar SGG son el _Recall@K_, el _mean Recall@K_ (mR@K) y el _zero‑shot Recall@K_ (zR@K) [neurosymbolic-ai-journal.com](https://neurosymbolic-ai-journal.com/system/files/nai-paper-719.pdf#:~:text=ported%20in%20existing%20literature,32%2C%20100).

Los **modelos de conocimiento simbólico** suelen estructurar sus datos mediante grafos de conocimiento (KG), que almacenan hechos como tripletas sujeto–relación–objeto [arxiv.org](https://arxiv.org/pdf/2411.03225.pdf#:~:text=Integrating%20intelligent%20behavior%20into%20AI,perform%20cognitive%20tasks%20more%20effectively). Existen **estándares de representación** como RDF, OWL y SPARQL para codificar y consultar KGs; herramientas como _Neo4j_ y _RDF triple stores_ son comunes para implementarlos. En el ámbito visual, los _scene graphs_ se crean a partir de anotaciones de datasets como Visual Genome y se alinean con conceptos de un KG mediante mapeos de labels.

## 2. Bases de datos y estándares para modelos simbólicos

### 2.1. Knowledge Graphs generales y de sentido común

  
|Dataset/ontología|Características|Utilidad en NeSy|
|---|---|---|
|**Freebase / FB15K**|Subconjunto de la base de datos de Freebase con 14 951 entidades y 1 345 tipos de relación [dgl.ai](https://www.dgl.ai/dgl_docs/generated/dgl.data.FB15kDataset.html#:~:text=The%20FB15K%20dataset%20was%20introduced,for%20each%20edge%20by%20default); se emplea para _link prediction_ y evaluación de embeddings.|Buena cobertura de dominios diversos; base para tareas de completado de KG pero no captura la complejidad multimodal de escenas reales [arxiv.org](https://arxiv.org/pdf/2411.03225.pdf#:~:text=on%20standard%20benchmark%20datasets%20like,7%20Nov%202024).|
|**FB15k‑237**|Versión depurada del FB15K con 237 relaciones para evitar la inferencia trivial; muy usada en benchmarks de enlace.|Sirve como línea base para KGE y permite comparaciones entre modelos.|
|**WordNet / WN18**|Versión de WordNet con 40 943 synsets (nodos) y 18 relaciones [dgl.ai](https://www.dgl.ai/dgl_docs/generated/dgl.data.WN18Dataset.html#:~:text=WN18%20dataset%20statistics%3A).|Proporciona jerarquías semánticas y relaciones léxicas; adecuado para tareas lingüísticas y para experimentar con inferencias en KG.|
|**ConceptNet / CSKG**|Grafo de sentido común que integra DBpedia, Wikidata y otras fuentes. Aporta relaciones como _is‑a_, _part‑of_, _used‑for_.|Fuente habitual para inyectar conocimiento en SGG; métodos como SGG‑CKI utilizan CSKG para mejorar la predicción de relaciones raras [neurosymbolic-ai-journal.com](https://neurosymbolic-ai-journal.com/system/files/nai-paper-719.pdf#:~:text=ported%20in%20existing%20literature,32%2C%20100).|
|**ATOMIC**|Grafo de inferencia social centrado en causas y efectos; útil para tareas de razonamiento sobre acciones humanas.|Permite reforzar modelos que necesitan inferir motivaciones o consecuencias de interacciones humanas.|
|**DBpedia / YAGO / Wikidata**|KG extraídos de Wikipedia; contienen millones de entidades y propiedades.|Sirven de base para KGs generales; se pueden recortar o fusionar con otros KGs para dominios específicos.|

### 2.2. Datasets de grafos de escena y razonamiento visual

El **survey sobre razonamiento visual neurosimbólico** enumera los conjuntos de datos más usados para _Scene Graph Generation_ y tareas asociadas [neurosymbolic-ai-journal.com](https://neurosymbolic-ai-journal.com/system/files/nai-paper-719.pdf#:~:text=%E2%88%97on%20GQA%20dataset%20,110%5D%20110K%20images):

- **Visual Genome (VG)** – 108 000 imágenes con 33 800 categorías de objetos y 42 000 de relaciones [neurosymbolic-ai-journal.com](https://neurosymbolic-ai-journal.com/system/files/nai-paper-719.pdf#:~:text=%E2%88%97on%20GQA%20dataset%20,8K%2042K%20%E2%9C%93%20%E2%9C%93%20%E2%9C%97). Es el benchmark principal para SGG y para tareas con grafos de escena.
    
- **VG150/200/80K** – Versiones reducidas de Visual Genome con 150–200 categorías de objetos y 50–100 relaciones; se usan para entrenar y evaluar modelos que manejan un vocabulario limitado [neurosymbolic-ai-journal.com](https://neurosymbolic-ai-journal.com/system/files/nai-paper-719.pdf#:~:text=88K%20images%20150%2050%20%E2%9C%93,53K%2029K%20%E2%9C%93%20%E2%9C%93%20%E2%9C%97).
    
- **VG‑MSDN** – Dataset de 95 000 imágenes adaptado para multitarea (detección de objetos, captioning y preguntas visuales).
    
- **VRD / NeSy4VRD** – 5 000 imágenes con 100 categorías de objeto y 70 relaciones, diseñado para evaluar algoritmos de predicción de relaciones; _NeSy4VRD_ añade anotaciones simbólicas [neurosymbolic-ai-journal.com](https://neurosymbolic-ai-journal.com/system/files/nai-paper-719.pdf#:~:text=VRD%20,109%2071%20%E2%9C%97%20%E2%9C%97%20%E2%9C%97).
    
- **GQA** – 113 000 imágenes, 1 700 categorías de objeto y 310 relaciones; se considera el estándar en _Visual Question Answering (VQA)_ basado en scene graphs [neurosymbolic-ai-journal.com](https://neurosymbolic-ai-journal.com/system/files/nai-paper-719.pdf#:~:text=113K%20images%201,%E2%9C%93%20%E2%9C%97).
    
- **MS COCO / Flickr30K** – Conjuntos de imágenes con captions; se utilizan principalmente en image captioning [neurosymbolic-ai-journal.com](https://neurosymbolic-ai-journal.com/system/files/nai-paper-719.pdf#:~:text=MS%20COCO%20,%E2%80%93%20%E2%80%93%20%E2%9C%93%20%E2%9C%97%20%E2%9C%97).
    
- **VQA‑v2, VCR, KB‑VQA, FVQA, OK‑VQA, KRVQA** – Datasets para preguntas y respuestas con variada cantidad de imágenes y Q&A, donde las preguntas requieren conocimiento externo o razonamiento multistep [neurosymbolic-ai-journal.com](https://neurosymbolic-ai-journal.com/system/files/nai-paper-719.pdf#:~:text=VQA,%E2%80%93%20%E2%80%93%20%E2%9C%97%20%E2%9C%93%20%E2%9C%93).
    
- **KANDY benchmark** – Genera tareas incrementales inspiradas en patrones de Kandinsky; permite crear curricula de clasificación binaria y analizar la interpretación de conceptos por modelos neurales o simbólicos [arxiv.org](https://arxiv.org/abs/2402.17431#:~:text=,symbolic%20methods%20trained%20over%20time).
    

### 2.3. Datasets neurosimbólicos y entornos específicos

- **DSceneKG (2024)** – Colección de grafos de conocimiento de escenas de conducción construidos a partir de datos multimodales de PandaSet, NuScenes, Waymo Open Dataset y KITTI [arxiv.org](https://arxiv.org/pdf/2411.03225.pdf#:~:text=To%20address%20these%20challenges%2C%20we,LiDAR%2C%20cameras%2C%20and%20GPS%20sensors). Incluye una _Driving Scenes Ontology_ para formalizar los objetos, eventos y condiciones de tráfico y se propone como benchmark realista para tareas de inferencia simbólica y aprendizaje neurosimbólico [arxiv.org](https://arxiv.org/pdf/2411.03225.pdf#:~:text=To%20address%20these%20challenges%2C%20we,a%20more%20realistic%20and%20practical).
    
- **rsbench (NeurIPS 2024)** – Conjunto de tareas diseñadas para evaluar la calidad de conceptos y detectar _reasoning shortcuts_ (RS). Proporciona tareas aritméticas, lógicas y de alto riesgo en las que los modelos pueden resolver la tarea mediante atajos; ofrece métricas y procedimientos de verificación formal para determinar si un modelo utiliza conceptos correctos [arxiv.org](https://arxiv.org/pdf/2406.10368.pdf#:~:text=knowledge%20often%20suffer%20from%20reasoning,solved).
    
- **CUBIC (2025)** – Método y dataset para _identificación de sesgos_ en modelos de visión utilizando _concept embeddings_ y clasificadores lineales. CUBIC detecta conceptos que desvían la representación de la etiqueta principal sin requerir ejemplos de fallos ni anotaciones de sesgos [^1]. Este recurso está asociado con el grupo de Natalia Díaz‑Rodríguez y resulta útil para estudiar sesgos en modelos de visión‑lenguaje.
    
- **NeSy4VRD** – Extiende VRD con anotaciones simbólicas y reglas para evaluar la coherencia lógica de las predicciones [neurosymbolic-ai-journal.com](https://neurosymbolic-ai-journal.com/system/files/nai-paper-719.pdf#:~:text=VRD%20,109%2071%20%E2%9C%97%20%E2%9C%97%20%E2%9C%97).
    
- **TLV datasets** – Conjuntos de vídeo con especificaciones de lógica temporal utilizados en **NSVS‑TL** (Neuro‑Symbolic Video Search con Temporal Logic). Se generan etiquetas de acontecimientos usando lógica temporal y se prueban en vídeos de Waymo y NuScenes [arxiv.org](https://arxiv.org/pdf/2403.11021.pdf#:~:text=herently%20capture%20memory.%20Our%20TL,Video%20Reasoning%20%C2%B7%20Neuro%20Symbolic).
    

### 2.4. Estándares de representación y herramientas

- **RDF/OWL/SPARQL** – Lenguajes estándar para codificar ontologías y consultar KGs.
    
- **Neo4j** – Base de datos de grafos con lenguaje de consulta Cypher; empleada para almacenar y consultar grafos de escena o conocimiento.
    
- **Logic Tensor Networks (LTNs)** – Marco para aprender reglas lógicas con incrustaciones diferenciables; se ha utilizado para inyectar axiomas en SGG, como en la tesis de Turín sobre escenas de tráfico [webthesis.biblio.polito.it](https://webthesis.biblio.polito.it/29354/#:~:text=frequency%2C%20which%20can%20lead%20to,optimal%20axioms%20based%20on%20domain).
    

## 3. Evolución de los modelos neurosimbólicos y panorama bibliográfico

### 3.1. Generación de grafos de escena y neuronas con conocimiento

En SGG, los primeros métodos se basaban únicamente en redes CNN/LSTM y anotaciones de datasets. Estos modelos tienen dificultades para predecir relaciones poco frecuentes o semánticamente complejas [researchrepository.universityofgalway.ie](https://researchrepository.universityofgalway.ie/bitstreams/5c908dc4-877c-411a-adaa-75e413e58592/download). Para abordar estas limitaciones, la investigación reciente incorpora conocimiento externo en forma de KGs o reglas:

- **SGG‑CKI** (2021) utiliza CNNs y LSTM, inyectando un grafo de sentido común (CSKG) mediante acoplamiento laxo; mejora el _recall_ en relaciones inusuales [neurosymbolic-ai-journal.com](https://neurosymbolic-ai-journal.com/system/files/nai-paper-719.pdf#:~:text=ported%20in%20existing%20literature,32%2C%20100).
    
- **DSGAT** (2022) aplica redes de atención de grafos (GAT) con un prior estadístico de relaciones; implementa un acoplamiento estricto y mejora la mR@K [neurosymbolic-ai-journal.com](https://neurosymbolic-ai-journal.com/system/files/nai-paper-719.pdf#:~:text=ported%20in%20existing%20literature,32%2C%20100).
    
- **IRT‑MSK** e **GB‑Net** combinan CNN con GNN y usan ConceptNet, WordNet y Visual Genome como fuentes de conocimiento; permiten el acoplamiento laxo o estricto y demuestran mejoras en mR@K [neurosymbolic-ai-journal.com](https://neurosymbolic-ai-journal.com/system/files/nai-paper-719.pdf#:~:text=ported%20in%20existing%20literature,32%2C%20100).
    
- **KERN** y **KB‑GAN** emplean GNN y técnicas generativas para incorporar estadísticas de relaciones; se centran en reducir el sesgo hacia relaciones frecuentes [neurosymbolic-ai-journal.com](https://neurosymbolic-ai-journal.com/system/files/nai-paper-719.pdf#:~:text=ported%20in%20existing%20literature,32%2C%20100).
    
- **MuRelSGG** (Khan et al., 2025) integra Transformer multimodal y grafo de conocimiento para enriquecer las relaciones. Utiliza módulos M‑HAT y ViT para predecir relaciones y posteriormente alinear con conocimiento de sentido común, logrando mejoras significativas en el _recall_ [researchrepository.universityofgalway.ie](https://researchrepository.universityofgalway.ie/bitstreams/5c908dc4-877c-411a-adaa-75e413e58592/download).
    

### 3.2. Modelos neurosimbólicos generales

Los enfoques NeSy pueden clasificarse en tres categorías [arxiv.org](https://arxiv.org/pdf/2302.07200#:~:text=and%20integrate%20expert%20knowledge,%EF%AC%81eld%20of%20research%20could%20evolve):

1. **Embeddings informados lógicamente** (_Neural for Symbol_): convierten reglas lógicas en vectores y ajustan redes neuronales con restricciones o regularizadores lógicos. Ejemplo: Graph Neural Networks con regularización por relaciones; knowledge graph embeddings con reglas.
    
2. **Restricciones lógicas sobre el entrenamiento** (_Symbol for Neural_): imponen axiomas durante la optimización, como en Logic Tensor Networks (LTNs) y las redes de probabilidad de LTN para grafos de escena [webthesis.biblio.polito.it](https://webthesis.biblio.polito.it/29354/#:~:text=frequency%2C%20which%20can%20lead%20to,optimal%20axioms%20based%20on%20domain).
    
3. **Aprendizaje de reglas** (hibrido): inducen reglas lógicas a partir de datos utilizando modelos neuronales como generadores; por ejemplo, **DeepProbLog**, **Neural Theorem Provers**, **Differentiable ILP**.
    

La tesis del Politécnico de Turín (2023/24) muestra un caso práctico en escenas de conducción: utiliza un Transformer relacional para generar scene graphs y propone dos mecanismos de inyección de conocimiento: (i) un embebido de grafo de conocimiento (KGE) basado en PandaSet y (ii) LTNs con axiomas sobre tráfico. Los dos métodos mejoran la precisión, pero la KGE depende de la cobertura del KG, mientras que LTN requiere un diseño cuidadoso de axiomas [webthesis.biblio.polito.it](https://webthesis.biblio.polito.it/29354/#:~:text=frequency%2C%20which%20can%20lead%20to,optimal%20axioms%20based%20on%20domain).

### 3.3. Modelos de visión‑lenguaje (VLMs) y fusión con razonamiento

Los **modelos de visión‑lenguaje** como **CLIP**, **BLIP**, **Flamingo** o **LLaVA** aprenden alineaciones entre imágenes y texto en espacios embebidos.

- **NeSyGPT** (2024) – Arquitectura que afina el modelo de visión‑lenguaje BLIP para extraer características simbólicas. Estas características se utilizan para aprender reglas lógicas mediante Answer Set Programming y resolver tareas complejas. NeSyGPT necesita menos datos etiquetados, escala a tareas con muchos símbolos y reduce la ingeniería manual [arxiv.org](https://arxiv.org/pdf/2402.01889.pdf#:~:text=foundation%20models%20to%20enhance%20the,significantly%20reducing%20the%20amount%20of). Se demuestra que su precisión supera a métodos base y permite utilizar LLMs para generar preguntas y la interfaz programática [arxiv.org](https://arxiv.org/pdf/2402.01889.pdf).
    
- **NSVS‑TL** (2024) – Marco neurosimbólico para búsqueda de escenas en vídeo. Usa modelos de percepción (VideoLLaMA, ViCLIP) para detectar proposiciones atómicas por fotograma y luego aplica autómatas probabilistas y lógica temporal para razonar sobre la evolución de eventos. La separación de percepción y razonamiento mejora la detección de eventos en secuencias largas y se valida en Waymo y NuScenes [arxiv.org](https://arxiv.org/pdf/2403.11021.pdf#:~:text=tem%20that%20leverages%20vision,code%20is%20available%20at%20GitHub3).
    
- **CUBIC** (2025) – Aprovecha embeddings de VLMs para identificar sesgos sin necesidad de anotaciones específicas. Estudia cómo los conceptos presentes en las imágenes desplazan la representación de la etiqueta principal y detecta conceptos que inducen desviaciones importantes [^1].
    

### 3.4. Revisiones y tendencias

- El **survey de Khan et al. (2025)** ofrece una revisión exhaustiva del razonamiento visual neurosimbólico con grafos de escena y sentido común. Destaca que inyectar conocimiento mejora la expresividad de las representaciones y permite tareas posteriores como VQA y recuperación de imágenes. Clasifica los enfoques según la arquitectura neuronal, la fuente de conocimiento y el acoplamiento NeSy, y discute datasets y métricas [neurosymbolic-ai-journal.com](https://neurosymbolic-ai-journal.com/system/files/nai-paper-719.pdf).
    

### 3.5. Tareas y objetivos del TFM

Para orientar el trabajo de fin de máster se han definido varias **tareas clave**. Estas tareas guiarán la revisión bibliográfica y el diseño experimental:

- **Revisar bases de datos y estándares simbólicos**: examinar datasets para grafos de conocimiento y grafos de escena, así como estándares de representación (RDF, OWL, SPARQL) y tecnologías de almacenamiento (Neo4j, RDF stores).
    
- **Realizar una revisión bibliográfica del campo**: estudiar la evolución de los modelos neurosimbólicos y de visión‑lenguaje, desde los enfoques clásicos (DeepProbLog, LTNs) hasta los más recientes. Se analizarán las tendencias en acoplamiento NeSy y las soluciones híbridas.
    
- **Definir la dirección del proyecto**: decidir si se adopta una **solución concreta** y se optimiza (por ejemplo, elegir un modelo de SGG y mejorarlo con un KG) o si se comparan varias arquitecturas (NeSy frente a VLMs).
    
- **Seleccionar modelos candidatos**: establecer qué modelos de SGG, NeSy y visión‑lenguaje resultan adecuados para el dominio elegido; por ejemplo, _SGTR+_, _MuRelSGG_, _OG‑SGG_ y _SGRec3D_ para grafos de escena, o _NeSyGPT_ y _NSVS‑TL_ como prototipos neurosimbólicos.
    
- **Evaluar y mejorar modelos**: valorar si conviene mejorar un modelo específico (por ejemplo, afinando un SGG con un KG) o combinar varias técnicas; esta decisión dependerá de las necesidades de explicabilidad y del dominio.
    
- **Comparar NeSy frente a modelos de lenguaje‑visión**: analizar las ventajas y limitaciones de los modelos neurosimbólicos (razonamiento explícito, reglas) frente a las redes multimodales (BLIP, LLaVA) que sólo ofrecen salidas neuronales.
    
- **Estudiar benchmarks neurosimbólicos**: identificar benchmarks existentes para evaluar componentes neuronales y simbólicos; analizar qué se mide (precisión, explicación, fidelidad simbólica) y qué métricas faltan.
    
- **Investigar formas de explicabilidad**: revisar técnicas de atribución (Grad‑CAM, SHAP), cuellos de botella conceptuales y modelos que ofrecen reglas lógicas; examinar cómo medir la calidad de la explicación.
    
- **Analizar sesgos con CUBIC**: utilizar el método **CUBIC** para detectar conceptos que inducen sesgos en modelos de visión‑lenguaje  [^1], integrándolo en la evaluación del proyecto.
    
- **Revisar papers clave**: profundizar en trabajos recientes de interés, como **OG‑SGG** [^2], **SGRec3D** [^3](https://arxiv.org/abs/2309.15702), la evaluación holística **HELM** [^4] y el benchmark **BIG‑Bench**[^5](https://arxiv.org/abs/2206.04615#:~:text=benchmark%20%28BIG,can%20be%20improved%20with%20prompting) para disponer de referencias actualizadas.
    
- La **revisión sistemática de 2024** analiza 167 publicaciones de 2020–2024. Concluye que la mayoría de trabajos se centran en aprendizaje e inferencia (63 %) y en lógica y razonamiento (35 %), mientras que las áreas de _explainability_ y _trustworthiness_ están poco exploradas (28 %) [arxiv.org](https://arxiv.org/html/2501.05435v1#:~:text=Results%3A%20From%20an%20initial%20pool,trustworthiness%20with%20other%20research%20areas). La revisión propone investigar la meta‑cognición y la integración de explicabilidad con otras áreas para avanzar hacia modelos fiables y transparentes [arxiv.org](https://arxiv.org/html/2501.05435v1#:~:text=Discussion%3A%20The%20findings%20reveal%20a,processes%2C%20enhancing%20autonomy%20and%20adaptability).
    
- Un estudio de 2024 propone una clasificación de métodos NeSy basada en la explicabilidad, considerando si las representaciones intermedias y las decisiones son explícitas o implícitas. Destaca que aún existen retos para lograr representaciones unificadas y colaboraciones eficientes entre componentes neuronales y simbólicos [arxiv.org](https://arxiv.org/html/2411.04383v1#:~:text=Explainability%20is%20an%20essential%20reason,representations%2C%20enhancing%20model%20explainability%2C%20ethical).
    

## 4. Modelos a considerar y posibles direcciones

### 4.1. Modelos de base

1. **Modelos para generación de grafos de escena**:
    

- **SGG‑CKI**, **DSGAT**, **IRT‑MSK**, **GB‑Net**, **KERN**, **KB‑GAN** – Diferentes combinaciones de CNN/LSTM/GNN con fuentes de conocimiento (CSKG, ConceptNet, WordNet) y acoplamiento laxo o estricto [neurosymbolic-ai-journal.com](https://neurosymbolic-ai-journal.com/system/files/nai-paper-719.pdf#:~:text=ported%20in%20existing%20literature,32%2C%20100).
    
- **SGTR+** (2024): versión basada en Transformer para SGG; ofrece un pipeline end‑to‑end con mayor capacidad de generalización.
    
- **MuRelSGG** (2025): integra Transformers multimodales y grafo de conocimiento, mejorando la predicción de relaciones largas [researchrepository.universityofgalway.ie](https://researchrepository.universityofgalway.ie/bitstreams/5c908dc4-877c-411a-adaa-75e413e58592/download).
    
- **OG‑SGG** (2022): marco guiado por ontologías que refina un generador de grafos de escena existente utilizando axiomas de una ontología. Su estudio de transferencia en **telepresencia robótica** muestra mejoras cuantitativas y cualitativas al incorporar conocimiento experto [^2]
    
- **SGRec3D** (2024): método de **preentrenamiento auto‑supervisado** para predicción de grafos de escena tridimensionales. Reconstruye la escena 3D a partir de un cuello de botella de grafo y no requiere etiquetas de relaciones durante el preentrenamiento; logra mejoras de +10 % en predicción de objetos y +4 % en relaciones usando sólo 10 % de datos etiquetados[^3].
    

2. **Modelos neurosimbólicos**:
    

- **DeepProbLog**, **Logic Tensor Networks**, **Neural Theorem Provers**, **Differentiable ILP** – Aprenden reglas lógicas a partir de datos y permiten entrenamiento supervisado o semi‑supervisado.
    
- **NeSyGPT** – Combina BLIP con Answer Set Programming, aprovechando el conocimiento implícito de modelos fundacionales [arxiv.org](https://arxiv.org/pdf/2402.01889.pdf#:~:text=foundation%20models%20to%20enhance%20the,amount%20of%20manual%20engineering%20required).
    
- **LTNs en tráfico** – La tesis del Politécnico de Turín demuestra que las LTNs permiten inyectar axiomas de conducción y mejoran la coherencia de las predicciones [webthesis.biblio.polito.it](https://webthesis.biblio.polito.it/29354/#:~:text=frequency%2C%20which%20can%20lead%20to,optimal%20axioms%20based%20on%20domain).
    
- **RSBench** – Proporciona tareas personalizables para evaluar si un modelo utiliza atajos de razonamiento y mide la calidad de los conceptos mediante métricas y verificación formal [arxiv.org](https://arxiv.org/pdf/2406.10368.pdf#:~:text=knowledge%20often%20suffer%20from%20reasoning,solved).
    
- **Res‑CBM** (2024) – Modelo con cuellos de botella conceptuales residuales que complementa el banco de conceptos y define la métrica _Concept Utilization Efficiency (CUE)_ para medir la eficiencia descriptiva de los conceptos [arxiv.org](https://arxiv.org/pdf/2404.08978#:~:text=limits%20the%20performance%20of%20CBMs,efficiency%20and%20achieves%20comparable%20perfor).
    

3. **Modelos de visión‑lenguaje**:
    

- **CLIP / BLIP / LLaVA / VideoLLaMA** – Aprenden correspondencias imagen‑texto; pueden utilizarse para extraer conceptos y como módulos de percepción.
    
- **NSVS‑TL** – Ejemplo de integración de VLMs con lógica temporal para búsqueda de vídeos [arxiv.org](https://arxiv.org/pdf/2403.11021.pdf#:~:text=tem%20that%20leverages%20vision,code%20is%20available%20at%20GitHub3).
    

### 4.2. Direcciones posibles para el TFM

1. **Pipeline DL + KG + Razonamiento simbólico**: adoptar un modelo de SGG (como SGTR+ o MuRelSGG) para extraer grafos de escena, alinear sus etiquetas con un KG de dominio (p. ej., CSKG, un KG propio sobre tráfico o patrimonio) y emplear un módulo de razonamiento simbólico (LTN o DeepProbLog) para verificar coherencia e inferir relaciones.
    

- _Ventaja_: Aprovecha la estructura de X‑NeSyL y MonuMAI; genera explicaciones simbólicas.
    
- _Desafío_: Requiere diseñar un KG específico y axiomas de coherencia; se deben equilibrar las métricas de precisión (R@K, mR@K) con la coherencia lógica.
    

2. **Modelos de visión‑lenguaje con lógica**: utilizar VLMs pre‑entrenados (BLIP, LLaVA) para generar descripciones semánticas de imágenes y traducirlas a conceptos simbólicos. Posteriormente, emplear programadores lógicos (ASP, Prolog) o LTNs para razonar sobre las descripciones. El enfoque **NeSyGPT** muestra que un modelo fundacional puede proporcionar conocimiento implícito y que es posible aprender reglas con pocos ejemplos [arxiv.org](https://arxiv.org/pdf/2402.01889.pdf#:~:text=foundation%20models%20to%20enhance%20the,significantly%20reducing%20the%20amount%20of).
    

- _Ventaja_: Reduce la necesidad de etiquetado manual; puede aprovechar LLMs para generar la interfaz simbólica.
    
- _Desafío_: La alineación imagen‑texto puede introducir sesgos; se deben definir métricas para evaluar la calidad de los conceptos extraídos.
    

3. **Evaluación y mitigación de sesgos**: incorporar herramientas como **CUBIC** para identificar conceptos que inducen sesgos en las representaciones y **RSBench** para detectar atajos de razonamiento. Esto permitirá medir la robustez y la equidad del modelo.
    

- _Ventaja_: Añade una dimensión de confianza y ética al proyecto; ofrece la posibilidad de publicar resultados novedosos sobre sesgos.
    
- _Desafío_: Requiere implementar métricas de concept shift y verificación formal.
    

4. **Vídeo y razonamiento temporal**: si el dominio incluye secuencias (por ejemplo, escenas de tráfico con vídeo), se puede explorar **NSVS‑TL**, que separa la percepción de la lógica temporal y demuestra mejoras en la identificación de eventos [arxiv.org](https://arxiv.org/pdf/2403.11021.pdf#:~:text=tem%20that%20leverages%20vision,code%20is%20available%20at%20GitHub3).
    

- _Ventaja_: Permite abordar la interacción humana con el entorno (punto sugerido por las tutoras) y la evolución temporal de eventos.
    
- _Desafío_: Aumenta la complejidad de los datos y exige la definición de especificaciones lógicas temporales.
    

## 5. Benchmarks y métricas de evaluación

### 5.1. Métricas en grafos de escena

- **Recall@K (R@K)** – Proporción de veces en que la relación correcta se encuentra entre las K con mayor confianza [neurosymbolic-ai-journal.com](https://neurosymbolic-ai-journal.com/system/files/nai-paper-719.pdf#:~:text=ported%20in%20existing%20literature,label%20prediction%20but%20also%20a).
    
- **Mean Recall@K (mR@K)** – Promedio de R@K por categoría de relación; mitiga el sesgo hacia relaciones frecuentes [neurosymbolic-ai-journal.com](https://neurosymbolic-ai-journal.com/system/files/nai-paper-719.pdf#:~:text=%E2%80%93%20mR%40K%20is%20the%20average,32%2C%20100).
    
- **Zero‑shot Recall@K (zR@K)** – R@K calculado solo para relaciones no vistas durante el entrenamiento [neurosymbolic-ai-journal.com](https://neurosymbolic-ai-journal.com/system/files/nai-paper-719.pdf#:~:text=%E2%80%93%20zR%40K%20is%20similar%20to,33%2C%20101).
    
- **Accuracy y F1** – Utilizadas en tareas de VQA, captioning y clasificación.
    

### 5.2. Métricas de calidad de conceptos y atajos de razonamiento

- **Concept Utilization Efficiency (CUE)** – Mide la eficiencia con la que los conceptos en un CBM describen la información relevante y permite compararlos con modelos sin cuello de botella [arxiv.org](https://arxiv.org/pdf/2404.08978#:~:text=limits%20the%20performance%20of%20CBMs,efficiency%20and%20achieves%20comparable%20perfor).
    
- **Métricas de RSBench** – Incluyen medidas de consistencia entre la predicción y el conjunto de conceptos correctos; RSBench proporciona también un algoritmo _countrss_ para verificar si una tarea permite atajos de razonamiento [arxiv.org](https://arxiv.org/pdf/2406.10368.pdf#:~:text=knowledge%20often%20suffer%20from%20reasoning,solved).
    
- **Métricas de explicabilidad** – Incluyen métodos de atribución (Grad‑CAM, SHAP, TCAV), comparación entre mapas de saliencia y reglas simbólicas (SHAP‑Backprop en X‑NeSyL), y las categorías de explicabilidad (implícita vs explícita) propuestas por el estudio de Zhang & Sheng [arxiv.org](https://arxiv.org/html/2411.04383v1#:~:text=Explainability%20is%20an%20essential%20reason,representations%2C%20enhancing%20model%20explainability%2C%20ethical).
    

### 5.3. Definición de benchmark y criterios de calidad

Un **benchmark neurosimbólico** se puede formalizar como un triple ⟨_dataset_, _tarea_, _métrica_⟩. Las métricas definen la forma en que se evalúan las salidas del modelo y deben ser coherentes con la tarea. La calidad de un benchmark depende de su diseño y documentación. El informe de Stanford HAI sobre **qué hace bueno un benchmark** identifica varias propiedades necesarias: 
1. **Utilidad descendente**, es decir, relación con aplicaciones reales.
2. **Validez y actualización periódica** de los datos
3. **Interpretabilidad de la puntuación**, de modo que las métricas sean comprensibles y trazables.
4. **Accesibilidad y reproducibilidad**[^6](https://hai.stanford.edu/assets/files/hai-policy-brief-what-makes-a-good-ai-benchmark.pdf#:~:text=tools,The%20core%20themes%20include). 
El estudio **BetterBench** desarrolla un marco de evaluación con **46 prácticas recomendadas** que cubren todo el ciclo de vida del benchmark (diseño, implementación, documentación, mantenimiento y retirada) y evalúa 24 benchmarks; concluye que muchos benchmarks populares carecen de significancia estadística y replicabilidad[^7](https://arxiv.org/html/2411.12990v1#:~:text=AI%20models%20are%20increasingly%20prevalent,assessments%20to%20support%20benchmark%20comparability).

### 5.4. Retos en la evaluación neurosimbólica

Los benchmarks tradicionales tienden a centrarse en medidas simples como _accuracy_ o _F1_ y rara vez distinguen entre la contribución del componente simbólico y el neuronal. Además, a menudo no analizan la sensibilidad del sistema a la calidad semántica de sus componentes ni su robustez ante la deriva de datos. Para evaluar de manera más completa un sistema **NeSy** se recomiendan propiedades complementarias como la **capacidad de explicación** del proceso de aprendizaje, la **verificabilidad** respecto al conocimiento de dominio, la **escalabilidad** mediante reglas ajustables, la **consistencia lógica** y la **adaptabilidad** a versiones dinámicas del benchmark. Estas propiedades amplían las métricas tradicionales y fomentan la fidelidad simbólica y la transparencia.

### 5.5. Benchmarks representativos

La literatura recoge diversos benchmarks neurosimbólicos que abarcan distintos dominios. La tabla siguiente resume algunos ejemplos y sus componentes simbólicos y métricas asociadas:

   
|Categoría|Ejemplos|Componentes simbólicos|Métricas principales|
|---|---|---|---|
|**Ontologías y KG**|OAEI, LLMs4OL, FB15K-237, YAGO3-10|Ontologías, taxonomías, reglas lógicas|Precision, Recall, F1, Hits@N, MRR|
|**Razonamiento visual / QA**|VQA v2, CLEVR, GQA, Outside-Knowledge VQA|Programas funcionales, grafos de escena, KB externos|Accuracy, Option Acc., R@K, mR@K|
|**Parsing y síntesis de programas**|Spider, WikiSQL, CFQ, SCAN|Lógicas formales (SQL, SPARQL)|Exact Match, Execution Acc.|
|**Razonamiento lógico**|CLUTRR, EntailmentBank, ProofWriter|Pruebas formales, árboles de inferencia|Accuracy, Calidad de explicación|
|**Robótica y planificación**|BabyAI, ALFRED, NSVQA, NS‑AP|Planes simbólicos, reglas en lógica de primer orden|Success Rate, Goal‑conditioned Success|

Estos benchmarks muestran la diversidad de tareas evaluadas, pero se han identificado lagunas como la falta de métricas de **fidelidad simbólica** y de **interpretabilidad**. Además, dominios como la biología o el derecho están poco representados, lo que invita a desarrollar nuevas colecciones de datos más amplias.

### 5.6. Marcos de evaluación holística

Recientemente han surgido iniciativas para evaluar modelos de manera holística con múltiples métricas y escenarios. Por ejemplo, el proyecto **HELM** de la Universidad de Stanford proporciona un **marco reproducible y transparente** para evaluar modelos fundacionales en distintos escenarios y modalidades. En su web, HELM ofrece tablas de líderes que combinan numerosas métricas y escenarios, y permite evaluar modelos de texto, audio, visión y combinados; el sitio enfatiza que las tablas son reproducibles y comparables[[4]](https://crfm.stanford.edu/helm/)[^4](https://crfm.stanford.edu/helm/). La página incluye categorías como _Capabilities_, _Audio_, _HEIM_ (text‑to‑image) y _VHELM_ (visión‑lenguaje), lo que muestra la amplitud del marco.

Otro referente es el benchmark **BIG‑Bench**. El artículo _Beyond the Imitation Game_ introduce un conjunto de **204 tareas** aportadas por **450 autores** de **132 instituciones**; las tareas abarcan temas como lingüística, matemáticas, razonamiento común y sesgos sociales[^5](https://arxiv.org/abs/2206.04615#:~:text=benchmark%20%28BIG,can%20be%20improved%20with%20prompting). Al evaluar modelos de distintas arquitecturas (GPT de OpenAI, transformadores densos y dispersos) y tamaños, los autores observan que la precisión y calibración mejoran con el tamaño del modelo pero siguen siendo bajas en términos absolutos. También informan de que los sesgos sociales tienden a aumentar con el tamaño en contextos ambiguos, aunque pueden mitigarse mediante prompts[^5](https://arxiv.org/abs/2206.04615#:~:text=benchmark%20%28BIG,can%20be%20improved%20with%20prompting). Un análisis de Deepgram profundiza en las conclusiones de BIG‑Bench y destaca que el benchmark incluye tareas específicas para medir el **sesgo social**, revelando que el sesgo suele aumentar en modelos grandes pero puede reducirse mediante prompts[^8](https://deepgram.com/learn/big-bench-llm-benchmark-guide#:~:text=Social%20Bias%20Discovered%20Through%20BigBench).

## 6. Explicabilidad y confianza en modelos neurosimbólicos

Aunque el objetivo de la IA neurosimbólica es lograr sistemas interpretables, muchos trabajos se centran en la precisión y descuidan la explicabilidad. La revisión de 2024 señala que sólo el 28 % de los artículos analizados estudian explicabilidad y confianza [arxiv.org](https://arxiv.org/html/2501.05435v1#:~:text=Results%3A%20From%20an%20initial%20pool,trustworthiness%20with%20other%20research%20areas). Existen diferentes niveles de explicabilidad:

1. **Representaciones y decisiones implícitas**: el modelo produce una salida sin exponer internamente conceptos; se puede aplicar saliency maps (Grad‑CAM, SHAP) pero no hay garantía de coherencia con el conocimiento [arxiv.org](https://arxiv.org/html/2411.04383v1#:~:text=Explainability%20is%20an%20essential%20reason,representations%2C%20enhancing%20model%20explainability%2C%20ethical).
    
2. **Representaciones parciales y decisiones parciales**: por ejemplo, redes que generan grafos de escena pero toman decisiones finales de forma opaca.
    
3. **Representaciones o decisiones explícitas**: modelos que exponen conceptos o reglas, como **DeepProbLog**, **LTNs** o **CBMs**. En CBM, el cuello de botella conceptual permite a los usuarios modificar conceptos para estudiar la sensibilidad del modelo [arxiv.org](https://arxiv.org/pdf/2404.08978#:~:text=limits%20the%20performance%20of%20CBMs,efficiency%20and%20achieves%20comparable%20perfor).
    
4. **Representaciones y decisiones totalmente explícitas**: sistemas lógicos donde la interpretación completa es visible (programas ASP). Estos modelos suelen ser más transparentes pero menos flexibles.
    
5. **Representación unificada**: investigación reciente busca una representación común para redes neuronales y lógicas; aún es un reto abierto [arxiv.org](https://arxiv.org/html/2411.04383v1#:~:text=Explainability%20is%20an%20essential%20reason,representations%2C%20enhancing%20model%20explainability%2C%20ethical).
    

El **survey de Khan et al.** señala que la inyección de conocimiento mediante KGs permite generar explicaciones más ricas, al enriquecer los grafos de escena con semántica y reglas [neurosymbolic-ai-journal.com](https://neurosymbolic-ai-journal.com/system/files/nai-paper-719.pdf). Por su parte, **CUBIC** demuestra que detectar sesgos requiere métodos basados en conceptos, ya que los mapas de saliencia no siempre reflejan la lógica del modelo [^1].

## 7. Recomendaciones y plan de trabajo

1. **Definir el dominio y construir un KG propio**: decida si el proyecto se centrará en escenas de tráfico, patrimonio o contextos urbanos. A partir de ahí, diseñe un _grafo de conocimiento_ con entidades, relaciones y reglas adecuadas. Herramientas como Neo4j o RDF permiten crear el KG.
    
2. **Seleccionar un pipeline base**: una opción sólida es adoptar un modelo de **Scene Graph Generation** (p. ej., **SGTR+** o **MuRelSGG**) para extraer grafos de escena, seguido de un módulo de mapeo de etiquetas al KG y un razonador simbólico (LTN, DeepProbLog o ASP). Esta arquitectura permite evaluar tanto la precisión visual como la coherencia lógica, y alinea el TFM con la metodología X‑NeSyL.
    
3. **Evaluar modelos de visión‑lenguaje**: experimente con **BLIP** o **LLaVA** para obtener descripciones semánticas y utilice **NeSyGPT** como inspiración para combinar estos modelos con reglas lógicas. Este enfoque puede reducir la necesidad de anotaciones manuales y facilitar la generación de conceptos.
    
4. **Incorporar métricas de sesgo y concepto**: utilice **CUBIC** para analizar la existencia de sesgos y **RSBench** para examinar si el modelo aprende atajos de razonamiento. Integre estas herramientas en el proceso de evaluación para asegurar que el sistema no sólo sea preciso sino también justo y explicable.
    
5. **Diseñar una estrategia de explicabilidad**: defina cómo se van a generar y presentar las explicaciones. Se pueden emplear técnicas de atribución (Grad‑CAM, SHAP) combinadas con reglas de KG o adoptar modelos con cuello de botella conceptual para obtener explicaciones editables.
    
6. **Plan de desarrollo**:
    

- **Revisión bibliográfica continua**: ampliar la lectura a surveys y artículos recientes (2023–2025) sobre NeSy, SGG y VLMs.
    
- **Construcción del KG y selección de datasets**: elegir datasets como Visual Genome, DSceneKG u otros según el dominio; mapear sus etiquetas al KG.
    
- **Implementación del pipeline**: entrenar el modelo de SGG (o VLM) y desarrollar el razonador simbólico; luego incorporar módulos de explicabilidad y sesgo.
    
- **Evaluación**: aplicar métricas R@K, mR@K, CUE, RSBench, entre otras, y comparar con benchmarks existentes.
    
- **Iteración y mejora**: ajustar el modelo mediante inyección de conocimiento o ampliación del KG; considerar la posibilidad de probar distintos modelos (NeSy vs VLM) y documentar las ventajas e inconvenientes de cada uno.
    

## 8. Conclusiones

El campo de la IA neurosimbólica ha evolucionado rápidamente y se encuentra en plena expansión. Los datasets como **Visual Genome**, **DSceneKG**, **rsbench** y **CUBIC** proporcionan distintos escenarios para evaluar modelos que combinan percepción y razonamiento. Las revisiones recientes destacan la importancia de integrar conocimiento simbólico para mejorar la predicción de relaciones y la explicabilidad [neurosymbolic-ai-journal.com](https://neurosymbolic-ai-journal.com/system/files/nai-paper-719.pdf) [researchrepository.universityofgalway.ie](https://researchrepository.universityofgalway.ie/bitstreams/5c908dc4-877c-411a-adaa-75e413e58592/download), al tiempo que señalan que la investigación en explicabilidad y confianza está todavía en ciernes [arxiv.org](https://arxiv.org/html/2501.05435v1#:~:text=Results%3A%20From%20an%20initial%20pool,trustworthiness%20with%20other%20research%20areas).

Para el TFM, es recomendable diseñar un pipeline que combine **Scene Graph Generation**, **grafos de conocimiento** y **razonamiento simbólico**, integrando también técnicas de **visión‑lenguaje** cuando sea beneficioso. Esta aproximación permitirá explorar la interacción entre distintas fuentes de conocimiento, evaluar las métricas de consistencia y sesgo, y contribuir a la creación de modelos más transparentes y fiables.




[https://arxiv.org/abs/2309.15702](https://arxiv.org/abs/2309.15702)

[https://crfm.stanford.edu/helm/](https://crfm.stanford.edu/helm/)



[https://deepgram.com/learn/big-bench-llm-benchmark-guide](https://deepgram.com/learn/big-bench-llm-benchmark-guide)

[^1]: (https://arxiv.org/pdf/2505.11060.pdf) CUBIC: Concept Embeddings for Unsupervised Bias Identification using VLMs
	

[^2]: (https://arxiv.org/abs/2202.10201#:~:text=,in%20the%20generated%20scene%20graphs) [2202.10201] OG-SGG: Ontology-Guided Scene Graph Generation. A Case Study in Transfer Learning for Telepresence Robotics

[^3]: (https://arxiv.org/abs/2309.15702) [2309.15702] SGRec3D: Self-Supervised 3D Scene Graph Learning via Object-Level Scene Reconstruction

[^4]:(https://crfm.stanford.edu/helm/) Holistic Evaluation of Language Models (HELM)


[^5]:(https://arxiv.org/abs/2206.04615#:~:text=benchmark%20%28BIG,can%20be%20improved%20with%20prompting) [2206.04615] Beyond the Imitation Game: Quantifying and extrapolating the capabilities of language models


[^6]:(https://hai.stanford.edu/assets/files/hai-policy-brief-what-makes-a-good-ai-benchmark.pdf#:~:text=tools,The%20core%20themes%20include) hai-policy-brief-what-makes-a-good-ai-benchmark.pdf


[^7]:(https://arxiv.org/html/2411.12990v1#:~:text=AI%20models%20are%20increasingly%20prevalent,assessments%20to%20support%20benchmark%20comparability) BetterBench: Assessing AI Benchmarks, Uncovering Issues, and Establishing Best Practices



[^8]:(https://deepgram.com/learn/big-bench-llm-benchmark-guide#:~:text=Social%20Bias%20Discovered%20Through%20BigBench) BIG-Bench: The New Benchmark for Language Models | Deepgram