# Revisión de bases de datos, modelos y benchmarks para un TFM neurosimbólico

## 1. Fundamentos teóricos

El **aprendizaje neurosimbólico (NeSy)** combina redes neuronales con técnicas simbólicas para conseguir modelos capaces de aprender de datos, razonar con reglas y ofrecer explicaciones formales [@amodeoOGSGGOntologyGuidedScene2022] . La mayor dificultad es la _asimilación_ entre los componentes: los modelos basados en datos requieren grandes cantidades de etiquetas, mientras que los enfoques end-to-end pueden escalar mal por la explosión combinatoria en la asignación de símbolos [@amodeoOGSGGOntologyGuidedScene2022] . Los trabajos recientes intentan resolver este problema mediante técnicas que extraen características simbólicas de modelos fundacionales, como BLIP, y que posteriormente aprenden reglas lógicas con métodos como Answer Set Programming [@amodeoOGSGGOntologyGuidedScene2022].

La **generación de grafos de escena (Scene Graph Generation, SGG)** detecta objetos y predice relaciones para representar una escena como un grafo con nodos (entidades) y aristas (relaciones). Esta representación facilita tareas de razonamiento visual como pregunta–respuesta, captioning y recuperación de información [@khanSurveyNeurosymbolicVisual2025]. Las métricas habituales para evaluar SGG son el _Recall@K_, el _mean Recall@K_ (mR@K) y el _zero-shot Recall@K_ (zR@K) [@khanSurveyNeurosymbolicVisual2025].

Los **modelos de conocimiento simbólico** suelen estructurar sus datos mediante grafos de conocimiento (KG), que almacenan hechos como tripletas sujeto–relación–objeto [@wickramarachchiKnowledgeGraphsDriving2024]. Existen **estándares de representación** como [[RDF]], [[OWL]] y SPARQL para codificar y consultar KGs; herramientas como _Neo4j_ y _RDF triple stores_ son comunes para implementarlos. En el ámbito visual, los _scene graphs_ se crean a partir de anotaciones de datasets como Visual Genome y se alinean con conceptos de un KG mediante mapeos de labels.

## 2. Bases de datos y estándares para modelos simbólicos

### 2.1. Knowledge Graphs generales y de sentido común

  
|Dataset/ontología|Características|Utilidad en NeSy|
|---|---|---|
|**Freebase / FB15K**|Subconjunto de la base de datos de Freebase con 14 951 entidades y 1 345 tipos de relación dgl.ai [@FB15kDatasetDGL25]; se emplea para _link prediction_ y evaluación de embeddings.|Buena cobertura de dominios diversos; base para tareas de completado de KG pero no captura la complejidad multimodal de escenas reales arxiv.org [@wickramarachchiKnowledgeGraphsDriving2024].|
|**FB15k-237**|Versión depurada del FB15K con 237 relaciones para evitar la inferencia trivial; muy usada en benchmarks de enlace.|Sirve como línea base para KGE y permite comparaciones entre modelos.|
|**WordNet / WN18**|Versión de WordNet con 40 943 synsets (nodos) y 18 relaciones dgl.ai [@WN18DatasetDGL25].|Proporciona jerarquías semánticas y relaciones léxicas; adecuado para tareas lingüísticas y para experimentar con inferencias en KG.|
|**ConceptNet / CSKG**|Grafo de sentido común que integra DBpedia, Wikidata y otras fuentes. Aporta relaciones como _is-a_, _part-of_, _used-for_.|Fuente habitual para inyectar conocimiento en SGG; métodos como SGG-CKI utilizan CSKG para mejorar la predicción de relaciones raras [@khanSurveyNeurosymbolicVisual2025].|
|**ATOMIC**|Grafo de inferencia social centrado en causas y efectos; útil para tareas de razonamiento sobre acciones humanas.|Permite reforzar modelos que necesitan inferir motivaciones o consecuencias de interacciones humanas.|
|**DBpedia / YAGO / Wikidata**|KG extraídos de Wikipedia; contienen millones de entidades y propiedades.|Sirven de base para KGs generales; se pueden recortar o fusionar con otros KGs para dominios específicos.|

### 2.2. Datasets de grafos de escena y razonamiento visual

El **survey sobre razonamiento visual neurosimbólico** enumera los conjuntos de datos más usados para _Scene Graph Generation_ y tareas asociadas [@khanSurveyNeurosymbolicVisual2025]:

- **Visual Genome (VG)** – 108 000 imágenes con 33 800 categorías de objetos y 42 000 de relaciones [@khanSurveyNeurosymbolicVisual2025]. Es el benchmark principal para SGG y para tareas con grafos de escena.
    
- **VG150/200/80K** – Versiones reducidas de Visual Genome con 150–200 categorías de objetos y 50–100 relaciones; se usan para entrenar y evaluar modelos que manejan un vocabulario limitado [@khanSurveyNeurosymbolicVisual2025].
    
- **VG-MSDN** – Dataset de 95 000 imágenes adaptado para multitarea (detección de objetos, captioning y preguntas visuales).
    
- **VRD / NeSy4VRD** – 5 000 imágenes con 100 categorías de objeto y 70 relaciones, diseñado para evaluar algoritmos de predicción de relaciones; _NeSy4VRD_ añade anotaciones simbólicas [@khanSurveyNeurosymbolicVisual2025].
    
