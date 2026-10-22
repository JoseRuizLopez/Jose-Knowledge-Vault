links: [[103 - TFM]]


# Blum (2024): Construct, Merge, Solve & Adapt. A Hybrid Metaheuristic for Combinatorial Optimization

## Resumen

Monografía de Christian Blum (IIIA-CSIC, Bellaterra) que recoge el estado de la investigación sobre CMSA, un algoritmo híbrido para problemas de optimización combinatoria difíciles. La idea central es aplicar de forma iterada un método exacto, normalmente un *ILP solver*, a subinstancias reducidas del problema original. Las subinstancias se construyen con los componentes de soluciones generadas de forma probabilística. El libro tiene 6 capítulos, un apéndice con el código C++ de CMSA para el problema MDS y 202 páginas.

El hilo del libro es este. El capítulo 1 presenta CMSA estándar y lo prueba con el *Minimum Dominating Set*. Los capítulos 2 y 3 proponen dos variantes que corrigen sus puntos débiles: ADAPT_CMSA, que reduce la sensibilidad a los parámetros, y LEARN_CMSA, que añade aprendizaje a la construcción de soluciones. Los capítulos 4 y 5 tratan dos cuestiones prácticas de aplicación: usar formulaciones de *set covering* en lugar de modelos de asignación, y qué hacer cuando el modelo ILP tiene variables no binarias. El capítulo 6 reúne líneas abiertas: un CMSA independiente del problema, el uso de una metaheurística como *solver* de subinstancias, la comparación con LNS y el trabajo futuro.

**Referencia completa:**
Blum, C. (2024). *Construct, Merge, Solve & Adapt: A Hybrid Metaheuristic for Combinatorial Optimization*. Computational Intelligence Methods and Applications. Springer, Cham. https://doi.org/10.1007/978-3-031-60103-3

ISBN 978-3-031-60102-6 (impreso), 978-3-031-60103-3 (eBook)

Clave BibTeX en la memoria: `blum2024cmsa`

PDF en `Sources/CMSA - Libro.pdf`. El capítulo 1 tiene su propia nota: [[CMSA-Capitulo1]].

---

## Origen de CMSA

El trabajo empezó en 2015 en la Universidad del País Vasco, cuando Blum era Ikerbasque Research Fellow, dentro de la tesis doctoral de Pedro Pinacho Davidson. Blum observó que la [[Large Neighborhood Search]] (LNS) basada en destruir parcialmente la solución actual se estanca en óptimos locales cuando las soluciones tienen pocos componentes, como en instancias muy restringidas del *multi-dimensional knapsack problem*. CMSA nació como alternativa para esos casos. El artículo original apareció en 2016 en *Computers and Operations Research* (Blum, Pinacho, López-Ibáñez y Lozano), donde se aplicó al *minimum covering arborescence* y al *minimum common string partition*. Según el prefacio ese artículo tenía 106 citas en febrero de 2024 y CMSA se había aplicado a 20 problemas distintos. Dos premios respaldan la línea: el mejor artículo del track ECOM de GECCO 2016 (aplicación al *multi-dimensional knapsack problem*) y el premio SEIO-Fundación BBVA 2021 a la mejor contribución metodológica en Investigación Operativa.

Entre los problemas a los que se ha aplicado CMSA (tabla 1.1 del libro) están el *minimum common string partition*, el *repetition-free longest common subsequence*, la maximización del valor presente neto en planificación de proyectos, el *minimum capacitated dominating set*, rutas para robots aire-tierra con restricciones de combustible, la planificación de recarga y mantenimiento de centrales nucleares, la generación de datos de test en líneas de producto software, el MPIDS, la partición numérica multidimensional, el *maximum disjoint dominating sets* y la planificación de conductores de autobús con descansos.

---

## Capítulo 1: CMSA estándar

### Contexto de optimización

