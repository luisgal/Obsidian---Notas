---
title: Java - Patrón Factory Method

tags:  
- Java/DesignPattern
- Java/CreationalPattern
- DesignPattern
aliases:
- Java/CreationalPattern/FactoryMethod
---

# Factory Method

## Propósito

***Factory Method*** es un patrón de diseño creacional que proporciona una interfaz para crear objetos en una superclase, mientras permite a las subclases alterar el tipo de objetos que se crearán.

![Engelbart | center | 400](https://refactoring.guru/images/patterns/content/factory-method/factory-method-es.png)

## Problema

Imagina que estás creando una aplicación de gestión logística. La primera versión de tu aplicación sólo es capaz de manejar el transporte en camión, por lo que la mayor parte de tu código se encuentra dentro de la clase `Camión`.

Al cabo de un tiempo, tu aplicación se vuelve bastante popular. Cada día recibes decenas de peticiones de empresas de transporte para que incorpores la logística por mar a la aplicación. 

Añadir una nueva clase al programa no es tan sencillo si el resto del código ya esta acoplado a clases existentes, en este caso la mayor parte del código está acoplado a la clase `Camión`. Para añadir  barcos a la aplicación habría que hacer cambios en toda la base del código. Además, si más tarde decides añadir otro tipo de transporte a la aplicación probablemente tendrás que volver a hacer todos estos cambios. 

Al final acabarás con un código bastante sucio, plagado de condicionales que cambian el comportamiento de la aplicación dependiendo de la clase de los objetos de transporte.

## Solución

El patrón *Factory Method* sugiere que, en lugar de llamar al operador `new`para construir objetos directamente, se invoque a un método *fábrica* especial. No te preocupes: los objetos se siguen creando a través del `new`, pero se invocan desde el método fábrica. Los objetos devueltos por el método fábrica a menudo se denomina `productos`.

![Embelgart | center | 400](https://refactoring.guru/images/patterns/diagrams/factory-method/solution1.png)

A simple vista parece que este cambio no tiene sentido, ya que tan solo hemos cambiado el lugar desde invocamos al constructor. Sin embargo, piensa que ahora puedes sobrescribir el método fábrica en una subclase y cambiar la clase de los productos creados por el método.

No obstante, hay una pequeña limitación: las subclases sólo pueden devolver productos de distintos tipos se dichos producto tienen una clase base o interfaz común. Además, el método fábrica en la clase base de tener su tipo de retorno declarado como dicha interfaz.

![Embelgart | center | 400](https://refactoring.guru/images/patterns/diagrams/factory-method/solution2-es.png)

Por ejemplo, tanto la clase `Camión` como la clase `Barco` deben implementar la interfaz `Transporte`, que declara un método llamado `entrega`. Cada clase implementa este método de forma diferente: los camiones entregan su carga por tierra, mientras que los barcos lo hacen por mar. El método fábrica dentro de la clase `LogisticaTerrestre` devuelve objetos de tipo camión, mientras que el método fábrica de la clase `LogisticaMaritima` devuelve barcos.

![Embelgart | center | 400](https://refactoring.guru/images/patterns/diagrams/factory-method/solution3-es.png)

El código que utiliza el método fábrica (a menudo denominado código cliente) no encuentra diferencias entre los productos devueltos por varias subclases y trata a todos los productos como la clase abstracta `Transporte`. El cliente sabe que todos los objetos de transporte deben tener el método `entrega`, pero no necesita saber cómo funciona exactamente.

## Estructura

![Embelgart | center | 400](https://refactoring.guru/images/patterns/diagrams/factory-method/structure-indexed.png)

1. El **Product** declara la interfaz, que es común a todos los objetos que pueden producir la clase creadora y subclases.
2. Las **Concrete Products** son distintas implementaciones de la interfaz **Product**
3. La clase **Creadora** declara el método fábrica que devuelve nuevos objetos de producto. Es importante que el tipo de retorno de este método coincida con la interfaz de producto.

	Puedes declarar el patrón Factory Method como abstracto para forzar a todas las subclases a implementar sus propias versiones del método. Como alternativa, el método fábrica base puede devolver algún tipo de producto por defecto.

	Observa que a pesar de su nombre la creación de producto no es la principal responsabilidad de la clase creadora. Normalmente, esta clase cuenta con alguna lógica de negocios central relacionada con los productos. El patrón Factory Method ayuda a desacoplar esta lógica de las clases concreta de producto. Aquí tienes una analogía: una gran empresa de desarrollo de software puede contar con un departamento de formación de programadores, sin embargo, la principal función de la empresa sigue siendo escribir código, no preparar programadores. 
4. Los **Concrete Creators** sobrescriben el Factory Method base, de modo que devuelva un tipo diferente de producto.

### Pseudocódigo
```java
// La clase creadora declara el método fábrica que debe devolver
// un objeto de una clase de producto. Normalmente, las
// subclases de la creadora proporcionan la implementación de
// este método.
class Creator is
    // La creadora también puede proporcionar cierta
    // implementación por defecto del método fábrica.
    abstract method createProduct():Product

    // Observa que, a pesar de su nombre, la principal
    // responsabilidad de la creadora no es crear productos.
    // Normalmente contiene cierta lógica de negocio que depende
    // de los objetos de producto devueltos por el método
    // fábrica. Las subclases pueden cambiar indirectamente esa
    // lógica de negocio sobrescribiendo el método fábrica y
    // devolviendo desde él un tipo diferente de producto.
    method someOperation() is
        // Invoca el método fábrica para crear un objeto de
        // producto.
        Product p = createProduct()
        // Ahora utiliza el producto.
        p.doSomething()

// Los creadores concretos sobrescriben el método fábrica para
// cambiar el tipo de producto resultante.
class ConcreteCreatorA extends Creator is
    method createProduct():Product is
        return new ConcreteProductA()

class ConcreteCreatorB extends Creator is
    method createProduct():Product is
        return new ConcreteProductB()

// La interfaz de producto declara las operaciones que todos los
// productos concretos deben implementar.
interface Product is
    method doSomething()

// Los productos concretos proporcionan varias implementaciones
// de la interfaz de producto.

class ConcreteProductA implements Product is
    method doSomething() is
	    print("Concrete Product A")

class ConcreteProductB implements Product is
	method doSomething() is
	    print("Concrete Product B")

class Main is
    field p: Product

    // La aplicación elige un tipo de creador dependiendo de la
    // configuración actual o los ajustes del entorno.
    method initialize( ProductType ) is
        if (ProductType == "A") then
            p = new ConcreteProductA()
        else if (ProductType == "B") then
            p = new ConcreteProductB()
        else
            throw new Exception("Error!")

    // El código cliente funciona con una instancia de un
    // creador concreto, aunque a través de su interfaz base.
    // Siempre y cuando el cliente siga funcionando con el
    // creador a través de la interfaz base, puedes pasarle
    // cualquier subclase del creador.
    method main() is
        this.initialize("A")
        p.doSomething()  // Salida: "Concrete Product A"
```

## Aplicabilidad

**Utiliza Factory Method cuando no conozcas de antemano las dependencias y los tipos exactos de los objetos con los que deba funcionar el código.**

> El patrón Factory Metho separa el código de construcción de producto del código que hace uso del producto. Por ello, es más fácil extender el código de construcción de producto de forma independiente al resto del código. 
> 
> Por ejemplo, para añadir un nuevo tipo de producto a la aplicación, sólo tendrás que crear una nueva subclase creadora y sobrescribir el Factory Method que contiene.

**Utiliza el Factory Method cuando quieras ofrecer a los usuario de tu biblioteca o framework, una forma de extender sus componentes internos.**

> La herencia es probablemente la forma más sencilla de extender el comportamiento por defecto de una biblioteca o framework. Pero, ¡cómo reconoce el framework si debe utilizar tu subclase un lugar de un componente estándar?.
> 
> La solución es reducir el código que contruye componentes en todo el framework a un único patrón Factory Method y permitir que cualquiera sobrescriba este método además de extender el propio componente.

**Utiliza el Factory Method cuando quieras ahorrar recursos del sistema mediante la reutilización de objetos existentes en lugar de reconstruirlos cada vez.**

> A menudo experimentas esta necesidad cuando trabajas con objetos grandes y consumen muchos recursos, como conexiones a bases de datos, sistemas de archivos y recursos de red.
> 
> Pensemos en lo que hay que hacer para reutilizar un objeto existente:
> 
>  1. Primero debemos crear un almacenamiento para llevar un registro de todos los objetos creados.
>  2. Cuando alguien necesite un objeto, el programa deberá buscar un objeto disponible dentro de ese agrupamiento y devolverlo al código cliente.
>  3. Si no hay objetos disponibles, el programa deberá crear uno nuevo (y añadirlo al agrupamiento).
>  
> Es probable que el lugar más evidente y cómodo para colocar este código sea el constructor de la clase cuyo objetos intentamos reutilizar. Sin embargo, un constructor debe devolver nuevos objetos por definición. No puede devolver instancias existentes.
> 
> Por lo tanto, necesitas un método capaz de crear nuevos objetos, además de reutilizar los existentes. Eso suena bastante a lo que hace un patrón Factory Method.

## Cómo implementarlo

1. Haz que todos los productos sigan la misma interfaz. Esta interfaz deberá declarar métodos que tengan sentido en todos los productos.
2. Añade un patrón Factory Method vacío dentro de la clase creadora. El tipo de retorno del método deberá coincidir con la interfaz común de los productos.
3. Encuentra todas las referencias a constructores de producto en el código de la clase creadora. Una a una, sustitúyelas por invocaciones al Factory Method, mientras extraes el código de creación de productos para colocarlo dentro del Factory Method.
	
	Puede ser que tengas que añadir un parámetro temporal al Factory Method para controlar el tipo de producto devuelto.
    
    A estas alturas, es posible que el aspecto del código del Factory Method luzca bastante desagradable. Puede ser que tenga un operador `switch` largo que elige qué clase de producto instanciar. Pero, no te preocupes, lo arreglaremos enseguida.
    
4. Ahora, crea un grupo de subclases creadoras para cada tipo de producto enumerado en el Factory Method. Sobrescribe el Factory Method en las subclases y extrae las partes adecuadas de código constructor del método base.
5. Si hay demasiados tipos de producto y no tiene sentido crear subclases para todos ellos, puedes reutilizar el parámetro de control de la clase base en las subclases.

	Por ejemplo, imagina que tienes la siguiente jerarquía de clases: la clase base `Correo` con las subclases `CorreoAéreo` y `CorreoTerrestre` y la clase `Transporte` con `Avión`, `Camión` y `Tren`. La clase `CorreoAéreo` sólo utiliza objetos `Avión`, pero `CorreoTerrestre` puede funcionar tanto con objetos `Camión`, como con objetos `Tren`. Puedes crear una nueva subclase (digamos, `CorreoFerroviario`) que gestione ambos casos, pero hay otra opción. El código cliente puede pasar un argumento al Factory Method de la clase `CorreoTerrestre` para controlar el producto que quiere recibir.
	
6. Si, tras todas las extracciones, el Factory Method base queda vacío, puedes hacerlo abstracto. Si queda algo dentro, puedes convertirlo en un comportamiento por defecto del método.

## Pros y contras

> [!success] Pro
> Evitas un acoplamiento fuerte entre el creador y los productos concretos.

> [!success] Pro
> *Principio de responsabilidad única*. Puedes mover el código de creación de producto a un lugar del programa, haciendo que el código sea más fácil de mantener.

> [!success] Pro
> *Principio de abierto/cerrado*. Puedes incorporar nuevos tipos de productos en el programa sin descomponer el código cliente existente

>[!fail] Contra
>Puede ser que el código se complique, ya que debes incorporar una multitud de nuevas subclases para implementar el patrón. La situación ideal sería introducir el patrón en una jerarquía existente de clases creadoras.