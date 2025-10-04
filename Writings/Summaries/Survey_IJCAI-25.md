links: [[008 Summaries|Summaries]] 

Reference: [[Survey_IJCAI-25.pdf|Neuro-Symbolic Artificial Intelligence: A Task-Directed Survey in the Black-Box Models Era]]

# Resumen del Survey_IJCAI-25
El artículo defiende que, tras el “boom” de modelos _black-box_, la IA **neuro-simbólica (NeSy)** vuelve a ser clave para ganar **explicabilidad, coherencia lógica y generalización composicional**. Lo hace con una revisión “**orientada a tareas**” (no solo por arquitecturas) de trabajos 2017–2024 en NLP y Visión, y compara el rendimiento NeSy vs. black-box en _benchmarks_ reales (no solo sintéticos). Además, sanea el término NeSy: aquí **solo** cuenta lo que **integra redes neuronales con componentes simbólicos** (solvers, reglas lógicas, autómatas, etc.), excluyendo “explicabilidad post-hoc” sin estructura simbólica real.

### Metodología de la revisión
- **Búsqueda sistemática (DBLP SPARQL)** en venues top (AAAI, IJCAI, NeurIPS, ICLR, ICML, ACL/EMNLP/CVPR/ICCV) para 2017–2024 con _keywords_ como “neuro-symbolic”, “probabilistic logic”, “ILP”, etc. Se cribaron **172 papers** tras excluir trabajos no NeSy o solo lógicos/sin interpretabilidad.

- Identifican **problemas de reproducibilidad y comparabilidad**, p. ej., **inconsistencias en WN18RR** y exceso de _benchmarks_ sintéticos “a medida”. Abogan por **comparaciones justas** en tareas reales.

## Taxonomía “orientada a tareas”
Propone **tres macro-familias** metodológicas y mapea para cada una **tareas y _benchmarks_** (fig. de la encuesta):

1. **Rule Mining** (aprender reglas) → Horn Clauses (ILP), Natural Logic, DFA.

2. **Rule Enforcement** (imponer reglas) → Lógica de Primer Orden (FOL), **Probabilistic Logic (PL/PSL)**, DFA (recompensas/escudos en RL).

3. **Program Synthesis** (sintetizar programas/DSL) → **Gramáticas Libres de Contexto** y **Semantic Parsing**.

La figura sitúa tareas como **DocRE (DWIE), NLI/FOLIO, Temporal Commonsense (McTACO, TIMEDIAL), VQA/ReferIt3D, verificación de redes, RL (reward shaping/shielding con DFA), edición de imagen basada en reglas**, etc.

### 1) Rule Mining (aprender reglas)
**Idea:** extraer reglas interpretables desde datos; la complejidad depende del formalismo (HC > NatLog > DFA).

- **Horn Clauses / ILP.**
    - **DFORL (2024)**: extracción eficiente con _propositionalization_ y búsqueda acotada por profundidad; mejora eficiencia pero rendimiento **inconsistente en WN18RR**.
    - **NCRL (2023)**: composicionalidad de reglas mediante muestreo de caminos y atención; **mejora WN18RR** en ausencia de relaciones inversas.
    - **Fold-SE / NeSyFOLD (2024):** ILP para tabular/visión (reglas desde máscaras de activación CNN). Limitaciones por dependencia de CNN y etiquetado semántico restringido.
    - **JMLR (2024) en DocRE-DWIE:** _soft-proof_ sobre HCs que **supera** al black-box DocRE-CLiP en DWIE (ver tabla de competitividad).

- **Natural Logic:** extracción de operadores lógicos “naturales” con preprocesado lingüístico + QA (casos en NLI/IE). (Marco dentro de la misma sección).

- **DFA (aprendizaje de autómatas):** aprender transiciones desde _traces_ + **SAT**. Útil cuando la **secuencia de estados** es clave.

**Cuándo usarlo:** cuando necesitas **reglas explícitas** y datos estructurados; cuidado con escalabilidad y “huecos” en _benchmarks_.

### 2) Rule Enforcement (hacer cumplir reglas)
**Idea:** inyectar reglas como **pérdidas/regularizadores** o **módulos** que **garantizan consistencia**.