El capítulo empieza con una introducción a la optimización (ejemplos continuos y combinatorios como el TSP y la mochila, modelado de un problema, métodos exactos frente a aproximados). Blum aclara que no espera del lector que sepa diseñar técnicas exactas: esa parte se delega por completo en *solvers* ILP de caja negra, concretamente CPLEX 22.1 en todos los experimentos del libro. Todo el código está en C++. En la sección de técnicas híbridas (matheurísticas) sitúa a LNS, a *very large-scale neighborhood search*, a las cadenas de eyección, a *local branching*, al método del corredor y a POPMUSIC como antecedentes de CMSA.

### Requisitos

Para aplicar CMSA a un problema hacen falta dos cosas:

1. Una forma probabilística de generar soluciones válidas.
2. Un modelo ILP del problema.

Además hay que definir el conjunto C de componentes de solución, de modo que toda solución válida S sea un subconjunto de C.

### Pseudocódigo

```
S_bsf := ∅ ; C' := ∅ ; age[c] := 0 para todo c ∈ C
mientras no se alcance el límite de tiempo:
    para i = 1..n_a:
        S := ProbabilisticSolutionConstruction(C)
        si f(S) < f(S_bsf): S_bsf := S
        para cada c ∈ S que no esté en C': age[c] := 0 ; C' := C' ∪ {c}
    S_ILP := SolveSubinstance(C', t_ILP)
    si f(S_ILP) < f(S_bsf): S_bsf := S_ILP
    Adapt(C', S_ILP, age_max)
devolver S_bsf
```

### Los cuatro pasos

En el paso *construct* se generan n_a soluciones válidas de forma probabilística, normalmente con una heurística voraz aleatorizada. En *merge* los componentes de esas soluciones que no estaban en la subinstancia C' se añaden con edad 0. En *solve* un *ILP solver* busca la mejor solución que solo usa componentes de C', con un límite de t_ILP segundos. En *adapt* la edad de los componentes de C' que no están en S_ILP aumenta en 1, la de los que sí están vuelve a 0, y se eliminan los que alcanzan age_max. La edad de un componente es, por tanto, el número de iteraciones seguidas que ha estado en C' sin aparecer en la solución del ILP. Así la subinstancia no crece hasta ralentizar al *solver*.

*Construct* y *solve* dependen del problema. *Merge* y *adapt* son genéricos. El modelo ILP de la subinstancia se obtiene del modelo completo restringiéndolo a los componentes de C', y hay margen de diseño en cómo se hace esa restricción. Los parámetros del algoritmo son n_a, age_max y t_ILP.

### Aplicación al Minimum Dominating Set (MDS)

El MDS pide el menor subconjunto D de vértices tal que todo vértice fuera de D tenga al menos un vecino en D. Se comparan dos definiciones de los componentes de solución. CMSA_INT usa un componente por vértice, y la construcción voraz aleatorizada elige en cada paso entre los vértices que cubren algún nodo aún descubierto, con una tasa de determinismo d_rate (elige el mejor) y, si no, una lista de candidatos restringida de tamaño l_size. CMSA_GEN es genérica y válida para cualquier modelo binario: dos componentes por variable, c_i^0 para x_i = 0 y c_i^1 para x_i = 1. En CMSA_GEN la subinstancia se resuelve fijando x_i = 0 si C' solo contiene c_i^0, x_i = 1 si solo contiene c_i^1, y dejando x_i libre si contiene ambos.

Los parámetros que se ajustaron con irace fueron n_a, age_max, d_rate, l_size, t_ILP y tres opciones de CPLEX (énfasis heurístico, *warm start* y abortar CPLEX cuando mejora la mejor solución). Ambas variantes se ajustaron una sola vez para todo el conjunto de pruebas, con 24 instancias de ajuste y un presupuesto de 3000 ejecuciones de irace.

### Experimentos con MDS

