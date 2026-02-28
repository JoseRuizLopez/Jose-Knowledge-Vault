links: [[001 - 020 Machine Learning|Machine Learning]]

# Optimizador Adam
El **optimizador Adam** (Adaptive Moment Estimation) representa un hito en la evolución de **algoritmos de optimización** para aprendizaje automático. Desarrollado por *Kingma* y *Ba* en 2014[^1], combina técnicas de **momentum** y **escalado adaptativo de gradientes**, ofreciendo ventajas en convergencia y adaptabilidad a geometrías complejas de funciones objetivo.

## Fundamentos Algorítmicos de Adam

### Arquitectura Matemática

**Adam** opera mediante la estimación de momentos de primer y segundo orden de los gradientes:

$$
m_t = \beta_1 m_{t-1} + (1 - \beta_1) g_t 
$$
$$
v_t = \beta_2 v_{t-1} + (1 - \beta_2) g_t^2
$$

donde $g_t$ es el gradiente en el paso $t$, y $\beta_1$,  $\beta_2$ son factores de decaimiento (típicamente 0.9 y 0.999 respectivamente)[^1] . Los momentos se corrigen para eliminar el sesgo inicial:

$$
\hat{m}_t = \frac{m_t}{(1 - \beta_1^t)} 
$$
$$
\hat{v}_t = \frac{v_t}{(1 - \beta_2^t)}
$$

La actualización final de parámetros sigue:

$$
\theta_{t+1} = \theta_t - \frac{\eta \hat{m}_t}{\sqrt{\hat{v}_t} + \epsilon}
$$

Este esquema adapta automáticamente las tasas de aprendizaje por parámetro, equilibrando momentum ($m_t$) y sensibilidad a gradientes raros ($v_t$)[^1][^5].

### Ventajas Comparativas

- **Invariancia a Escalas Diagonales**: Adaptación automática a variaciones en la magnitud de gradientes[^1]
- **Eficiencia en No Convexidad**: Manejo robusto de paisajes de optimización complejos mediante estimación de momentos[^4]
- **Configuración Simplificada**: [[hiperparámetros|Hiperparámetros]] menos sensibles que en SGD clásico o RMSProp[^3]

En aplicaciones como redes convolucionales para clasificación de imágenes, **Adam** muestra reducciones del 30-50% en tiempo de convergencia comparado con SGD con momentum[^6].

## Aplicaciones Prácticas y Configuraciones

### Procesamiento de Lenguaje Natural

En el análisis de sentimiento de tweets en español[^3], Adam demostró superioridad frente a otros optimizadores:

- Tasa de aprendizaje inicial ($\eta$): 1e-4
- Parámetros de decaimiento ($\beta_1$,  $\beta_2$): 0.9 y 0.999
- Dropout (0.5) para regularización, combinado con early stopping

Este enfoque logró un F1-score de 0.78 en clasificación de polaridad, superando en 12 puntos a modelos basados únicamente en Bag-of-Words[^3].

### Visión Computacional

En la detección de tizón tardío en plantas mediante *VGG16*[^6]:

- Tasa de aprendizaje: 1e-5 (ajustada para fine-tuning)
- Batch size: 32
- Pérdida validación: 0.15 vs 0.23 en SGD

La adaptabilidad de Adam permitió ajustar eficientemente capas convolucionales preentrenadas, evitando oscilaciones comunes en optimizadores no adaptativos[^6].

## Debates Teóricos y Limitaciones

### Cuestionamiento a la Convergencia

*Akrout* (2021)[^4] identificó inconsistencias en el análisis original de convergencia de **Adam**:

- El lema 10.3 de *Kingma-Ba* asume acotamiento no realista de gradientes
- Contraejemplos muestran divergencia en funciones convexas simples

La modificación propuesta:

$$
\theta_{t+1} = \theta_t - \eta \frac{\hat{m}_t}{\sqrt{\hat{v}_t} + \epsilon}
$$

con $\epsilon$ adaptativo, restaura garantías de convergencia bajo condiciones más débiles[^4].

### Sensibilidad a [[hiperparámetros|Hiperparámetros]]

Aunque menos crítica que en SGD, la elección de $\beta_1$ y $\beta_2$ afecta:
- Velocidad de adaptación a cambios en la geometría del gradiente
- Estabilidad en fases iniciales de entrenamiento

Estudios empíricos recomiendan:

- $\beta_1 < 0.95$ para problemas con gradientes muy ruidosos
- $\beta_2 \approx 0.999$ mantiene memoria suficiente para escalado[^5]


## Avances Recientes y Variantes

### CaAdam: Optimización Consciente de la Arquitectura

Propuesto en 2024[^5], *CaAdam* introduce factores de escala basados en propiedades estructurales de la red:

- Profundidad de capa
- Número de conexiones
- Distribución de gradientes por capa

La regla de actualización se modifica como:

$$
\eta_{layer} = \eta \cdot \frac{\log(d + 1)}{\sqrt{c}}
$$

donde $d$ es la profundidad y $c$ el número de conexiones. En ResNet-50 sobre CIFAR-10:
- +2.1% de precisión vs **Adam** estándar
- 15% menos iteraciones para convergencia

### AdamW: Desacoplamiento de Peso y Actualización

**AdamW** separa el término de decaimiento de pesos:

$$
\theta_{t+1} = \theta_t - \eta (\hat{m}_t / (\sqrt{\hat{v}_t} + \epsilon) + \lambda \theta_t)
$$

Esto mejora generalización en transformers, reduciendo overfitting en datasets pequeños[^1].

## Guías Prácticas de Implementación