- **GQA** – 113 000 imágenes, 1 700 categorías de objeto y 310 relaciones; se considera el estándar en _Visual Question Answering (VQA)_ basado en scene graphs [@khanSurveyNeurosymbolicVisual2025].
    
- **MS COCO / Flickr30K** – Conjuntos de imágenes con captions; se utilizan principalmente en image captioning [@khanSurveyNeurosymbolicVisual2025].
    
- **VQA-v2, VCR, KB-VQA, FVQA, OK-VQA, KRVQA** – Datasets para preguntas y respuestas con variada cantidad de imágenes y Q&A, donde las preguntas requieren conocimiento externo o razonamiento multistep [@khanSurveyNeurosymbolicVisual2025].
    
- **KANDY benchmark** – Genera tareas incrementales inspiradas en patrones de Kandinsky; permite crear curricula de clasificación binaria y analizar la interpretación de conceptos por modelos neurales o simbólicos [@lorelloKANDYBenchmarkIncremental2024].
    

### 2.3. Datasets neurosimbólicos y entornos específicos

- **DSceneKG (2024)** – Colección de grafos de conocimiento de escenas de conducción construidos a partir de datos multimodales de PandaSet, NuScenes, Waymo Open Dataset y KITTI arxiv.org [@wickramarachchiKnowledgeGraphsDriving2024]. Incluye una _Driving Scenes Ontology_ para formalizar los objetos, eventos y condiciones de tráfico y se propone como benchmark realista para tareas de inferencia simbólica y aprendizaje neurosimbólico arxiv.org [@wickramarachchiKnowledgeGraphsDriving2024].
    
- **rsbench (NeurIPS 2024)** – Conjunto de tareas diseñadas para evaluar la calidad de conceptos y detectar _reasoning shortcuts_ (RS). Proporciona tareas aritméticas, lógicas y de alto riesgo en las que los modelos pueden resolver la tarea mediante atajos; ofrece métricas y procedimientos de verificación formal para determinar si un modelo utiliza conceptos correctos [@bortolottiNeuroSymbolicBenchmarkSuite2024].
    
- **CUBIC (2025)** – Método y dataset para _identificación de sesgos_ en modelos de visión utilizando _concept embeddings_ y clasificadores lineales. CUBIC detecta conceptos que desvían la representación de la etiqueta principal sin requerir ejemplos de fallos ni anotaciones de sesgos [@mendezCUBICConceptEmbeddings2025]. Este recurso está asociado con el grupo de Natalia Díaz-Rodríguez y resulta útil para estudiar sesgos en modelos de visión-lenguaje.
    
- **NeSy4VRD** – Extiende VRD con anotaciones simbólicas y reglas para evaluar la coherencia lógica de las predicciones [@khanSurveyNeurosymbolicVisual2025].
    
- **TLV datasets** – Conjuntos de vídeo con especificaciones de lógica temporal utilizados en **NSVS-TL** (Neuro-Symbolic Video Search con Temporal Logic). Se generan etiquetas de acontecimientos usando lógica temporal y se prueban en vídeos de Waymo y NuScenes [@choiNeuroSymbolicVideoUnderstanding2025].


### 2.4. Estándares de representación y herramientas

- **[[RDF]]/[[OWL]]/SPARQL** – Lenguajes estándar para codificar ontologías y consultar KGs.
    
- **Neo4j** – Base de datos de grafos con lenguaje de consulta Cypher; empleada para almacenar y consultar grafos de escena o conocimiento.
    
- **Logic Tensor Networks (LTNs)** – Marco para aprender reglas lógicas con incrustaciones diferenciables; se ha utilizado para inyectar axiomas en SGG, como en la tesis de Turín sobre escenas de tráfico [@dimasiSceneGraphGeneration2023].

## 3. Evolución de los modelos neurosimbólicos y panorama bibliográfico

### 3.1. Generación de grafos de escena y neuronas con conocimiento

En SGG, los primeros métodos se basaban únicamente en redes CNN/LSTM y anotaciones de datasets. Estos modelos tienen dificultades para predecir relaciones poco frecuentes o semánticamente complejas [@junaidkhanMuRelSGGMultimodalRelationship2025]. Para abordar estas limitaciones, la investigación reciente incorpora conocimiento externo en forma de KGs o reglas:

- **SGG-CKI** (2021) utiliza CNNs y LSTM, inyectando un grafo de sentido común (CSKG) mediante acoplamiento laxo; mejora el _recall_ en relaciones inusuales [@khanSurveyNeurosymbolicVisual2025].
    
- **DSGAT** (2022) aplica redes de atención de grafos (GAT) con un prior estadístico de relaciones; implementa un acoplamiento estricto y mejora la mR@K [@khanSurveyNeurosymbolicVisual2025].
    
- **IRT-MSK** e **GB-Net** combinan CNN con GNN y usan ConceptNet, WordNet y Visual Genome como fuentes de conocimiento; permiten el acoplamiento laxo o estricto y demuestran mejoras en mR@K [@khanSurveyNeurosymbolicVisual2025].
    
