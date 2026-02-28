links: [[001 - 020 Machine Learning|Machine Learning]]

# Prunning

El *pruning* es una **técnica de optimización** que consiste en **eliminar componentes innecesarios** de un modelo para **reducir su complejidad**, mejorar **su eficiencia** y evitar el *overfitting*. Se aplica en redes neuronales, [[Decision Trees|árboles de decisión]] y otros modelos, adaptándose a distintas arquitecturas y objetivos[^4][^6].


### Objetivos principales

- **Reducir la complejidad**: Elimina conexiones, neuronas o capas redundantes sin afectar significativamente la precisión[^4].
- **Acelerar la inferencia**: Modelos más ligeros consumen menos memoria y recursos computacionales, ideales para dispositivos con capacidades limitadas[^1][^6].
- **Mejorar generalización**: Mitiga el sobreajuste al eliminar componentes que capturan ruido en los datos de entrenamiento[^5][^6].


### Tipos de pruning

1. **Estructurado vs. No estructurado**:
    - **Estructurado**: Elimina componentes completos (ej. filtros en capas convolucionales). Favorece la eficiencia en hardware convencional[^1][^6].
    - **No estructurado**: Remueve pesos individuales, generando modelos dispersos. Requiere hardware especializado para máxima eficiencia[^3].
2. **Cronología**:
    - **Pre-poda** (*pre-pruning*): Limita el crecimiento del modelo durante el entrenamiento (ej. restringiendo la profundidad de un árbol de decisión)[^6].
    - **Post-poda** (*post-pruning*): Simplifica un modelo ya entrenado, evaluando el impacto de cada componente en un conjunto de validación[^5][^6].


### Técnicas destacadas

- **Basadas en magnitud**: Eliminan pesos con valores cercanos a cero, asumiendo baja contribución[^2][^6].
- **Guíadas por activaciones**: Priorizan capas con baja actividad neuronal, usando métricas como mapas de atención[^1][^2].
- **Entropía**: Enfoque teórico que identifica capas con baja entropía para removerlas progresivamente (ej. algoritmo EGP)[^3].


### Aplicaciones prácticas
#### Árboles de decisión

```python
# Ejemplo de post-poda en Scikit-learn
clf_pruned = DecisionTreeClassifier(max_depth=3)
clf_pruned.fit(X_train, y_train)
```

La poda reduce la profundidad del árbol, mejorando interpretabilidad y rendimiento[^5].
#### Redes neuronales
- En redes convolucionales, se eliminan filtros redundantes basándose en su contribución a las activaciones[^1].
- Técnicas iterativas ajustan progresivamente la red para mantener la precisión mientras reducen parámetros[^2][^3].


### Ventajas y desafíos

| **Beneficios** | **Limitaciones** |
| :-- | :-- |
| Modelos 10-50% más eficientes[^1][^3] | Requiere ajuste fino de [[hiperparámetros]][^4] |
| Reduce tiempos de inferencia[^6] | Pérdida de precisión si se aplica agresivamente[^6] |
| Facilita despliegue en edge devices[^1] | Complejidad en implementación[^2] |


Esta técnica es fundamental para equilibrar capacidad computacional y rendimiento, especialmente en aplicaciones donde la eficiencia es crítica, como visión por computador o procesamiento de lenguaje natural en dispositivos móviles[^1][^3]. Su elección depende del modelo, los recursos disponibles y los requisitos de precisión.


---
tags:
	#Concept #MachineLearning #Prunning

[^1]: https://arxiv.org/abs/2201.10520

[^2]: https://arxiv.org/html/2109.10795v3

[^3]: https://arxiv.org/abs/2308.06619

[^4]: https://gamco.es/glosario/pruning/

[^5]: https://www.linkedin.com/pulse/pruning-yeshwanth-n

[^6]: https://www.appliedaicourse.com/blog/pruning-in-machine-learning/

[^7]: https://arxiv.org/abs/1901.10539

[^8]: https://arxiv.org/pdf/2001.04062.pdf

[^9]: https://arxiv.org/abs/1909.08174

[^10]: https://arxiv.org/pdf/1901.10539.pdf

[^11]: https://arxiv.org/pdf/2101.09671.pdf

[^12]: https://pdfs.semanticscholar.org/9ea8/ea62d968634e4c2559f5fd7b22c8e56c39c2.pdf

[^13]: https://arxiv.org/abs/2212.01977

[^14]: https://arxiv.org/abs/2403.04805

[^15]: https://pdfs.semanticscholar.org/4724/2bab3917ed0366b1ae3cab52732d17e0e5ea.pdf

[^16]: https://arxiv.org/abs/2403.18955

[^17]: https://arxiv.org/pdf/2006.04981.pdf

[^18]: https://pdfs.semanticscholar.org/5db5/310f172e3889dbdcb7ba8eded98ebcaacd3c.pdf

[^19]: https://arxiv.org/abs/2207.03384

[^20]: https://citeseerx.ist.psu.edu/document?repid=rep1\&type=pdf\&doi=bdda0945f7ef5f434f65fb5e72505d9febe00b52

[^21]: https://pdfs.semanticscholar.org/913a/dbbc9ae266293696c355eb1670dcd1da7495.pdf

[^22]: https://arxiv.org/html/2402.05356v2

[^23]: https://arxiv.org/pdf/1609.05401.pdf

[^24]: https://es.linkedin.com/advice/3/what-most-effective-techniques-pruning-0mlef?lang=es

[^25]: https://en.wikipedia.org/wiki/Decision_tree_pruning

[^26]: https://gpttutorpro.com/machine-learning-pruning-techniques-an-introduction/

[^27]: https://opendatascience.com/what-is-pruning-in-machine-learning/

[^28]: https://www.displayr.com/machine-learning-pruning-decision-trees/

[^29]: https://www.datature.io/blog/a-comprehensive-guide-to-neural-network-model-pruning

[^30]: https://es.linkedin.com/advice/3/how-can-you-use-neural-network-pruning-irx6e?lang=es\&lang=es

[^31]: https://wandb.ai/authors/pruning/reports/Diving-Into-Model-Pruning-in-Deep-Learning--VmlldzoxMzcyMDg

[^32]: https://www.ultralytics.com/es/glossary/pruning

[^33]: https://upload.wikimedia.org/wikipedia/commons/2/23/Before_after_pruning.png?sa=X\&ved=2ahUKEwiSr_mfpv-LAxV9UqQEHTKIAFwQ_B16BAgDEAI

[^34]: https://cienciadedatos.net/documentos/py07_arboles_decision_python

[^35]: https://www.ultralytics.com/es/glossary/model-pruning

[^36]: https://www.youtube.com/watch?v=LcPDBLr9XVw

[^37]: https://azurebrains.com/2020/04/17/fundamentos-para-mejorar-el-rendimiento-y-la-eficiencia-de-tus-modelos-de-deep-learning/

[^38]: https://es.linkedin.com/pulse/pruning-cuantización-y-destilación-jordi-pompas-gutiérrez-p38hf

