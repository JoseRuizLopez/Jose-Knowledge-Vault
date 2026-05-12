links: [[103 - TFM]]


# Christian Blum (2024) — Construct, Merge, Solve & Adapt: Hybrid Combinatorial Optimization
El primer capítulo del libro sienta las bases teóricas y prácticas del algoritmo _Construct, Merge, Solve & Adapt_ (CMSA) y se divide en los siguientes temas principales:

## Introducción a la Optimización
El capítulo inicia explicando el concepto de optimización y cómo modelar matemáticamente los problemas utilizando funciones objetivo y variables de decisión. Diferencia los problemas de optimización continua de los de optimización combinatoria, ilustrando estos últimos con ejemplos clásicos como el Problema del Agente Viajero (TSP) y el Problema de la Mochila. También menciona las técnicas de resolución básicas, dividiéndolas en métodos exactos y aproximados o heurísticos.

## Herramientas de Evaluación
Se describen tres herramientas fundamentales que se utilizan a lo largo del libro para evaluar y comparar los algoritmos:

- **irace:** Una herramienta empleada para la sintonización automática y configuración óptima de los parámetros de los algoritmos.
- **STNWeb:** Una aplicación web basada en Redes de Trayectorias de Búsqueda (STNs) que permite comparar visualmente el comportamiento y progreso de distintos algoritmos iterativos.
- **scmamp:** Un paquete de R para realizar comparaciones estadísticas rigurosas de los algoritmos mediante gráficos de Diferencia Crítica (CD plots).

## El Algoritmo CMSA
Es el concepto central del capítulo y del libro. CMSA es un algoritmo híbrido (metaheurística) para resolver problemas difíciles de optimización combinatoria mediante la aplicación iterada de un enfoque exacto (como un solver ILP) a subinstancias reducidas del problema original. Su funcionamiento se basa en cuatro pasos iterativos:

- **Construct (Construir):** Genera de forma probabilística una serie de soluciones válidas.
- **Merge (Fusionar):** Actualiza la subinstancia agregando los componentes presentes en las soluciones recién construidas.
- **Solve (Resolver):** Aplica un solver exacto a la subinstancia para encontrar la mejor solución posible dentro de un límite de tiempo.
- **Adapt (Adaptar):** Filtra la subinstancia utilizando un valor de "edad", eliminando aquellos componentes que no han aparecido en las mejores soluciones exactas para evitar que el solver se ralentice en futuras iteraciones.

## Aplicación Práctica: Problema del Conjunto Dominante Mínimo (MDS)
Para demostrar la aplicabilidad del método, el capítulo aplica el algoritmo estándar de CMSA al problema MDS en grafos no dirigidos. El autor contrasta dos maneras de definir los componentes de la solución: una forma "intuitiva" basada en los vértices del grafo (CMSA_INT) y una forma "genérica" aplicable a cualquier modelo binario (CMSA_GEN). Seguidamente, presenta una evaluación experimental que compara ambas variantes de CMSA contra un solver estándar (CPLEX) y un algoritmo voraz (_greedy_), utilizando instancias de redes aleatorias (Erdös-Rényi), redes de mundo pequeño (Watts-Strogatz) y redes libres de escala (Barabási-Albert).

## Trabajos Algorítmicos Relacionados
Por último, el capítulo concluye analizando la relación metodológica de CMSA con otros enfoques de la literatura, discutiendo principalmente sus diferencias y similitudes con la Búsqueda de Vecindario Grande (_Large Neighborhood Search_, LNS) y algoritmos como _Merge Search_ y el ruteo de vehículos.



---
tags:
	#TFM #Libro #OptimizacionCombinatoria #TeoriaDeGrafos #EvaluacionAlgoritmica