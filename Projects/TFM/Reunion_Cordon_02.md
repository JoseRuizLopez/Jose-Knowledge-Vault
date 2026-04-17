links: [[103 - TFM]]

*Jose Ruiz (alumno), Oscar Cordon (tutor) e Isaac (Cotutor)*
# Reunión Cordón 01

### 1. Objetivos Inmediatos (Próximos Pasos)
La prioridad actual es la toma de contacto con el entorno técnico y la replicación de resultados previos.

- **Entorno de desarrollo:** Descargar el repositorio del [[Reformulating_Infuence_Maximization_throughDescriptive_Clustering.pdf|paper principal]], verificar su funcionamiento y realizar ingeniería inversa para comprender la lógica interna.
    
- **Gestión de repositorios:** Una vez validado el primer repositorio, Isaac proporcionará acceso a recursos adicionales.
    
- **Configuración de Solvers:** Investigar la herramienta **PuLP**, y como se podria introducir debilmente en el paper de [[Reformulating_Infuence_Maximization_throughDescriptive_Clustering.pdf|Motaz Ben]]. Para la resolución de modelos, se plantean tres alternativas de acceso a solvers potentes:
    
    1. **Gurobi:** Utilizar la licencia académica gratuita.
        
    2. **Servidor dedicado:** Uso de un servidor proporcionado por el cotutor.
        
    3. **CPLEX:** Alternativa de optimización disponible.
        

### 2. Estructura y Metodología del Trabajo

Se ha definido una estrategia para la narrativa del TFM (o de un posible artículo derivado), dividida en dos fases principales:

**Fase 1: Justificación y Limitaciones (Estudio de Escalabilidad)**

- **Objetivo:** Demostrar el punto de ruptura de los métodos actuales.
    
- **Acción:** Realizar pruebas de rendimiento utilizando instancias de tamaño creciente.
    
- **Resultado esperado:** Probar que, incluso utilizando los mejores solvers comerciales, el problema se vuelve inabordable (no termina en un tiempo razonable) ante grandes volúmenes de datos. Esto servirá como justificación crítica para la necesidad de la Fase 2.
    

**Fase 2: Propuesta de Soluciones**

- Desarrollo y exposición de las soluciones propuestas que superen las limitaciones de escalabilidad identificadas en la fase anterior.
    

---

### 3. Recursos y Referencias

Se han facilitado los siguientes materiales de apoyo:

- **Escritura colaborativa:** Invitación al proyecto en **Overleaf** (revisar correo/enlace compartido).
    
- **Bibliografía base:** * _Libro de referencia:_ [[CMSA - Libro.pdf|Springer - Optimización/Modelado]]. El **Capítulo 1** es fundamental para el marco teórico. Archivo disponible en el Google Drive compartido.
    
- **Documentación Técnica:**
    
    - Librería de optimización: [Documentación de PuLP](https://coin-or.github.io/pulp/).
        
- **Código de referencia:**
    
    - [Repositorio GitHub - Alpha-HCIM.py](https://github.com/2x254/IPMU_RegularPaper/blob/main/Alpha-HCIM.py): Script específico para replicar el modelo _Alpha-HCIM_, del [[Reformulating_Infuence_Maximization_throughDescriptive_Clustering.pdf|Paper de Motaz Ben Hassine]]
        

---

### 🚀 Tareas Pendientes (To-Do List)

- [ ] Clonar el repo de GitHub y ejecutar el script `Alpha-HCIM.py`. Probar a tocatear el paper.
    
- [ ] Solicitar/Configurar licencia de estudiante en Gurobi o acceso al servidor de Isaac.
    
- [ ] Leer el Capítulo 1 del libro en el Drive.
    
- [x] Acceder al Overleaf y revisar la estructura del documento.
    
- [ ] Diseñar (y empezar a probar) el plan de pruebas para el estudio de escalabilidad.