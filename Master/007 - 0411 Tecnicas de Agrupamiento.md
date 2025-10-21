Link: [[007 - 041 Teoria|Teoría]]


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

> [!FÓRMULA]
> 1. Eligir de forma aleatoria k puntos (el punto refiriéndose a alguno de los datos). Donde cada punto sería el centro del claster.
> 2. Mientras haya cambios:
> 	1. Reasignar cada objeto al claster mas cercano
> 	2. Recalcular los claster como el punto medio de cada claster

