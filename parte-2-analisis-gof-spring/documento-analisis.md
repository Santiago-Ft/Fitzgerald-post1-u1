DOCUMENTO DE ANALISIS DE PATRONES GOF EN SPRING BOOT 

UNIDAD 2 

  

 

PARTICIPANTES 

SANTIAGO ANDRES FITZGERALD VILLAMIZAR 02240131009 

  

  

UNIERSIDAD DE SANTANDER 

CÚCUTA, NORTE DE SANTANDER 

 16 DE AGOSTO DE 2026 

 

  

ASIGNATURA PATRONES DE DISEÑO DE SOFTWARE 

PROGRAMA DE INGENIERIA DE SISTEMAS 

SEXTO SEMESTRE 

 

 

 

 

 

 

INTRODUCCION 

 

El desarrollo de aplicaciones empresariales en Java ha experimentado una evolución constante en contextos relacionados con la eficiencia y robustez, impulsada por la necesidad de crear sistemas escalabres y mantenibles. Spring Framework se ha consolidado como la estructura fundamental para múltiples contextos y escenarios, proporcionando herramientas adicionales del lenguaje base (Java). Pero debido a su alta complejidad manual se llevó a la creación de Spring Booy, tecnología que simplifica el despliegue de aplicaciones mediante configuración automática y un modelo basado en convenciones establecidas para las dependencias. 

El propósito del documento es realizar un análisis de los patrones de diseño GoF, establecidos en el proyecto Spring Framework y su código fuente se encuentra almacenado en GitHub. El repositorio (spring-projects/spring-framework) se denomina como una “clase maestra” dentro de la ingeniería de software, también es la base de proyectos relacionados con Srping y uno de los proyectos más extensos. El Framework se caracteriza por ser mantenible y flexible esto se debe a la utilización y aplicación de principios SOLID, reglas diseñadas para que el código sea limpio, organizado y reducir los fallos durante modificaciones, pero para esto se usan los patrones GoF que cumplen los objetivos del proyecto mencionados anteriormente. 

En este documento se evidenciará como Spring Boot logra ocultar la complejidad de la infraestructura mediante patrones de diseños, basándose en los 23 modelos de diseño clásico. Además de las ventajas que ofrecen los patrones de diseño dentro del proyecto y su relevancia en el contexto de desarrollo moderno. 

 

 

 

 

 

 

 

 

 

 

 

ANALISIS DEL PATRON FACTORY METHOD 

 

También conocido como Método Fabrica o Constructor Virtual, pertenece a la categoría de creacional dentro las subdivisiones de los patrones GoF (Shvets, 2014-2026). Su principio es proporcionar una interfaz para la creación de objetos en una superclase, permitiendo que sean las subclases las que decidan qué tipo de objetos específicos se deban instanciar. Se centran en desacoplar la lógica de construcción de los productos del código que realmente los utiliza (Shvets, 2014-2026). 

El patrón dentro de la arquitectura de Spring Framework se representa como un buen ejemplo y comprobante de la aplicación de sus principios las cuales se representan en el manejo de beans y la infraestructura de acceso a datos (Broadcom, 2026). Su presencia reside en el módulo de spirng-boot-jdbc y en BeanFactory del módulo spring-beans del core del framework (Broadcom, 2026).  Donde el patrón se encarga de gestionar la creación de componentes (conexiones a bases de datos y demás) de forma flexible y extensible sin conocer de antemano el tipo exacto de objeto con el que trabaja el desarrollador, utilizando una de las ventajas del patrón la abstracción (dividir las clases en subclases para crear objetos con diferentes comportamientos) (Broadcom, 2026). Un ejemplo para analizar su importancia dentro de Spring Framework es: si Spring Boot utilizara el operador “new” directamente para crear un pool de conexiones como HikariCP, el código estaría fuertemente acoplado a esa implementación especifica. Si un usuario quisiera cambiar a otro proveedor como DBCP2, el Framework tendría que ser modificado de manera interna (Shvets, 2014-2026). La solución que proporciona Factory Method permite que Spring detecte que librerías están presentes en el “classpath”, mediante un método de fábrica, devuelva la instancia correcta del “DataSource” solicitando y tratándolo siempre bajo una interfaz común (libro). Evitando “ensuciar” el código con múltiples condiciones y permitiendo una configuración automática eficiente. 

La ejemplificación de lo anteriormente mencionada de la implementación del patrón Factory Method se observa en la clase “DataSourcerBuilder”, específicamente en su capacidad de construir productos concretos basados en tipos genéricos:  

 

