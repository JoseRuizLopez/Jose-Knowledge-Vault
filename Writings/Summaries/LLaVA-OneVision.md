links: [[008 Summaries|Summaries]] 


Reference: [[LLaVA-OneVision.pdf|LLaVA-OneVision: Easy Visual Task Transfer]]
# Resumen del LLaVA-OneVision

## Contexto

LLaVA (“**Large Language and Vision Assistant**”) comenzó como un modelo _vision-language_ basado en CLIP + LLaMA, centrado en **instrucción-tuning multimodal**: enseñar a un modelo de lenguaje grande (LLM) a “razonar” sobre imágenes.

**LLaVA-OneVision (OV)** es su evolución más ambiciosa:

> un modelo que **unifica comprensión visual, grounding, generación y razonamiento multimodal**, entrenado con _instrucción-tuning_ en **múltiples niveles de granularidad visual** (píxel, objeto, escena, texto).



## Arquitectura

El sistema adopta una **arquitectura modular unificada**:

|Módulo|Descripción|
|---|---|
|**Visual Encoder**|Un **ViT-L o EVA-CLIP** pre-entrenado que produce embeddings densos multi-resolución.|
|**Projector multimodal**|Proyecta las features visuales al espacio del lenguaje (alineación con LLaMA-3 / Vicuna).|
|**LLM Backbone**|LLaMA-3-7B / 13B con _multimodal adapters_ integrados en los tokens visuales.|
|**Unified Interface**|Entrada multimodal (texto, imagen, vídeo, OCR, diagramas) y salida textual libre o estructurada.|

En lugar de tener arquitecturas separadas para captioning, VQA o grounding, **OneVision usa una sola red instruccionada**, capaz de razonar sobre:

- Preguntas visuales (“¿cuántas personas hay?”)
    
- Análisis espaciales (“¿qué objeto está a la izquierda del perro?”)
    
- OCR / lectura de documentos (“¿qué precio aparece en la factura?”)
    
- Diagramas / tablas / escenas 3D
    



## Entrenamiento y datasets

**LLaVA-OneVision** fue entrenado en **tres fases**:

1. **Pre-training multimodal** con datasets como LAION, COCO, OpenImages, TextCaps, y web data (billones de pares imagen-texto).
    
2. **Stage 2 – Visual Instruction Tuning:** pares pregunta-respuesta visuales generados con GPT-4V (≈ 8M instrucciones), cubriendo desde VQA a OCR y reasoning.
    
3. **Stage 3 – Unification + RLHF:** aprendizaje por refuerzo con feedback humano multimodal para mejorar precisión factual y consistencia visual-textual.
    



## Principios de diseño

1. **“One Model for All Vision Tasks”**  
    → Un solo modelo que aprende tareas _de percepción (what/where)_ y _de razonamiento (why/how)_ simultáneamente.
    
2. **Granularidad visual jerárquica:**
    
    - **Nivel 1:** píxeles → texturas, colores.
        
    - **Nivel 2:** objetos → bounding boxes, relaciones.
        
    - **Nivel 3:** escenas → contexto global.
        
    - **Nivel 4:** conocimiento textual y conceptual → grounding semántico.
        
3. **Instrucciones multimodales unificadas:**  
    Todo se formula como texto, e.g.:
    
    `<image> Question: Describe the relationship between the man and the car.`
    
    → “The man is washing the car.”
    
4. **Cross-modal alignment progresivo:**  
    Usa un alineamiento _contrastive + instruction-based_, mejor que el CLIP clásico.
    



## Evaluación

**LLaVA-OneVision** se evalúa en más de **20 benchmarks multimodales**, destacando:

|Área|Benchmark|Resultado|
|---|---|---|
|**VQA**|GQA, VQAv2, VizWiz|supera a LLaVA-1.6 y GPT-4V|
|**OCR / DocQA**|TextVQA, ChartQA, DocVQA|+10-20 puntos sobre InstructBLIP|
|**Razonamiento espacial**|CLEVR, NLVR2|>90 % accuracy|
|**Multimodal reasoning**|ScienceQA, MMMU|comparable a GPT-4V|
|**Imagen-a-texto**|COCO Caption|rendimiento tipo state-of-the-art|

Además, incorpora métricas de **grounding y consistencia factual visual**, donde obtiene mejor correlación humano-modelo.



## Contribuciones clave

1. **Unificación multimodal:**  
    Un solo modelo para captioning, grounding, OCR, QA, descripción, razonamiento.
    
2. **Instrucción jerárquica:**  
    El modelo aprende a “ver como un humano”, pasando de detalles locales a razonamiento abstracto.
    
3. **Alineación continua visión-lenguaje:**  
    Un _projector_ adaptativo que preserva estructura espacial mientras alinea con tokens lingüísticos.
    
4. **Capacidad de razonamiento explícito:**  
    Puede explicar su respuesta paso a paso (“chain-of-thought” multimodal).
    



## Conexión con tu TFM

|Elemento|Relación con tu línea neuro-simbólica|
|---|---|
|**Scene understanding**|OneVision razona sobre relaciones espaciales y funcionales (similar a tus “scene graphs”).|
|**Representación simbólica implícita**|Los embeddings visuales + textual reasoning actúan como una forma de “knowledge grounding”.|
|**Razonamiento visual-lingüístico**|Ejemplo moderno de integración DL + razonamiento simbólico textual.|
|**Explicabilidad**|Genera respuestas explicadas, útil para _post-hoc symbolic alignment_ con KGs o reglas.|
|**Base para integración NeSy**|Puedes usar su salida textual como capa intermedia para alinear con un **Knowledge Graph semántico** (p. ej., mediante _concept mapping_ o _rule enforcement_).|



## Limitaciones y futuro

- Aún **no razona simbólicamente de forma explícita** (no usa FOL ni grafos lógicos).
    
- Su razonamiento es **lingüístico y probabilístico**, no formal.
    
- Podría ampliarse con **capas simbólicas (rule-checkers, ontologías)** para validar coherencia lógica.
    
- Próximos pasos en la comunidad: **LLaVA-3, InternVL-2, Kosmos-3**, con objetivos similares.
    



## En síntesis

> **LLaVA-OneVision es un modelo multimodal unificado (visión + lenguaje) entrenado con instrucciones visuales jerárquicas, capaz de entender, describir y razonar sobre escenas, texto y gráficos con alto nivel de coherencia y explicabilidad.**

Para el TFM, puede servirme como **componente perceptual y de razonamiento lingüístico previo a la parte simbólica**:

- Detecta relaciones humanas-entorno.
    
- Describe semánticamente la escena.
    
- Tú puedes luego **mapear esas descripciones a un grafo de conocimiento** y aplicar razonamiento simbólico (tipo HAKE + KG).


---
tags:
	#Writing #Summary
