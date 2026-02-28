links: [[MineriaMediosSociales/DifusionDeInformacion/IndependentCascadeModel]]


# Independent Cascade Model

El *Independent Cascade Model* (ICM) es un modelo probabilístico de [[DifusionDeInformacion]] sobre redes que simula cómo una información, comportamiento o influencia se propaga a través de los nodos de un grafo. Forma parte de los modelos clásicos de [[InfluenceMaximization]] y es ampliamente estudiado en el contexto de [[SocialNetworkAnalysis]].

## Definición formal

Dado un grafo dirigido $G = (V, E)$ donde cada arista $(u, v) \in E$ tiene asociada una probabilidad de propagación $p_{u,v} \in [0, 1]$, el proceso de difusión se define como sigue:

- Un conjunto inicial de nodos $S \subseteq V$ se activa en el tiempo $t = 0$ (la *seed set*).
- En cada paso de tiempo $t$, cada nodo $u$ recién activado en $t-1$ tiene **exactamente un intento** de activar a cada vecino inactivo $v$, con probabilidad $p_{u,v}$.
- Si el intento tiene éxito, $v$ se activa en el paso $t$.
- Un nodo que fracasa en activar a $v$ no vuelve a intentarlo.
- El proceso termina cuando no hay más nodos que puedan activarse.

La propagación es **independiente** porque cada arista actúa de forma autónoma, sin tener en cuenta los demás intentos recibidos por un mismo nodo.

## Funcionamiento paso a paso

1. Seleccionar la *seed set* $S$ (nodos inicialmente activos).
2. Para cada nodo activo $u$ en el tiempo $t$:
   - Para cada vecino inactivo $v$ de $u$:
     - Generar un número aleatorio $r \in [0, 1]$.
     - Si $r \leq p_{u,v}$, activar $v$ en $t+1$.
3. Marcar a $u$ como "ya intentó propagar" (no vuelve a actuar).
4. Repetir hasta que no haya activaciones nuevas.
5. El resultado es el conjunto de todos los nodos activados $\sigma(S)$.

## Relacion con Influence Maximization

El problema de [[InfluenceMaximization]] consiste en encontrar la *seed set* $S$ de tamaño $k$ que maximiza la **esperanza** del número de nodos activados $\mathbb{E}[\sigma(S)]$:

$$S^* = \arg\max_{S \subseteq V,\, |S|=k} \mathbb{E}[\sigma(S)]$$

Kempe, Kleinberg y Tardos (2003) demostraron que bajo el ICM:
- La función $\sigma(S)$ es [[Submodular]] y monótona.
- El problema es NP-hard.
- El algoritmo *greedy* con $k$ iteraciones garantiza una aproximación de $(1 - 1/e) \approx 63\%$ del óptimo.

## Estimacion de la difusion esperada

Como $\mathbb{E}[\sigma(S)]$ no tiene forma cerrada en general, se estima mediante [[MonteCarloCascade]]:

1. Repetir $R$ simulaciones independientes del proceso de cascada.
2. Promediar el número de nodos activados sobre las $R$ ejecuciones.
3. Usar el valor estimado como función objetivo del algoritmo greedy.

El coste computacional de esta estimación es $O(R \cdot (|V| + |E|))$ por evaluación.

## Variantes y modelos relacionados

| Modelo | Diferencia principal |
|--------|----------------------|
| [[LinearThresholdModel]] (LTM) | Un nodo se activa cuando la influencia acumulada supera un umbral interno |
| *Weighted Cascade* | $p_{u,v} = 1 / \deg_{in}(v)$, pesos derivados del grado |
| *Triggering Model* | Generalización que engloba ICM y LTM |
| *Time-aware Cascade* | Incorpora retardos temporales en la propagación |

## Ventajas y limitaciones

| Ventajas | Limitaciones |
|----------|--------------|
| Interpretable y matematicamente bien fundado | Estimar $\mathbb{E}[\sigma(S)]$ es costoso (#MonteCarlo) |
| Garantias de aproximacion con greedy | Asumir independencia puede ser poco realista |
| Facil de simular | Escala mal en grafos muy grandes sin tecnicas de pruning |
| Extensible a variantes con correlacion o temporalidad | Probabilidades $p_{u,v}$ dificiles de aprender en la practica |

## Casos de uso

- **Marketing viral**: identificar los $k$ usuarios mas influyentes para lanzar una campana.
- **Epidemiologia computacional**: modelar la propagacion de enfermedades en redes de contacto.
- **Deteccion de rumores**: estudiar como la desinformacion se expande en redes sociales.
- **Analisis de redes**: evaluar la robustez de una red ante perturbaciones.

## Complejidad computacional

- Calcular $\sigma(S)$ de forma exacta es #P-hard.
- La aproximacion greedy con estimacion Monte Carlo tiene complejidad $O(k \cdot R \cdot |V| \cdot (|V| + |E|))$.
- Algoritmos modernos como *CELF*, *CELF++* y *IMM* reducen drasticamente el numero de simulaciones necesarias mediante tecnicas de *lazy evaluation* y *reverse reachable sets*.


---
tags:
	#MineriaMediosSociales #SocialNetworkAnalysis #InfluenceMaximization #DifusionDeInformacion
