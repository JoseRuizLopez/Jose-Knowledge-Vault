links: [[001 - 020 Machine Learning|Machine Learning]]

# Desvanecimiento de Gradientes
Este fenómeno ocurre cuando los gradientes se vuelven **extremadamente pequeños** durante el **entrenamiento de redes neuronales profundas**, lo que dificulta la actualización efectiva de los pesos y puede detener el aprendizaje[^2][^4].

## Como solucionarlo

1. **Conexiones residuales**: Implementar conexiones residuales entre capas ayuda a prevenir inestabilidades en el entrenamiento y permite que la información fluya más fácilmente a través de la red[^2].
2. **Funciones de activación adecuadas**: La elección de la función de activación es crucial. Por ejemplo, la función *Leaky ReLU* ($LeakyReLU(x) = x si x ≥ 0, αx si x < 0$, donde $α$ es típicamente 0.2) puede ayudar a mitigar el problema del desvanecimiento[^2].
3. **Inicialización de pesos**: Una inicialización adecuada de los pesos de la red puede ayudar a prevenir el desvanecimiento del gradiente[^2].
4. **Batch Normalization**: Esta técnica ayuda a estabilizar el entrenamiento y puede reducir el problema del desvanecimiento del gradiente.
5. **Arquitecturas específicas**: Algunas arquitecturas, como las redes neuronales convolucionales (CNN), están diseñadas para manejar mejor este problema en tareas específicas como el procesamiento de imágenes[^4].
6. **Optimizadores avanzados**: El uso de optimizadores como [[Optimizador Adam|Adam]], que adaptan las tasas de aprendizaje durante el entrenamiento, puede ayudar a mitigar el problema del desvanecimiento[^2].
7. **Entrenamiento por etapas**: En algunos casos, como en las redes generativas adversarias (GAN), se puede utilizar un enfoque de entrenamiento por etapas para mejorar la estabilidad y evitar el desvanecimiento[^2].

Estas técnicas, aplicadas de manera individual o en combinación, pueden ayudar significativamente a abordar el problema del desvanecimiento del gradiente en redes neuronales profundas, permitiendo un entrenamiento más efectivo y estable.

---
tags:
	#Concept  #MachineLearning #Desvanecimiento
	

[^1]: https://pdfs.semanticscholar.org/9b22/6c8fdf520ceb29c453677db72be1fcf95287.pdf

[^2]: https://pdfs.semanticscholar.org/0a1d/d639897892251e1452a9f11332ca752bd105.pdf

[^3]: https://pdfs.semanticscholar.org/19e7/a4c4f794cbf7d8ea166a329869b0540c0fad.pdf

[^4]: https://pdfs.semanticscholar.org/e770/34332b18102aaedaf400811ffc9c51b90dc2.pdf

[^5]: https://arxiv.org/pdf/1701.00893.pdf

[^6]: https://pdfs.semanticscholar.org/4261/3b020b8a462a30fa14cbe3f614ed9db39495.pdf

[^7]: https://arxiv.org/html/2402.15039v1

[^8]: https://pdfs.semanticscholar.org/a798/cf7ba84d522bbf8a234ed982c78c14e26dea.pdf

[^9]: https://cursos.frogamesformacion.com/pages/blog/problema-del-desvanecimiento-del-gradiente-en-ia

[^10]: https://msmk.university/hidden-layer/

[^11]: https://www.datacamp.com/es/tutorial/introduction-to-activation-functions-in-neural-networks

[^12]: https://mlearninglab.com/2018/05/06/problema-de-desvanecimiento-del-gradiente-vanishing-gradient-problem/

[^13]: https://es.innovatiana.com/post/overfitting-in-ai

[^14]: https://www.hackio.com/blog/redes-neuronales

[^15]: https://es.wikipedia.org/wiki/Red_neuronal_artificial

[^16]: https://repositorio.uam.es/bitstream/handle/10486/697849/velazquez_pazos_antonio_tfg.pdf

[^17]: https://www.ibm.com/mx-es/think/topics/gradient-descent

[^18]: https://www.youtube.com/watch?v=k3qvYATh77I

[^19]: https://pglez82.github.io/DeepLearningWeb/slides/tema3.pdf

