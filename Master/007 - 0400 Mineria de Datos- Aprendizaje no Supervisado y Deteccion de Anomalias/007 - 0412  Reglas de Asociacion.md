Link: [[007 - 0410 Teoria|Teoría]]


# Reglas de Asociación
**Aprendizaje no Supervisado**: No tenemos los datos esperados, o si los tenemos, no nos interesa predecir los resultados.

Si se da el **antecedente**, entonces con una alta probabilidad se da el **consecuente**.
Donde en esos datos, no pueden ser de los 2 tipos a la vez.


## Medidas Clásicas
**Soporte**: Porcentaje del dato que aparece en un **itemset**. Por ejemplo, hay 50 pañales en las compras de 100 compras totales.
**Confianza**: De toda las transacciones, en cuantas pasan que están las 2 transacciones. Es decir, de cuantas en todas las que se compran pañales, ¿también se compran cervezas?

## Métodos Clásicos
Todos comprueban todo el abanico de soluciones, todo el espacio de muestras. Generan todas las soluciones posibles.
#### Apriori
==Repasar este método==
Usa la propiedad de **Soporte**. Poda los que no superan el umbral.
- Ahorra coste, pero no sabemos el umbral.

#### Eclat
- Funcionaria bien, pero ocupa demasiada memoria.
#### FP-growth
Genera la estructura primero, y luego calcula. Solo consulta una vez la BD, solo para construir la estructura.
_El año pasado, en el examen hubo un ejercicio pidiendo generar el árbol._
- Es super paralelizable.

## Conjuntos maximales y cerrados 
Los **maximales** son los frecuentes que si le añadáis cualquier elemento mas, dejan de ser frecuentes.
Los **cerrados** son los frecuentes que si le añades un ítem, su soporte baja. Los maximales son también cerrados.

Pero lo interesante son las relaciones que no son super altas, y que tampoco son bajas. Por ejemplo, el 90% de los clientes si compran X producto, eso el jefe ya lo sabría, pero no con el 50% por ejemplo.
