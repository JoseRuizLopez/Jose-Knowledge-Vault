links: [[102 - no-TFM]] 

## Parte simbólica en un modelo neurosimbólico

En general, el componente simbólico:
- **Representa conocimiento explícito** (conceptos, relaciones, reglas lógicas).
- **Permite razonar o verificar consistencia** de las predicciones neuronales.
- **Actúa como guía o regulador** del aprendizaje profundo.

Es decir, es **todo lo que no se aprende directamente de los datos**, sino que viene de **estructura conceptual o conocimiento experto**.  
En este caso, eso incluye desde un **grafo de conocimiento (KG)** hasta un **sistema de reglas o restricciones** sobre cómo deberían comportarse los objetos y sus relaciones en una escena.

---

## Niveles de la parte simbólica

### 🔹 Nivel 1 — _Representación simbólica del conocimiento_
Aquí se define **qué sabes del mundo**, y lo formalizas. 

El DL detecta objetos y relaciones, pero se define un **grafo de conocimiento (KG)** que describe:
- Qué entidades existen (`Persona`, `Coche`, `Edificio`, `Semáforo`...)
- Qué relaciones son posibles (`está_cerca_de`, `conduce`, `entra_en`, `parte_de`...)
- Qué tipos de relaciones no son válidas (`Persona dentro de Coche` sí, pero `Coche dentro de Persona` no). 

🔧 Ejemplo:
`Regla 1: "Si un objeto es Persona y otro es Coche, la relación válida es 'conduce' o 'entra_en', pero no 'parte_de'." Regla 2: "Si un objeto es Edificio, puede tener relación 'parte_de' con Ciudad."`

📘 Técnicamente, esto se puede almacenar como:
- Un **ontología [[OWL]]/[[RDF]]** (para razonamiento lógico formal).
- Un **Neo4j graph** (si priorizas consultas estructuradas).
- Un **diccionario Python + reglas simbólicas** (más simple, pero suficiente para un prototipo).

---

### 🔹 Nivel 2 — _Alineación semántica entre DL y KG_
La red neuronal detecta “cosas” (perros, coches, semáforos...), pero **esas detecciones deben alinearse con conceptos simbólicos** del KG.

Esto implica:
1. Crear un **mapeo entre labels neuronales** (provenientes del dataset visual) **y nodos del grafo simbólico**. 
> ¿Se debería usar un embeding semántico/tabla de equivalencias para alinear automáticamente términos similares para resolver la ambigüedad posible en el mapeo??
2. Traducir las salidas de la red (por ejemplo, tripletas “Persona – junto_a – Coche”) a nodos y aristas en un grafo semántico.
3. Verificar si esas relaciones **tienen sentido según el conocimiento simbólico**.

🔧 Ejemplo:
> El modelo detecta “Coche sobre Persona” → la parte simbólica verifica que **esa relación no es coherente** → puede marcarla como inconsistente o corregirla.

📘 Aquí entra el razonamiento simbólico:  
usando lógicas tipo **Datalog, Prolog, [[OWL]] Reasoner** o incluso comprobaciones heurísticas, puedes **detectar contradicciones o anomalías**.

---
### 🔹 Nivel 3 — _Razonamiento y explicabilidad simbólica_
El paso final es **usar ese conocimiento simbólico para razonar o explicar**.

Esto puede adoptar tres formas:
#### (a) Verificación / consistencia
Compruebas si las relaciones detectadas son válidas:

> “El sistema detectó una persona conduciendo un coche, lo cual es coherente con las reglas del conocimiento.”
#### (b) Inferencia / enriquecimiento
Generas nuevas relaciones a partir del conocimiento:

> “Si un objeto es semáforo y está en rojo, infiero que los coches cercanos deberían estar detenidos.”
#### (c) Explicación
Usas el grafo simbólico para **justificar** lo que el modelo visual predijo:

> “Clasifiqué esto como una calle porque hay coches, peatones y semáforos alineados.”
---
## 🧩 3. Cómo interactúan parte neuronal y simbólica
Se podría entender como **dos cerebros colaborando**:

| Parte                       | Función      | Ejemplo                                                                |
| --------------------------- | ------------ | ---------------------------------------------------------------------- |
| **Neuronal (DL)**           | Percepción   | “Veo un coche, una persona y un semáforo.”                             |
| **Simbólica (KG + reglas)** | Razonamiento | “Un coche y una persona pueden estar juntos si la persona lo conduce.” |
| **Neuro-simbólica**         | Integración  | Ajusta predicciones según conocimiento, o explica resultados.          |

📘 Puedes visualizarlo como un pipeline:
`Imagen → CNN/Transformer (detector) → Scene Graph → Alineación con KG → Razonamiento simbólico`

La parte simbólica empieza **a partir del Scene Graph**, y es la que le da **sentido semántico y explicabilidad estructural**.

---
## ⚙️ 4. Cómo implementarlo (opciones prácticas)

Dependiendo del enfoque y tiempo que tengas, hay varios niveles de profundidad:

| Nivel    | Herramienta / Enfoque                       | Dificultad | Qué aporta                                           |
| -------- | ------------------------------------------- | ---------- | ---------------------------------------------------- |
| Básico   | Diccionario + comprobación lógica en Python | 🟢Fácil    | Verificación básica de relaciones válidas            |
| Medio    | Neo4j o [[RDF]] + SPARQL                    | 🟠Media    | Consultas estructuradas y reglas declarativas        |
| Avanzado | [[OWL]] Reasoner / Prolog / Datalog         | 🔴Alta     | Razonamiento lógico complejo e inferencia automática |

---
## 🧭 5. Cómo explicárselo a Siham Tabik

> “La parte simbólica de mi TFM será el módulo que represente y razone sobre el conocimiento. Mi idea es diseñar un grafo de conocimiento con las entidades y relaciones válidas entre objetos de la escena.  
> Este grafo servirá tanto para verificar la coherencia de las relaciones detectadas por el modelo visual como para generar explicaciones estructuradas del tipo ‘por qué se ha clasificado así’.  
> Además, me gustaría estudiar cómo integrar parte de ese conocimiento como restricciones suaves durante el entrenamiento, siguiendo la línea de [[X-NeSyL]].”

Esto conecta directamente con el marco de Siham y con los papers (EXPLANet, SHAP-Backprop, Semantic Loss...).

---
## 🧱 6. En resumen

| Rol             | Qué hace la parte simbólica                                   |
| --------------- | ------------------------------------------------------------- |
| **Representar** | Define un modelo conceptual del mundo (KG, reglas, ontología) |
| **Verificar**   | Comprueba que las predicciones neuronales sean coherentes     |
| **Inferir**     | Deduce relaciones adicionales basadas en reglas               |
| **Explicar**    | Justifica decisiones con conocimiento explícito               |
| **Guiar**       | (Opcional) Aporta restricciones o feedback al entrenamiento   |



---
tags:
	#Index #no-TFM