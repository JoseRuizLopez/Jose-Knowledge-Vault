links: [[001 - 020 Machine Learning|Machine Learning]]

# Shapes Constraint Language (SHACL)
**SHACL (Shapes Constraint Language)** es un estándar del **W3C** diseñado para **validar, describir y restringir datos [[RDF]]**. Su objetivo principal es **comprobar que un grafo [[RDF]] cumple con una estructura o reglas esperadas**, algo esencial cuando se trabaja con **grafos de conocimiento** en aplicaciones de inteligencia artificial, web semántica o sistemas neuro-simbólicos como el del [[TFM]].

Mientras que **[[OWL]]** se usa para **razonar** (inferir nuevo conocimiento), **SHACL** se usa para **validar** (detectar incoherencias o errores estructurales). Por eso, [[OWL]] y **SHACL** son **complementarios**.

---

### 🧩 Estructura conceptual

SHACL introduce el concepto de **Shape** (forma o molde), que define **cómo deben ser los nodos** de un grafo [[RDF]]:

- **Node Shapes** → definen restricciones sobre un tipo de entidad (por ejemplo, que toda `Persona` tenga nombre y edad).
    
- **Property Shapes** → definen restricciones sobre una propiedad (por ejemplo, que `edad` sea un número entero positivo).
    

---

### 🧱 Ejemplo (en sintaxis Turtle)

```
@prefix sh: <http://www.w3.org/ns/shacl#> .
@prefix ex: <http://example.org/> .

ex:PersonaShape
    a sh:NodeShape ;
    sh:targetClass ex:Persona ;
    sh:property [
        sh:path ex:nombre ;
        sh:datatype xsd:string ;
        sh:minCount 1 ;
    ] ;
    sh:property [
        sh:path ex:edad ;
        sh:datatype xsd:integer ;
        sh:minInclusive 0 ;
    ] .
```

**Significado:**

- Todo nodo del tipo `ex:Persona` debe tener:
    
    - Al menos un `ex:nombre` (tipo texto).
        
    - Una `ex:edad` (entero ≥ 0).
        

---

### ⚙️ Usos principales

1. **Validación de datos [[RDF]]** antes de integrarlos en un grafo de conocimiento.
    
2. **Detección de incoherencias** estructurales o semánticas.
    
3. **Control de calidad** en bases de conocimiento y sistemas de razonamiento.
    
4. **Documentación semántica** (describe explícitamente la forma esperada de los datos).
    

---

### 🧠 Comparativa resumida

| Lenguaje    | Propósito principal                                   | Tipo de razonamiento | Ejemplo de uso                                         |
| ----------- | ----------------------------------------------------- | -------------------- | ------------------------------------------------------ |
| **[[RDF]]** | Representar datos como tripletas                      | Ninguno              | Describir recursos                                     |
| **[[OWL]]** | Inferir conocimiento y garantizar consistencia lógica | Deductivo            | “Si A es hijo de B y B es hijo de C → A es nieto de C” |
| **SHACL**   | Validar que los datos cumplen reglas estructurales    | Comprobación         | “Toda Persona debe tener edad ≥ 0 y un nombre”         |

---

En el contexto del **[[TFM]]**, **SHACL** puede servir para **verificar la coherencia estructural** del grafo resultante tras la alineación neuronal–simbólica, asegurando que las relaciones detectadas por el modelo cumplen las **restricciones semánticas** definidas en el **Knowledge Graph (KG)**.

---
tags:
	#Concept  #MachineLearning #KAG  