- **KERN** y **KB-GAN** emplean GNN y técnicas generativas para incorporar estadísticas de relaciones; se centran en reducir el sesgo hacia relaciones frecuentes [@khanSurveyNeurosymbolicVisual2025].
    
- **MuRelSGG** (Khan et al., 2025) integra Transformer multimodal y grafo de conocimiento para enriquecer las relaciones. Utiliza módulos M-HAT y ViT para predecir relaciones y posteriormente alinear con conocimiento de sentido común, logrando mejoras significativas en el _recall_ researchrepository.universityofgalway.ie [@junaidkhanMuRelSGGMultimodalRelationship2025].
    

### 3.2. Modelos neurosimbólicos generales

Los enfoques NeSy pueden clasificarse en tres categorías [@delongNeurosymbolicAIReasoning2025]:

1. **Embeddings informados lógicamente** (_Neural for Symbol_): convierten reglas lógicas en vectores y ajustan redes neuronales con restricciones o regularizadores lógicos. Ejemplo: Graph Neural Networks con regularización por relaciones; knowledge graph embeddings con reglas.
    
2. **Restricciones lógicas sobre el entrenamiento** (_Symbol for Neural_): imponen axiomas durante la optimización, como en Logic Tensor Networks (LTNs) y las redes de probabilidad de LTN para grafos de escena webthesis.biblio.polito.it [@dimasiSceneGraphGeneration2023].
    
3. **Aprendizaje de reglas** (híbrido): inducen reglas lógicas a partir de datos utilizando modelos neuronales como generadores; por ejemplo, **DeepProbLog**, **Neural Theorem Provers**, **Differentiable ILP**.
    

La tesis del Politécnico de Turín (2023/24) muestra un caso práctico en escenas de conducción: utiliza un Transformer relacional para generar scene graphs y propone dos mecanismos de inyección de conocimiento: 
1. Un embebido de grafo de conocimiento (KGE) basado en PandaSet.
2. LTNs con axiomas sobre tráfico.
 
Los dos métodos mejoran la precisión, pero la KGE depende de la cobertura del KG, mientras que LTN requiere un diseño cuidadoso de axiomas webthesis.biblio.polito.it [@dimasiSceneGraphGeneration2023].

### 3.3. Modelos de visión-lenguaje (VLMs) y fusión con razonamiento

Los **modelos de visión-lenguaje** como **[[CLIP]]**, **BLIP**, **Flamingo** o **LLaVA** aprenden alineaciones entre imágenes y texto en espacios embebidos.

- **NeSyGPT** (2024) – Arquitectura que afina el modelo de visión-lenguaje BLIP para extraer características simbólicas. Estas características se utilizan para aprender reglas lógicas mediante Answer Set Programming y resolver tareas complejas. NeSyGPT necesita menos datos etiquetados, escala a tareas con muchos símbolos y reduce la ingeniería manual [@amodeoOGSGGOntologyGuidedScene2022]. Se demuestra que su precisión supera a métodos base y permite utilizar LLMs para generar preguntas y la interfaz programática [@amodeoOGSGGOntologyGuidedScene2022].
    
- **NSVS-TL** (2024) – Marco neurosimbólico para búsqueda de escenas en vídeo. Usa modelos de percepción (VideoLLaMA, ViCLIP) para detectar proposiciones atómicas por fotograma y luego aplica autómatas probabilistas y lógica temporal para razonar sobre la evolución de eventos. La separación de percepción y razonamiento mejora la detección de eventos en secuencias largas y se valida en Waymo y NuScenes arxiv.org [@choiNeuroSymbolicVideoUnderstanding2025].
    
- **CUBIC** (2025) – Aprovecha embeddings de VLMs para identificar sesgos sin necesidad de anotaciones específicas. Estudia cómo los conceptos presentes en las imágenes desplazan la representación de la etiqueta principal y detecta conceptos que inducen desviaciones importantes [@mendezCUBICConceptEmbeddings2025].
    

### 3.4. Revisiones y tendencias

- El **survey de Khan et al. (2025)** ofrece una revisión exhaustiva del razonamiento visual neurosimbólico con grafos de escena y sentido común. Destaca que inyectar conocimiento mejora la expresividad de las representaciones y permite tareas posteriores como VQA y recuperación de imágenes. Clasifica los enfoques según la arquitectura neuronal, la fuente de conocimiento y el acoplamiento NeSy, y discute datasets y métricas [@khanSurveyNeurosymbolicVisual2025].
    

### 3.5. Síntesis de hallazgos y líneas emergentes

La revisión bibliográfica muestra que los estudios recientes priorizan la integración de conocimiento simbólico y razonamiento explícito para mejorar la **explicabilidad** y la **robustez** de los sistemas neurosimbólicos. A partir del análisis de más de 160 publicaciones entre 2020 y 2025 se identifican tres direcciones principales:

- **Evaluación y detección de sesgos con CUBIC.** El método **CUBIC** [@mendezCUBICConceptEmbeddings2025] emplea _concept embeddings_ para identificar conceptos que inducen sesgos en modelos de visión-lenguaje sin requerir anotaciones de fallos. Este enfoque permite diagnosticar _concept shifts_ y analizar cómo los modelos fundacionales desplazan la representación semántica de la etiqueta principal.
    
