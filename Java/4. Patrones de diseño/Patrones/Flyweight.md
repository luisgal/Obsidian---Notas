---
title: Java - Patrón Flyweight
tags:  
- Java/DesignPattern
- Java/StructuralPattern
- DesignPattern
---

# Flyweight

## Propósito

**Flyweight** es un patrón de diseño estructural que te permite mantener más objetos dentro de la cantidad disponible de RAM compartiendo las partes comunes del estado entre varios objetos en lugar de mantener toda la información en cada objeto.

![center | 400](https://refactoring.guru/images/patterns/content/flyweight/flyweight.png)

## Problema

Para divertirte un poco después de largas horas de trabajo, decides crear un sencillo videojuego en el que los jugadores se tienen que mover por un mapa disparándose entre sí. Decides implementar un sistema de partículas realistas que lo distinga de otros juegos. Grandes cantidades de balas, misiles y metralla de las explosiones volarán por todo el mapa, ofreciendo una apasionante experiencia al jugador.

Al terminarlo, subes el último cambio, compilas el juego y se lo envias a un amigo para una partida de prueba. Aunque el juego funcionaba sin problemas en tu máquina, tu amigo no logró jugar durante mucho tiempo. En su computadora el juego se paraba a los pocos minutos de empezar. Tras dedicar varias horas a revisar los registros de depuración, descubres que el juego se paraba debido a una cantidad insuficiente de RAM. Resulta que el equipo de tu amigo es mucho menos potente que tu computadora, y esa es la razón por la que el problema surgió tan rápido en su máquina.

El problema estaba relacionado con tu sistema de partículas. Cada partícula, como una bala, un misil o un trozo de metralla, estaba representada por un objeto separado que contenía gran cantidad de datos. En cierto momento, cuando la masacre alcanzaba su punto culminante en la pantalla del jugador, las partículas recién creadas ya no cabían en el resto de RAM, provocando que el programa fallara.

![center | 400](https://refactoring.guru/images/patterns/diagrams/flyweight/problem-es.png)

## Solución

Observando más atentamente la clase `Partícula`, puede ser que te hayas dado cuenta de que los campos de color y *sprite* consumen mucha más memoria que otros campos. Lo que es peor, esos dos campos almacenan información casi idéntica de todas las partículas. Por ejemplo, todas las balas tienen el mismo color y sprite.

![center | 400](https://refactoring.guru/images/patterns/diagrams/flyweight/solution1-es.png)

Otras partes del estado de una partícula, como las coordenadas, vector de movimiento y velocidad, son únicas en cada partícula. Después de todo, los valores de estos campos cambian a lo largo del tiempo. Estos datos representan el contexto siempre cambiante en el que existe la partícula, mientras que el color y el sprite se mantienen constantes.

Esta información constante de un objeto suele denominarse su _estado intrínseco_. Existe dentro del objeto y otros objetos únicamente pueden leerla, no cambiarla. El resto del estado del objeto, a menudo alterado “desde el exterior” por otros objetos, se denomina el _estado extrínseco_.

El patrón Flyweight sugiere que dejemos de almacenar el estado extrínseco dentro del objeto. En lugar de eso, debes pasar este estado a métodos específicos que dependen de él. Tan solo el estado intrínseco se mantiene dentro del objeto, permitiendo que lo reutilices en distintos contextos. Como resultado, necesitarás menos de estos objetos, ya que sólo se diferencian en el estado intrínseco, que cuenta con muchas menos variaciones que el extrínseco.

![center | 250](https://refactoring.guru/images/patterns/diagrams/flyweight/solution3-es.png)

Regresemos a nuestro juego. Dando por hecho que hemos extraído el estado extrínseco de la clase de nuestra partícula, únicamente tres objetos diferentes serán suficientes para representar todas las partículas del juego: una bala, un misil y un trozo de metralla. Como probablemente habrás adivinado, un objeto que sólo almacena el estado intrínseco se denomina _Flyweight_ (peso mosca).

**Almacenamiento del estado extrínseco**

¿A dónde se mueve el estado extrínseco? Alguna clase tendrá que almacenarlo, ¿verdad? En la mayoría de los casos, se mueve al objeto contenedor, que reúne objetos antes de que apliquemos el patrón.

En nuestro caso, se trata del objeto principal `Juego`, que almacena todas las partículas en su campo `partículas`. Para mover el estado extrínseco a esta clase, debes crear varios campos matriz para almacenar coordenadas, vectores y velocidades de cada partícula individual. Pero eso no es todo. Necesitas otra matriz para almacenar referencias a un objeto _flyweight_ específico que represente una partícula. Estas matrices deben estar sincronizadas para que puedas acceder a toda la información de una partícula utilizando el mismo índice.

![center | 400](https://refactoring.guru/images/patterns/diagrams/flyweight/solution2-es.png)

Una solución más elegante sería crear una clase de contexto separada que almacene el estado extrínseco junto con la referencia al objeto flyweight. Esta solución únicamente exigiría tener una matriz en la clase contenedora.

¡Espera un momento! ¿No deberíamos tener tantos de estos objetos contextuales como teníamos al principio? Técnicamente, sí. Pero el caso es que estos objetos son mucho más pequeños que antes. Los campos que consumen más memoria se han movido a unos pocos objetos flyweight. Ahora, cientos de pequeños objetos contextuales pueden reutilizar un único objeto flyweight pesado, en lugar de almacenar cientos de copias de sus datos.

**Flyweight y la inmutabilidad**

Debido a que el mismo objeto flyweight puede utilizarse en distintos contextos, debes asegurarte de que su estado no se pueda modificar. Un objeto flyweight debe inicializar su estado una sola vez a través de parámetros del constructor. No debe exponer ningún método _set_ (modificador) o campo público a otros objetos.

**Fábrica flyweight**

Para un acceso más cómodo a varios objetos flyweight, puedes crear un método fábrica que gestione un grupo de objetos flyweight existentes. El método acepta el estado intrínseco del flyweight deseado por un cliente, busca un objeto flyweight existente que coincida con este estado y lo devuelve si lo encuentra. Si no, crea un nuevo objeto flyweight y lo añade al grupo.

Existen muchas opciones para colocar este método. El lugar más obvio es un contenedor flyweight. Alternativamente, podrías crea un nueva clase fábrica y hacer estático el método fábrica para colocarlo dentro de una clase flyweight real.

## Estructura

![center | 400](https://refactoring.guru/images/patterns/diagrams/flyweight/structure-indexed.png)

1.  El patrón Flyweight es simplemente una optimización. Antes de aplicarlo, asegúrate de que tu programa tenga un problema de consumo de RAM provocado por tener una gran cantidad de objetos similares en la memoria al mismo tiempo. Asegúrate de que este problema no se pueda solucionar de otra forma sensata.

2.  La clase **Flyweight** contiene la parte del estado del objeto original que pueden compartir varios objetos. El mismo objeto flyweight puede utilizarse en muchos contextos diferentes. El estado almacenado dentro de un objeto flyweight se denomina _intrínseco_, mientras que al que se pasa a sus métodos se le llama _extrínseco_.

3.  La clase **Contexto** contiene el estado extrínseco, único en todos los objetos originales. Cuando un contexto se empareja con uno de los objetos flyweight, representa el estado completo del objeto original.

4.  Normalmente, el comportamiento del objeto original permanece en la clase flyweight. En este caso, quien invoque un método del objeto flyweight debe también pasar las partes adecuadas del estado extrínseco dentro de los parámetros del método. Por otra parte, el comportamiento se puede mover a la clase de contexto, que utilizará el objeto flyweight vinculado como mero objeto de datos.

5.  El **Cliente** calcula o almacena el estado extrínseco de los objetos flyweight. Desde la perspectiva del cliente, un flyweight es un objeto plantilla que puede configurarse durante el tiempo de ejecución pasando información contextual dentro de los parámetros de sus métodos.

6.  La **Fábrica flyweight** gestiona un grupo de objetos flyweight existentes. Con la fábrica, los clientes no crean objetos flyweight directamente. En lugar de eso, invocan a la fábrica, pasándole partes del estado intrínseco del objeto flyweight deseado. La fábrica revisa objetos flyweight creados previamente y devuelve uno existente que coincida con los criterios de búsqueda, o bien crea uno nuevo si no encuentra nada.
### Pseudocódigo

En este ejemplo, el patrón **Flyweight** ayuda a reducir el uso de memoria a la hora de representar millones de objetos de árbol en un lienzo.

![center | 400](https://refactoring.guru/images/patterns/diagrams/flyweight/example.png)

El patrón extrae el estado intrínseco repetido de una clase principal `Árbol` y la mueve dentro de la clase flyweight `TipodeÁrbol`.

Ahora, en lugar de almacenar la misma información en varios objetos, se mantiene en unos pocos objetos flyweight vinculados a los objetos de `Árbol` adecuados que actúan como contexto. El código cliente crea nuevos objetos árbol utilizando la fábrica flyweight, que encapsula la complejidad de buscar el objeto adecuado y reutilizarlo si es necesario.

```java
// La clase flyweight contiene una parte del estado de un árbol.
// Estos campos almacenan valores que son únicos para cada árbol
// en particular. Por ejemplo, aquí no encontrarás las
// coordenadas del árbol. Pero la textura y los colores que
// comparten muchos árboles sí están aquí. Ya que esta cantidad
// de datos suele ser GRANDE, dedicarás mucha memoria a
// mantenerla en cada objeto árbol. En lugar de eso, podemos
// extraer la textura, el color y otros datos repetidos y
// colocarlos en un objeto independiente que muchos objetos
// individuales del árbol pueden referenciar.
class TreeType is
    field name
    field color
    field texture
    constructor TreeType(name, color, texture) { ... }
    method draw(canvas, x, y) is
        // 1. Crea un mapa de bits de un tipo, color y textura
        // concretos.
        // 2. Dibuja el mapa de bits en el lienzo con las
        // coordenadas X y Y.

// La fábrica flyweight decide si reutiliza el flyweight
// existente o si crea un nuevo objeto.
class TreeFactory is
    static field treeTypes: collection of tree types
    static method getTreeType(name, color, texture) is
        type = treeTypes.find(name, color, texture)
        if (type == null)
            type = new TreeType(name, color, texture)
            treeTypes.add(type)
        return type

// El objeto contextual contiene la parte extrínseca del estado
// del árbol. Una aplicación puede crear millones de ellas, ya
// que son muy pequeñas: dos coordenadas en números enteros y un
// campo de referencia.
class Tree is
    field x,y
    field type: TreeType
    constructor Tree(x, y, type) { ... }
    method draw(canvas) is
        type.draw(canvas, this.x, this.y)

// Las clases Tree y Forest son los clientes de flyweight.
// Puedes fusionarlas si no tienes la intención de desarrollar
// más la clase Tree.
class Forest is
    field trees: collection of Trees

    method plantTree(x, y, name, color, texture) is
        type = TreeFactory.getTreeType(name, color, texture)
        tree = new Tree(x, y, type)
        trees.add(tree)

    method draw(canvas) is
        foreach (tree in trees) do
            tree.draw(canvas)
```

## Aplicabilidad

**Utiliza el patrón Flyweight únicamente cuando tu programa deba soportar una enorme cantidad de objetos que apenas quepan en la RAM disponible.**

> La ventaja de aplicar el patrón depende en gran medida de cómo y dónde se utiliza. Resulta más útil cuando:
>
> -  La aplicación necesita generar una cantidad enorme de objetos similares
> -  Esto consume toda la RAM disponible de un dispositivo objetivo
> -  Los objetos contienen estados duplicados que se pueden extraer y compartir entre varios objetos

## Cómo implementarlo

1. Divide los campos de una clase que se convertirá en flyweight en dos partes:

    - El estado intrínseco: los campos que contienen información invariable duplicada a través de varios objetos
    - El estado extrínseco: los campos que contienen información contextual única de cada objeto

2. Deja los campos que representan el estado intrínseco en la clase, pero asegúrate de que sean inmutables. Deben llevar sus valores iniciales únicamente dentro del constructor.
   
3.  Repasa los métodos que utilizan campos del estado extrínseco. Para cada campo utilizado en el método, introduce un nuevo parámetro y utilízalo en lugar del campo.

4. Opcionalmente, crea una clase fábrica para gestionar el grupo de objetos flyweight, buscando uno existente antes de crear uno nuevo. Una vez que la fábrica esté en su sitio, los clientes sólo deberán solicitar objetos flyweight a través de ella. Deberán describir el flyweight deseado pasando su estado intrínseco a la fábrica.

5. El cliente deberá almacenar o calcular valores del estado extrínseco (contexto) para poder invocar métodos de objetos flyweight. Por comodidad, el estado extrínseco puede moverse a una clase contexto separada junto con el campo referenciador del flyweight.

## Pros y contras

> [!success] Pro
> Puedes ahorrar mucha RAM, siempre que tu programa tenga toneladas de objetos similares.

> [!fail] Contra
> Puede que estés cambiando RAM por ciclos CPU cuando deba calcularse de nuevo parte de la información de contexto cada vez que alguien invoque un método flyweight.

> [!fail] Contra
> El código se complica mucho. Los nuevos miembros del equipo siempre estarán preguntándose por qué el estado de una entidad se separó de tal manera.