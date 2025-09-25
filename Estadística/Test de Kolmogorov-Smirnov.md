links: [[001 - 010 Estadística|Estadística]]

# Test de Kolmogorov-Smirnov
El **test de Kolmogorov-Smirnov** es una prueba estadística no paramétrica que se utiliza para comparar distribuciones. Su versatilidad le permite usarse tanto en el contexto de un **test de bondad de ajuste** (one-sample KS test) —para evaluar si una muestra proviene de una distribución teórica específica— como en el **test de dos muestras** (two-sample KS test), en el que se comparan dos muestras independientes para determinar si proceden de la misma distribución.

Este método es ampliamente empleado en diversas áreas como la estadística, la econometría y la ciencia de datos, sobre todo cuando se desea **evaluar la diferencia entre distribuciones** sin asumir una forma funcional específica (por ejemplo, la normalidad).


## Fundamentos y Definición

### Objetivo del Test

El propósito del **test KS** es evaluar la hipótesis nula (H₀) de que:

- En el caso **one-sample**: la muestra analizada sigue una distribución teórica dada.
- En el caso **two-sample**: dos muestras provienen de la misma distribución continua.

### Estadística de Prueba

La estadística de prueba en el **test KS** se define como la **máxima diferencia absoluta** entre la función de distribución acumulada (CDF) empírica de la muestra y la CDF teórica (o entre dos CDF empíricas, en el caso de dos muestras):

- **One-sample KS test**:
    $$
    D = \sup_{x} | F_n(x) - F(x) |
    $$
    donde $F_n(x)$ es la CDF empírica de la muestra y $F(x)$ es la CDF de la distribución teórica.
    
- **Two-sample KS test**:
    $$
    D = \sup_{x} | F_{n1}(x) - F_{n2}(x) |
    $$
    donde $F_{n1}(x)$ y $F_{n2}(x)$ son las CDF empíricas de cada una de las muestras.
    

El valor $D$ se **compara con valores críticos** o se utiliza para calcular un **p-valor**, de forma que se decide si se rechaza la hipótesis nula.


## Procedimiento y Aplicaciones

### Procedimiento General

1. **Cálculo de la CDF empírica:** Se ordena la muestra y se construye su función de distribución acumulada.
2. **Comparación con la CDF teórica o entre muestras:** Se evalúa la diferencia en cada punto $x$ y se toma la diferencia máxima.
3. **Determinación del p-valor:** Se calcula un p-valor asociado a la estadística $D$ utilizando tablas o métodos computacionales, permitiendo evaluar la significación de la diferencia.
4. **Conclusión:** Si el p-valor es menor al nivel de significancia establecido (por ejemplo, 0.05), se rechaza la hipótesis nula.

### Aplicaciones Prácticas

- **Validación de modelos:** Comprobar si los residuos de un modelo siguen una distribución normal o cualquier otra distribución teórica.
- **Comparación de distribuciones:** Evaluar si dos muestras independientes se comportan de manera similar sin asumir parámetros específicos.
- **Control de calidad:** En áreas como la ingeniería, para comparar la distribución de medidas de producción con la distribución esperada.


## Supuestos y Limitaciones

### Supuestos del Test

- **Independencia:** Los datos de la muestra deben ser independientes.
- **Continuidad:** El test asume que la distribución es continua. La presencia de valores repetidos (empates) puede afectar la validez de la prueba.
- **Parámetros fijos:** En el one-sample KS test, si se estiman parámetros de la distribución a partir de los datos, la prueba se vuelve más conservadora. En estos casos, pueden requerirse ajustes o el uso de métodos alternativos.

### Limitaciones

- **Sensibilidad variable:** La prueba es especialmente sensible en el centro de la distribución y puede ser menos sensible en las colas, lo que puede limitar su capacidad para detectar diferencias en los extremos.
- **Estimación de parámetros:** La necesidad de conocer a priori la distribución teórica y sus parámetros puede ser un inconveniente si éstos deben ser estimados a partir de la muestra.


## Comparación con Otros Métodos

Existen otros métodos que pueden ser preferibles al test KS en determinadas situaciones, dependiendo de la naturaleza de los datos y de los objetivos específicos del análisis. Entre estos destacan:

### Test de Anderson-Darling

- **Descripción:** Este test da mayor peso a las colas de la distribución, siendo más sensible a discrepancias en los extremos.
- **Ventajas:** Mejor rendimiento cuando las diferencias entre distribuciones se manifiestan en las colas.
- **Uso típico:** Evaluación de la bondad de ajuste en análisis donde las colas son de particular interés (por ejemplo, en estudios financieros o en la detección de valores atípicos).

### Test de Cramér-von Mises

- **Descripción:** Se basa en la integral de la diferencia al cuadrado entre las CDFs, lo que permite capturar diferencias en toda la distribución de manera equilibrada.
- **Ventajas:** Considera las diferencias en todos los puntos de la distribución, no solo el máximo absoluto.
- **Uso típico:** Comparaciones generales cuando se busca una evaluación global de las diferencias.

### Test de Chi-cuadrado (χ²) de Bondad de Ajuste

- **Descripción:** Se utiliza para datos categóricos o cuando se agrupan datos continuos en intervalos.
- **Ventajas:** Útil cuando se dispone de datos en formato de frecuencias.
- **Limitaciones:** La elección de los intervalos puede influir en el resultado y es menos adecuado para distribuciones continuas sin agrupación.

### Test de Shapiro-Wilk y Test de Lilliefors

- **Shapiro-Wilk:** Específicamente diseñado para evaluar la normalidad. Suele tener mayor poder estadístico que el KS cuando se trata de detectar desviaciones de la normalidad.
- **Lilliefors:** Una adaptación del test KS para el caso en que los parámetros de la distribución normal se estiman a partir de la muestra.
- **Ventajas:** Estos tests son recomendados cuando la hipótesis de normalidad es crítica para el análisis.

### ¿Existen métodos “mejores”?

La elección del método “mejor” depende del contexto:

- **Sensibilidad en colas vs. centro:** Si la diferencia entre distribuciones se manifiesta en las colas, el test de Anderson-Darling puede ser superior.
- **Tipo de datos:** Para datos categóricos o cuando se requieren intervalos, el test de chi-cuadrado puede ser más adecuado.
- **Estimación de parámetros:** Si los parámetros se estiman a partir de la muestra, pruebas como Lilliefors pueden ofrecer resultados más realistas.

En resumen, no existe un test universalmente “mejor”, sino que la elección depende de la naturaleza de los datos, la hipótesis a contrastar y las características específicas de la distribución que se desea analizar.

---
tags:
	#Concept 