public T build() { 
   // 1. Se identifica el tipo de producto concreto (Hikari, Tomcat, etc.) [5] 
   Class<? extends DataSource> type = getType(); 
   if (type == null) { 
       type = findType(); // Detecta automáticamente según el classpath [5] 
   } 
    
   // 2. El Factory Method instancia el producto concreto 
   DataSource dataSource = BeanUtils.instantiateClass(type); 
    
   // 3. Se retorna el objeto, permitiendo al cliente usar la interfaz DataSource [14] 
   return (T) dataSource; 
} 

En este fragmento el método build() actúa como el corazón del patrón. No decide de forma rígida que crear, sino delega en el estado de configuración o el entorno para devolver un “Producto” que cumple con la interfaz esperada en el sistema (Broadcom, 2026). 

Adicionalmente de implementar el patrón Factory Method y su solución de crear diferentes productos (objetos) con diferentes comportamientos, también añade y fortalece el proyecto de Spring en base a los principios SOLID. El patrón permite que el Framework sea abierto a la extensión (específicamente en DataSource”) y cerrado para la modificación, queriendo decir que posee una estructura flexible para añadir código nuevo o funciones adicionales sin tener que modificar lo que ya existe, siendo esto fundamental para que sea más fácil la construcción de las nuevas extensiones y no generar errores en lo que ya está implementando (EBIS Business Techschool, 2025). También el patrón crea un nivel de abstracción elevado, el cual hace que no se depende de una clase concreta (como HikariDataSource), y también de separar la lógica de negocio de la aplicación separando sus responsabilidades (métodos) en diferentes clases (EBIS Business Techschool, 2025). 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

ANALISIS DEL PATRON DECORATOR 

 

Conocido también como envoltorio o wrapper, pertenece a la categoría de los patrones estructurales dentro de las subdivisiones GoF, su principio consiste en permitir la adición de funcionalidades a objetos específicos de manera dinámica, colocando dichos objetos dentro de otros objetos encapsuladores que contiene las nuevas capacidades (Shvets, 2014-2026). 

Dentro del proyecto de Spring Framwork, el rol de este patrón es la gestión de flujos reactivos y comunicación web dentro de las clases: “org.springframework.http.server.reactive.ServerHttpRequestDecorator” en el módulo spring-web y “orf.springframework.web.socket.handler.WebSocketHandlerDecorator” dentro de spring-websocket (Broadcom, 2026). Dentro del contexto del proyecto, el patrón resuelve el problema de la rigidez de la herencia estativa, la cual no permite alterar la funcionalidad de un objeto existente durante el tiempo de ejecución ni heredar comportamientos de varias clases simultaneas (Shvets, 2014-2026). Para Spring esto es crítico, un ejemplo es el manejar una solicitud HTTP, el framework a menudo necesita añadir lógica de registro, seguridad o transformación de datos sin modificar la implementación de la solicitud o crear una explosión combinatoria de subclases para cada variante posible (decorator). Ahora al utilizar Decorator, Spring puede encapsular la solicitud original y sobrescribirla selectivamente solo en los métodos necesarios (Broadcom, 2026). 

La evidencia de la implementación del patrón Decorator dentro del repositorio, representa la estructura de la clase “SeverHttpRequestDecorator” que muestra cómo se mantiene la referencia del objeto encapsulado o envuelto para cumplir con la interfaz requerida: 

 

public class ServerHttpRequestDecorator implements ServerHttpRequest { 
   private final ServerHttpRequest delegate; // El objeto original envuelto [9] 
 
   public ServerHttpRequestDecorator(ServerHttpRequest delegate) { 
       this.delegate = delegate; // Se inicializa el componente base [9] 
   } 
 
   @Override 
   public HttpMethod getMethod() { 
       return this.delegate.getMethod(); // Delegación simple al objeto real [13] 
   } 
 
   @Override 
   public Flux<DataBuffer> getBody() { 
       return this.delegate.getBody(); // Las subclases pueden sobrescribir esto para "decorar" el cuerpo [14] 
   } 
} 

 

La estructura permite que cualquier subclase de este decorador pueda, por ejemplo, interceptar el método “getBody()” para descomprimir datos sobre la marcha antes de entregarlos al resto del framework, cumpliendo con la idea de añadir responsabilidades sin romper la interfaz principal o base (Shvets, 2014-2026). 

Por último, el patrón implementado refuerza los principios SOLID dentro del proyecto, permite dividir las diferentes responsabilidades de las clases concretas en clases monolíticas o especializadas en métodos únicos para evitar dependencia y exceso de métodos (EBIS Business Techschool, 2025). También permite extender el comportamiento de los componentes de red sin alterar el código fuente, es decir, genera flexibilidad para añadir extensiones de nuevas funciones al código sin tener que modificar el ya establecido, generando estabilidad al sistema (EBIS Business Techschool, 2025). 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