El banco de pruebas tiene 30 grafos por cada combinación de |V| en {500, 1000, 1500, 2000} y cuatro densidades, con los tres modelos de red (Erdős-Rényi, Watts-Strogatz y Barabási-Albert) generados con igraph, es decir 480 grafos por modelo. Los límites de tiempo fueron 150, 300, 450 y 600 segundos CPU según el tamaño. Se compararon GREEDY, CPLEX, CMSA_INT y CMSA_GEN.

Las conclusiones del autor son cuatro. Las dos variantes de CMSA superan en general a GREEDY y a CPLEX. Con grafos pequeños o poco densos CPLEX rinde igual que CMSA, y a medida que el grafo crece o se densifica CMSA se separa de CPLEX, que incluso queda por debajo del voraz en los grafos Erdős-Rényi y Watts-Strogatz más grandes y densos. Entre las dos variantes de CMSA las diferencias son pequeñas: CMSA_GEN gana en Erdős-Rényi y Watts-Strogatz y CMSA_INT en Barabási-Albert, con significación estadística en Watts-Strogatz (a favor de GEN) y en Barabási-Albert (a favor de INT). Por último, el MDS resulta bastante más fácil en Barabási-Albert (redes libres de escala) que en los otros dos modelos. El ajuste de parámetros dio valores bajos de n_a y age_max, lo que indica que las subinstancias no deben ser grandes, y un t_ILP alto. CMSA_GEN prefirió menos determinismo que CMSA_INT y usó el aborto de CPLEX, que CMSA_INT no usó.

Con STNWeb se ve que las trayectorias de CMSA_GEN son más largas que las de CMSA_INT. La razón es que CMSA_GEN genera una sola solución nueva por iteración, da pasos más pequeños y empieza desde soluciones peores porque aborta CPLEX en cuanto hay mejora.

### Herramientas de evaluación

irace hace ajuste automático de parámetros por carreras iteradas, en paralelo y sin depender del algoritmo. STNWeb construye *Search Trajectory Networks* para comparar visualmente la trayectoria de varios algoritmos, con la opción de agrupar soluciones parecidas (partición del espacio de búsqueda). scmamp es un paquete de R para comparaciones estadísticas de varios algoritmos sobre varios problemas, que produce los gráficos de diferencia crítica (CD plots) usados en todo el libro.

### Trabajos relacionados

Blum distingue CMSA de varias familias cercanas. En LNS no hay una solución parcial impuesta: CMSA reduce las opciones para construir soluciones factibles y deja al método exacto buscar el óptimo dentro de ese conjunto reducido. Applegate et al. y Cook y Seymour fusionaban tours de TSP generados por una metaheurística y resolvían el problema reducido de forma exacta. Cavaliere et al. hacen algo parecido para el CVRP con un conjunto de rutas y *set partitioning*. El marco *generate-and-solve* tiene dos fases (generar candidatas y resolver el problema reducido), y la *kernel search* identifica un núcleo de componentes prometedores. La fusión de soluciones en algoritmos evolutivos es otro antecedente. La diferencia con *merge search* (Kenny et al.) está en cómo se genera la subinstancia: CMSA fija las variables que toman valor fijo en soluciones buenas y optimiza el resto, mientras que *merge search* agrupa variables que toman valores idénticos entre sí en soluciones buenas y deja libre el valor del grupo.

---

## Capítulo 2: Self-adaptive CMSA (ADAPT_CMSA)

### Motivación

Una metaheurística es sensible a los parámetros cuando su rendimiento depende mucho de valores concretos y esos valores cambian entre grupos de instancias. CMSA estándar mostró esa sensibilidad en su primera aplicación al *Minimum Positive Influence Dominating Set* (MPIDS). ADAPT_CMSA (Akbay, López Serrano y Blum, 2022) se diseñó para reducirla.

### Cómo funciona

Se eliminan las edades. La edad máxima queda fijada en 1, de modo que tras cada iteración la subinstancia vuelve a contener solo la mejor solución encontrada. El tamaño de la subinstancia se regula con dos mecanismos:

