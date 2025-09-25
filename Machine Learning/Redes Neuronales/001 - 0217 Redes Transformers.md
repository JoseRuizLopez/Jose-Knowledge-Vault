links: [[001 - 021 Redes Neuronales|Redes Neuronales]]

# Redes Transformers
![[transformer.png|263x365]]

La **arquitectura Transformer** fue introducida en 2017, esta estructura neuronal superó las limitaciones de modelos anteriores mediante un enfoque basado en **mecanismos de atención auto-supervisada**, permitiendo procesar secuencias textuales de forma no secuencial[^1][^3]. Su impacto trasciende el ámbito académico, siendo la base tecnológica de sistemas como *ChatGPT*, *BERT* y *T5*, que han redefinido las capacidades de generación y comprensión de lenguaje artificial.

## Principios Fundamentales de la Arquitectura

### Mecanismo de Atención Multi-Head

El núcleo innovador de los Transformers reside en su **sistema de atención**, que calcula relaciones contextuales entre todas las palabras de una secuencia simultáneamente. A diferencia de las [[001 - 0213 Redes Neuronales Recurrentes|RNN]] que procesan elementos en orden secuencial, este mecanismo **genera representaciones dinámicas** ponderando la importancia relativa de cada palabra respecto a las demás[^3]. La **implementación multi-head** amplía esta capacidad mediante **múltiples subespacios de atención** que capturan distintos tipos de dependencias semánticas y sintácticas.

### Codificadores y Decodificadores Apilados

La arquitectura estándar combina **módulos de codificación** y **decodificación** en capas sucesivas. Cada codificador transforma las representaciones de entrada mediante **atención auto-dirigida** y redes **feed-forward**, mientras los decodificadores **integran información del codificador** y las **posiciones anteriores de la secuencia objetivo**[^1]. Esta disposición permite manejar tareas de secuencia a secuencia como traducción automática, donde el modelo debe considerar tanto el contexto fuente como el objetivo generado.

### Codificación Posicional No Recurrente

Para compensar la ausencia de procesamiento secuencial, los Transformers incorporan **embeddings posicionales** que inyectan **información** sobre el **orden de las palabras**. Estas codificaciones utilizan **funciones sinusoidales** o **aprendidas** que asignan vectores únicos a cada posición, permitiendo al modelo discernir relaciones de proximidad y estructura sintáctica sin recurrir a recursividad[^3].

## Avances sobre Modelos Previos

### Superación de Limitaciones de RNN/LSTM

Las redes recurrentes tradicionales sufrían de problemas de [[Desvanecimiento de Gradientes]] de gradientes y capacidad limitada para **capturar dependencias a larga distancia**. Un estudio comparativo demostró que los Transformers logran un 38% mayor precisión en tareas de co-referencia que las LSTM, particularmente en secuencias superiores a 200 tokens[^1]. La paralelización completa de las operaciones reduce además los tiempos de entrenamiento en órdenes de magnitud respecto a arquitecturas secuenciales.

### Unificación de Paradigmas de Entrenamiento

Los Transformers permitieron por primera vez el **pre-entrenamiento masivo en corpus no etiquetados** seguido de **fine-tunning para tareas específicas**. Esta aproximación, ejemplificada por BERT y GPT, aprovecha el aprendizaje auto-supervisado mediante objetivos como el enmascaramiento de palabras (MLM) o predicción de siguiente token[^2]. La transferencia de conocimiento entre dominios reduce la necesidad de datos etiquetados, disminuyendo los costes de desarrollo de aplicaciones especializadas.

## Componentes Clave y Funcionamiento

### Capa de Autoatención
La autoatención permite que cada palabra de una secuencia **asigne importancia a otras palabras** en la misma secuencia, ayudando a capturar dependencias a largo plazo.

Cada cabeza de atención calcula **tres vectores** (query, key, value) **para cada palabra**, determinando su relevancia respecto al resto mediante **producto escalar normalizado**. 
Donde cada vector tiene un funcionamiento:
- Vector **Q**uery: Representa la pregunta que una palabra hace sobre el contexto.
- Vector **K**ey: Representa la información que cada palabra ofrece sobre sí misma.
- Vector **V**alue: Representa la información que se transfiere a la siguiente capa si una palabra es relevante.
La operación matemática fundamental se expresa como:

