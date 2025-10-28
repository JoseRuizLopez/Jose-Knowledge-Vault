Link: [[007 - 0410 Teoria|Teoría]]


# Técnicas de Agrupamiento

**Aprendizaje Semi-Supervisado**: Tienes unas cuantas datos etiquetados pero otras no.
**Aprendizaje Auto-Supervisado**: Cuando no tiene etiquetas de partida, te las inventas. Para ir cambiando las características y que sean mas representativas. Por ejemplo, se usa el enmascaramiento. 

El **clustering** se usa para:
- Se buscan grupos que se parezcan los datos entre ellos. 
- Se puede usar en las primeras etapas de un EDA.
- También se ha usado para paralelizar comparaciones entre datos, por ejemplo tu huella con las huellas en un BD, para agrupar las huellas en grupos de mismo tamaños y clasificamos solo en ese grupo.
- Muchos otros ejemplos.

Para medir similitud, por ejemplo en imágenes se usa Deep Learning y distancia entre esos embeddings.

El clustering real nunca se puede asegurar.

## K-means

Intenta minimizar la distancia a los centroides de manera rápida, siendo un algoritmo buzzy. Pero tiene el riesgo de caer en un mínimo local.

> No se aplica de forma explicita la función de coste. 


> [!FÓRMULA]
> 1. Eligir de forma aleatoria k puntos (el punto refiriéndose a alguno de los datos). Donde cada punto sería el centro del claster.
> 2. Mientras haya cambios:
> 	1. Reasignar cada objeto al claster mas cercano
> 	2. Recalcular los claster como el punto medio de cada claster

### Fortalezas
- Simple y detecta clústeres esféricos
- Es un algoritmo relativamente eficiente
  O(k\*m\*iteraciones)
### Debilidades:
- Mínimos locales
- ¿Cómo manejamos datos nominales?
- ¿Determinar el valor de k?
-  Sensibilidad al ruido
- Hay tipos de clusters que no podrán ser encontrados (p.ej non-convex) Como los que no son esféricos o si los tamaños y/o densidades no son uniformes, ya que K-means saca agrupaciones uniformes.


### Posibles preguntas en el examen sobre k-means
- Dibuja un dataset donde no funcione el k-means y otro si.
- Te pone un caso donde razones y expliques porque se va a la mierda.
- ¿Se puede aplicar el descenso de gradiente como función de coste?
- ¿Por que no se usa la función de coste dentro del k-means, pero siempre el coste va hacia abajo?
	- Intrínsecamente no se usa, pero matemáticamente la función de coste no empeora por mover los centroides, aunque no garantice que caiga en un máximo local.
- ¿Tiene sentido cambiar la función de distancia? 
	- No porque se pierde el garantizado de obtener la mejora de la función de coste, pierde el sentido para el calculo del centroide.


### Scikit-Learn
Usar pipelines de Scikit-Learn para realizar los pre-procesamientos.
- Se debe de entender que hacen los parámetros.
- labels_: son los colores para las clases.
- innercia_: nos guarda la función de coste.

## Elementos básicos de un algoritmo de clustering
Se pueden usar algunas medidas de distancia o similitud.
La diferencia entre ambas medidas es que en similitud, la similaridad puede ser del 100% pero no son el mismo objeto, en cambio la de distancia si se consideraría el mismo objeto.


## Medidas de calidad
- Pregunta si nos da la formula,si tiene sentido usar esa medida para esos datos.
- Pregunta siempre entre los métodos intrínsecos y los métodos extrínsecos.

## K-medoids (PAM)
Sigue sin resolver el problema de seleccionar el k. Hay que seguir usando la regla del codo o de la rodilla; ni arregla la inicialización.

Una de las preguntas típicas del examen es:

**Ventajas**
- Es menos sensible al ruido.
- Es mas interpretable.
- Ahora si se puede cambiar el calculo de distancia y aun así sigue garantizando que se recude la funciona de coste.

**Desventajas**
- Tiene mayor computo.


## GMMs
Gaussian Mixture Models. Generan los clusteres asignando un porcentaje de probabilidad a cada cluster.
Sigue sin resolver el problema de selección de k ni arregla la inicialización.
**Ventajas**
- Puede generar Clusters que no son esféricos (elipsoides), a diferencia de PAM y k-means.

## DBSCAN
**Ventajas**
- No hay que establecer numero de clusters.
- Funciona bien para separar clusters que no tienen una formas concretas.

**Desventajas**
- Hay datos que no podrá clasificar, como cuando la dispersion de los datos de los clasters no son iguales entre distintos clusteres.

