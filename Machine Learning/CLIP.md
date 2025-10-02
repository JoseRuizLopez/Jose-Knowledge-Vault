# CLIP (Contrastive Language–Image Pre-training)  
Es un modelo desarrollado por **OpenAI (2021)** que **aprende a relacionar imágenes con texto**.  
Su objetivo es lograr que el modelo “entienda” lo que ve, no solo como píxeles, sino **en términos de lenguaje natural**.

📘 Nombre completo:
> “Learning Transferable Visual Models From Natural Language Supervision”  
> _Alec Radford et al., 2021 (OpenAI)_
---
## Cómo funciona
CLIP **entrena dos redes al mismo tiempo**:

|Parte|Entrada|Objetivo|
|---|---|---|
|🧠 **Visual Encoder**|Imagen|Convierte la imagen en un vector (embedding)|
|🗣️ **Text Encoder**|Texto|Convierte la descripción en otro vector|
Durante el entrenamiento, CLIP **aprende a acercar en el espacio vectorial** las imágenes y los textos que se corresponden, y a **alejar** los que no.

💡 Ejemplo:
- Imagen: 🖼️ una foto de un perro.
- Textos: “a photo of a dog”, “a photo of a cat”, “a car in motion”.

CLIP aprende que la imagen y el texto “photo of a dog” deben tener **embeddings cercanos**, y los otros deben estar lejos. 
Este proceso se llama **contrastive learning** (aprendizaje contrastivo).

---
## Qué representa CLIP
CLIP **aprende una representación multimodal** (imagen + texto).  
Eso significa que puede:
- “Entender” qué hay en una imagen sin etiquetas específicas.
- Ser usado para **zero-shot classification** (clasificar sin entrenamiento adicional).
- Conectar **conceptos lingüísticos** con **elementos visuales**.

Por ejemplo:  
Si le das una imagen de un gato y los textos “un perro”, “un gato”, “una silla”,  
CLIP predice que el texto más cercano es “un gato”.

---
## Por qué es útil para tu TFM
Mi [[TFM]] busca **reconocimiento semántico de escenas**, donde los objetos y relaciones tienen **significado simbólico**.

CLIP es una **excelente capa de percepción** para esto, porque:
1. **Ya asocia texto ↔ imagen**, así que puedes mapear fácilmente los objetos detectados con **conceptos simbólicos** del KG.
2. Puedes usar sus **embeddings** para crear **alineaciones semánticas automáticas** entre:
    
    - Labels neuronales (“car”, “person”, “dog”)
        
    - Conceptos simbólicos (“Automóvil”, “Persona”, “Perro”)
        
3. Permite generar **descripciones textuales explicativas** de una escena (“A person driving a car near a traffic light”).

💡 Así, CLIP puede ser el **puente natural** entre la parte neuronal y la simbólica.

---
## 🧱 Estructura técnica (simplificada)

Imagen  ──► Visual Encoder  ─┐                                           
						├──► Espacio conjunto (embedding)
 Texto ───► Text Encoder   ───┘

🔹 Si las representaciones son similares → la imagen y el texto coinciden.  
🔹 Si son diferentes → no coinciden.

---
## 🧠 En resumen

|Aspecto|CLIP|
|---|---|
|**Acrónimo**|Contrastive Language–Image Pre-training|
|**Desarrollado por**|OpenAI (2021)|
|**Qué hace**|Alinea imágenes y texto en un mismo espacio semántico|
|**Entrenamiento**|400 millones de pares imagen-texto|
|**Utilidad**|Clasificación zero-shot, embeddings semánticos, transferencia|
|**Por qué te sirve**|Puente entre visión profunda (DL) y conocimiento simbólico (KG)|

---
## 💡 Ejemplo aplicado a tu TFM
Por ejemplo, CLIP detecta la relación:

> Imagen → “a man driving a car near a traffic light”

Se puede extraer tripletas semánticas:
`(Persona, conduce, Automóvil)`
`(Automóvil, cerca_de, Semáforo)`

Y luego alinear eso con el **grafo simbólico**, que sabrá si:
- Es coherente con las reglas del dominio.
- Faltan relaciones.
- Se puede explicar (“es una calle urbana porque hay coche + semáforo + peatón”).