- **Revisión de modelos y benchmarks clave.** Destacan propuestas como **OG-SGG** [@amodeoOGSGGOntologyGuidedScene2022], que incorpora axiomas ontológicos para guiar la generación de grafos de escena; **SGRec3D** [@kochSGRec3DSelfSupervised3D2023], que introduce un preentrenamiento auto-supervisado tridimensional; la evaluación holística **HELM** [@HolisticEvaluationLanguage], centrada en reproducibilidad y transparencia; y el benchmark **BIG-Bench** [@BIGBenchNewBenchmark2024], que proporciona una referencia unificada para tareas de razonamiento y sesgos en modelos fundacionales.
    
- **Tendencias en explicabilidad y meta-cognición.** La revisión sistemática de 2024 [@coleloughNeuroSymbolicAI20242025] evidencia que solo un 28 % de los trabajos aborda explícitamente la explicabilidad o la confianza. Se propone investigar la _meta-cognición_ y la integración de explicaciones con otras áreas del aprendizaje para avanzar hacia sistemas más interpretables y fiables. De forma complementaria, el estudio de Zhang y Sheng [@zhangNeuroSymbolicAIExplainability2024] ofrece una clasificación de métodos NeSy según el grado de explicitud de sus representaciones intermedias, destacando el reto aún abierto de lograr **representaciones unificadas** entre componentes neuronales y simbólicos.
    

En conjunto, estas líneas consolidan la necesidad de combinar **métricas de sesgo y fidelidad conceptual** con evaluaciones de **explicabilidad simbólica**, así como de diseñar benchmarks que permitan medir no solo la precisión, sino la coherencia y la transparencia de los modelos.

## 4. Modelos a considerar y posibles direcciones

### 4.1. Modelos de base

1. **Modelos para generación de grafos de escena**:
    

- **SGG-CKI**, **DSGAT**, **IRT-MSK**, **GB-Net**, **KERN**, **KB-GAN** – Diferentes combinaciones de CNN/LSTM/GNN con fuentes de conocimiento (CSKG, ConceptNet, WordNet) y acoplamiento laxo o estricto [@khanSurveyNeurosymbolicVisual2025].
    
- **SGTR+** (2024): versión basada en Transformer para SGG; ofrece un pipeline end-to-end con mayor capacidad de generalización.
    
- **MuRelSGG** (2025): integra Transformers multimodales y grafo de conocimiento, mejorando la predicción de relaciones largas researchrepository.universityofgalway.ie [@junaidkhanMuRelSGGMultimodalRelationship2025].
    
- **OG-SGG** (2022): marco guiado por ontologías que refina un generador de grafos de escena existente utilizando axiomas de una ontología. Su estudio de transferencia en **telepresencia robótica** muestra mejoras cuantitativas y cualitativas al incorporar conocimiento experto [@amodeoOGSGGOntologyGuidedScene2022].
    
- **SGRec3D** (2024): método de **preentrenamiento auto-supervisado** para predicción de grafos de escena tridimensionales. Reconstruye la escena 3D a partir de un cuello de botella de grafo y no requiere etiquetas de relaciones durante el preentrenamiento; logra mejoras de +10 % en predicción de objetos y +4 % en relaciones usando sólo 10 % de datos etiquetados [@kochSGRec3DSelfSupervised3D2023].
    

2. **Modelos neurosimbólicos**:
    

- **DeepProbLog**, **Logic Tensor Networks**, **Neural Theorem Provers**, **Differentiable ILP** – Aprenden reglas lógicas a partir de datos y permiten entrenamiento supervisado o semi-supervisado.
    
- **NeSyGPT** – Combina BLIP con Answer Set Programming, aprovechando el conocimiento implícito de modelos fundacionales [@amodeoOGSGGOntologyGuidedScene2022].
    
- **LTNs en tráfico** – La tesis del Politécnico de Turín demuestra que las LTNs permiten inyectar axiomas de conducción y mejoran la coherencia de las predicciones webthesis.biblio.polito.it [@dimasiSceneGraphGeneration2023].
    
- **RSBench** – Proporciona tareas personalizables para evaluar si un modelo utiliza atajos de razonamiento y mide la calidad de los conceptos mediante métricas y verificación formal arxiv.org [@bortolottiNeuroSymbolicBenchmarkSuite2024].
    
- **Res-CBM** (2024) – Modelo con cuellos de botella conceptuales residuales que complementa el banco de conceptos y define la métrica _Concept Utilization Efficiency (CUE)_ para medir la eficiencia descriptiva de los conceptos [@shangIncrementalResidualConcept2024].
    

3. **Modelos de visión-lenguaje**:
    

- **[[CLIP]] / BLIP / LLaVA / VideoLLaMA** – Aprenden correspondencias imagen-texto; pueden utilizarse para extraer conceptos y como módulos de percepción.
    
- **NSVS-TL** – Ejemplo de integración de VLMs con lógica temporal para búsqueda de vídeos arxiv.org [@choiNeuroSymbolicVideoUnderstanding2025]..
    

### 4.2. Direcciones posibles para el TFM

#### 1. Pipeline DL + KG + Razonamiento simbólico
Adoptar un modelo de SGG (como SGTR+ o MuRelSGG) para extraer grafos de escena, alinear sus etiquetas con un KG de dominio (p. ej., CSKG, un KG propio sobre tráfico o patrimonio) y emplear un módulo de razonamiento simbólico (LTN o DeepProbLog) para verificar coherencia e inferir relaciones.
    

