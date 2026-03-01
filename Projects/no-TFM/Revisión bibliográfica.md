links: [[102 - no-TFM]] 

# Revisión bibliográfica (síntesis ejecutiva)

## Taxonomía del campo

- **Arquitecturas NeSy:** lógica probabilística y diferenciable ([DeepProbLog](https://arxiv.org/abs/1805.10872), [Logic Tensor Networks](https://arxiv.org/abs/2012.13635)), razonamiento abductivo (Evans & Grefenstette, 2018); _[concept-based models](https://arxiv.org/abs/2007.04612)_ y _bottlenecks_ interpretables; pipelines percepción→razonamiento ([Garcez et al., 2019](https://arxiv.org/abs/1905.06088)).
    
- **LVLMs/MLLMs:** modelos de visión-lenguaje generales: LLaVA-1.6, InternVL2, Qwen2-VL, Idefics2; frente a modelos HOI/SGG específicos: HOTR, QPIC, FGAHOI, STIP y SGRec3D.
    
- **Explicabilidad y sesgos:** reglas auditables (HAKE), diagnóstico de conceptos ([CUBIC](https://arxiv.org/abs/2505.11060)), benchmarks de razonamiento (rsbench), y evaluación global ([HELM](https://crfm.stanford.edu/helm/), BIG-Bench, [BIG-Bench Hard](https://arxiv.org/abs/2210.09261)).
    

---

## Evolución 2017–2025 (hitos)

- **2017–2019:** datasets diagnósticos (CLEVR, GQA); primer _Neuro-Symbolic Concept Learner_ (Mao et al., 2019).
    
- **2020–2022:** _Concept Bottleneck Models_ (Koh et al., 2020); lógica diferenciable (DeepProbLog, LTN); modelos HOI DETR-based (QPIC, HOTR); HAKE como base de primitivas + reglas.
    
- **2023–2024:** _Instruction-tuned LVLMs_ (LLaVA-1.6), escalado multimodal (InternVL2, Qwen2-VL), MLLMs eficientes (Idefics2); splits SG (HICO-DET-SG / V-COCO-SG); benchmark de razonamiento (rsbench).
    
- **2025:** sesgos y fairness ([CUBIC](https://arxiv.org/abs/2505.11060)); [HELM 2.0](https://crfm.stanford.edu/helm/) amplía evaluación multimodal; consolidación de _test-time scaling_ (Qwen2-VL).
    

---

## Modelos representativos y aportes

- **LLaVA-1.6**: fine-tuning instruccional multimodal, excelente en VQA/caption.
    
- **InternVL2/2.5**: escalado progresivo, alto rendimiento en OCR y documentos.
    
- **Qwen2-VL**: resolución dinámica y _multi-scale RoPE_.
    
- **Idefics2 (8B)**: modelo eficiente y reproducible con alineamiento texto-imagen.
    
- **HOI/SGG:** QPIC, HOTR, STIP, FGAHOI, SGRec3D.
    
- **NeSy/Explicabilidad:** HAKE; rsbench para _shortcuts_; [CUBIC](https://arxiv.org/abs/2505.11060) para sesgos.
    

---

## Benchmarks y lecciones

- Generalización sistemática: HICO-DET-SG / V-COCO-SG muestran caída de mAP en combinaciones no vistas.
    
- _Reasoning Shortcuts:_ rsbench evidencia errores de concepto en NeSy/CBM.
    
- Fairness y robustez: [HELM](https://crfm.stanford.edu/helm/), BIG-Bench, [BBH](https://arxiv.org/abs/2210.09261), [CUBIC](https://arxiv.org/abs/2505.11060).
    

---

## Tendencias actuales

- Integración **NeSy + LVLM** como clarificador/juez.
    
- Uso de **reglas y ontologías ligeras** para _repair post-hoc_.
    
- Eficiencia en LVLMs con **resolución dinámica** y **reducción de tokens visuales**.
    
- Evaluación OOD y fairness centrada en **conceptos y trazas de razonamiento**.
    

---

## Implicaciones para el TFM

- Usar HICO-DET-SG / V-COCO-SG como benchmark principal.
    
- Integrar capa simbólica tipo HAKE con LVLM (LLaVA/InternVL/Qwen2-VL).
    
- Analizar sesgos con [CUBIC](https://arxiv.org/abs/2505.11060) y razonamiento con rsbench.
    
- Reportar métricas de fairness (HELM) y generalización sistemática (SG-splits).
    

---

## Posible línea de proceso


---
tags:
	#Index #no-TFM