- La construcción de soluciones recibe la mejor solución S_bsf y un parámetro α_bsf en [0, 1) que sesga la construcción hacia ella: cuanto mayor es α_bsf, más se parecen las soluciones nuevas a S_bsf. Empieza en α_UB. Si el ILP se resuelve en menos de t_prop · t_ILP, la subinstancia es pequeña y α_bsf baja en α_red (hasta α_LB) para generar soluciones más diversas y dar más variables libres al ILP.
- El número de construcciones por iteración n_a empieza en 1. Si el ILP mejora S_bsf, n_a vuelve a 1. Si el ILP devuelve algo peor, la subinstancia fue demasiado grande para resolverse en t_ILP: con n_a = 1 se sube ligeramente α_bsf, y con n_a mayor que 1 se reinicia n_a. Si el ILP iguala a S_bsf se resolvió bien pero sin mejorar, así que n_a aumenta en 1 para ampliar la subinstancia.

La solución inicial es una solución voraz. Los parámetros de entrada son t_prop, t_ILP, α_LB, α_UB y α_red. No se usa el aborto de CPLEX, porque el tiempo de resolución es una señal que necesita el mecanismo adaptativo.

### MPIDS

El MPIDS es un problema sobre redes sociales: encontrar el menor conjunto de nodos tal que cada vértice tenga al menos la mitad de sus vecinos en el conjunto. La construcción parte de un conjunto de nodos que se sabe que están en toda solución óptima (preprocesado) y añade vecinos del nodo descubierto de menor grado. En CMSA_GEN la elección usa el grado de cobertura con determinismo d_rate y lista restringida, y en ADAPT_CMSA pondera el grado de cobertura por α_bsf o (1 − α_bsf) según el vecino esté o no en S_bsf.

El banco de pruebas son 450 grafos Barabási-Albert, con |V| en {1000, 5000, 10000, 50000, 100000}, tres densidades y 30 grafos por combinación, hasta 100000 vértices y 2 millones de aristas.

Resultados: CPLEX solo compite en los grafos pequeños y más densos, empieza a fallar con 10000 vértices y en los grafos de 50000 y 100000 solo devuelve la solución trivial con todos los vértices. CMSA_GEN funciona hasta 10000 vértices, pero con 50000 y 100000 apenas mejora al voraz, porque sus subinstancias dejan de ser resolubles por CPLEX. ADAPT_CMSA funciona en todo el rango y supera al resto con significación estadística. En un segundo experimento con ajuste especializado por tamaño de grafo, el rendimiento de CMSA_GEN mejora de forma significativa hasta 50000 vértices, mientras que el de ADAPT_CMSA no mejora de forma significativa. Con 100000 vértices CMSA_GEN sigue sin producir subinstancias resolubles. La consecuencia práctica es que ADAPT_CMSA necesita un único ajuste de parámetros para todo el conjunto de instancias.

### Far From Most String (FFMS)

El FFMS es un problema de consenso de secuencias, útil por ejemplo para diseñar sondas diagnósticas. Dado un conjunto de n cadenas de longitud m y un umbral t, se busca una cadena que difiera de la mayoría de las cadenas de entrada según la distancia de Hamming y el umbral. El valor de la función objetivo original solo puede tomar valores entre 0 y n, lo que crea mesetas anchas donde la búsqueda no recibe guía. Por eso se usa una función objetivo aumentada y lexicográfica: primero el objetivo original y, como criterio de desempate, una función h(s) que suma las distancias de Hamming a las cadenas que están a distancia al menos t y añade la mayor distancia entre las que están por debajo de t. Los componentes de solución son pares (posición, letra) y la construcción usa la frecuencia de cada letra en cada posición de las cadenas de entrada.

### Conclusiones del capítulo

Según el autor, la ventaja de ADAPT_CMSA es mayor en instancias a gran escala, donde CMSA estándar tiene problemas para ajustar los parámetros de modo que las subinstancias sigan siendo resolubles por CPLEX en tiempo razonable.