- _Ventaja_: Aprovecha la estructura de X-NeSyL y MonuMAI; genera explicaciones simbólicas.
    
- _Desafío_: Requiere diseñar un KG específico y axiomas de coherencia; se deben equilibrar las métricas de precisión (R@K, mR@K) con la coherencia lógica.
    

#### 2. Modelos de visión-lenguaje con lógica:
Utilizar VLMs pre-entrenados (BLIP, LLaVA) para generar descripciones semánticas de imágenes y traducirlas a conceptos simbólicos. Posteriormente, emplear programadores lógicos (ASP, Prolog) o LTNs para razonar sobre las descripciones. El enfoque **NeSyGPT** muestra que un modelo fundacional puede proporcionar conocimiento implícito y que es posible aprender reglas con pocos ejemplos [@amodeoOGSGGOntologyGuidedScene2022].
    

- _Ventaja_: Reduce la necesidad de etiquetado manual; puede aprovechar LLMs para generar la interfaz simbólica.
    
- _Desafío_: La alineación imagen-texto puede introducir sesgos; se deben definir métricas para evaluar la calidad de los conceptos extraídos.
    

#### 3. Evaluación y mitigación de sesgos
Incorporar herramientas como **CUBIC** para identificar conceptos que inducen sesgos en las representaciones y **RSBench** para detectar atajos de razonamiento. Esto permitirá medir la robustez y la equidad del modelo.
    

- _Ventaja_: Añade una dimensión de confianza y ética al proyecto; ofrece la posibilidad de publicar resultados novedosos sobre sesgos.
    
- _Desafío_: Requiere implementar métricas de concept shift y verificación formal.
    

#### 4. Vídeo y razonamiento temporal
Si el dominio incluye secuencias (por ejemplo, escenas de tráfico con vídeo), se puede explorar **NSVS-TL**, que separa la percepción de la lógica temporal y demuestra mejoras en la identificación de eventos arxiv.org [@choiNeuroSymbolicVideoUnderstanding2025]..
    

- _Ventaja_: Permite abordar la interacción humana con el entorno (punto sugerido por las tutoras) y la evolución temporal de eventos.
    
- _Desafío_: Aumenta la complejidad de los datos y exige la definición de especificaciones lógicas temporales.
    

## 5. Benchmarks y métricas de evaluación

### 5.1. Métricas en grafos de escena

- **Recall@K (R@K)** – Proporción de veces en que la relación correcta se encuentra entre las K con mayor confianza [@khanSurveyNeurosymbolicVisual2025].
    
- **Mean Recall@K (mR@K)** – Promedio de R@K por categoría de relación; mitiga el sesgo hacia relaciones frecuentes [@khanSurveyNeurosymbolicVisual2025].
    
- **Zero-shot Recall@K (zR@K)** – R@K calculado solo para relaciones no vistas durante el entrenamiento [@khanSurveyNeurosymbolicVisual2025].
    
- **Accuracy y F1** – Utilizadas en tareas de VQA, captioning y clasificación.
    

### 5.2. Métricas de calidad de conceptos y atajos de razonamiento

- **Concept Utilization Efficiency (CUE)** – Mide la eficiencia con la que los conceptos en un CBM describen la información relevante y permite compararlos con modelos sin cuello de botella arxiv.org [@shangIncrementalResidualConcept2024].
    
- **Métricas de RSBench** – Incluyen medidas de consistencia entre la predicción y el conjunto de conceptos correctos; RSBench proporciona también un algoritmo _countrss_ para verificar si una tarea permite atajos de razonamiento arxiv.org [@bortolottiNeuroSymbolicBenchmarkSuite2024].
    
- **Métricas de explicabilidad** – Incluyen métodos de atribución (Grad-CAM, SHAP, TCAV), comparación entre mapas de saliencia y reglas simbólicas (SHAP-Backprop en X-NeSyL), y las categorías de explicabilidad (implícita vs explícita) propuestas por el estudio de Zhang & Sheng arxiv.org [@zhangNeuroSymbolicAIExplainability2024].
    

### 5.3. Definición de benchmark y criterios de calidad

Un **benchmark neurosimbólico** se puede formalizar como un triple ⟨_dataset_, _tarea_, _métrica_⟩. Las métricas definen la forma en que se evalúan las salidas del modelo y deben ser coherentes con la tarea. La calidad de un benchmark depende de su diseño y documentación. El informe de Stanford HAI sobre **qué hace bueno un benchmark** identifica varias propiedades necesarias: 
1. **Utilidad descendente**, es decir, relación con aplicaciones reales.
2. **Validez y actualización periódica** de los datos
3. **Interpretabilidad de la puntuación**, de modo que las métricas sean comprensibles y trazables.
4. **Accesibilidad y reproducibilidad** [@reuelBetterBenchAssessingAI2024] 
El estudio **BetterBench** desarrolla un marco de evaluación con **46 prácticas recomendadas** que cubren todo el ciclo de vida del benchmark (diseño, implementación, documentación, mantenimiento y retirada) y evalúa 24 benchmarks; concluye que muchos benchmarks populares carecen de significancia estadística y replicabilidad [@reuelBetterBenchAssessingAI2024].

