links: [[001 - 020 Machine Learning|Machine Learning]]


# Hiperparámetros

Los **hiperparámetros** son configuraciones externas que **controlan la arquitectura** y el **comportamiento de los modelos** de ML durante su entrenamiento. A diferencia de los parámetros internos (como los pesos en redes neuronales), **no se aprenden de los datos** sino que se establecen previamente[^5][^6].

### 📌 Características clave de los hiperparámetros:

1. **Control estructural**: Definen la complejidad del modelo (ej. número de capas en redes neuronales)[^2][^5]
2. **Regulación del aprendizaje**: Determinan cómo se ajustan los parámetros internos (ej. tasa de aprendizaje)[^4][^7]
3. **Prevención de sobreajuste**: Parámetros como el **dropout rate** ayudan a regularizar el modelo[^1][^2]

### Principales métodos de ajuste:

| Método | Mecanismo | Ventajas | Desventajas |
| :-- | :-- | :-- | :-- |
| **Búsqueda en cuadrícula (Grid Search)** | Prueba todas las combinaciones predefinidas[^2][^4] | Exhaustivo | Costo computacional alto |
| **Búsqueda aleatoria (Random Search)** | Muestreo aleatorio del espacio de parámetros[^1][^4] | Más eficiente que Grid Search | Puede omitir combinaciones óptimas |
| **Optimización bayesiana** | Modela probabilísticamente el espacio de búsqueda[^5][^7] | Eficiente para espacios complejos | Requiere configuración experta |
| **Algoritmos evolutivos** | Usa principios de selección natural[^5] | Adaptable a diferentes problemas | Alto costo computacional |

### Proceso típico de optimización:

1. **Definición del espacio de búsqueda**: Seleccionar hiperparámetros críticos y sus rangos (ej. número de neuronas: 10-30)[^2][^4]
2. **Elección de métricas**: Usar [[Cross Validation]] para evaluar rendimiento[^1][^4]
3. **Implementación iterativa**:
    - Entrenamiento con diferentes combinaciones[^4][^7]
    - Evaluación mediante métricas como precisión o AUC[^1][^3]
4. **Selección final**: Elegir la combinación con mejor rendimiento en datos de validación[^4][^5]

Los hiperparámetros críticos varían por modelo: en [[001 - 021 Redes Neuronales|redes neuronales]] incluyen **funciones de activación**(|ReLU, sigmoide)[^2][^3], mientras que en [[Decision Trees|árboles de decisión]] sería la **profundidad máxima**[^6]. Su correcto ajuste mejora la capacidad predictiva y reduce problemas como el sobreajuste, logrando un equilibrio óptimo entre sesgo y varianza[^1][^5].

---
tags:
	#Concept #MachineLearning 

[^1]: https://pdfs.semanticscholar.org/7717/e95bead815f804c24fb66667ec9b5b3b431c.pdf

[^2]: https://pdfs.semanticscholar.org/f5f8/9aa81d02cd786db96b6df78dbbede14709ae.pdf

[^3]: https://pdfs.semanticscholar.org/913a/dbbc9ae266293696c355eb1670dcd1da7495.pdf

[^4]: https://pdfs.semanticscholar.org/c591/2631e59e6af06a0d61034fd448884f137475.pdf

[^5]: https://foqum.io/blog/termino/hiperparametro/

[^6]: https://metodotrading.com/hiperparametros/

[^7]: https://aws.amazon.com/es/what-is/hyperparameter-tuning/

[^8]: https://www.datasource.ai/es/data-science-articles/optimizacion-de-hiper-parametros-para-modelos-de-aprendizaje-automatico

[^9]: https://es.wikipedia.org/wiki/Optimización_de_hiperparámetros

[^10]: https://es.linkedin.com/advice/3/what-top-hyperparameter-tuning-techniques-ai-lr7ne?lang=es

