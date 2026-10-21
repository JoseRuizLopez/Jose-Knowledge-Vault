links: [[103 - TFM]]


# Lancichinetti, Fortunato & Radicchi (2008): Benchmark graphs for testing community detection algorithms

## Resumen

Artículo que presenta el *benchmark* LFR (por las iniciales de los autores), una familia de grafos sintéticos con estructura de comunidades conocida, pensada para evaluar algoritmos de [[Community Detection]]. Su aportación frente al *benchmark* de Girvan y Newman (GN) es que reproduce la heterogeneidad de las redes reales: tanto la distribución de grados como la de tamaños de comunidad siguen leyes de potencia.

**Referencia completa:**
Lancichinetti, A., Fortunato, S., & Radicchi, F. (2008). *Benchmark graphs for testing community detection algorithms*. Physical Review E, 78(4), 046110. https://doi.org/10.1103/PhysRevE.78.046110

Preprint: arXiv:0805.4770

Clave BibTeX en la memoria: `lancichinetti2008benchmark`

---

## Problema que aborda

Para medir la calidad de un algoritmo de detección de comunidades hace falta un grafo cuya partición real se conozca. El *benchmark* GN, el más usado hasta entonces, tiene tres limitaciones según los autores:

- Todos los nodos tienen aproximadamente el mismo grado (16).
- Todas las comunidades tienen el mismo tamaño (4 grupos de 32 nodos).
- La red es pequeña (128 nodos), mientras que ya existían métodos capaces de analizar grafos con millones de nodos.

Las redes reales tienen distribuciones de grado con colas largas (leyes de potencia) y comunidades de tamaños muy distintos, así que GN no es un buen sustituto de una red real.

---

## El benchmark LFR

### Parámetros

| Parámetro | Significado |
|---|---|
| N | Número de nodos |
| ⟨k⟩, k_max | Grado medio y grado máximo |
| γ | Exponente de la ley de potencia de los grados |
| β | Exponente de la ley de potencia de los tamaños de comunidad |
| μ | *Mixing parameter*: fracción de aristas de cada nodo que van fuera de su comunidad |
| s_min, s_max | Tamaño mínimo y máximo de comunidad |

Los autores usan valores típicos de redes reales: 2 ≤ γ ≤ 3 y 1 ≤ β ≤ 2.

### Construcción

1. Se asigna a cada nodo un grado extraído de una ley de potencia con exponente γ, y los nodos se conectan con el *configuration model* para respetar esa secuencia de grados.
2. Cada nodo comparte una fracción 1 − μ de sus aristas con su comunidad y una fracción μ con el resto de la red.
3. Los tamaños de comunidad se extraen de una ley de potencia con exponente β, de forma que sumen N. Se impone s_min > k_min y s_max > k_max para que cualquier nodo quepa en alguna comunidad.
4. Los nodos se asignan a comunidades de forma aleatoria. Si una comunidad está llena, se expulsa a un nodo al azar, que queda pendiente de asignar. El proceso termina cuando todos los nodos tienen comunidad.
5. Se recablea la red manteniendo los grados para que la proporción de aristas internas y externas de cada nodo se aproxime a μ.

### Coste

El tiempo de generación crece de forma lineal con el número de aristas, lo que permite construir redes de 10⁵ a 10⁶ nodos en un tiempo razonable. Como μ no se puede fijar con exactitud en nodos de grado bajo, la distribución real de μ en el grafo generado tiene forma de campana alrededor del valor pedido.

### Interpretación de μ

Con μ < 0,5 cada nodo tiene más vecinos dentro de su comunidad que fuera, y las comunidades están bien definidas en sentido fuerte. A partir de μ = 0,5 esa definición deja de cumplirse.

---

## Resultados experimentales

Se prueban dos métodos sobre el *benchmark*, midiendo la calidad con la *normalized mutual information* (NMI) entre la partición real y la encontrada:

- **[[Modularity Optimization]]** (con *simulated annealing*): en GN recupera la partición casi perfectamente hasta k_out = 6. En LFR empeora con μ, empeora cuando las comunidades tienen tamaños parecidos (β alto) y mejora con ⟨k⟩. Al pasar de N = 1000 a N = 5000 los resultados bajan. Los autores atribuyen parte del fallo al límite de resolución de la modularidad, que funde comunidades pequeñas en otras mayores.
- **Potts model clustering** (Reichardt y Bornholdt): rendimiento aceptable, peor que la optimización de modularidad y también decreciente con el tamaño de la red.

Conclusión: LFR es una prueba más exigente que GN y deja ver limitaciones que no aparecen con el *benchmark* clásico, en particular la dependencia del tamaño y de la densidad de la red.

---

## Relevancia para el TFM

- Es la referencia de las redes sintéticas LFR usadas en el estudio de escalabilidad de DCIIM ([[motaz_2026_reformulating_influence_maximization]]). El propio Ben Hassine usa redes LFR para estudiar la sensibilidad de φ.
- Permite generar redes de tamaño creciente (1.000 a 50.000 nodos en el bloque A del estudio) con estructura comunitaria controlada, que es lo que hace falta para medir cómo escala un método.
- El parámetro μ es el que se fijó a 0,3 en el bloque A y se varió en el bloque B. En [[Reunion_Cordon_03]] se decidió que μ no tiene interés propio para el TFM porque solo controla la generación de las redes.
- Se cita en la memoria al describir el estudio de escalabilidad y el conjunto de datos.

---

## Conceptos clave

- [[Community Detection]]
- [[Configuration Model]]
- [[Power Law]]
- [[Modularity Optimization]]
- [[Normalized Mutual Information]]
- [[Datasets de Redes]]


---
tags:
	#TFM #CommunityDetection #NetworkScience #Benchmark #Articulo
