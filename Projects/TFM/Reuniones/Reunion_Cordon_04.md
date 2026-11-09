

Asegurarme de usar PyPy.

Ejecutar antes el coverage_count y el cover previamente para cargarlo, dejar apuntado lo que tarda y ya poder ejecutar el resto. En esa parte, dejar que sobresalga del limite de los 1800s. Puede ser adecuado 1 o 2h. Isaac lo dejo 6h.


El calculo en todos los métodos, el Montecarlo se queda dentro, por lo que se podría quedar fuera de los tiempos el Montecarlo. Solo se ejecuta el Montecarlo con las semillas candidatas.

Oscar piensa que con k=3, no se limite el phi a que sea menor que 3. Simplemente es que las redes con ese phi ya se obtienen lo máximo.

CSNA usa modelos matemáticos, pero crea un nuevo grafo pero solo con las aristas mas importantes. Entonces es mucho mas rápido y viable.

Confirmar si usaban previamente paralelismo. En nuestra propuesta también podríamos paralelizar nuestro Montecarlo.

No tiene pinta que vayamos a aumentar mas el max-iter de 10 (porque no tiene mucha influencia).
Además, se suele hacer un estudio de parámetros y luego en el estudio final con los parámetros mas prometedores.

Siguientes pasos

- Seguir con el apartado de literatura (No hay mucha). Hasta que no estén los 3 apartados primeros hechos, ya le daremos una revisión. 
- Objetivo principal: análisis de la escalabilidad de los problemas y como abordarlo. Sobre todo lo que voy a aprender (objetivos específicos), mejorar en Python, usar solvers, modelo matemático, etc.
- Me van a pedir un titulo y un párrafo para definir el TFM. 
- Tarea técnica: implementar el Montecarlo en paralelo y sacar el modelo aparte y guardarlo en disco,  y hacer los experimentos con las redes grandes (lo mas importante). Para generar el modelo fuera no tener limite de tiempo (o un tiempo laxo). Para los modelos si tener un timeout para el ILP(3-4h)
- [A futuro] Ver como hacer ese modelo mas pequeño y como hacerlo mas escalable. (con CSNA).