[^11]: https://es.eitca.org/inteligencia-artificial/eitc-ai-gcml-google-nube-aprendizaje-automático/primeros-pasos-en-el-aprendizaje-automático/los-7-pasos-del-aprendizaje-automático/¿Cuáles-son-algunos-ejemplos-de-ajuste-de-hiperparámetros%3F/

[^12]: https://www.paradigmadigital.com/dev/optimizando-hiper-parametros-una-perspectiva-teorica/

[^13]: https://pdfs.semanticscholar.org/f471/91be377a5a5765697f63dd7abd09ea4a8de0.pdf

[^14]: https://pdfs.semanticscholar.org/fc11/a7fa69eeef5005849cb581d197a534117831.pdf

[^15]: https://pdfs.semanticscholar.org/58ad/94db3aaa2fa2e05c84d0d4c1e99d3b0a8dfc.pdf

[^16]: https://pdfs.semanticscholar.org/2c67/59bbfa65cc4c2d8b660171863998cdb5a761.pdf

[^17]: https://scholar.google.com/citations?user=yV-zsPgAAAAJ\&hl=es

[^18]: https://pdfs.semanticscholar.org/9b22/6c8fdf520ceb29c453677db72be1fcf95287.pdf

[^19]: https://scholar.google.com/citations?user=v0yL02gAAAAJ\&hl=es

[^20]: https://pdfs.semanticscholar.org/40fa/321047c2c15024f24dc49e8e89f5d0f6ea17.pdf

[^21]: https://pdfs.semanticscholar.org/cc1a/23a291a1b813a438e9a77000a3f6a7e3e05e.pdf

[^22]: https://pdfs.semanticscholar.org/91ac/ff0ad951a7c50a21b1db2a0f947b32bdd566.pdf

[^23]: https://pdfs.semanticscholar.org/eda5/cb13db63d7fe5dae1928c0176335b6a2634b.pdf

[^24]: https://pdfs.semanticscholar.org/9fa3/f7d50c53d3b80dc2e52f1b32f8fe0c5d2c9c.pdf

[^25]: https://arxiv.org/html/2402.15039v1

[^26]: https://developers.google.com/machine-learning/crash-course/linear-regression/hyperparameters?hl=es-419

[^27]: https://iartificial.blog/glossary/hiperparametro/

[^28]: https://forum.huawei.com/enterprise/intl/es/hiperparámetros-en-machine-learning/thread/840089-100263?isInitURL=true

[^29]: https://www.ibm.com/es-es/think/topics/hyperparameter-tuning

[^30]: https://www.youtube.com/watch?v=3Iu5m166rnE

[^31]: https://keepcoding.io/blog/que-son-hiperparametros-en-prompt-engineering/

[^32]: https://es.wikipedia.org/wiki/Hiperparámetro_(aprendizaje_automático)

[^33]: https://codificandobits.com/blog/parametros-hiperparametros-machine-learning/

[^34]: https://dataplatform.cloud.ibm.com/docs/content/wsj/analyze-data/ml_dlaas_hpo.html?locale=es

[^35]: https://www.datacamp.com/es/tutorial/parameter-optimization-machine-learning-models

[^36]: https://www.mundoposgrado.com/optimizacion-de-hiperparametros-modelos-deep-learning/

[^37]: https://es.eitca.org/artificial-intelligence/eitc-ai-gcml-google-cloud-machine-learning/first-steps-in-machine-learning/the-7-steps-of-machine-learning/what-are-some-examples-of-hyperparameter-tuning/

[^38]: https://www.youtube.com/watch?v=EgklwkyieOY

[^39]: https://es.eitca.org/artificial-intelligence/eitc-ai-gcml-google-cloud-machine-learning/introduction/what-is-machine-learning/what-is-the-difference-between-hyperparameters-and-model-parameters/

[^40]: https://es.eitca.org/inteligencia-artificial/eitc-ai-gcml-google-nube-aprendizaje-automático/introducción/que-es-el-aprendizaje-automatico/¿Qué-son-los-hiperparámetros-de-los-algoritmos%3F/

