links: [[008 Summaries|Summaries]] 


Reference: [[HAKE.pdf|HAKE: A Knowledge Engine Foundation for Human Activity Understanding]]
# Resumen del HAKE
**Problema:**  
Los modelos de visión profunda tradicionales (CNN, DETR, etc.) funcionan bien para reconocer **objetos**, pero fallan al interpretar **actividades humanas** (como _“lavar el coche”_ o _“montar a caballo”_).  
Las actividades requieren **relaciones entre partes del cuerpo, objetos y acciones**; no basta con detectar píxeles o categorías.

**Hipótesis clave:**  
El reconocimiento de actividades humanas necesita un nivel intermedio de representación —los llamados **primitivos**— que describen **estados semánticos de partes del cuerpo y objetos**.  
Luego, un **motor simbólico** combina estos primitivos con **reglas lógicas** (AND, OR, NOT) para inferir la actividad final.

> 👉 En resumen:  
> Imagen → Detección de primitivos → Razonamiento simbólico → Actividad reconocida + explicación.

## Arquitectura general

HAKE propone una **arquitectura neuro-simbólica en dos etapas**:

### (1) Detección de primitivos

- Usa CNNs (como Faster R-CNN o Swin Transformer) para detectar **partes del cuerpo** (cabeza, brazos, manos, piernas…) y **objetos**.
    
- Cada parte obtiene una serie de **estados semánticos (PaSta, Part States)** como:
    
    - `hand-hold-sth`, `head-eat-sth`, `leg-bend`, `foot-stand-on-sth`, etc.
        
- Estos “primitivos” representan **acciones atómicas transferibles** entre actividades.
    

HAKE construye una **base de conocimiento gigante** con:

- 357 000 imágenes / frames.
    
- 26 millones de etiquetas primitivas.
    
- 156 clases de actividades (combinaciones de verbos y objetos).
    

Cada primitivo se etiqueta manualmente y con crowdsourcing.


### (2) Razonamiento simbólico

- Los primitivos detectados se **programan mediante reglas lógicas**.
    
- Ejemplo de regla:
    
    `(hand-hold-sth) ∧ (head-eat-sth) ∧ (object=apple) → activity=eat apple`
    
- Usa dos módulos MLP diferenciables para implementar las operaciones:
    
    - **NOT(x)** → negación.
        
    - **OR(x, y)** → disyunción.
        
- Estas operaciones se entrenan con **regularización lógica** para cumplir leyes como:
    
    - ¬¬x = x
        
    - x ∨ x = x
        
    - x ∨ ¬x = True
        
- Se calculan **pérdidas por consistencia lógica** + **pérdidas de clasificación**.
    

> Resultado: un **modelo diferenciable** que razona simbólicamente pero entrena con backpropagation.


## Módulos clave

### 🔹 a) Activity2Vec

Transforma cada primitivo a un vector semántico combinando:

- Representación visual (de CNN).
    
- Representación lingüística (usando BERT).
    

Permite razonar con una representación multimodal que mezcla visión + lenguaje.


### 🔹 b) Motor de reglas (Inductivo–Deductivo)

- Empieza con reglas simbólicas básicas escritas por humanos (“human prior”).
    
- Luego **descubre nuevas reglas automáticamente**:
    
    - **Inducción:** genera reglas candidatas observando co-ocurrencias entre primitivos y actividades.
        
    - **Deducción:** evalúa esas reglas según la pérdida durante el entrenamiento y conserva las más efectivas.
        
- Este proceso iterativo produce una **base de reglas dinámica y autoajustable** (≈ 4 000 reglas finales).
    


### 🔹 c) Integración con modelos neuronales existentes

HAKE no reemplaza a las CNN/Transformers, sino que se **acopla encima**:

- Puede mejorar modelos previos como TIN, QPIC o SlowFast.
    
- Fusiona resultados neuronales (`S_ins`) y simbólicos (`S_LR`) como:  
    `S_total = S_ins × S_LR`
    



## Resultados experimentales

### Datasets

- **HICO-DET** → imágenes de interacciones humano-objeto.
    
- **V-COCO** → detección de acciones sobre objetos (basado en COCO).
    
- **AVA** → actividades en vídeo.
    
- **Ambiguous-HOI** → escenas ambiguas para poner a prueba el razonamiento.
    



### Rendimiento

|Dataset|Baseline|+ HAKE|Mejora relativa|
|---|---|---|---|
|HICO-DET|TIN: 17.0 mAP|23.2 mAP|+36 %|
|HICO-DET|VCL: 23.6 mAP|28.3 mAP|+20 %|
|AVA|SlowFast: 28.2 mAP|29.3 mAP|+4 %|
|V-COCO|TIN: 54.2 mAP|59.7 mAP|+10 %|

Con **primitivos perfectos (GT)** llega a **~72 mAP**, lo que marca el _upper bound_ del sistema.

Además, demuestra **gran capacidad de transferencia** entre datasets sin reentrenar, gracias a su representación simbólica general.



## Insights teóricos y prácticos

1. **Los primitivos son la clave de la composicionalidad.**  
    → Permiten que el modelo generalice a combinaciones nuevas (_wash horse_ tras ver _wash car_ y _ride horse_).
    
2. **El razonamiento lógico aumenta la explicabilidad.**  
    → Cada decisión puede trazarse mediante reglas (no una caja negra).
    
3. **El aprendizaje inductivo-deductivo descubre conocimiento.**  
    → El modelo puede aprender reglas nuevas (“si hay ski\_board y leg-bend, probablemente es _ride ski_board_”).
    
4. **Es un sistema diferenciable y escalable.**  
    → Integra lógica y deep learning sin sacrificar el entrenamiento end-to-end.
    



## Limitaciones y futuro

- Dependencia de anotaciones costosas (26 M primitivos).
    
- El módulo lógico aún es simplificado (solo usa ¬, ∨).
    
- Puede fallar en razonamientos multi-paso o en escenas sin partes corporales claras.
    
- Futuro: integrar **Knowledge Graphs** externos (commonsense), **ontologías [[OWL]]** y **razonamiento simbólico más completo (FOL o PSL)**.
    



## Conexión directa con el [[102 - no-TFM]]

|Objetivo de tu TFM|Relación con HAKE|
|---|---|
|**Reconocimiento semántico de escenas**|HAKE interpreta interacciones complejas (humano-objeto) mediante representación simbólica.|
|**Integración DL + KG**|Su motor de reglas es un primer paso hacia razonamiento sobre un grafo de conocimiento.|
|**Explicabilidad simbólica**|Explica cada predicción con lógica humana (`A ∧ B → C`).|
|**Evaluación de generalización sistemática**|Puede probarse con los _splits_ HICO-DET-SG y V-COCO-SG.|
|**Modelo neuro-simbólico**|Combina CNNs/Transformers (percepción) con razonamiento lógico (símbolo).|



## En resumen

> **HAKE transforma la comprensión de actividades humanas en una tarea neuro-simbólica jerárquica:**  
> detecta **primitivos visuales**, los **combina mediante reglas lógicas**, y obtiene **actividades explicables y generalizables**.

**Contribuye a el [[102 - no-TFM]]** como ejemplo de:

- Integración explícita de conocimiento simbólico en visión.
    
- Arquitectura diferenciable DL + lógica.
    
- Modelo ideal para extender con **Knowledge Graphs espaciales/funcionales** en _scene understanding_.


---
tags:
	#Writing #Summary