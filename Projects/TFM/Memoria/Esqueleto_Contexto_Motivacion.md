links: [[103 - TFM]]


# Esqueleto: Contexto y motivación

Guía para redactar la sección *Contexto y motivación* (`\ref{sec:contexto}`) del capítulo de Introducción en [[tfm.tex]]. Sigue una estructura en embudo: de lo general (difusión en redes sociales) al hueco concreto (escalabilidad de DCIIM) que justifica el TFM. Cada bloque se corresponde con un `\subsection` del documento de Overleaf.

## 1. La difusión de información en redes sociales

`\subsection{La difusión de información en redes sociales}`

- Por qué importa cómo se propaga la información en una red social.
- Aplicaciones: *viral marketing* (`domingos2001mining`), campañas de salud pública, contención de desinformación (`Budak2011`, `Kimura2009`).
- Definición intuitiva del problema: elegir *k* nodos semilla (*seed nodes*) que maximicen la propagación esperada.

## 2. El problema de la maximización de la influencia

`\subsection{El problema de la maximización de la influencia}`

- Formulación clásica de [[kempe_kleinberg_tardos_2003]] (`kempe2003maximizing`) con el [[Independent Cascade Model]].
- El problema es [[NP-Hard]].
- El [[Greedy Hill-Climbing]] ofrece una garantía de (1 - 1/e) gracias a que σ(S) es una [[Submodular Function]], pero estimar σ(S) mediante [[Monte Carlo Simulation]] es muy costoso (`metropolis1949monte`, `kalos2008monte`).
- Familias de enfoques según [[banerjee_2020_survey_influence_maximization 1]] (`banerjee2020survey`):
	- Greedy y optimizaciones: [[CELF]] (`leskovec2007cost`), [[CELF++]] (`goyal2011celf++`), *lazier than lazy greedy* (`mirzasoleiman2015lazier`).
	- Heurísticas eficientes y algoritmos casi lineales: `chen2009efficient`, `tang2014influence`, `tang2015influence`.
	- Implementaciones escalables y paralelas: `minutoli2019fast`, `wang2024fast`.
	- Heurísticas de centralidad: [[Degree Centrality Heuristic]], [[Distance Centrality Heuristic]].
	- Métodos basados en comunidades: [[Community-Based Seed Selection]].
	- Metaheurísticas: GRASP de [[lozano_2023_grasp_snimp]] (`LozanoOsorio2021`), `lozano2024efficient`, `resende2013grasp`.
- Solo un párrafo: el detalle va en el Estado del arte.

## 3. Una reformulación basada en clustering descriptivo

`\subsection{Una reformulación basada en clustering descriptivo}`

- Propuesta de Ben Hassine: [[motaz_2026_reformulating_influence_maximization]] (`motaz`).
- El grafo se representa como un *transactional dataset*: el vecindario de cada nodo es una transacción.
- Formulación como *Integer Linear Programming* (ILP), resuelta con [[PuLP]] y [[CBC Solver|CBC]] o Gurobi.
- Modelo DCIIM: inspirado en *descriptive clustering*, con solapamiento controlado mediante *overlap allowance* (φ).
- Cierre: por qué es atractiva (optimalidad para su formulación, resultados competitivos).

## 4. Limitaciones de escalabilidad y motivación del trabajo

`\subsection{Limitaciones de escalabilidad y motivación del trabajo}`

- El autor solo evalúa redes de tamaño reducido; la escalabilidad no se analiza.
- Estudio de escalabilidad propio con redes sintéticas (LFR) y reales de tamaño creciente.
- Hallazgos a adelantar sin cifras (detalle en Resultados):
	- El tiempo crece de forma muy pronunciada con el tamaño de la red.
	- El cuello de botella no es el ILP, sino la simulación Monte Carlo y la construcción de `coverage_count`.
	- En redes grandes, ciertos valores de φ pueden impedir encontrar una solución factible.
- Motivación final y puente a Objetivos: la reformulación es valiosa pero no aplicable a redes reales medianas o grandes.
- Pregunta guía: ¿cómo conservar la perspectiva de *descriptive clustering* haciéndola escalable?
- Anunciar la línea de solución (por ejemplo CMSA, ver [[CMSA-Capitulo1]], o metaheurísticas) sin entrar en detalle.

## Pendientes de redacción

- [X] Redactar el bloque 1 desde cero.
- [ ] Completar el bloque 2 con la formulación de Kempe y el repaso de enfoques.
- [ ] Completar la entrada `motaz` de `bibliografia.bib` con la venue cuando se publique (ahora es `@unpublished`).
- [X] Pasar los `\cite` de los comentarios TODO al texto conforme se redacte.
- [ ] Cerrar el bloque 4 con la pregunta de investigación una vez definidos los Objetivos.

Reuniones relacionadas: [[Reunion_Cordon_02]], [[Reunion_Cordon_03]].


---
tags:
	#TFM #InfluenceMaximization #Escritura