---

## Capítulo 3: Añadir aprendizaje a CMSA (LEARN_CMSA)

### Motivación

En CMSA estándar la distribución de construcción es la misma en todas las iteraciones, así que la calidad media de las soluciones construidas se mantiene estacionaria. LEARN_CMSA intenta que las soluciones construidas mejoren con el tiempo.

### El algoritmo bacteriano (BA)

El BA es un algoritmo evolutivo en el que el operador de cruce se inspira en la resistencia bacteriana a los antibióticos (versión simplificada de Pinninghoff, Orellana y Contreras). La población se divide, según un nivel separador, en soluciones donantes y receptoras, y los operadores son la conjugación (receptoras que toman material de donantes) y la regeneración. Los parámetros del BA son el tamaño de población p_size, la proporción de soluciones iniciales heurísticas pr_heur, el determinismo d_rate y las probabilidades de conjugación pr_con y de regeneración pr_reg.

Blum insiste en que el BA es solo un ejemplo ilustrativo y que casi cualquier metaheurística poblacional serviría. Atribuye el éxito de LEARN_CMSA a cómo se entrelaza con CMSA y no a rasgos concretos del BA.

### Cómo se integra con CMSA

Al principio de cada iteración de CMSA se ejecutan b_iter iteraciones del BA. En lugar de construir soluciones con una heurística voraz aleatorizada, se extraen n_a soluciones de la población: la mejor más n_a − 1 donantes elegidos al azar, que se fusionan en C'. Al final de la iteración, la mejor solución del ILP se inyecta en la población del BA sustituyendo floor(r_inject · |P_receptoras|) soluciones receptoras. Las dos memorias, la subinstancia C' del CMSA y la población P del BA, se influyen mutuamente.

### Resultados

En MDS, el BA solo es claramente peor que ambas variantes de CMSA, sobre todo en grafos poco densos, aunque mejora al crecer la densidad y supera a CPLEX en los más densos. Aun así su mecanismo de aprendizaje aporta a LEARN_CMSA: en Erdős-Rényi es solo algo mejor que CMSA_INT, y en Watts-Strogatz lo supera con significación estadística. En FFMS, con umbrales t = 0.8m y t = 0.85m, LEARN_CMSA supera a todos los competidores (CMSA_INT, ADAPT_CMSA y BA) con significación global. La ventaja es mayor con t = 0.8m. Con t = 0.85m y cadenas cortas (m = 100), CMSA_INT y ADAPT_CMSA rinden mejor que LEARN_CMSA.

### Limitaciones

El autor señala que el enfoque depende de muchos parámetros y que CMSA y el BA conservan cada uno su identidad, de modo que el nivel de integración es bajo (solo se intercambian sus mejores soluciones). Sugiere que una componente de *machine learning* mejor integrada podría desplazar la construcción hacia zonas mejores del espacio de búsqueda.

---

## Capítulo 4: Sustituir modelos difíciles por formulaciones de set covering

### Idea

Muchos problemas de empaquetado, rutas y mochila se pueden formular como un ILP de tipo asignación o como un ILP basado en *set covering*. Los *solvers* de caja negra suelen tener muchas dificultades con el primero, y la comunidad de Investigación Operativa ha desarrollado técnicas basadas en el segundo, sobre todo generación de columnas. El capítulo muestra que el modelo de *set covering* también funciona dentro de CMSA y que resulta más sencillo de implementar que la generación de columnas. En ambos problemas estudiados las mejores variantes de CMSA obtienen resultados del estado del arte.

### Variable-Sized Bin Packing (VSBP)

