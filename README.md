# Post-contenido — Unidad 1: Fundamentos de Patrones de Diseño y Buenas Prácticas

## Descripción
Repositorio del post-contenido de la Unidad 1 de Patrones de Diseño
de Software — Sexto Semestre. Contiene dos partes: refactorización
SOLID de un God Object (parte-1-refactorizacion-solid/) y análisis
de patrones GoF en Spring Framework (parte-2-analisis-gof-spring/).

## Parte 1 — Refactorización SOLID
Proyecto Maven que refactoriza OrderProcessor aplicando SRP, OCP y
DIP. Ver parte-1-refactorizacion-solid/.

## Análisis de Violaciones SOLID

| Principio | Método/Sección afectada | Descripción de la violación |
|-----------|-------------------------|-----------------------------|
| SRP       | calculateTotal + applyDiscount + saveOrder + sendEmail + printReport | La clase OrderProcessor contiene múltiples responsabilidades y, a su vez, diferentes razones independientes para cambiar, debido a que concentra operaciones de cálculo, persistencia, notificación y generación de reportes |
| OCP       | applyDiscount (if/else sobre customerType) | El método applyDiscount() utiliza una estructura if/else basada en customerType, por lo que debe modificarse cada vez que se incorpora un nuevo tipo de cliente o una nueva regla de descuento, dificultando la extensión del comportamiento sin modificar el código existente |
| DIP       | Toda la clase (dependencias internas sin abstracciones) | a clase OrderProcessor depende directamente de implementaciones concretas y carece de abstracciones que permitan desacoplar las dependencias de alto nivel de los componentes de bajo nivel, dificultando el intercambio de implementaciones |

## Parte 2 — Análisis de Patrones GoF en Spring
| # | Patrón | Categoría | Clase en Spring |
|---|--------|-----------|-----------------|
| 1 | [Factory Method] | Creacional | [org.springframework.boot.jdbc.DataSourceBuilder] |
| 2 | [Decorator] | Estructural | [org.springframework.http.server.reactive.ServerHttpRequestDecorator] |
| 3 | [Strategy] | Comportamiento | [org.springframework.web.reactive.accept.ApiVersionResolver] |

Ver parte-2-analisis-gof-spring/documento-analisis.md.

## Herramientas utilizadas
- Java 17, Apache Maven, VS Code, Git, GitHub
- Código fuente de Spring Framework (investigación)

## Conclusiones
[Párrafo de 3-5 oraciones con los aprendizajes más relevantes de ambas partes]