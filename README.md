# Fitzgerald-post1-u1
Post-contenido — Refactorización SOLID y análisis de patrones GoF en Spring
## Análisis de Violaciones SOLID

| Principio | Método/Sección afectada | Descripción de la violación |
|-----------|-------------------------|-----------------------------|
| SRP       | calculateTotal + applyDiscount + saveOrder + sendEmail + printReport | La clase OrderProcessor.java contiene multiples responsabilidades y a su vez diferentes razones independientes para cambiar, concentrando operaciones de calculo, descuendo, ordenes y correos |
| OCP       | applyDiscount (if/else sobre customerType) | El metodo applyDiscount() contiene una estructura if/else que se ve obligada a modificarse ante un cambio, como lo es agregar un nuevo tipo de cliente requiere modificar el codigo existente |
| DIP       | Toda la clase (dependencias internas sin abstracciones) | La clase OrderProcessor.java depende directamente de implementaciones directas pero carece de abstracciones para facilitar las dependencias de alto nivel con las de bajo nivel  |