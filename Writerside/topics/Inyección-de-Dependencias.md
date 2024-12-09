# Inyección de Dependencias

## ¿Qué es la Inversión de Control (IoC)?
La inversión de control es un principio de software, en el que el control de nuestros objetos es transferido a un contenedor o framework. Delegamos, por lo general, en el framework para que tome el flujo del programa. Este sistema tiene una serie de ventajas tales como:

+ Desacopla el código
+ Mayor facilidad a la hora de testear debido a que se pueden probar partes aisladas
+ Tiene una gran modularidad

Este sistema se logra implementando diferentes patrones de Software como pueden ser:

+ patrón estrategia (Strategy Pattern)
+ patrón Factoría (Factory pattern)
+ patrón Servicio Localizador (Service Locator Pattern)
+ Inyección de Dependencias (DI)
+ Inyección de Dependencias (DI)

La inyección de dependencias es un patrón de Software, que implementa la inversión de control para resolver dependencias; a partir de la cual los objetos definen sus dependencias con otros objetos.

Todas estas dependencias se encuentran en un contenedor, que será el responsable de inyectarlas y crear los bean necesarios, por un proceso que se llama Inversión de Control (IoC).

Podríamos definir y resumir este concepto como que la Inyección de Dependencias es el proceso de suministrar una dependencia externa a un componente de software.

## El contenedor IoC en Spring
En Spring el contenedor IoC esta representado por la interfaz ApplicationContext, la cual es la responsable de configurarn e instanciar todos los objetos (los beans) y manejar su ciclo de vida.

La Inyección de Dependencia en Spring puede ser hecha a través de campos, setters o constructores.

Inyección de Dependencias basada en Constructor
En el caso de la Inyección de Dependencias basada en constructor, el contenedor invocará a un constructor con argumentos en la que cada una representa a una dependencia.

Vamos a ver la configuración de un bean y sus dependencias mediante anotaciones de Spring:

```java
@Configuration
Public class AppConfig {
@Bean
public Book book() {
return new Book();
}
```

La primera anotación que vemos @Configuration es necesaria porque nos dice que es una clase de definición de beans.

La siguiente anotación que vemos @Bean, indica que vamos a definir un bean, por defecto Spring crea el bean como Singleton Scope,

En Spring también se puede crear un bean a través de configuración XML, aunque no suele ser lo más común:

```xml
<bean id="book" class="org.refactorizando.library.BookImpl" /> 
<bean id="library" class="org.refactorizando.library.Library"> <constructor-arg type="BookImpl" index="0" name="book" ref="book" /></bean>
```

## Inyección de Dependencias basada en Setter
La Inyección basada en Setter es parecida a la Inyección basada en constructor con la única diferencia que una vez se crea el objeto se hace un set con los atributos del mismo.

Vamos a ver un ejemplo con anotaciones:

```java
@Configuration
Public class AppConfig {
@Bean
public Library library() {
Library library =  new Library();
library.setBook(book());
return library;
}
```
Al igual que en el caso anterior podemos hacer la Inyección de Dependenias a través del XML:

```xml
<bean id="library" class="org.refactorizando.library.Library">
  <property name="book" ref="book"/>
</bean>
```

Spring recomienda por lo general hacer uso de la inyección por constructor para aquellas dependencias que son obligatorias y basado en setter para la inyección de aquellas dependencias que son opcionales.

## Inyección de Dependencias basada en campos

En el caso de la Inyección de Dependencias (DI) para campos, podemos realizar añadiendo la anotación @Autowired en el campo de la clase de la que queremos su dependencia:

```java
public class Library {
@Autowired
private Book book;
}
```

En este caso se usará reflexión para inyectar el «book» in la «library».

Sin lugar a dudas este es un sistema sencillo, práctico y bastante limpio, pero no siempre debería ser usado.

Al tener que utilizar reflexión para realizar la inyección de dependencia es mucho más costoso que hacerlo a través de constructor o por setter.
Se puede llegar a violar el Principio de Responsabilidad única, ya que es realmente sencillo añadir dependencias, y si caemos en el error de añadir muchas, va a parecer que esa clase pueda estar haciendo más funciones de las que debiera.
Autowiring dependencias
Hacer uso del autowiring nos permite resolver dependencias de manera automática entre los bean de colaboración inspeccionando los bean que se han definido.

Hay cuatro modos diferentes de autowiring de un bean:

+ no: valor por defecto, no se realizará autowiring y habrá que decir de manera explicita el nombre de la dependencia.

+ byName: se realizará autowiring en función del nombre de la propiedad, es decir, Spring buscará un bean con el mismo nombre de la propiedad que quiere setear.

+ byType: es como el anterior solo que se basa en el tipo de la propiedad. Es decir, la única diferencia con el anterior es que Spring buscará un bean con el mismo tipo de la propiedad.

+ constructor: el autowiring se realizará por constructor y Spring buscará beans con el mismo tipo de los argumentos del constructor.

## Bibliografía

[https://martinfowler.com/bliki/InversionOfControl.html](https://martinfowler.com/bliki/InversionOfControl.html)