Hay n ítems con peso y m tipos de contenedor con capacidad y coste, y se busca empaquetar todo al menor coste. En el modelo de *set covering* cada componente de solución es un contenedor válido (un conjunto de ítems), y su coste es el del tipo más barato que lo aloja. La restricción de cobertura se escribe con ≥ en lugar de =, lo que da una relajación LP numéricamente más estable. Tras resolver, se eliminan al azar los ítems duplicados hasta que cada ítem aparezca en un único contenedor. CMSA_SETCOV usa ese modelo para las subinstancias y CMSA_GEN usa el modelo de asignación.

Resultados sobre 150 instancias (clases B1, B2 y B3, con 50 cada una): CMSA_SETCOV supera claramente a CMSA_GEN, que solo iguala su calidad en 2 de 150 casos. Encuentra nuevas mejores soluciones conocidas en 68 de 150 instancias (27 en B1, 26 en B2 y 15 en B3) y solo queda ligeramente por debajo del mejor valor conocido en 7. En los CD plots, que incluyen un VNS y un algoritmo genético de la literatura, CMSA_SETCOV es mejor con significación global y no hay diferencia significativa con el VNS en la clase B3.

### Electric Vehicle Routing Problem con ventanas de tiempo y recogida y entrega simultáneas (EVRP-TW-SPD)

Es una variante de rutas de vehículos eléctricos que añade ventanas de tiempo, recogidas y entregas simultáneas (logística inversa) y recarga parcial en las estaciones. El modelo de asignación da cotas inferiores débiles y CPLEX tiene dificultades hasta para encontrar soluciones factibles de subinstancias pequeñas. En el modelo de *set covering* los componentes son tours completos. ADAPT_CMSA_SETCOV usa la misma construcción que ADAPT_CMSA, pero con tours como componentes, y elimina los clientes duplicados quitando primero los que más ahorro aportan.

En 36 instancias pequeñas CPLEX resolvió 31 hasta el óptimo y ambas variantes de CMSA alcanzaron esos óptimos. En las cinco restantes, ADAPT_CMSA y ADAPT_CMSA_SETCOV mejoraron a CPLEX en un 0.15% (r202C15) y un 1.51% (rc204C15). En las instancias grandes, ADAPT_CMSA_SETCOV supera a ADAPT_CMSA con significación global y en las instancias aleatorias y aleatorias agrupadas, sin diferencia significativa en las agrupadas, y pierde ventaja en instancias con horizonte largo (c2, r2, rc2), que tienen menos rutas con más clientes cada una. Ambas variantes superan con claridad a las heurísticas multiarranque probabilísticas (pC&W y pSI).

### Conclusiones y trabajo futuro

Las variantes con modelos de *set covering* superan a las que usan modelos de asignación. Blum las considera un marco ideal para problemas de este tipo por ser menos complejas que la generación de columnas y por poder explorar el espacio de búsqueda mejor que las heurísticas simples que aprovechan estos modelos. Propone consolidar los hallazgos en otros problemas modelables con *set covering* y añadir más heurísticas de construcción en VSBP y EVRP.

---

## Capítulo 5: Variables no binarias

Los problemas de los capítulos anteriores se expresan con ILP binarios, donde quitar un valor del dominio de una variable la fija de inmediato. Con variables enteras no binarias aparece un problema: si en una iteración se construyen dos soluciones con x_i = 1 y x_i = 3 en un dominio {1, 2, 3}, el dominio reducido {1, 3} no es contiguo y los *ILP solvers* no admiten dominios así. Para modelos mixtos con pocas variables no binarias se puede ignorarlas y aplicar CMSA solo a las binarias. Si hay muchas, la propuesta es binarizar el ILP.

Se ilustra con el *Bounded Knapsack Problem with Conflicts* (BKPWC), una extensión de la mochila acotada en la que se pueden poner hasta m_j copias de cada ítem y los pares de ítems en conflicto (aristas de un grafo de conflictos) no pueden estar juntos. El modelo usa variables enteras x_j (número de copias) e indicadoras binarias y_j. Al binarizar, los componentes de solución pasan a ser pares c_{j,l} (ítem j con l copias). La construcción elige un ítem al azar, decide cuántas copias poner y elimina del conjunto de candidatos los ítems en conflicto o que ya no caben.