- **FOL en generación de imágenes:** derivar **restricciones FOL** con _dependency parsing_ y aplicarlas como **regularización** en mapas de atención; soporta **cuantificadores**. (Control + confianza, no “minería” de reglas).

- **Probabilistic Logic (PSL/PL):**
    - **SLEER (2022)** para **temporal commonsense**: multitarea + **reglas PSL** (t-norms) enlazando _heads_ → **mejoras marginales** en McTACO.
    - **LECTER (2023)**: contexto + **inducción lógica temporal** + **DeepProbLog**; **fuerte _zero-shot_ en TIMEDIAL** (2-best acc. > GPT-3.5 en esa métrica), aunque falta demostrar **generalización** fuera de TIMEDIAL.

- **Verificación neuro-simbólica (NeSAL):** especificar propiedades de redes con un fragmento de FOL y verificar **estabilidad/ equivalencias**; útil en tareas de **alto riesgo**.

- **RL con reglas (seguridad):** _shielding_ y **reward shaping** con **DFA** y **PL**, mejorando **convergencia** y **seguridad** de políticas (cuándo _shaping_ vs _shielding_ funciona mejor).

**Cuándo usarlo:** cuando requieres **confianza, seguridad o cumplimiento de invariantes** (p. ej., medicina, conducción, diálogo seguro).

### 3) Program Synthesis (programas/DSL)
**Idea:** generar **programas estructurados** (general-purpose o DSL) que guían predicción/razonamiento; **muy expresivo**, pero más coste (parsing/búsqueda).

- **CFG / DSL.** **NESTER (2024)**: A* sobre una **gramática** para **causal effect estimation**; competitivo en _benchmarks_ pequeños, dudas en **escalabilidad**.

- **Semantic Parsing a FOL.** **LINC (2023)**: _parsing_ a **FOL** y prueba con **Prover9**; **mejor que CoT/scratchpad** en sintéticos, competitivo en expertos; **limitación**: **errores de _parsing_** y sugieren **CFG + comprobación de consistencia**.

**Cuándo usarlo:** cuando necesitas **razonamiento simbólico fuerte** y **explicaciones ejecutables**; vigilar robustez del _parser_.

## Competitividad NeSy vs. Black-Box
La encuesta aporta una **tabla comparativa**: NeSy **gana** en tareas de **regulación explícita** (DocRE con JMLR en DWIE; TIMEDIAL con LECTER _2-best_), pero **pierde** con frecuencia en **open-domain** donde los _black-box_ explotan datos masivos no etiquetados (FEVER, Sr3D). Conclusión: hay **trade-off interpretabilidad ↔ eficiencia/datos** y **sensibilidad al _benchmark_**.

## Limitaciones y líneas futuras
- **Sensibilidad al _benchmark_** (p. ej., McTACO vs TIMEDIAL) y a decisiones de muestreo; urge **estandarizar evaluaciones**.

- **Razonamiento visual** tipo NS-CL podría estar **cerca de la saturación**; se pide trasladar lecciones a dominios más complejos/realistas.

- **Regularización simbólica** como antídoto al sesgo y a la dependencia de datos sintéticos; **composicionalidad** con menor dependencia del tamaño de entrenamiento.

---
## Qué te aporta al [[TFM]]

1. **Escoge familia según objetivo:**
    - **Enforzamiento de reglas** (FOL/PSL) para **coherencia y seguridad** en _scene understanding_ (p. ej., reglas espaciales/funcionales en HOI/SG).
    - **Synthesis** (LINC-style) si quieres **pruebas/explicaciones ejecutables**; añade **CFG + chequeos** para robustez.
    - **Mining** (ILP/HC) si necesitas **descubrir** relaciones en tus datos/KB (p. ej., reglas en KG).

2. **Evalúa en _splits_ que midan generalización composicional** (evita solo sintéticos; usa _benchmarks_ realistas y métricas de **consistencia lógica** además de AP/Acc).

3. **Acepta el trade-off**: donde prime explicabilidad/seguridad, NeSy es **ventajoso**; en abierto y data-hungry, un _black-box_ puede rendir más—o combinar ambos.


---
tags:
	#Writing #Summary