### 5.4. Retos en la evaluación neurosimbólica

Los benchmarks tradicionales tienden a centrarse en medidas simples como _accuracy_ o _F1_ y rara vez distinguen entre la contribución del componente simbólico y el neuronal. Además, a menudo no analizan la sensibilidad del sistema a la calidad semántica de sus componentes ni su robustez ante la deriva de datos. Para evaluar de manera más completa un sistema **NeSy** se recomiendan propiedades complementarias como la **capacidad de explicación** del proceso de aprendizaje, la **verificabilidad** respecto al conocimiento de dominio, la **escalabilidad** mediante reglas ajustables, la **consistencia lógica** y la **adaptabilidad** a versiones dinámicas del benchmark. Estas propiedades amplían las métricas tradicionales y fomentan la fidelidad simbólica y la transparencia.

### 5.5. Benchmarks representativos

La literatura recoge diversos benchmarks neurosimbólicos que abarcan distintos dominios. La tabla siguiente resume algunos ejemplos y sus componentes simbólicos y métricas asociadas:

|Categoría|Ejemplos|Componentes simbólicos|Métricas principales|
|---|---|---|---|
|**Ontologías y KG**|OAEI, LLMs4OL, FB15K-237, YAGO3-10|Ontologías, taxonomías, reglas lógicas|Precision, Recall, F1, Hits@N, MRR|
|**Razonamiento visual / QA**|VQA v2, CLEVR, GQA, Outside-Knowledge VQA|Programas funcionales, grafos de escena, KB externos|Accuracy, Option Acc., R@K, mR@K|
|**Parsing y síntesis de programas**|Spider, WikiSQL, CFQ, SCAN|Lógicas formales (SQL, SPARQL)|Exact Match, Execution Acc.|
|**Razonamiento lógico**|CLUTRR, EntailmentBank, ProofWriter|Pruebas formales, árboles de inferencia|Accuracy, Calidad de explicación|
|**Robótica y planificación**|BabyAI, ALFRED, NSVQA, NS-AP|Planes simbólicos, reglas en lógica de primer orden|Success Rate, Goal-conditioned Success|

Estos benchmarks muestran la diversidad de tareas evaluadas, pero se han identificado lagunas como la falta de métricas de **fidelidad simbólica** y de **interpretabilidad**. Además, dominios como la biología o el derecho están poco representados, lo que invita a desarrollar nuevas colecciones de datos más amplias.

### 5.6. Marcos de evaluación holística

Recientemente han surgido iniciativas para evaluar modelos de manera holística con múltiples métricas y escenarios. Por ejemplo, el proyecto **HELM** de la Universidad de Stanford proporciona un **marco reproducible y transparente** para evaluar modelos fundacionales en distintos escenarios y modalidades. En su web, HELM ofrece tablas de líderes que combinan numerosas métricas y escenarios, y permite evaluar modelos de texto, audio, visión y combinados; el sitio enfatiza que las tablas son reproducibles y comparables [@HolisticEvaluationLanguage]. La página incluye categorías como _Capabilities_, _Audio_, _HEIM_ (text-to-image) y _VHELM_ (visión-lenguaje), lo que muestra la amplitud del marco.

Otro referente es el benchmark **BIG-Bench**. El artículo _Beyond the Imitation Game_ introduce un conjunto de **204 tareas** aportadas por **450 autores** de **132 instituciones**; las tareas abarcan temas como lingüística, matemáticas, razonamiento común y sesgos sociales [@srivastavaImitationGameQuantifying2023]. Al evaluar modelos de distintas arquitecturas (GPT de OpenAI, transformadores densos y dispersos) y tamaños, los autores observan que la precisión y calibración mejoran con el tamaño del modelo pero siguen siendo bajas en términos absolutos. También informan de que los sesgos sociales tienden a aumentar con el tamaño en contextos ambiguos, aunque pueden mitigarse mediante prompts [@BIGBenchNewBenchmark2024].

## 6. Explicabilidad y confianza en modelos neurosimbólicos

Aunque el objetivo de la IA neurosimbólica es lograr sistemas interpretables, muchos trabajos se centran en la precisión y descuidan la explicabilidad. La revisión de 2024 señala que sólo el 28 % de los artículos analizados estudian explicabilidad y confianza [@coleloughNeuroSymbolicAI20242025]. Existen diferentes niveles de explicabilidad:

1. **Representaciones y decisiones implícitas**: el modelo produce una salida sin exponer internamente conceptos; se puede aplicar saliency maps (Grad-CAM, SHAP) pero no hay garantía de coherencia con el conocimiento arxiv.org [@zhangNeuroSymbolicAIExplainability2024].
    
2. **Representaciones parciales y decisiones parciales**: por ejemplo, redes que generan grafos de escena pero toman decisiones finales de forma opaca.
    
3. **Representaciones o decisiones explícitas**: modelos que exponen conceptos o reglas, como **DeepProbLog**, **LTNs** o **CBMs**. En CBM, el cuello de botella conceptual permite a los usuarios modificar conceptos para estudiar la sensibilidad del modelo arxiv.org [@shangIncrementalResidualConcept2024].
    