Resultados: CMSA supera en general a CPLEX y a CPLEX con el modelo binarizado (CPLEX_BIN). Como en el MDS, con instancias fáciles el rendimiento es comparable y con instancias grandes o difíciles CMSA gana. Las instancias se vuelven más difíciles con el tamaño, la capacidad y el número de conflictos. En casi todas las de 5000 ítems las dos versiones de CPLEX solo encuentran la solución trivial sin ningún ítem. Entre las dos versiones de CPLEX, CPLEX_BIN gana con significación en las instancias con menos y más conflictos y el modelo original en las de conflictos intermedios, y el autor no tiene explicación.

Advertencia del autor: binarizar aumenta el número de variables y restricciones, y con cotas muy altas de copias podría no compensar. Aplicar CMSA a ILP mixtos es una línea de investigación abierta.

---

## Capítulo 6: Líneas de investigación adicionales

### CMSA independiente del problema para programas binarios (CMSA_GEN)

La limitación de CMSA es que hace falta una construcción probabilística específica del problema. Blum y Gambini Santos (2019) proponen un CMSA para cualquier programa binario min{cᵀx : Ax ≤ b, x binaria}, con los componentes genéricos c_j^0 y c_j^1. La primera solución se intenta obtener con la heurística de nodos de CPLEX. Si no la hay, se resuelve la relajación LP con un límite de tiempo t_LP. Las soluciones nuevas se generan por redondeo aleatorizado de un vector de muestreo, calculado a partir de S_bsf y de una tasa de determinismo si existe una mejor solución, o a partir de la relajación LP si no. De forma opcional, un motor de propagación de restricciones (cprop) preprocesa el problema, usa un orden aleatorio de variables y corrige asignaciones que llevarían a infactibilidad (variante CMSA_GEN_CP). Los parámetros d_rate y t_ILP se adaptan al estilo de VNS: parten de su cota inferior, vuelven a ella si hay mejora y, si no, aumentan en (cota superior − cota inferior)/5 con reinicio al pasar de la cota superior.

Se probó en 30 instancias (27 de MIPLIB 2010 y 3 propias) contra CPLEX 12.7 con configuración por defecto (CPLEX-OPT) y con énfasis heurístico máximo (CPLEX-HEUR). En instancias fáciles CPLEX llega antes al óptimo. En las clasificadas como difíciles o abiertas, las dos variantes de CMSA encuentran buenas soluciones mucho antes y acaban con mejores resultados. El autor reconoce que es un primer paso: falta capacidad para hallar rápido soluciones factibles en problemas muy restringidos, para lo que sugiere el BRKGA híbrido.

### Una metaheurística como solver de subinstancias

Pinacho Davidson, Blum y Lozano (2018) sustituyen el ILP por una metaheurística en el problema *Weighted Independent Domination* (WID). Un conjunto independiente dominante es un conjunto independiente maximal, y el WID busca el de menor peso. Se usa un *Population-Based Iterated Greedy* (PBIG) con una heurística voraz como base. CMSA_PBIG, que resuelve las subinstancias con PBIG en lugar de con CPLEX, supera a PBIG con significación estadística, en todos los tamaños y densidades de grafo. El autor advierte de que no es una mejora que se pueda esperar en general: al meter el BA del capítulo 3 dentro de CMSA, tanto en MDS como en FFMS, el BA por sí solo daba mejores resultados. Su hipótesis es que CMSA ayuda a PBIG a salir de la zona de atracción de óptimos locales, algo que el BA necesita menos.

### CMSA frente a LNS

