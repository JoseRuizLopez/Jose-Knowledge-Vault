Link: [[007 - 0410 Teoria|Teoría]]


# Reglas de Asociación
**Aprendizaje no Supervisado**: No tenemos los datos esperados, o si los tenemos, no nos interesa predecir los resultados.

Si se da el **antecedente**, entonces con una alta probabilidad se da el **consecuente**.
Donde en esos datos, no pueden ser de los 2 tipos a la vez.


## Medidas Clásicas
**Soporte**: Porcentaje del dato que aparece en un **itemset**. Por ejemplo, hay 50 pañales en las compras de 100 compras totales.
**Confianza**: De toda las transacciones, en cuantas pasan que están las 2 transacciones. Es decir, de cuantas en todas las que se compran pañales, ¿también se compran cervezas?

## Métodos Clásicos
#### Apriori
Ahorra coste, pero no sabemos el umbral.
#### Eclat
Funcionaria bien, pero ocupa demasiada memoria.
#### FP-growth
Genera la estructura primero, y luego calcula. Solo consulta una vez la BD, solo para construir la estructura.
