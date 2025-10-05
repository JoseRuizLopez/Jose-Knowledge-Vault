links: [[TFM]] 

# Revisión bibliográfica (síntesis ejecutiva)

## Taxonomía del campo

- Arquitecturas NeSy: lógica probabilística/difusa (DeepProbLog, LTN), verificación/constraints, abducción; concept-based models (CBM) y bottlenecks; razonamiento en embedding space; pipelines modulares percepción→razonamiento.
    
- LVLMs/MLLMs: modelos visión-lenguaje de propósito general (LLaVA, InternVL2/2.5, Qwen2‑VL, Idefics2) frente a modelos de HOI/SGG específicos (HOTR, QPIC, FGAHOI, STIP) y 3D (SGRec3D).
    
- Explicabilidad y sesgos: reglas auditables (HAKE), diagnósticos de conceptos (CUBIC), suites generales (HELM, BIG-bench/BBH), reasoning shortcuts (rsbench).
    

## Evolución 2017–2025 (hitos)

- 2017–2019: datasets diagnósticos (CLEVR, GQA); primeros NeSy modernos (Neuro‑Symbolic Concept Learner); auge SGG/HOI.
    
- 2020–2022: paradigmas de lógica diferenciable y CBM; métodos HOI DETR‑based (QPIC/HOTR); HAKE como KB de primitivas + reglas.
    
- 2023–2024: LVLMs de instrucción (LLaVA), escalado (InternVL2/2.5, Qwen2‑VL), MLLMs eficientes (Idefics2). Aparecen splits SG para medir generalización sistemática (HICO‑DET‑SG, V‑COCO‑SG). rsbench unifica evaluación de reasoning shortcuts.
    
- 2025: CUBIC para detectar sesgos con embeddings de conceptos en VLM/LVLM; HELM amplía evaluación multimodal y de seguridad; consolidación de dinámicas de resolución variable y test‑time scaling en LVLMs.
    

## Modelos representativos y aportes

- LLaVA/1.6: afinado por instrucciones multimodales; fuerte en VQA y caption, coste accesible.
    
- InternVL2/2.5: escalado progresivo, destaca en OCR, documentos y gráficos; test‑time scaling.
    
- Qwen2‑VL: tokens visuales a resolución dinámica; versiones 2B/8B/72B.
    
- Idefics2 (8B): diseño eficiente y resultados SOTA en su tamaño; buenas prácticas de preentrenamiento.
    
- HOI/SGG: QPIC/HOTR (DETR‑based) y STIP (dos etapas) como baselines; FGAHOI (features agrupadas). SGRec3D en 3D.
    
- NeSy/Explicabilidad: HAKE (PaSta+reglas) para razonamiento composicional y transferencia; rsbench para medir atajos de razonamiento y calidad de conceptos; CUBIC para descubrir sesgos sin anotación.
    

## Benchmarks y lecciones

- HOI generalización sistemática: fuertes caídas de mAP en HICO‑DET‑SG y V‑COCO‑SG; la diversidad del entrenamiento mitiga parcialmente.
    
- Concept quality y RS: rsbench muestra que incluso NeSy/CBM sufren baja calidad de conceptos y OOD drops; necesidad de verificar semántica de conceptos.
    
- Fairness/robustez: HELM (múltiples dimensiones, incluye seguridad); BIG‑bench/BBH para razonamiento general. CUBIC identifica conceptos sesgados de forma no supervisada.
    

## Tendencias actuales

- Modularidad NeSy + LVLM como “juez/clarificador”.
    
- Reglas/ontologías ligeras para verificación/repair post‑hoc.
    
- Resolución dinámica y reducción de tokens visuales en LVLMs.
    
- Evaluación OOD y sesgos a nivel de conceptos, no solo accuracy.
    

## Implicaciones para el [[TFM]]

- Adoptar HICO‑DET‑SG/V‑COCO‑SG como núcleo de evaluación.
    
- Combinar baseline HOI (STIP o QPIC) con capa simbólica tipo [[HAKE]] y un LVLM (LLaVA/InternVL/Qwen2‑VL) como clarificador.
    
- Incluir diagnóstico de sesgos con CUBIC y tareas rsbench para validar calidad de conceptos.
    
- Reportar métricas de fairness y trazas de reglas junto con mAP/Recall@K.



---
tags:
	#Index #TFM