Se compara con LNS basado en destrucción: en cada iteración destruye parcialmente S_bsf con una tasa de destrucción variable (entre L y U, al estilo VNS), y el ILP busca la mejor solución que contenga la parte conservada. Mientras que CMSA restringe el ILP a los componentes de C', LNS lo obliga a incluir la solución parcial. La hipótesis de Lizárraga, Blesa y Blum (2017) es que CMSA funciona mejor cuando las soluciones tienen pocos componentes, y LNS cuando tienen muchos. Se probó con el *multi-dimensional knapsack problem* (MDKP), con n en {100, 500, 1000, 5000, 10000}, 30 recursos, α entre 0.1 y 0.9 para controlar la tensión de las capacidades y 30 instancias por combinación (1350 instancias en total), y con el *minimum common string partition*. Los resultados confirman la hipótesis. La explicación propuesta, que no está validada, es que LNS conserva siempre parte de la solución y, si arranca lejos de las zonas buenas, difícilmente da saltos grandes con soluciones pequeñas, mientras que CMSA genera soluciones en cualquier parte del espacio y las fusiona.

### Trabajo futuro

El autor ve tres líneas. Mejorar el CMSA independiente del problema hasta que supere de forma fiable a CPLEX y Gurobi, lo que lo haría útil como referencia para evaluar algoritmos hechos a mano. Usar *solvers* específicos del problema, por ejemplo MaxSAT, en lugar de *solvers* ILP genéricos para las subinstancias. Y añadir *machine learning* a la construcción, por ejemplo *reinforcement learning* con algoritmos de *multi-armed bandits*. El grupo de optimización del IIIA-CSIC seguirá con esta línea.

---

## Apéndice A

Contiene el código C++ de CMSA estándar aplicado al MDS con la definición intuitiva de componentes (CMSA_INT).

---

## Relevancia para el TFM

- DCIIM ([[motaz_2026_reformulating_influence_maximization]]) cumple los dos requisitos de CMSA: tiene un modelo ILP y admite una construcción voraz aleatorizada de conjuntos de semillas. CMSA es una de las líneas candidatas para hacerlo escalable.
- Resolver el ILP solo sobre una subinstancia C' de patrones candidatos reduciría el tamaño de la estructura `coverage_count`, que es el cuello de botella identificado en el estudio de escalabilidad ([[Reunion_Cordon_02]]).
- El problema de selección de semillas tiene soluciones pequeñas (k nodos), el caso en el que CMSA supera a LNS según el capítulo 6.
- El capítulo 4 es relevante porque DCIIM ya es un modelo de cobertura de transacciones, y el patrón del VSBP (componentes que son conjuntos completos, ≥ en la cobertura y eliminación posterior de duplicados) se parece a cómo podrían representarse los patrones candidatos.
- ADAPT_CMSA y su aplicación a MPIDS son el antecedente más cercano dentro del libro: un problema de influencia en redes sociales resuelto con CMSA, con instancias Barabási-Albert de hasta 100000 vértices. El resultado de que CMSA estándar falla a 50000 y 100000 vértices porque las subinstancias dejan de ser resolubles es una advertencia directa para un DCIIM de gran escala.
- El CMSA_GEN genérico (dos componentes por variable binaria) es la vía más rápida para probar CMSA sobre el ILP de DCIIM sin diseñar componentes propios, aunque el libro lo presenta como primer paso con limitaciones en problemas muy restringidos.
- irace, scmamp y STNWeb pueden usarse en la parte experimental del TFM. El diseño experimental del libro (30 instancias por combinación de tamaño y densidad, un único ajuste con irace, CD plots) es un modelo a seguir.
- Fue recomendado como bibliografía base en [[Reunion_Cordon_02]], con el capítulo 1 como lectura fundamental.

---

## Conceptos clave

- [[CMSA]]
- [[ADAPT_CMSA]]
- [[Large Neighborhood Search]]
- [[Integer Linear Programming]]
- [[Minimum Dominating Set]]
- [[Minimum Positive Influence Dominating Set]]
- [[Set Covering]]
- [[irace]]


---
tags:
	#TFM #CMSA #Metaheuristicas #OptimizacionCombinatoria #Libro
---
