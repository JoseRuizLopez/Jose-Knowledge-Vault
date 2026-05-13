links: [[InvestigacionOperativa/Metaheuristicas/MPIDS]]


# TFG: Metaheurísticas para el Problema MPIDS

Trabajo de Fin de Grado centrado en la aplicación de [[Metaheuristicas|metaheurísticas]] para resolver el problema de *Minimum Positive Influence Dominating Set* (MPIDS) en redes sociales. Se trata de un problema [[NP-Hard]] de [[OptimizacionCombinatoria|optimización combinatoria]] cuyo objetivo es identificar el conjunto mínimo de vértices de un grafo con propiedades de influencia positiva sobre el resto de la red.

## Definición del problema

Dado un grafo no dirigido que modela una red social, el problema MPIDS busca un conjunto de vértices $S$ de mínima cardinalidad tal que cada vértice $v_i$ fuera de $S$ tenga al menos la mitad de sus vecinos en $S$. Formalmente:

$$\min \sum_i x_i \quad \text{s.t.} \quad \sum_{j \in N(v_i)} x_j \geq \left\lceil \frac{\deg(v_i)}{2} \right\rceil, \quad \forall v_i$$

donde $x_i \in \{0, 1\}$ indica si el vértice $i$ pertenece a la solución.

## Metodología

### Gestión del proyecto

El desarrollo siguió la metodología [[SCRUM]] con 10 sprints semanales. El stack de herramientas incluyó Java 21, Maven, IntelliJ IDEA, Python (scripts auxiliares y bot de Telegram), Git/GitHub y VisualVM para profiling.

### Instancias utilizadas

Las instancias proceden del trabajo de Sun R. et al., obtenidas de *Network Repository*, *Stanford Network Analysis Project* (SNAP) y GitHub. Se normalizaron a un formato unificado donde la primera línea contiene $N$ y $M$ (nodos y aristas), seguida de $M$ pares de vértices por arista.

### Algoritmos implementados

**Aleatorio (RandomF1 / RandomF2)**
Parte del conjunto completo de nodos y elimina aleatoriamente aquellos que mantengan la factibilidad. RandomF2 incorpora una comprobación de factibilidad más eficiente, lo que se traduce en tiempos menores y resultados ligeramente superiores.

**Voraz (*Greedy*)**
Construye la solución seleccionando nodos por criterio de prioridad basado en grado. Más rápido que el aleatorio (0.19 s de media frente a 59.29 s) y con mejor valor promedio y menor desviación.

**Búsqueda Local (LS)**
Mejora la solución del algoritmo voraz mediante intercambios iterativos nodo a nodo (movimiento 2-a-1), reduciendo el valor de la función objetivo. Con un límite de 300 s, `Voraz + LS` alcanza desviación 0.00% en todas las instancias evaluadas.

**[[BVNS]] (*Basic Variable Neighborhood Search*)**
[[Metaheuristicas|Metaheurística]] que escapa de óptimos locales cambiando sistemáticamente la estructura de los entornos mediante un parámetro $k$. El ciclo consiste en perturbar la solución con el entorno $k$ y aplicar [[BusquedaLocal|búsqueda local]], incrementando $k$ si no hay mejora y reiniciando si la hay.

## Resultados

| Algoritmo | Mejor sol. (instancias) | Desviación media | Tiempo medio |
|---|---|---|---|
| RandomF2 | Superior a RandomF1 | - | ~59.29 s |
| Voraz | Inferior al aleatorio en mejores | Menor desviación | ~0.19 s |
| Voraz + LS (300 s) | 100% instancias | 0.00% | 300 s |
| BVNS k=10 | 39 mejores (preliminar) | Mayor desviación | - |
| BVNS k=5 | 4 empates con SOTA | Próxima al SOTA | - |

El algoritmo del estado del arte (Sun R. et al.) obtuvo la mejor solución en las 195 instancias del experimento final. BVNS con $k=5$ empató en 4 ocasiones, con un valor promedio cercano al SOTA, lo que sugiere potencial con ajustes adicionales.

## Conclusiones

El sistema desarrollado es capaz de importar instancias, generar soluciones factibles y exportar resultados, con arquitectura modular que facilita añadir nuevos algoritmos o exportadores. Se cumplieron todos los objetivos algorítmicos propuestos y el trabajo permitió introducirse en la investigación académica aplicando el método científico.

## Trabajo futuro

- Paralelización de la función objetivo en BVNS para acelerar comprobaciones de factibilidad.
- Análisis de rendimiento con VisualVM sobre nuevas implementaciones.
- Estudio de movimientos más eficientes en la búsqueda local.
- Reducción inteligente de vecindarios para optimizar el tiempo de ejecución.


---
tags:
	#Metaheuristicas #OptimizacionCombinatoria #GrafosYRedes #TFG #BVNS