$$
\text{Atención}(Q,K,V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V
$$

Donde $d_k$ representa la dimensionalidad de los vectores key, factor que estabiliza los gradientes durante el entrenamiento[^3]. La concatenación de múltiples cabezas **permite capturar distintos tipos de relaciones contextuales en paralelo**.

### Redes Feed-Forward por Capa

Tras la etapa de atención, cada capa aplica una red neuronal totalmente conectada con activación ReLU a las representaciones transformadas. Este componente **introduce no-linealidades y capacidad de modelado de características locales**, complementando el enfoque global de la atención[^1].

### Normalización y Residualidad

Cada subcapa (atención y feed-forward) incorpora **conexiones residuales** y **normalización de capa**, técnica que **mitiga el problema de [[Desvanecimiento de Gradientes|desvanecimiento de gradientes]] en redes profundas**. Este diseño permite apilar decenas de capas sin comprometer la estabilidad del entrenamiento, facilitando el escalado a modelos con billones de parámetros[^2].

## Aplicaciones y Modelos Derivados

### Generación Autoregresiva (GPT)

La serie GPT (Generative Pre-trained Transformer) utiliza únicamente el componente **decodificador**, generando texto token por token mediante atención causal (que considera únicamente posiciones anteriores). GPT-3 demostró capacidades emergentes como **razonamiento few-shot** y **composición de instrucciones no vistas durante el entrenamiento**[^2].

### Modelos Bidireccionales (BERT)

BERT emplea el stack de **codificadores con pre-entrenamiento** mediante **enmascaramiento de tokens** y **predicción de secuencias contiguas**. Su diseño bidireccional permite crear representaciones contextuales que capturan relaciones semánticas complejas, logrando state-of-the-art en tareas como respuesta a preguntas y análisis de sentimiento[^1].

### Arquitecturas Texto-a-Texto (T5)

El modelo T5 unifica todas las tareas de NLP bajo un esquema de **transformación texto-a-texto**, demostrando **versatilidad** sin precedentes. Por ejemplo, traduce frases reformulando "Traducir al francés: {texto}" y resume documentos mediante "Resumir: {documento}"[^1]. Este enfoque simplifica la pipeline de desarrollo al reducir la necesidad de arquitecturas especializadas.

## Desafíos Actuales y Limitaciones

### Coste Computacional Exponencial

La complejidad cuadrática del mecanismo de atención respecto a la longitud de secuencia (O(n²)) limita su aplicabilidad a textos extensos. Técnicas como **atención esparsa** o **basada en patrones reducen este costo**, pero introducen **trade-offs** en capacidad de modelado. Entrenar modelos como GPT-3 requiere clusters de miles de GPUs durante semanas, con huellas energéticas equivalentes a 120 hogares anuales[^1].

### Sesgos y Alineación Ética

Los modelos basados en Transformers **heredan** y **amplifican** **sesgos** presentes en los **datos de entrenamiento**. Estudios muestran disparidades del 15-20% en calidad de generación entre idiomas mayoritarios y lenguas minoritarias, reflejando desequilibrios en los corpus de pre-entrenamiento[^2]. El ajuste por refuerzo con retroalimentación humana (RLHF) mitiga pero no elimina estos problemas, requiriendo marcos de auditoría continuos.

## Futuras Direcciones de Investigación

### Transformers Especializados

Líneas emergentes buscan adaptar la arquitectura base a dominios específicos:

- **BioTransformers**: Modelado de secuencias proteicas y ADN
- **Codex**: Generación de código con comprensión de contexto programático
- **Vision Transformers**: Aplicación a imágenes mediante división en parches

### Optimizaciones de Eficiencia

Técnicas como **mezcla de expertos** (MoE), **cuantización dinámica** y **podado selectivo** permiten reducir el coste inferencial sin pérdida significativa de rendimiento. El modelo Switch Transformer demostró que sistemas con billones de parámetros pueden ejecutarse eficientemente mediante enrutamiento adaptativo[^1].


---
tags:
	#Concept  #RedesNeuronales 


[^1]: https://pdfs.semanticscholar.org/533e/6c09d4229cb0aa3d4ea2589a35a0cc080cdf.pdf

[^2]: https://arxiv.org/pdf/2502.18205.pdf

[^3]: https://pdfs.semanticscholar.org/4b89/c294fa7c33eef07a734d0c3dfdd1b3dbb9b2.pdf

[^4]: https://pdfs.semanticscholar.org/3885/bb174a773214acca49cb80c5d3d550646a1d.pdf

[^5]: https://pdfs.semanticscholar.org/811e/a95912fdef85a9a3230c24b3fdbf159caf53.pdf

[^6]: https://pdfs.semanticscholar.org/5a8a/6cf8db531afd5a41200997c438e9bd0edc48.pdf

[^7]: https://pdfs.semanticscholar.org/1644/9829845fb88990ac37c078f932ff024b4586.pdf

[^8]: https://aws.amazon.com/es/what-is/transformers-in-artificial-intelligence/

[^9]: https://es.linkedin.com/pulse/de-principiante-pro-descubre-los-secretos-en-ia-tres-jordi-85bqf

[^10]: https://mindfulml.vialabsdigital.com/post/el-mecanismo-de-atencion-en-modelos-transformer/

[^11]: https://www.youtube.com/watch?v=h-kwi6il0t8

[^12]: https://dbaexperts.tech/wp/database/arquitectura-transformer/

[^13]: https://mindfulml.vialabsdigital.com/post/los-transformers-estan-transformando-la-inteligencia-artificial/

[^14]: https://huggingface.co/docs/transformers/es/attention

[^15]: https://www.athos-cap.com/positional-encoding-el-gps-de-los-modelos-transformer/

[^16]: https://pdfs.semanticscholar.org/c8bb/f75ffbf962898e316b34c7b587b5df120490.pdf

[^17]: https://pdfs.semanticscholar.org/2f5d/42896d146ecfaa84baaf51e672f1fdc5d3be.pdf

[^18]: https://pdfs.semanticscholar.org/7426/e5b8d5c894bb8918db6863cf1cb2a74c6ce6.pdf

[^19]: https://citeseerx.ist.psu.edu/document?repid=rep1\&type=pdf\&doi=816cb4c894ca79c28ec6238e4b9047e6a346f0a1

[^20]: https://arxiv.org/abs/2410.02703

[^21]: https://arxiv.org/pdf/2302.09327.pdf

[^22]: https://pdfs.semanticscholar.org/e474/a3407399a25735e013002e7a35b58ff74ff8.pdf

[^23]: https://arxiv.org/html/2402.15039v1

[^24]: https://pdfs.semanticscholar.org/25cb/f93eb0098478f7ef35577542c05957216713.pdf

[^25]: https://www.semanticscholar.org/paper/77247e4b1a0cf58996d9bc9f2c45435340af1935

[^26]: https://www.semanticscholar.org/paper/ChattGPT,-ventajas,-desventajas-y-el-uso-en-la-Guamán/92c84def8a66da89ea08480abb61de7b9dcfb828

[^27]: https://arxiv.org/abs/2309.16354

[^28]: https://pdfs.semanticscholar.org/825f/167de606b769f0b6f7d85e85b8c61969e70b.pdf

[^29]: https://www.datacamp.com/es/tutorial/how-transformers-work

[^30]: https://www.youtube.com/watch?v=aL-EmKuB078

[^31]: https://www.juansensio.com/blog/060_attention

[^32]: https://codificandobits.com/curso/fundamentos-deep-learning-python/redes-transformer-5-codificacion-codificador-posicional/

[^33]: https://aiexplorers.blog/2023/05/16/que-es-la-arquitectura-transformer-en-la-que-se-basa-chatgpt/

[^34]: https://www.themachinelearners.com/transformer/

[^35]: https://www.robertocrespo.net/transformer-attention-is-all-you-need/

[^36]: https://www.youtube.com/watch?v=RA4Po-yN-Rw

[^37]: https://www.victormolla.com/transformers

[^38]: https://delatorre.ai/los-transformers-en-la-inteligencia-artificial-una-explicacion-sencilla/

[^39]: https://www.dlsi.ua.es/~japerez/materials/transformers/attention/

[^40]: https://codificandobits.com/blog/redes-transformer/

