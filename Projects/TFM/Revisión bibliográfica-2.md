links: [[TFM]] 

# Revisión bibliográfica
## Bases de datos y estándares actuales para modelos simbólicos  
### Estándares de representación y validación  
[[RDF]]/[[OWL]]/RDFS como núcleo del stack Semántico (razonamiento monotónico bajo OWA). Buenas prácticas y límites en control de calidad y consistencia. [2024.eswc-conferences.org+1](https://2024.eswc-conferences.org/wp-content/uploads/2024/05/77770375.pdf)  


[[SHACL]] como estándar W3C para validar grafos [[RDF]] con constraints (complementa [[OWL]]; útil para “reglas de coherencia” de escenas). Hay trabajos recientes proponiendo frameworks de calidad y aceleración de validación. **Implicación:** usar [[SHACL]] para verificar consistencia simbólica de las salidas del modelo. [OUP Academic+2openreview.net+2](https://academic.oup.com/dsh/advance-article/doi/10.1093/llc/fqaf067/8213722?searchresult=1)


### Recursos de conocimiento reutilizables (para mapeo y “commonsense”)  
- Wikidata/DBpedia/Schema.org (ontologías generales + tipologías). 
- WordNet/ConceptNet/CSKG (commonsense para enriquecer relaciones). 

En 2025 aparecen propuestas de “enriquecimiento NeSy” de grafos de escena con CSKG para VQA/razonamiento. **Implicación:** usar grafos generales + CSKG como capa simbólica inicial y especializar con [[SHACL]]. [SpringerLink](https://link.springer.com/article/10.1007/s41060-025-00827-7)

## Revisión bibliográfica del campo: modelos, evolución y hacia dónde va  
### Neuro-Simbólico (NeSy)  
Encuestas y síntesis 2024–2025 destacan integración de aprendizaje neuronal con representación/razonamiento simbólico para ganar robustez, explicabilidad y datos-eficiencia. 

**Líneas:** restricciones lógicas durante el entrenamiento, razonamiento post-hoc sobre KG, y pipelines NeSy para visión y VQA. [ACM Digital Library+1](https://dl.acm.org/doi/abs/10.1007/s00521-024-09960-z)  

***Marcos recientes:*** CCN+ (compliance “by design” con requisitos lógicos sobre la salida del modelo), grafos de escena enriquecidos con conocimiento (NeSyGRN). 

**Implicación:** imponer requisitos simbólicos ([[SHACL]]/[[OWL]]) y/o enrutar razonamiento paso a paso sobre grafos. [ScienceDirect+1](https://www.sciencedirect.com/science/article/pii/S0888613X24000112)

### Modelos Visión-Lenguaje (LVLM/MLLM)  
Las encuestas 2025 reportan transición hacia modelos de razonamiento visual más generalistas, con nuevas técnicas de alineación y un ecosistema de benchmarks amplio (MMBench, MMMU, etc.). El énfasis actual: fidelidad del razonamiento, reducción de alucinaciones y evaluación multimodal más válida. [arXiv+2arXiv+2](https://arxiv.org/abs/2501.02189)  


Modelos recientes para razonamiento visual multimodal muestran avances en tareas tipo CLEVR/VSR, pero persisten fallos de consistencia y “spurious shortcuts”, lo que abre espacio a integrar capas simbólicas. [arXiv](https://arxiv.org/html/2505.20753v1)

## ¿Por dónde tirar? Estrategias de solución  
### Opción A: Pipeline NeSy “ligero” y verificable  
Percepción con LVLM sólido → construcción de scene graph → mapeo a KG de dominio → validación [[SHACL]] + reglas [[OWL]]/SPARQL → métricas de coherencia simbólica.  
- Pros: ingeniería controlada, explicabilidad clara, rápido de iterar. 
- Contras: el razonamiento simbólico es post-hoc. Soporte bibliográfico en [[SHACL]]/[[OWL]] y enriquecimiento con CSKG. [OUP Academic+22024.eswc-conferences.org+2](https://academic.oup.com/dsh/advance-article/doi/10.1093/llc/fqaf067/8213722?searchresult=1)


### Opción B: NeSy “profundo” con restricciones durante entrenamiento o decodificación  
Integrar restricciones lógicas/semánticas en el propio modelo (p.ej., CCN+ o pérdidas por violación de constraints). 
- Pros: menos incoherencias.
- Contras: más complejidad y coste. [ScienceDirect](https://www.sciencedirect.com/science/article/pii/S0888613X24000112)

### Opción C: LVLM + módulo simbólico de enrutado/razonamiento  
Razonamiento paso a paso sobre grafos de escena con enriquecimiento commonsense (NeSyGRN). 
- Pros: mejor control del “chain-of-thought” estructurado.
- Contras: mayor diseño de grafos/operadores. [SpringerLink](https://link.springer.com/article/10.1007/s41060-025-00827-7)

**Recomendación inicial:** empezar por A y preparar camino a C. Minimiza riesgo, aporta resultados medibles y te deja publicar una mejora incremental (A→C) en 1–2 iteraciones.

## ¿Qué modelos usar?  
### Percepción/LVLM base  
Elegir un LVLM actual con buen trade-off y tool-use: p.ej., familias recientes recogidas en encuestas 2025 (Qwen2-VL/InternVL2.5/LLaVA-Next y equivalentes) evaluadas en MMBench/MMMU. Selección basada en resultados de encuestas y tablas comparativas de benchmarks. [hai.stanford.edu+3arXiv+3arXiv+3](https://arxiv.org/abs/2501.02189)


### Capa simbólica  
KG en [[RDF]]/[[OWL]], validación con [[SHACL]]; añadir reglas [[SHACL]]-SPARQL para “políticas” de escena (p.ej., incompatibilidades espaciales/funcionales). 

**Opcional:** CSKG/ConceptNet para completar relaciones no observadas. Evidencia de efectividad en VQA razonado. [MDPI+2OUP Academic+2](https://www.mdpi.com/2078-2489/15/12/759)

### NeSy “compliance by design”  
Si se opta por entrenamiento con restricciones: [[001 - 0212 Redes Neuronales Convolucionales|CCN]]+ como referencia metodológica para imponer requisitos sobre salidas. [ScienceDirect](https://www.sciencedirect.com/science/article/pii/S0888613X24000112)

## ¿Mejorar un modelo o qué hacer?  
- Mejora focalizada y publicable: añadir “validador simbólico” ([[SHACL]]) que detecte/penalice incoherencias del LVLM y retroalimente prompts/decodificación (self-refine simbólico). Literatura reciente sostiene la validez de [[SHACL]] para garantizar consistencia de KG, un vacío habitual en LVLMs. [OUP Academic+1](https://academic.oup.com/dsh/advance-article/doi/10.1093/llc/fqaf067/8213722?searchresult=1)  
- Alternativa: entrenar/ajustar un decodificador con pérdidas por violación de constraints (inspiración CCN+). [ScienceDirect](https://www.sciencedirect.com/science/article/pii/S0888613X24000112)  
- En paralelo: enriquecer el scene graph con CSKG y usar un enrutador simbólico paso a paso (NeSyGRN). [SpringerLink](https://link.springer.com/article/10.1007/s41060-025-00827-7)

## Modelos NeSy o Visión-Lenguaje: decisión  
Tendencia 2025: los LVLMs avanzan en benchmarks de razonamiento, pero la fidelidad explicativa y la consistencia siguen siendo críticas; comunidad impulsa combinarlos con módulos simbólicos para robustez/explicabilidad. Conclusión práctica: usar un LVLM actual como “perceptor/razonador blando” + capa simbólica verificadora y enriquecedora. [arXiv+1](https://arxiv.org/abs/2501.02189)

## Benchmarks dentro de NeSy y multimodal para evaluar  
- Clásicos de razonamiento visual: CLEVR/GQA/VSR para composicionalidad y pasos razonados. 
- Suites modernas multimodales: MMBench (colección curada y mantenida), MMMU (razonamiento académico multi-dominio), NTSEBENCH (razonamiento cognitivo), VERIFY (fidelidad de explicación y consistencia). Relevancia: medir comprensión, razonamiento y, específicamente, fidelidad de explicaciones. [arXiv+3GitHub+3hai.stanford.edu+3](https://github.com/open-compass/MMBench)

## Formas de explicabilidad aplicables a NeSy/LVLM  
- Concept Bottleneck Models (CBM) y variantes recientes (p.ej., CB-LLMs): predicción mediada por conceptos etiquetables; facilitan auditoría y edición de conceptos. 
- Explanation Bottleneck Models (XBM): generan explicación textual intermedia y la usan como cuello de botella. 
- Concept-based (TCAV/CAVs, concept generation) para explicar señales de alto nivel en visión/lenguaje. 
- Prototipos y saliency (Grad-CAM, etc.) como apoyo, aunque menos semánticos. Relevancia 2024–2025: consolidación de explicabilidad basada en conceptos y cuellos de botella para alinear con conocimiento experto. [ResearchGate+3openreview.net+3ojs.aaai.org+3](https://openreview.net/forum?id=RC5FPYVQaH)

## Propuesta concreta (MVP publicable)  
### Pipeline  
- Paso 1: LVLM SOTA (evaluado previamente en MMBench/MMMU) produce detecciones, relaciones y caption/CoT. 
- Paso 2: Construcción de scene graph y mapeo a KG ([[RDF]]/[[OWL]]). 
- Paso 3: Validación [[SHACL]] de restricciones espaciales/funcionales; registro de violaciones (log de incoherencias). 
- Paso 4: Bucle de auto-refinamiento guiado por violaciones (prompting penaliza hipótesis incoherentes; o reranking simbólico). 
- Paso 5: Explicación híbrida: conceptos intermedios + reglas satisfechas/violadas (tabla de trazabilidad).  


**Justificación:** se alinea con la dirección 2025 de evaluar fidelidad del razonamiento/explícito, aprovechando estándares W3C y benchmarks nuevos de explicación. [arXiv+1](https://arxiv.org/html/2503.11557v1)

### Métricas  
- Métricas LVLM: exactitud/EM/F1 en QA multimodal (MMBench/MMMU). 
- Métricas simbólicas: tasa de violación de constraints [[SHACL]], número de inferencias válidas, cobertura conceptual. 
- Métricas de explicabilidad: fidelidad (VERIFY), precisión/consistencia de conceptos (CBM/TCAV). [GitHub+2hai.stanford.edu+2](https://github.com/open-compass/MMBench)
###  Riesgos y mitigaciones  
- Hallucinations del LVLM → reforzar con constraints y enriquecimiento CSKG. 
- Coste de ingeniería de KG → empezar con ontología mínima + reglas [[SHACL]] de alto impacto y crecer iterativamente. 
- Generalización a dominios nuevos → separar claramente perceptrón (LVLM) de reglas ([[SHACL]]) para portabilidad.

## Plan de trabajo  
- Selección LVLM y línea base en MMBench/MMMU; curación de ontología mínima y 10–15 reglas [[SHACL]] de alto valor. 
- Implementación del validador simbólico y logging de incoherencias; análisis de errores.
- Lazo de auto-refinamiento (prompts/decodificación condicionada por violaciones) + evaluación en VERIFY/NTSEBENCH. 
- Estudio ablation: sin [[SHACL]] vs con [[SHACL]]; sin enriquecimiento vs con CSKG; reporte de explicabilidad con CBM/TCAV en subconjunto anotado. [ACL Anthology+3GitHub+3hai.stanford.edu+3](https://github.com/open-compass/MMBench)


---
tags:
	#Index #TFM