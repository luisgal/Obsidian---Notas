---
title: Patrón Decorator
tags:  
- StructuralPattern
- DesignPattern
---

# Decorator
___
___

## Propósito
---

**Decorator** es un patrón de diseño estructural que te permite añadir funcionalidades a objetos colocando estos objetos dentro de objetos encapsulados especiales que contienen estas funcionalidades.

![center | 400](https://refactoring.guru/images/patterns/content/decorator/decorator.png)

<br>
<br>

## Problema
---

Imagina que estás trabajando en una biblioteca de notificaciones que permite a otros programas notificar a sus usuarios acerca de eventos importantes.

La versión inicial de la biblioteca se basaba en la clase `Notificator` que solo contaba con unos cuantos campos, un constructor y un único método `send`. El método podría aceptar un argumento de mensaje de un cliente y enviar el mensaje a una lista de correros electrónicos que se pasaban a la clase notificadora a través de constructor. Una aplicación de un tercero que actuaba como cliente debía creer y configurar el objeto notificador una vez y después utilizarlo cada vez que sucediera algo importante.

![center | 400](https://refactoring.guru/images/patterns/diagrams/decorator/problem1-es.png)

En cierto momento te das cuenta de que los usuarios de la biblioteca esperan algo más que unas simples notificaciones por correo. A muchos de ellos les gustaría recibir mensajes SMS sobre asuntos importantes. Otros querrían recibir las notificaciones por Facebook y, por supuesto, a los usuarios corporativos les encantaría recibir notificaciones Slack.

![center | 400](https://refactoring.guru/images/patterns/diagrams/decorator/problem2.png)

No puede ser muy complicado ¡verdad? Extendiste la clase `Notificador` y metiste los métodos adicionales de notificación dentro de nuevas subclase. Ahora el cliente debería instanciar la clase notificadora deseada y utilizarla para el resto de notificaciones.

Pero entonces alguien te hace una pregunta razonable: "¿Por qué no se pueden utilizar varios tipos de notificación al mismo tiempo? Si tu casa está en llamas, probablemente quieras que te informen a través de todos los canales".

Intentaste solucionar ese problema creando subclases especiales que combinaban varios métodos de notificación dentro de una clase. Sin embargo, enseguida resulto evidente que esta solución inflaría el código en gran medida, no sólo el de la biblioteca, sino también el código cliente

![center | 400](https://refactoring.guru/images/patterns/diagrams/decorator/problem3.png)

Debes encontrar alguna otra forma de estructurar las clases de las notificaciones.

<br>
<br>

## Solución
---

Cuando tenemos que alterar la funcionalidad de un objeto, lo primero que se viene a la mente es extender una clase. No obstante, la herencia tiene varias limitaciones importante de las que debes ser consciente.

* La herencia es estática. No se puede alterar la funcionalidad de un objeto existente durante el tiempo de ejecución. Sólo se puede sustituir el objeto completo por otro creado  a partir de una subclase diferente.
* Las subclases sólo pueden tener una clase padre. En la mayoría de lenguajes, la herencia no permite a una clase heredar comportamientos de varias clases al mismo tiempo.

Una de las formas de superar estas limitaciones es empleando la *Agregación o la Composición* en lugar de la *Herencia*. Ambas alternativas funcionan prácticamente del mismo modo: un objeto tiene una referencia a otro y le delga parte del trabajo, mientras que con la herencia, el propio objeto puede realizar este trabajo, heredando el comportamiento de su superclase.

Con esta nueva solución puedes sustituir fácilmente el objeto "ayudante" vinculado por otro, cambiando el comportamiento del contenedor durante el tiempo de ejecución. Un objeto puede utilizar el comportamiento de varias clases con referencias a varios objetos, delegándolos todo tipo de tareas. la *agregación/composición* es el pincipio clave que se esconde tras muchos patrones de diseño, incluyendo al *Decorator*.

![center | 400](https://refactoring.guru/images/patterns/diagrams/decorator/solution1-es.png)

"Wrapper" (envoltorio, en inglés) es el sobrenombre alternativo del patrón *Decorator*, que expresa claramente su idea principal. Un *wrapper* es un objeto que puede vincularse con un objeto *objetivo*. El wrapper contiene el mismo grupo de métodos que el objetivo y le delega todas las solicitudes que recibe. No obstante, el wrapper puede alterar el resultado haciendo algo antes o después de pasar la solicitud al objetivo.

¿Cuándo se convierte un simple wrapper en el verdadero decorador? Como se menciono, el wrapper implementa la misma interfaz que el objeto envuelto. Éste es el motivo por el que, desde la perspectiva del cliente, estos objetos son idénticos. Haz que el campo de referencia del wrapper acepte cualquier objeto que siga esa interfaz. Esto permitirá envolver un objeto en varios wrappers, añadiéndole el comportamiento combinado de todos ellos.

En nuestro ejemplo de las notificaciones, dejemos la sencilla funcionalidad de las notificaciones por correo electrónico dentro de la clase base `Notificador`, pero contamos el resto de los métodos de notificación en decoradores.

![center | 400](https://refactoring.guru/images/patterns/diagrams/decorator/solution2.png)

El código cliente debe envolver un objeto notificador básico dentro de un grupo de decoradores que satisfagan las preferencias del cliente. Los objetos resultante se estructurarán como una pila.

![center | 225](https://refactoring.guru/images/patterns/diagrams/decorator/solution3-es.png)

El último decorador de la pila será el objeto con el que el cliente trabaja. Debido a que todos los decoradores implementan la misma interfaz que la notificadora base, el resto del código cliente no le importa si está trabajando con el objeto notificador "puro" o con el decorado.

Podemos aplicar la misma solución a otra funcionalidades, como el formateo de mensajes o la composición de una lista de destinatarios. El cliente puede decorar el objeto con los decoradores personalizados que desee, siempre y cuando sigan la misma interfaz que los demás.

<br>
<br>

## Analogía en el mundo real
---

Vestir ropa es un ejemplo del uso de decoradores. Cuando tienes frío, te cubres con un suéter. Si sigues teniendo frío a pesar del suéter, ponerte una chaqueta encima. Si está lloviendo, puede ponerte un impermeables. Todas estas prendas "extienden" tu comportamiento básico pero no son parte de ti, y puedes quitarte fácilmente cualquier prenda cuando lo desees.

<br>
<br>

## Estructura
---

![center | 300](https://refactoring.guru/images/patterns/diagrams/decorator/structure-indexed.png)

1. El **Componente** declara la interfaz común tanto para wrappers como para objetos envueltos.

2. **Componente Concreto** es una clase de objetos envueltos. Define el comportamiento básico, que los decoradores pueden alterar.
 
3. La clase **Decoradora Base** tiene un campo para referenciar un objeto envuelto. El tipo del campo debe declararse como la interfaz del componente para que pueda contener tanto los componentes concretos como los decoradores. La clase decoradora base delega todas las operaciones al objeto envuelto.

4. Los **Decoradores Concretos** definen funcionalidades adicionales que se pueden añadir dinámicamente a los componentes. Los decoradores concretos sobrescriben métodos de la clase decoradora base y ejecutan su comportamiento, ya sea antes o después de invocar al método padre.

5. El **Cliente** puede envolver componentes en varias capas de decoradores, siempre y cuando trabajen con todos los objetos a través de la interfaz del componente.

### Pseudocódigo

En este ejemplo, el patrón *Decorator* te permite comprimir y encriptar información delicada independientemente del código que utiliza esos datos.

![center | 300](https://refactoring.guru/images/patterns/diagrams/decorator/example.png)

La aplicación envuelve el objeto de la fuente de datos con un par de decoradores. Ambos wrappers cambian el modo en que los datos se escriben y se leen en el disco:

* Justo antes de que los datos se escriban en el disco, los decoradores los encriptan y comprimen. La clase original escribe en el archivo los datos encripados y protegidos, sin conocer el cambio.
* Después de que los datos son leídos del disco, pasan por los mismo decoradores, que los descomprimen y decodifican.

Los decoradores y la clase fuente de datos implementan la misma interfaz, lo que los hace intercambiables en el código cliente.

```java
// La interfaz de componente define operaciones que los
// decoradores pueden alterar.
interface DataSource is
    method writeData(data)
    method readData():data

// Los componentes concretos proporcionan implementaciones por
// defecto para las operaciones. En un programa puede haber
// muchas variaciones de estas clases.
class FileDataSource implements DataSource is
    constructor FileDataSource(filename) { ... }

    method writeData(data) is
        // Escribe datos en el archivo.

    method readData():data is
        // Lee datos del archivo.

// La clase decoradora base sigue la misma interfaz que los
// demás componentes. El principal propósito de esta clase es
// definir la interfaz de encapsulación para todos los
// decoradores concretos. La implementación por defecto del
// código de encapsulación puede incluir un campo para almacenar
// un componente envuelto y los medios para inicializarlo.
class DataSourceDecorator implements DataSource is
    protected field wrappee: DataSource

    constructor DataSourceDecorator(source: DataSource) is
        wrappee = source

    // La decoradora base simplemente delega todo el trabajo al
    // componente envuelto. En los decoradores concretos se
    // pueden añadir comportamientos adicionales.
    method writeData(data) is
        wrappee.writeData(data)

    // Los decoradores concretos pueden invocar la
    // implementación padre de la operación en lugar de invocar
    // directamente al objeto envuelto. Esta solución simplifica
    // la extensión de las clases decoradoras.
    method readData():data is
        return wrappee.readData()

// Los decoradores concretos deben invocar métodos en el objeto
// envuelto, pero pueden añadir algo de su parte al resultado.
// Los decoradores pueden ejecutar el comportamiento añadido
// antes o después de la llamada a un objeto envuelto.
class EncryptionDecorator extends DataSourceDecorator is
    method writeData(data) is
        // 1. Encripta los datos pasados.
        // 2. Pasa los datos encriptados al método writeData
        // (escribirDatos) del objeto envuelto.

    method readData():data is
        // 1. Obtiene datos del método readData (leerDatos) del
        // objeto envuelto.
        // 2. Intenta descifrarlo si está encriptado.
        // 3. Devuelve el resultado.

// Puedes envolver objetos en varias capas de decoradores.
class CompressionDecorator extends DataSourceDecorator is
    method writeData(data) is
        // 1. Comprime los datos pasados.
        // 2. Pasa los datos comprimidos al método writeData del
        // objeto envuelto.

    method readData():data is
        // 1. Obtiene datos del método readData del objeto
        // envuelto.
        // 2. Intenta descomprimirlo si está comprimido.
        // 3. Devuelve el resultado.

// Opción 1. Un ejemplo sencillo del montaje de un decorador.
class Application is
    method dumbUsageExample() is
        source = new FileDataSource("somefile.dat")
        source.writeData(salaryRecords)
        // El archivo objetivo se ha escrito con datos sin
        // formato.

        source = new CompressionDecorator(source)
        source.writeData(salaryRecords)
        // El archivo objetivo se ha escrito con datos
        // comprimidos.

        source = new EncryptionDecorator(source)
        // La variable fuente ahora contiene esto:
        // Cifrado > Compresión > FileDataSource
        source.writeData(salaryRecords)
        // El archivo se ha escrito con datos comprimidos y
        // encriptados.

// Opción 2. El código cliente que utiliza una fuente externa de
// datos. Los objetos SalaryManager no conocen ni se preocupan
// por las especificaciones del almacenamiento de datos.
// Trabajan con una fuente de datos preconfigurada recibida del
// configurador de la aplicación.
class SalaryManager is
    field source: DataSource

    constructor SalaryManager(source: DataSource) { ... }

    method load() is
        return source.readData()

    method save() is
        source.writeData(salaryRecords)
    // ...Otros métodos útiles...

// La aplicación puede montar distintas pilas de decoradores
// durante el tiempo de ejecución, dependiendo de la
// configuración o el entorno.
class ApplicationConfigurator is
    method configurationExample() is
        source = new FileDataSource("salary.dat")
        if (enabledEncryption)
            source = new EncryptionDecorator(source)
        if (enabledCompression)
            source = new CompressionDecorator(source)

        logger = new SalaryManager(source)
        salary = logger.load()
    // ...
```

<br>
<br>

## Aplicabilidad
---

**Utiliza el patrón Decorator cuando necesites asignar funcionalidades adicionales a objetos durante el tiempo de ejecución sin descomponer el código que utiliza esos objetos.**

> El patrón Decorator te permite estructurar tu lógica de negocio en capas, crear un decorador para cada capa y componer objetos con varias combinaciones de esta lógica, durante el tiempo de ejecución. El código cliente puede tratar a todos estos objetos de las misma forma, ya que todos siguen una interfaz común.

**Utiliza el patrón cuando resulte extraño o no sea posible extender el comportamiento de un objeto utilizando la herencia.**

> Muchos lenguajes de programación cuentan con la palabra clave `final` que puede utilizarse para evitar que una clase siga extendiéndose. Para una clase final, la única forma de reutilizar el comportamiento existente será envolver la clase con tu propio wrapper, utilizando el patrón Decorator.

<br>
<br>

## Cómo implementarlo
---

1.  Asegúrate de que tu dominio de negocio puede representarse como un componente primario con varias capas opcionales encima.

2.  Decide qué métodos son comunes al componente primario y las capas opcionales. Crea una interfaz de componente y declara esos métodos en ella.

3.  Crea una clase concreta de componente y define en ella el comportamiento base.

4.  Crea una clase base decoradora. Debe tener un campo para almacenar una referencia a un objeto envuelto. El campo debe declararse con el tipo de interfaz de componente para permitir la vinculación a componentes concretos, así como a decoradores. La clase decoradora base debe delegar todas las operaciones al objeto envuelto.

5.  Asegúrate de que todas las clases implementan la interfaz de componente.

6.  Crea decoradores concretos extendiéndolos a partir de la decoradora base. Un decorador concreto debe ejecutar su comportamiento antes o después de la llamada al método padre (que siempre delega al objeto envuelto).

7.  El código cliente debe ser responsable de crear decoradores y componerlos del modo que el cliente necesite.

<br>
<br>

## Pros y contras
---

> [!success] Pro
> Puedes extender el comportamiento de un objeto sin crear una nueva subclase.

> [!success] Pro
> Puedes añadir o eliminar responsabilidades de un objeto durante el tiempo de ejecución.

> [!success] Pro
> Puedes combinar varios comportamientos envolviendo un objeto con varios decoradores.

> [!success] Pro
> Principio de responsabilidad única. Puedes dividir una clase monolítica que implementa muchas variantes posibles de comportamiento, en varias clases más pequeñas.

> [!fail] Contra
> Resulta difícil eliminar un wrapper específico de la pila de wrappers.

> [!fail] Contra
> Es difícil implementar un decorador de tal forma que su comportamiento no dependa del orden en la pila de decoradores.

> [!fail] Contra
> El código de configuración inicial de las capas pueden tener un aspecto desagradable.

<br>
<br>

## Relación con otros patrones
---

![[5. Relación entre patrones#Decorator]]