4. **Representaciones y decisiones totalmente explícitas**: sistemas lógicos donde la interpretación completa es visible (programas ASP). Estos modelos suelen ser más transparentes pero menos flexibles.
    
5. **Representación unificada**: investigación reciente busca una representación común para redes neuronales y lógicas; aún es un reto abierto arxiv.org [@zhangNeuroSymbolicAIExplainability2024].
    

El **survey de Khan et al.** señala que la inyección de conocimiento mediante KGs permite generar explicaciones más ricas, al enriquecer los grafos de escena con semántica y reglas [@khanSurveyNeurosymbolicVisual2025]. Por su parte, **CUBIC** demuestra que detectar sesgos requiere métodos basados en conceptos, ya que los mapas de saliencia no siempre reflejan la lógica del modelo [@mendezCUBICConceptEmbeddings2025].


## 7. Conclusiones

El campo de la IA neurosimbólica ha evolucionado rápidamente y se encuentra en plena expansión. Los datasets como **Visual Genome**, **DSceneKG**, **rsbench** y **CUBIC** proporcionan distintos escenarios para evaluar modelos que combinan percepción y razonamiento. Las revisiones recientes destacan la importancia de integrar conocimiento simbólico para mejorar la predicción de relaciones y la explicabilidad [@khanSurveyNeurosymbolicVisual2025]. researchrepository.universityofgalway.ie [@junaidkhanMuRelSGGMultimodalRelationship2025], al tiempo que señalan que la investigación en explicabilidad y confianza está todavía en ciernes [@coleloughNeuroSymbolicAI20242025].

## 8.Bibliografía
[1]

