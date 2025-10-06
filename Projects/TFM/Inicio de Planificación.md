	links: [[TFM]] 

# Inicio de Planificación
Modelos neurosimbólicos para el reconocimiento semántico de escenas (visión‑lenguaje) con razonamiento y mitigación de sesgos

## Objetivo general

Diseñar y evaluar un pipeline neuro‑simbólico para reconocimiento semántico de escenas e interacciones humano‑objeto, combinando un LVLM (p.ej., LLaVA, InternVL, Qwen‑VL) con componentes simbólicos (conocimiento, gramáticas, reglas) y módulos de explicabilidad, evaluando su capacidad de generalización sistemática y mitigación de sesgos.

## Hipótesis

1. La introducción de una capa simbólica explícita (reglas u ontologías) sobre representaciones de un LVLM mejora la generalización sistemática en tareas HOI/SGG respecto a modelos puramente neuronales.
    
2. El uso de embeddings de conceptos y diagnósticos tipo CUBIC sobre LVLMs permite identificar y mitigar sesgos sin anotaciones exhaustivas.
    
3. La modularidad (en dos etapas) y el pre‑entrenamiento mejoran la generalización OOD frente a arquitecturas end‑to‑end.
    

## Alcance

Tareas centradas en imagen (y opcionalmente vídeo corto) con foco en interacciones humano‑objeto (HOI) y relaciones espaciales/funcionales en SGG. No se aborda generación de imágenes.

## Entregables

1. Revisión del estado del arte (métodos, datasets, benchmarks).
    
2. Dataset y protocolo experimental (splits, métricas, semillas).
    
3. Baselines reproducibles (LVLM y HOI/SGG).
    
4. Módulo simbólico + reglas/ontología + explicabilidad.
    
5. Informe de resultados con análisis de sesgos y ablations.
    
6. Código y documentación reproducible.
    

## Métricas y evaluación

- **HOI/SGG:** mAP por triplete, Recall@K, rare‑triplets, zero‑shot.
    
- **LVLM‑VQA:** Exact match, Accuracy.
    
- **Sesgos:** Diferencias de desempeño por atributo, paridad de odds, CUBIC/HELM/BIG‑bench.
    
- **Explicabilidad:** Fidelidad, suficiencia, coherencia semántica.
    

## Plan de trabajo

**Fase 0. Preparación (1 semana)**: Curación bibliográfica, checklist reproducibilidad.  
**Fase 1. Revisión y decisiones (2 semanas)**: Tablas comparativas, elección de LVLMs y HOI/SGG.  
**Fase 2. Baselines (3 semanas)**: Montar inferencia LVLM y HOI/SGG.  
**Fase 3. Capa simbólica y explicabilidad (4–6 semanas)**: Ontología, reglas, integración.  
**Fase 4. Mitigación de sesgos (2–3 semanas)**: Diagnóstico con CUBIC/HELM, intervención.  
**Fase 5. Experimentos finales y redacción (3–4 semanas)**: Ablations, análisis, redacción.

## Tareas accionables

**T1.** Revisar bases de datos simbólicas (ConceptNet, ATOMIC, WordNet, FrameNet, Visual Genome, HAKE, HICO‑DET, V‑COCO, GQA, Bongard‑HOI).  
**T2.** Revisión bibliográfica sobre modelos NeSy, HOI/SGG, LVLMs y sesgos.  
**T3.** Decidir estrategia: solución única mejorada vs varios modelos comparados.  
**T4.** Selección de modelos candidatos: LLaVA‑1.6, InternVL2, Qwen‑VL, IDEFICS2; STIP, QPIC, HOTR, FGAHOI; SGRec3D.  
**T5.** Diseñar capa simbólica: ontología de acciones/partes/objetos y reglas lógicas diferenciables.  
**T6.** Adoptar benchmarks HICO‑DET‑SG y V‑COCO‑SG para evaluación sistemática.  
**T7.** Implementar técnicas de explicabilidad: attribution maps, racionales, trazas simbólicas.  
**T8.** Integrar pipeline CUBIC y benchmarks HELM/BIG‑bench para análisis de sesgos.  
**T9.** Desarrollar caso piloto Sierra Nevada: detección de interacciones humano‑entorno.  
**T10.** Estandarizar repositorio y scripts para reproducibilidad.  
**T11.** Preparar manuscrito para workshop o conferencia (NeSy, CVPR‑W, IJCAI‑W).

## Tablas de referencia (resumen inicial)

|Recurso|Tipo|Licencia|Tamaño|Cobertura|Observaciones|
|---|---|---|---|---|---|
|ConceptNet|KG commonsense|CC‑BY‑SA|~34M edges|General|Relaciones semánticas amplias|
|ATOMIC‑2020|Commonsense social|CC|~1.3M|If‑Then social|Useful para reasoning social|
|WordNet|Léxico‑semántico|Open|117k synsets|Verbos/sustantivos|Base lingüística|
|Visual Genome|Imagen‑relación|CC‑BY|~100k imágenes|HOI/SGG|Tripletes ⟨obj‑rel‑obj⟩|
|HAKE|HOI simbólico|Open|120k imágenes|Acciones humanas|Primitivas PaSta|
|HICO‑DET|HOI dataset|CC|600k|117 verbos|Benchmark estándar|
|V‑COCO|HOI dataset|CC|10k|26 acciones|Validación SG‑splits|
|SGRec3D|3D Scene Graph|Open|10k escenas|3D relaciones|Pre‑training auto‑sup.|

## Diseño del pipeline propuesto

1. **Entrada imagen.** Detección de personas y objetos + extracción de tripletes ⟨humano, verbo, objeto⟩.
    
2. **Razonamiento simbólico (tipo HAKE).** Aplicación de reglas lógicas diferenciables para verificar o corregir predicciones.
    
3. **LVLM clarificador.** Prompts estructurados con ontología para obtener veredictos y justificaciones.
    
4. **Mitigación de sesgos.** Diagnóstico con CUBIC y re‑prompting guiado por reglas.
    
5. **Evaluación.** mAP/Recall@K, fairness metrics, coherencia simbólica.
    

## Riesgos y mitigación

- **Licencias:** usar datasets abiertos.
    
- **Complejidad:** iniciar con inferencia LVLMs open‑source.
    
- **Entrenamiento HOI:** reutilizar modelos pre‑entrenados.
    
- **Escalabilidad:** modularidad y pruebas incrementales.
    

## Checklist de reproducibilidad

- Semillas fijas.
    
- Scripts únicos por experimento.
    
- Hardware y tiempo reportado.
    
- Licencias documentadas.
    
- Evaluación doble ciega de reglas.
    

## Roadmap inmediato 
1. Completar tablas de datasets y benchmarks; decidir LVLMs y HOI/SGG.  
2. Correr baselines iniciales.  
3. Implementar capa simbólica mínima y validarla.  
4. Primera evaluación de sesgos con CUBIC + informe preliminar.
## Posible Pipeline
**Pipeline:** HOI/SGG → capa simbólica tipo HAKE → LVLM clarificador (LLaVA / InternVL / Qwen2-VL) → diagnóstico [CUBIC](https://arxiv.org/abs/2505.11060)/rsbench → métricas [HELM](https://crfm.stanford.edu/helm/) / fairness + SG-splits

## Referencias clave

- HAKE (Human Activity Knowledge Engine)
    
- HICO‑DET y V‑COCO (HOI y SG‑splits)
    
- SGRec3D (pre‑training 3D)
    
- HELM y BIG‑bench (evaluación de sesgos)
    
- CUBIC (detección de sesgos en embeddings LVLM)

---
tags:
	#Index #TFM