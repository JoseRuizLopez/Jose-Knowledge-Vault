links: [[103 - TFM]]


# Kempe, Kleinberg & Tardos (2003) — Maximizing the Spread of Influence through a Social Network

## Resumen

Paper fundacional del área de [[Influence Maximization]]. Publicado en KDD 2003 (Knowledge Discovery and Data Mining), con más de 5.700 citas. Ganador de uno de los primeros SIGKDD Test of Time Awards. Establece por primera vez el problema de maximización de influencia como un problema de **optimización discreta**, proporciona garantías teóricas de aproximación para algoritmos eficientes, y valida los resultados con experimentos en redes reales.

**Referencia completa:**
Kempe, D., Kleinberg, J., & Tardos, É. (2003). *Maximizing the spread of influence through a social network*. KDD '03, pp. 137–146. https://doi.org/10.1145/956750.956769

📄 PDF gratuito: https://www.cs.cornell.edu/home/kleinber/kdd03-inf.pdf

---

## Contribuciones principales

1. **Formulación rigurosa** del problema de *influence maximization* como problema de optimización combinatoria.
2. **Demostración de NP-hardness**: no existe algoritmo eficiente que encuentre la solución exacta.
3. **Garantía de aproximación del 63%** para el algoritmo *greedy hill-climbing*, válida para los modelos IC y LT.
4. **Marco teórico basado en *submodularity*** que unifica ambos modelos de difusión.
5. **Validación experimental** en redes de co-autoría de física (10.748 nodos, ~53.000 pares de nodos).

---

## Metodología

### El problema

Dado un grafo dirigido G y un entero k, encontrar el conjunto A de k nodos que maximice σ(A), donde σ(A) es el número esperado de nodos activos al final del proceso de difusión cuando A es el conjunto inicial de nodos activados.

### Modelos de difusión estudiados

#### [[Independent Cascade Model]] (IC) *el que se usará en el TFM*

- El proceso avanza en pasos discretos.
- Cuando un nodo *u* se activa en el paso *t*, tiene **una única oportunidad** de activar a cada vecino inactivo *v*, con probabilidad *p(u,v)*.
- Si falla, no puede volver a intentarlo en rondas posteriores.
- El proceso termina cuando no hay más activaciones posibles.
- Conceptualmente: cada nodo "lanza una moneda" por cada vecino al activarse.

#### [[Linear Threshold Model]] (LT)

- Cada nodo *v* tiene un umbral aleatorio θᵥ ∈ [0,1].
- Cada vecino *w* ejerce una influencia con peso *b(v,w)* sobre *v*, con Σ b(v,w) ≤ 1.
- El nodo *v* se activa cuando la suma de influencias de sus vecinos activos supera su umbral.
- Conceptualmente: un nodo adopta algo cuando "suficiente gente" a su alrededor ya lo ha adoptado.

### El algoritmo *greedy*

Estrategia de *hill-climbing*:
1. Comenzar con el conjunto vacío S = ∅.
2. En cada paso, añadir el nodo *v* que maximiza la ganancia marginal σ(S ∪ {v}) − σ(S).
3. Repetir k veces.

Como σ(A) no se puede calcular de forma exacta eficientemente, se estima mediante **simulaciones Monte Carlo** (el paper usa 10.000 simulaciones por conjunto evaluado).

### La clave teórica: *submodularity*

Una función f es **submodular** si satisface la propiedad de rendimientos decrecientes:

$$f(S \cup \{v\}) - f(S) \geq f(T \cup \{v\}) - f(T) \quad \text{para todo } S \subseteq T$$

Es decir: añadir un nodo a un conjunto pequeño aporta más beneficio que añadirlo a uno más grande.

El resultado central del paper es demostrar que **σ(·) es submodular** para los modelos IC y LT. Una vez probado esto, se aplica el teorema de Nemhauser et al. (1978):

> Para cualquier función no-negativa, monótona y submodular, el algoritmo *greedy* obtiene una solución dentro del factor **(1 − 1/e) ≈ 0,6321** del óptimo.

#### Truco de demostración para IC (*live-edge formulation*)
Se reformula el proceso IC asumiendo que todas las monedas se lanzan al principio del proceso y sus resultados se almacenan. Bajo esta vista equivalente, un nodo *x* termina activo si y solo si existe un camino de "aristas vivas" (*live-edge path*) desde algún nodo del conjunto inicial hasta *x*. Esto convierte el problema en uno de alcanzabilidad en grafos, que es mucho más manejable para el análisis de submodularidad.

---

## Resultados experimentales

Red usada: grafo de co-autorías de física (arXiv), 10.748 nodos.

Algoritmos comparados:
- **Greedy** (el propuesto)
- **High-degree heuristic**: seleccionar nodos en orden decreciente de grado
- **Distance centrality**: seleccionar nodos con menor distancia media al resto
- **Random**: selección aleatoria (baseline)

| Comparación | Ventaja del greedy |
|---|---|
| Greedy vs. High-degree | ~18% más activaciones |
| Greedy vs. Distance centrality | >40% más activaciones |

**Conclusión clave**: las heurísticas basadas en centralidad fallan porque ignoran los efectos de red. En particular, no tienen en cuenta que los nodos más centrales suelen estar agrupados (*clustered*), haciendo que seleccionarlos conjuntamente sea redundante.

---

## Relevancia para el TFM

Este paper es el punto de partida de toda la literatura de *influence maximization*. En el TFM:

- El [[Independent Cascade Model]] definido aquí es el modelo de difusión que se usará.
- El algoritmo *greedy* con garantía del 63% sirve como **método genérico de referencia** (*baseline*).
- La comparación experimental de este paper (greedy vs. heurísticas de centralidad) es el antecedente directo de la comparación que se hará: **métodos genéricos vs. métodos basados en [[Community Detection]]**.
- La hipótesis del TFM es que explotar la estructura de comunidades puede dar resultados competitivos con menor coste computacional.

---

## Limitaciones del paper

- El algoritmo *greedy* es **lento en la práctica**: requiere estimar σ(A) miles de veces. Esto motivó trabajo posterior como [[CELF]] y [[CELF++]] para optimizar las evaluaciones.
- Los experimentos se limitan a redes de co-autoría; la generalización a otros tipos de redes no está garantizada.
- Las probabilidades de activación se asignan de forma uniforme o simplificada, no inferidas de datos reales.

---

## Conceptos clave que aparecen

- [[Submodular Function]]
- [[Greedy Hill-Climbing]]
- [[NP-Hard]]
- [[Monte Carlo Simulation]]
- [[Independent Cascade Model]]
- [[Linear Threshold Model]]
- [[Degree Centrality]]
- [[Community Detection]]
- [[Viral Marketing]]

---

## Ver también

- [[CELF|CELF — Cost-Effective Lazy Forward]] — optimización del greedy (Leskovec et al., 2007)
- [[Influence Maximization with Community Detection]] — extensión relevante para el TFM
- [[Independent Cascade Model]] — definición detallada del modelo


---
tags:
	#NetworkScience #InfluenceMaximization #SocialNetworks #TFM #PaperFundacional
