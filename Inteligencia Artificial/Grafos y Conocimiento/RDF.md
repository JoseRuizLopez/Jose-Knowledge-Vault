links: [[001 - 020 Inteligencia Artificial|Inteligencia Artificial]]

# Resource Description Framework (RDF)
**Resource Description Framework (RDF)** es un modelo estándar del **W3C** (World Wide Web Consortium) diseñado para representar y describir información estructurada en la **Web Semántica**. Su objetivo es permitir que los datos puedan ser **compartidos, integrados y entendidos por máquinas**, no solo por humanos.

En RDF, la información se expresa mediante **tripletas** de la forma:  
**(Sujeto, Predicado, Objeto)**

- **Sujeto**: el recurso del que se habla (por ejemplo, una persona o una imagen).
    
- **Predicado**: la propiedad o relación (por ejemplo, “tieneAutor”, “esParteDe”).
    
- **Objeto**: el valor o recurso relacionado (por ejemplo, “José Ruiz López”, “Escena Urbana”).
    

Estas tripletas pueden representarse como un **grafo dirigido**, donde los nodos son entidades (sujetos u objetos) y las aristas son las relaciones (predicados). RDF usa identificadores únicos llamados **URIs** para garantizar que cada recurso sea inequívoco en la red.

**Ejemplo:**

`<http://ejemplo.org/imagen1> <http://ejemplo.org/tieneAutor> "José Ruiz López".`

En la práctica, RDF se utiliza para construir **grafos de conocimiento (Knowledge Graphs)**, que pueden ser consultados mediante el lenguaje **SPARQL** y sirven de base para el razonamiento simbólico en sistemas neuro-simbólicos.

---
tags:
	#Concept  #MachineLearning #KAG  
