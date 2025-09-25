links: [[Métricas de Evaluación]]


# ROUGE

ROUGE (Recall-Oriented Understudy for Gisting Evaluation) es un **conjunto de métricas** utilizadas en el procesamiento del lenguaje natural (NLP) para **evaluar la calidad de textos generados automáticamente**, como resúmenes o traducciones. **Compara el contenido generado** con **textos de referencia escritos por humanos**, asignando puntuaciones entre 0 y 1, donde valores más altos indican mayor similitud[^1][^3][^6].

## Principales métricas de ROUGE

Las variantes más destacadas incluyen:

- **ROUGE-N**: Evalúa la superposición de *n-gramas* (secuencias de N palabras).
    - *ROUGE-1*: Analiza coincidencias de palabras individuales (unigramas)[^1][^4].
    - *ROUGE-2*: Examina pares de palabras consecutivas (bigramas)[^1][^3].
- **ROUGE-L**: Mide la subsecuencia común más larga (LCS), considerando la estructura y orden de las palabras[^1][^6].
- **ROUGE-S**: Usa *skip-bigrams* (pares de palabras no necesariamente consecutivas)[^3][^5].
- **ROUGE-SU**: Combina skip-bigrams con unigramas para mayor flexibilidad[^5].


## Funcionamiento y aplicaciones

ROUGE se centra en la **cobertura (recall)**, priorizando que el texto generado incluya toda la información relevante del texto de referencia. Esto lo diferencia de métricas como [[BLEU]], usada en traducción automática, que enfatiza la precisión[^6].

Es especialmente útil para:

1. Comparar **modelos de generación de texto**[^4].
2. Evaluar **cambios en modelos** (como reducción de tamaño o cuantización)[^4].
3. Validar la **efectividad de resúmenes** automáticos en tareas como extracción de información[^6].

## Consideraciones clave

- La **calidad de los textos de referencia** determina la fiabilidad de las métricas[^4].
- No evalúa **coherencia semántica**, solo **superposición léxica**[^6].
- Implementaciones populares incluyen bibliotecas Python como `rouge-score`, que requieren preprocesamiento de textos (ej. añadir saltos de línea)[^4].

> "ROUGE proporciona una medida cuantitativa de similitud, pero no sustituye la evaluación humana de la calidad lingüística"[^6].

Estas métricas son estándar en investigación de NLP, aunque se recomienda complementarlas con otros métodos de evaluación para capturar aspectos como la fluidez o la relevancia contextual[^5][^6].



---
tags:
	#Concept #MachineLearning #Metricas

[^1]: https://es.wikipedia.org/wiki/ROUGE_(métrica)

[^2]: https://docs.aws.amazon.com/es_es/sagemaker/latest/dg/autopilot-llms-finetuning-metrics.html

[^3]: https://en.wikipedia.org/wiki/ROUGE_(metric)

[^4]: https://martra.uadla.com/como-evaluar-la-calidad-de-los-resumenes-generados-por-grandes-modelos-de-lenguaje-mediante-rouge/

[^5]: https://spotintelligence.com/2024/08/12/rouge-metric-in-nlp/

[^6]: https://foqum.io/blog/termino/rouge/

[^7]: https://es.linkedin.com/pulse/importancia-de-las-métricas-en-evaluación-modelos-llm-lanas-ocampo-nsiqe

[^8]: https://www.toolify.ai/es/ai-news-es/rouge-mtrica-de-resmenes-3099693

[^9]: https://aws.amazon.com/es/what-is/nlp/

[^10]: https://www.linkedin.com/advice/1/what-rouge-score-how-can-you-use-evaluate-nlp-euj9e

[^11]: https://es.linkedin.com/advice/1/what-rouge-score-how-can-you-use-evaluate-nlp-euj9e?lang=es\&lang=es

[^12]: https://pub.aimind.so/unveiling-the-power-of-rouge-metrics-in-nlp-b6d3f96d3363

[^13]: https://blog.cubed.run/understanding-evaluation-metrics-bleu-rouge-and-perplexity-explained-f8b00e5ac89f

[^14]: https://es.linkedin.com/advice/1/what-best-ways-evaluate-sequence-to-sequence-models-ybiuf?lang=es

[^15]: https://www.freecodecamp.org/news/what-is-rouge-and-how-it-works-for-evaluation-of-summaries-e059fb8ac840/

[^16]: https://huggingface.co/spaces/evaluate-metric/rouge

[^17]: https://rua.ua.es/dspace/bitstream/10045/11707/1/PLN_43_15.pdf

[^18]: https://www.galileo.ai/blog/rouge-ai

