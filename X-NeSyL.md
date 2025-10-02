# EXplainable Neural-Symbolic Learning (X-NeSyL) methodology to fuse deep learning representations with expert knowledge graphs: the MonuMAI cultural heritage use case
_(N. Díaz-Rodríguez, A. Lamas, J. Sánchez, G. Franchi, I. Donadello, S. Tabik, D. Filliat, P. Cruz, R. Montes, F. Herrera) [arXiv](https://arxiv.org/abs/2104.11914)_

**Objetivo / Problema**  
Este es el paper base de **X-NeSyL**. Su meta es fusionar representaciones aprendidas por redes profundas con conocimiento experto (KG) de modo que el modelo sea más explicable para humanos. Se orienta al trade-off entre rendimiento predictivo y explicabilidad simbólica. [arXiv+1](https://arxiv.org/abs/2104.11914?utm_source=chatgpt.com)

**Metodología / enfoque técnico**
- Usa una arquitectura llamada **EXPLANet** (Expert-aligned eXplainable Part-based cLAssifier NETwork), que integra partes visuales con conocimiento simbólico. [arXiv+2ScienceDirect+2](https://arxiv.org/abs/2104.11914?utm_source=chatgpt.com)
- Emplea un procedimiento llamado **SHAP-Backprop**, que utiliza explicaciones tipo SHAP durante el entrenamiento para alinear la atención/atribución de la red con lo que dicta el conocimiento simbólico (el KG). [Semantic Scholar+3arXiv+3ScienceDirect+3](https://arxiv.org/abs/2104.11914?utm_source=chatgpt.com)
- Genera lo que llaman un **SAG** (Semantic Attribution Graph) en tiempo de inferencia, que se puede comparar con el KG como grafo de atribución empírica vs teórico. [ACM Digital Library+3arXiv+3Semantic Scholar+3](https://arxiv.org/abs/2104.11914?utm_source=chatgpt.com)
- Plantea una métrica de **alineación explicativa** que mide cuán bien las explicaciones del modelo coinciden con las del experto (KG). [ACM Digital Library+3arXiv+3ScienceDirect+3](https://arxiv.org/abs/2104.11914?utm_source=chatgpt.com)

**Contribución clave**
- Probar que incorporar conocimiento simbólico en el entrenamiento no solo aporta explicabilidad, sino que puede mejorar el rendimiento (o al menos no degradar mucho). [Semantic Scholar+3arXiv+3ScienceDirect+3](https://arxiv.org/abs/2104.11914?utm_source=chatgpt.com)
- Mostrar que se puede “alinear” la interpretación interna del modelo (via SHAP) con el conocimiento experto.
- Un caso de estudio concreto: [[MonuMAI]], con clasificación de fachadas monumentales. [ACM Digital Library+3arXiv+3ScienceDirect+3](https://arxiv.org/abs/2104.11914?utm_source=chatgpt.com)

**Cómo mencionarlo en mi [[TFM]]**
- Puedo usarlo como **referente metodológico central**: “mi propuesta adapta la metodología X-NeSyL al dominio de escenas complejas, extendiéndola hacia relaciones espaciales y funcionales”.
- Puedo reutilizar su métrica de alineación explicativa, o adaptarla para tu dominio.
- Puedo estudiar y comparar tu **SAG vs KG** como ellos hacen, pero en un dominio diferente.
- Si Siham pregunta “¿qué se ha hecho ya con X-NeSyL?”, este es el paper que se debe dominar.