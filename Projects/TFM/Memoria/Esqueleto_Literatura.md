links: [[103 - TFM]]


# Esqueleto: Literatura

Guía para redactar el capítulo de *Literatura* (`\ref{chap:lit}`) de [[tfm.tex]]. Debe ser **corto** (4–6 páginas) y **sin saltos entre secciones**: cada sección termina con la limitación o pregunta que abre la siguiente. Hace de embudo, de los modelos de difusión al hueco que cubre el TFM (escalabilidad de una formulación ILP).

> [!important] Regla de conexión
> Cada sección cierra con una frase puente que nombre la limitación que resuelve la siguiente. No se repite lo que ya está en la sección 1.2 de la Introducción: aquí se amplía.

## Hilo conductor

```
2.1 Modelos de difusión  ──► σ(S) no tiene expresión cerrada: hay que estimarla
2.2 Algoritmos           ──► todos tratan el grafo como un todo; ignoran su estructura de grupos
2.3 Comunidades          ──► son heurísticos: no garantizan nada sobre su propia formulación
2.4 Programación mat.    ──► los ILP son exactos pero su tamaño crece con la red → CMSA
2.5 Síntesis             ──► nadie ha medido la escalabilidad de DCIIM → este TFM
```

## 2.1 Modelos de difusión

`\section{Modelos de difusión}`

- IC (ya presentado en 1.2; aquí solo se recuerda) y LT, en un párrafo cada uno. La formalización de IC va en el capítulo 3.
- Por qué el TFM usa IC: es el modelo de DCIIM y de los baselines.
- Referencias: `kempe2003maximizing`, `banerjee2020survey`. Opcional: Granovetter (1978) para LT.
- **Puente →** Con cualquiera de los dos modelos, σ(S) solo se puede estimar por simulación, y eso marca el diseño de todos los algoritmos.

## 2.2 Algoritmos de maximización de la influencia

`\section{Algoritmos de maximización de la influencia}`

- Organizar por cómo cada familia se enfrenta al coste de estimar σ(S):
	- Reducir evaluaciones: [[CELF]] (`leskovec2007cost`), [[CELF++]] (`goyal2011celf++`), `mirzasoleiman2015lazier`.
	- Sustituir la simulación por muestreo con garantías: TIM/IMM (`tang2014influence`, `tang2015influence`).
	- Sustituir la simulación por reglas: degree discount (`chen2009efficient`), PMIA ([[chen_2010_scalable]], **falta en el .bib**), centralidad.
	- Metaheurísticas: GRASP (`LozanoOsorio2021`, `lozano2024efficient`) frente a PSO.
	- Paralelismo: `minutoli2019fast`, `wang2024fast`. Conecta con la tarea de paralelizar Monte Carlo.
- **Puente →** Todas estas familias tratan el grafo como un todo. Una línea distinta aprovecha que la influencia se propaga mejor dentro de grupos densos.

## 2.3 Enfoques basados en comunidades

`\section{Enfoques basados en comunidades}`

- Esquema en dos fases: detectar comunidades y repartir las semillas entre ellas. Referencias de Motaz: CIM (Chen et al. 2014), Kazemzadeh et al. 2022, Tyagi y Asawa 2024, FIP (Bouyer et al. 2023).
- **HCIM y α-HCIM** (Hassine 2025, arXiv 2512.03095): son los baselines del TFM. Pesos de arista por similitud de vecindarios, detección de comunidades y reparto de k.
- LFR (`lancichinetti2008benchmark`) como banco de pruebas con comunidades conocidas, que es el que usa el estudio de escalabilidad.
- **Puente →** Siguen siendo heurísticos. La programación matemática permite formular la selección de semillas como un problema de optimización con solución óptima garantizada para esa formulación.

## 2.4 Programación matemática y reducción de instancias

`\section{Programación matemática y reducción de instancias}`

- Formulaciones ILP de IM: Keskin y Güler 2018, Güney 2019, Baghbani et al. 2019. Suelen aproximar la difusión estocástica con escenarios muestreados, y el tamaño del modelo crece con la red y con el número de escenarios.
- DCIIM (`motaz`), que viene del clustering conceptual con minería de patrones (Hassine et al. ECAI 2024): sin escenarios, como un problema de cobertura con solapamiento acotado. Solo la idea: el modelo va en el capítulo 3.
- El problema general de los ILP grandes y las matheurísticas: **CMSA** (`blum2024cmsa`).
	- Construct, merge, solve y adapt: el ILP se resuelve sobre subinstancias reducidas.
	- Aplicado a problemas sobre grafos (MDS, MPIDS). En MPIDS, CPLEX falla a partir de 10 000 nodos y ADAPT_CMSA funciona hasta 100 000.
	- Capítulo 4 del libro: las formulaciones de *set covering* funcionan bien dentro de CMSA, y DCIIM es un modelo de cobertura.
- **Puente →** DCIIM y CMSA comparten ingredientes (ILP, cobertura, grafos), pero nadie ha medido hasta dónde escala DCIIM ni dónde está su coste.

## 2.5 Síntesis y posicionamiento

`\section{Síntesis y posicionamiento}`

- Tabla: familia | cómo estima σ(S) | garantía | escalabilidad | usa solver.
- Hueco: la reformulación de Motaz solo se ha probado en redes pequeñas y no se ha analizado su escalabilidad.
- Cierre hacia el capítulo 3 (Formulación del problema).

## Referencias que faltan en `bibliografia.bib`

- [ ] `hassine2025community`: Hassine (2025), *Community quality and influence maximization: an empirical study*, arXiv 2512.03095 (HCIM, α-HCIM).
- [ ] `hassine2024conceptual`: Hassine et al. (2024), ECAI 2024, pp. 1648–1655.
- [ ] `keskin2018influence`, `guney2019efficient`, `baghbani2019integer`: ILP para IM. **Leer los artículos antes de citar los detalles.**
- [ ] `chen2014cim`, `kazemzadeh2022influence`: comunidades.
- [ ] `chen2010scalable`: PMIA.
