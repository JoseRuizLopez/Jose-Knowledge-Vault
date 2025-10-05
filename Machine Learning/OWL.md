links: [[001 - 020 Machine Learning|Machine Learning]]

# Web Ontology Language (OWL)
**Web Ontology Language (OWL)** es un lenguaje estándar del **W3C** diseñado para **definir, estructurar y razonar sobre ontologías** en la **Web Semántica**. Se construye sobre **RDF (Resource Description Framework)** y **RDFS (RDF Schema)**, añadiendo una capa más expresiva que permite representar **conocimiento complejo, relaciones jerárquicas y restricciones lógicas**.

En términos simples, OWL permite **definir clases, propiedades y axiomas** que describen cómo se relacionan los conceptos entre sí, de forma que los sistemas automáticos puedan **inferir nuevo conocimiento** a partir de lo ya descrito.

### Estructura básica

OWL se basa en **tripletas RDF**, pero introduce una semántica más rica:

- **Clases**: categorías o conceptos (p. ej. `Persona`, `Vehículo`, `Edificio`).
    
- **Individuos**: instancias concretas de una clase (p. ej. `José` es una `Persona`).
    
- **Propiedades**: relaciones entre individuos o atributos (p. ej. `conduce`, `tieneColor`).
    
- **Axiomas y restricciones**: reglas lógicas que describen comportamientos válidos o incompatibles (p. ej. “Una Persona no puede ser al mismo tiempo un Vehículo”).
    

### Ejemplo (en sintaxis Turtle)

```
:Persona rdf:type owl:Class . 
:Vehiculo rdf:type owl:Class . 
:conduce  rdf:type owl:ObjectProperty ;          
		rdfs:domain :Persona ;          
		rdfs:range :Vehiculo . 
:Jose rdf:type :Persona . 
:Coche1 rdf:type :Vehiculo . 
:Jose :conduce :Coche1 .`
```

### Ventajas

- Permite **razonamiento automático** (deducir relaciones no explícitas).
    
- Garantiza **consistencia lógica** en los grafos de conocimiento.
    
- Facilita la **interoperabilidad semántica** entre sistemas y dominios.
    

### Versiones

- **OWL Lite**: más simple, orientado a taxonomías básicas.
    
- **OWL DL (Description Logic)**: balance entre expresividad y razonamiento automático.
    
- **OWL Full**: máxima expresividad, pero menos eficiente para el razonamiento.
    

En el [[TFM]], OWL sería la herramienta adecuada para **formalizar el conocimiento simbólico del dominio** (por ejemplo, las relaciones válidas entre objetos de una escena) y **verificar coherencia o inferir nuevas relaciones** cuando se alinea con las predicciones de la red neuronal.

---
tags:
	#Concept  #MachineLearning #KAG  