### Elección de [[hiperparámetros|Hiperparámetros]]

| Parámetro | Rango Recomendado | Efecto Principal |
| :-- | :-- | :-- |
| $\eta$ | [1e-5, 1e-3] | Velocidad inicial de aprendizaje |
| $\beta_1$ | [0.85, 0.95] | Memoria de momentum |
| $\beta_2$ | [0.99, 0.999] | Ventana de adaptación escalado |
| $\epsilon$ | [1e-8, 1e-6] | Estabilidad numérica |

### Diagnóstico de Problemas

- **Oscilaciones Persistentes**: Reducir $\beta_1$ o aumentar $\beta_2$
- **Convergencia Lenta**: Escalonar $\eta$ o usar warm-up
- **Overfitting**: Añadir decaimiento de pesos (AdamW) o dropout[^3]


## Conclusiones y Perspectivas Futuras

**Adam** continúa evolucionando como herramienta central en optimización neural. Su éxito radica en la síntesis efectiva de momentum y escalado adaptativo, aunque desafíos persisten en:

1. Garantías teóricas para arquitecturas no convexas profundas
2. Adaptación automática de [[hiperparámetros]] basada en meta-aprendizaje
3. Integración con técnicas de regularización espectral

Variantes como *CaAdam*[^5] y análisis como los de *Akrout*[^4] señalan caminos hacia optimizadores más robustos y conscientes de la estructura de modelos. La sinergia entre intuición geométrica y formalismo matemático seguirá impulsando innovaciones en esta área crítica del aprendizaje profundo.

---
tags:
	#Concept  #MachineLearning 

[^1]: https://arxiv.org/abs/1412.6980

[^2]: https://pdfs.semanticscholar.org/9b29/1907f32135ab5c416a3c78c66a265b72ef8c.pdf

[^3]: https://arxiv.org/pdf/1710.06393.pdf

[^4]: https://arxiv.org/abs/2111.08162

[^5]: https://arxiv.org/abs/2410.24216

[^6]: https://pdfs.semanticscholar.org/f8b8/386348616994df719cbb1516776d88b9c408.pdf

[^7]: https://www.semanticscholar.org/paper/Adam:-A-Method-for-Stochastic-Optimization-Kingma-Ba/a6cb366736791bcccc5c8639de5a8f9636bf87e8

[^8]: https://arxiv.org/html/2410.24216v1

[^9]: https://es.eitca.org/artificial-intelligence/eitc-ai-dltf-deep-learning-with-tensorflow/tensorflow/neural-network-model/examination-review-neural-network-model/how-does-the-adam-optimizer-optimize-the-neural-network-model/

[^10]: https://www.machinelearningmastery.com/adam-optimization-algorithm-for-deep-learning/

[^11]: https://dare.uva.nl/search?identifier=a20791d3-1aff-464a-8544-268383c33a75

[^12]: https://www.ultralytics.com/es/glossary/adam-optimizer

[^13]: https://github.com/xbeat/Machine-Learning/blob/main/Adam Optimizer in Python.md

[^14]: https://www.datacamp.com/es/tutorial/adam-optimizer-tutorial

[^15]: https://docs.pennylane.ai/en/stable/code/api/pennylane.AdamOptimizer.html

[^16]: https://interactivechaos.com/es/manual/tutorial-de-machine-learning/adam

[^17]: https://pdfs.semanticscholar.org/171b/dce4750a79ae7118795fcb03426dddba6a92.pdf

[^18]: https://pdfs.semanticscholar.org/d39a/5ec0670177a2da4b31654e764228d2a51a92.pdf

[^19]: https://arxiv.org/abs/1904.03590

[^20]: https://arxiv.org/html/2405.12807v5

[^21]: https://arxiv.org/pdf/2302.09327.pdf

[^22]: https://scholar.google.com/citations?user=yyIoQu4AAAAJ\&hl=en

[^23]: https://arxiv.org/abs/2403.13704

[^24]: https://pdfs.semanticscholar.org/a4be/db99d7a540d0696f9df70f31c045bda44048.pdf

[^25]: https://arxiv.org/pdf/1412.6980.pdf

[^26]: https://arxiv.org/abs/2405.12807

[^27]: https://pdfs.semanticscholar.org/355a/9d4d266971a159a98cbe7b7144cfb3c54f04.pdf

[^28]: https://scholar.google.com/scholar_lookup?title=Adam%3A+A+method+for+stochastic+optimization\&author=Kingma%2C+D.P.\&author=Ba%2C+J.\&publication_year=2014\&journal=arXiv

[^29]: https://global-strategy.org/modelo-adam-una-estrategia-conceptual-para-repensar-los-ataques-digitales-y-actualizar-las-estrategias-de-seguridad-y-control-vigente/

[^30]: https://www.youtube.com/watch?v=iyMFcF36gO0

[^31]: https://codificandobits.com/curso/fundamentos-deep-learning-python/redes-neuronales-24-adam/

[^32]: https://stackoverflow.com/questions/41305918/in-cntk-implementation-of-adam-optimizer-how-the-parameters-alpha-beta1-beta2

[^33]: https://www.riego.mx/congresos/comeii2021/files/ponencias/extenso/COMEII-21005.pdf

[^34]: https://docs.aws.amazon.com/es_es/sagemaker/latest/dg/object-detection-tensorflow-Hyperparameter.html

[^35]: https://www.researchgate.net/publication/269935079_Adam_A_Method_for_Stochastic_Optimization

[^36]: https://www.datacamp.com/es/tutorial/adamw-optimizer-in-pytorch

[^37]: https://d2l.ai/chapter_optimization/adam.html
