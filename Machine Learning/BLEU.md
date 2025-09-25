links: [[Métricas de Evaluación]]


# BLEU

BLEU (BiLingual Evaluation Understudy) es una **métrica fundamental** en el procesamiento del lenguaje natural (NLP) utilizada para **evaluar la calidad de traducciones automáticas mediante la comparación con textos de referencia generados por humanos**. Desarrollado por IBM en 2001[^1], se ha convertido en un estándar para medir similitudes lingüísticas de forma automatizada.

### Funcionamiento del BLEU

**Mecanismo de comparación**:

- Analiza secuencias de palabras (n-gramas) entre el texto generado y las traducciones de referencia.
- Calcula una **precisión modificada** que penaliza repeticiones excesivas y favorece la diversidad léxica[^3][^4].
- Aplica una **penalización por brevedad** ($BP = e^{1 - r/c}$) cuando la traducción es más corta que la referencia, evitando puntuaciones infladas en textos truncados[^1][^6].

La fórmula integra estos componentes:

$$
BLEU = BP \cdot \exp\left(\sum_{i=1}^n w_i \log p_i\right)
$$

Donde $p_i$ es la precisión de los i-gramas y $w_i$ son pesos (normalmente uniformes)[^3][^4].

### Interpretación de puntuaciones

| Puntuación BLEU | Calidad percibida |
| :-- | :-- |
| < 0.1 | Casi inútil |
| 0.2-0.29 | Comprensible con errores |
| 0.3-0.39 | Traducciones aceptables |
| 0.5-0.59 | Alta calidad |
| ≥ 0.6 | Excepcional (rara vez alcanzado)[^4] |

### Fortalezas y limitaciones

##### Ventajas:

- **Correlación humana**: Evalúa eficazmente mejoras en sistemas de traducción[^1][^6].
- **Eficiencia**: Automatiza pruebas que requerirían evaluadores humanos[^2][^7].

##### Críticas:

- **Dependencia de referencias**: Asume que existe una única traducción correcta, ignorando variaciones válidas[^2][^7].
- **Ceguera semántica**: No evalúa coherencia gramatical o significado real[^1][^6].
- **Sensibilidad tokenización**: Resultados varían según métodos de separación de palabras[^1].

Para mitigar estos problemas, se recomienda usar variantes como **SacreBLEU** (estandariza tokenización)[^1] y combinar BLEU con métricas como TER o METEOR para evaluaciones holísticas[^4][^7]. Aunque sigue siendo un referente, su utilidad está siendo reevaluada ante enfoques modernos basados en redes neuronales[^2][^7].


---
tags:
	#Concept #MachineLearning #BLEU 


[^1]: https://en.wikipedia.org/wiki/BLEU

[^2]: https://www.acolad.com/es/servicios/traduccion/machine-translation-quality.html

[^3]: https://codelabsacademy.com/en/blog/understanding-bleu-score-in-nlp-evaluating-translation-quality

[^4]: https://codelabsacademy.com/es/blog/understanding-bleu-score-in-nlp-evaluating-translation-quality

[^5]: https://spotintelligence.com/2024/08/13/bleu-score-in-nlp/

[^6]: https://learn.microsoft.com/es-es/azure/ai-services/translator/custom-translator/concepts/bleu-score

[^7]: https://blog.pangeanic.com/es/garantizar-una-buena-traduccion-automatica-mediante-puntuacion-bleu

[^8]: https://es.linkedin.com/advice/1/what-best-ways-evaluate-sequence-to-sequence-models-ybiuf?lang=es

[^9]: https://flashcards.world/flashcards/sets/7d27d3ac-2957-4cbe-bec1-3a2fc662668d/

[^10]: https://huggingface.co/spaces/evaluate-metric/bleu

[^11]: https://learn.microsoft.com/es-es/azure/ai-studio/concepts/evaluation-metrics-built-in

[^12]: https://www.traceloop.com/blog/demystifying-the-bleu-metric

[^13]: https://towardsdatascience.com/foundations-of-nlp-explained-bleu-score-and-wer-metrics-1a5ba06d812b/

[^14]: https://es.wikipedia.org/wiki/BLEU

[^15]: https://www.baeldung.com/cs/nlp-bleu-score

[^16]: https://es.linkedin.com/advice/0/how-do-you-compute-bleu-score-nlp-model-skills-machine-learning-cjsue?lang=es\&lang=es

[^17]: https://fineproxy.org/es/wiki/bleu-score/

[^18]: https://rua.ua.es/dspace/bitstream/10045/11714/1/PLN_43_22.pdf

[^19]: https://help.blip.ai/hc/es-mx/articles/15776146547735-BLU-Blip-Language-Understanding

[^20]: https://dataplatform.cloud.ibm.com/docs/content/wsj/model/wxgov-bleu-metric.html?context=wx\&locale=es