ANALISIS DEL PATRON STRATEGY 

 

Conocido también como estrategia, es un modelo de diseño de comportamiento que permite definir una familia de algoritmos, encapsular cada uno de ellos en una clase separada y hacer que sus objetos sean intercambiables en tiempos de ejecución (Shvets, 2014-2026). Su principio es permitir que una clase “contexto” delegue un trabajo específico a un objeto de estrategia vinculado, permitiendo alterar el comportamiento del objeto final sin necesidad de modificar el código fuente (Shvets, 2014-2026). 

Dentro del proyecto de Spring Framework el patrón añade flexibilidad al sistema dentro del módulo spring-webflux específicamente en la gestión de versiones de API a través de la interfaz “org.springframework.web.reactive.accept.ApiVersionResolver” (Broadcom, 2026). Spring utiliza este patrón como solución al problema de rigidez en la toma de decisiones lógicas, por ejemplo, una aplicación requiere identificar la versión de una API mediante diferentes métodos, como encabezados HTTP, parámetros de consultar, etiquetas y demás métodos (Broadcom, 2026. En vez de implementar varios operadores condicionales que determinen cual variante es de las tantas existentes, Spring extrae los algoritmos en clases de estrategia independientes, permitiendo que el desarrollador elija de forma personal o combine múltiples estrategias dependiendo del contexto de la aplicación (Broadcom, 2026). 

Como evidencia de la implementación del patrón Strategy dentro del código fuente del proyecto de Spring, se puede observar el framework que permite la configuracion e inyección de estas estrategias de forma dinámica: 

@Override 
public void configureApiVersioning(ApiVersioningConfigurer configurer) { 
   // El cliente define qué estrategia concreta utilizará el contexto 
   configurer.useHeader("X-Version") // Se selecciona la estrategia de encabezado 
             .defaultVersion("1.0.0");  
 
   // Internamente, Spring inyectará un bean de tipo ApiVersionResolver. 
   // El "Contexto" de Spring ejecutará el método de resolución delegando  
   // la lógica al objeto de estrategia inyectado [2, 13]. 
} 

En el fragmento de código se ilustra la implementación del patrón, donde el sistema se vuelve independiente de las estrategias concretas, permitiendo intercambiar el algoritmo de resolución de versiones simplemente cambiando la configuración y como consecuencia se garantiza un código limpio, desacoplado y mantenible (Shvets, 2014-2026). 

Por último, la implementación del patrón refuerza los principios SOLID dentro del proyecto Spring, permitiendo de forma exequible la introducción de nuevas extensiones al código sin alterar el ya establecido y dependencia de abstracciones en uso de los componentes del framework para resolver encabezados específicos (EBIS Business Techschool, 2025). 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

CONCLUSION 

 

La integración de los patrones GoF como lo son Factory Method, Strategy y Decorator implementados en proyectos grandes como Spring Framework son fundamentales, no solo a nivel estético sino en la forma de simplificar las tareas y responsabilidades del código de una forma flexible y sostenible. Además, con el análisis de diferentes patrones se observa que cada uno presenta una solución específica para ciertos problemas, pero llegan al mismo resultado el cual es reducir complejidad y errores en el código. También fortalecen el programa utilizando los principios SOLID, específicamente en dividir responsabilidades (SRP), reducir complejidad mediante abstracciones (DIP) y facilitar el añadimiento de nuevas extensiones (OCP). El uso correcto y adecuado de los patrones conlleva al desarrollo de proyecto grandes como Spring, su mantenimiento y futuras modificaciones. 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

REFERENCIAS 

 

Broadcom. (2026). Reference :: Spring Boot (Versión 4.1.0). https://docs.spring.io/spring-boot/reference/. 

Debrauwer, L. (s. f.). Patrones de diseño en Java: los 23 modelos de diseño: descripciones y soluciones ilustradas en UML 2. Google Libros; Ediciones ENI. 

EBIS Business Techschool. (28 de octubre de 2025). Principios SOLID: Guía Completa (2026). https://www.ebiseducation.com/principios-solid-guia-completa. 

Shvets, A. (2014-2026). The Catalog of Design Patterns. Refactoring.Guru. https://refactoring.guru/design-patterns/catalog. 

Spring Projects. (2026). Spring Framework (Código fuente en Java/Kotlin). GitHub. https://github.com/spring-projects/spring-framework. 

Walls, C. (2015). Spring Boot in Action. Manning Publications 