F. Amodeo, F. Caballero, N. Díaz-Rodríguez, and L. Merino, “OG-SGG: Ontology-Guided Scene Graph Generation. A Case Study in Transfer Learning for Telepresence Robotics,” _IEEE Access_, vol. 10, pp. 132564–132583, 2022, doi: [10.1109/ACCESS.2022.3230590](https://doi.org/10.1109/ACCESS.2022.3230590). Available: [http://arxiv.org/abs/2202.10201](http://arxiv.org/abs/2202.10201). [Accessed: Oct. 08, 2025]

[2]

M. J. Khan, F. Ilievski, J. G. Breslin, and E. Curry, “A survey of neurosymbolic visual reasoning with scene graphs and common sense knowledge,” _Neurosymbolic Artificial Intelligence_, vol. 1, p. NAI-240719, Mar. 2025, doi: [10.3233/NAI-240719](https://doi.org/10.3233/NAI-240719). Available: [https://journals.sagepub.com/doi/10.3233/NAI-240719](https://journals.sagepub.com/doi/10.3233/NAI-240719). [Accessed: Oct. 08, 2025]

[3]

R. Wickramarachchi, C. Henson, and A. Sheth, “Knowledge Graphs of Driving Scenes to Empower the Emerging Capabilities of Neurosymbolic AI.” arXiv, Nov. 07, 2024. doi: [10.48550/arXiv.2411.03225](https://doi.org/10.48550/arXiv.2411.03225). Available: [http://arxiv.org/abs/2411.03225](http://arxiv.org/abs/2411.03225). [Accessed: Oct. 08, 2025]

[4]

“FB15kDataset — DGL 2.5 documentation.” Available: [https://www.dgl.ai/dgl_docs/generated/dgl.data.FB15kDataset.html](https://www.dgl.ai/dgl_docs/generated/dgl.data.FB15kDataset.html). [Accessed: Oct. 08, 2025]

[5]

“WN18Dataset — DGL 2.5 documentation.” Available: [https://www.dgl.ai/dgl_docs/generated/dgl.data.WN18Dataset.html](https://www.dgl.ai/dgl_docs/generated/dgl.data.WN18Dataset.html). [Accessed: Oct. 08, 2025]

[6]

L. S. Lorello, M. Lippi, and S. Melacci, “The KANDY Benchmark: Incremental Neuro-Symbolic Learning and Reasoning with Kandinsky Patterns.” arXiv, Feb. 27, 2024. doi: [10.48550/arXiv.2402.17431](https://doi.org/10.48550/arXiv.2402.17431). Available: [http://arxiv.org/abs/2402.17431](http://arxiv.org/abs/2402.17431). [Accessed: Oct. 08, 2025]

[7]

S. Bortolotti _et al._, “A Neuro-Symbolic Benchmark Suite for Concept Quality and Reasoning Shortcuts.” arXiv, Oct. 29, 2024. doi: [10.48550/arXiv.2406.10368](https://doi.org/10.48550/arXiv.2406.10368). Available: [http://arxiv.org/abs/2406.10368](http://arxiv.org/abs/2406.10368). [Accessed: Oct. 08, 2025]

[8]

D. Méndez, G. Bontempo, E. Ficarra, R. Confalonieri, and N. Díaz-Rodríguez, “CUBIC: Concept Embeddings for Unsupervised Bias Identification using VLMs.” arXiv, May 16, 2025. doi: [10.48550/arXiv.2505.11060](https://doi.org/10.48550/arXiv.2505.11060). Available: [http://arxiv.org/abs/2505.11060](http://arxiv.org/abs/2505.11060). [Accessed: Oct. 08, 2025]

[9]

M. Choi, H. Goel, M. Omama, Y. Yang, S. Shah, and S. Chinchali, “Towards Neuro-Symbolic Video Understanding,” 2025, pp. 220–236. doi: [10.1007/978-3-031-73229-4_13](https://doi.org/10.1007/978-3-031-73229-4_13). Available: [http://arxiv.org/abs/2403.11021](http://arxiv.org/abs/2403.11021). [Accessed: Oct. 08, 2025]

[10]

P. E. I. Dimasi, “Scene Graph Generation in Autonomous Driving: a Neuro-symbolic approach,” laurea, Politecnico di Torino, 2023. Available: [https://webthesis.biblio.polito.it/29354/](https://webthesis.biblio.polito.it/29354/). [Accessed: Oct. 08, 2025]

[11]

M. Junaid Khan, A. Masood Siddiqui, H. Saeed Khan, F. Akram, and M. Jaleed Khan, “MuRelSGG: Multimodal Relationship Prediction for Neurosymbolic Scene Graph Generation,” _IEEE Access_, vol. 13, pp. 47042–47054, 2025, doi: [10.1109/ACCESS.2025.3551267](https://doi.org/10.1109/ACCESS.2025.3551267). Available: [https://ieeexplore.ieee.org/document/10925205/](https://ieeexplore.ieee.org/document/10925205/). [Accessed: Oct. 08, 2025]

[12]

L. N. DeLong, R. F. Mir, and J. D. Fleuriot, “Neurosymbolic AI for Reasoning over Knowledge Graphs: A Survey,” _IEEE Trans. Neural Netw. Learning Syst._, vol. 36, no. 5, pp. 7822–7842, May 2025, doi: [10.1109/TNNLS.2024.3420218](https://doi.org/10.1109/TNNLS.2024.3420218). Available: [http://arxiv.org/abs/2302.07200](http://arxiv.org/abs/2302.07200). [Accessed: Oct. 08, 2025]

[13]

S. Koch, P. Hermosilla, N. Vaskevicius, M. Colosi, and T. Ropinski, “SGRec3D: Self-Supervised 3D Scene Graph Learning via Object-Level Scene Reconstruction.” arXiv, Nov. 06, 2023. doi: [10.48550/arXiv.2309.15702](https://doi.org/10.48550/arXiv.2309.15702). Available: [http://arxiv.org/abs/2309.15702](http://arxiv.org/abs/2309.15702). [Accessed: Oct. 08, 2025]

[14]

“Holistic Evaluation of Language Models (HELM).” Available: [https://crfm.stanford.edu/helm/](https://crfm.stanford.edu/helm/). [Accessed: Oct. 08, 2025]

[15]

A. Srivastava _et al._, “Beyond the Imitation Game: Quantifying and extrapolating the capabilities of language models.” arXiv, June 12, 2023. doi: [10.48550/arXiv.2206.04615](https://doi.org/10.48550/arXiv.2206.04615). Available: [http://arxiv.org/abs/2206.04615](http://arxiv.org/abs/2206.04615). [Accessed: Oct. 08, 2025]

[16]

B. C. Colelough and W. Regli, “Neuro-Symbolic AI in 2024: A Systematic Review.” arXiv, Apr. 05, 2025. doi: [10.48550/arXiv.2501.05435](https://doi.org/10.48550/arXiv.2501.05435). Available: [http://arxiv.org/abs/2501.05435](http://arxiv.org/abs/2501.05435). [Accessed: Oct. 08, 2025]

[17]

X. Zhang and V. S. Sheng, “Neuro-Symbolic AI: Explainability, Challenges, and Future Trends.” arXiv, Nov. 07, 2024. doi: [10.48550/arXiv.2411.04383](https://doi.org/10.48550/arXiv.2411.04383). Available: [http://arxiv.org/abs/2411.04383](http://arxiv.org/abs/2411.04383). [Accessed: Oct. 08, 2025]

[18]

C. Shang, S. Zhou, H. Zhang, X. Ni, Y. Yang, and Y. Wang, “Incremental Residual Concept Bottleneck Models.” arXiv, Apr. 17, 2024. doi: [10.48550/arXiv.2404.08978](https://doi.org/10.48550/arXiv.2404.08978). Available: [http://arxiv.org/abs/2404.08978](http://arxiv.org/abs/2404.08978). [Accessed: Oct. 08, 2025]

[19]

A. Reuel, A. Hardy, C. Smith, M. Lamparth, M. Hardy, and M. J. Kochenderfer, “BetterBench: Assessing AI Benchmarks, Uncovering Issues, and Establishing Best Practices.” arXiv, Nov. 20, 2024. doi: [10.48550/arXiv.2411.12990](https://doi.org/10.48550/arXiv.2411.12990). Available: [http://arxiv.org/abs/2411.12990](http://arxiv.org/abs/2411.12990). [Accessed: Oct. 08, 2025]

[20]

“BIG-Bench: The New Benchmark for Language Models,” _Deepgram_, June 27, 2024. Available: [https://deepgram.com/learn/big-bench-llm-benchmark-guide](https://deepgram.com/learn/big-bench-llm-benchmark-guide). [Accessed: Oct. 08, 2025]