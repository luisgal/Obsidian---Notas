# Programación Orientada a objetos (POO)

Antes de la POO, nace un nueva forma de pensar y ver las cosas para la resolución de problemas en el mundo del desarrollo de software, con esto nos referimos al paradigma orientado a objetos.

¿Qué es un paradigma? Un paradigma en una metodología que intenta unificar y simplificar la manera en que se resuelve un cierto grupo de problemas. En el contexto de la programación, un paradigma es un conjunto de principios y métodos que sirven para resolver los problemas a los que se enfrenta los desarrolladores de software al construir sistemas grandes y complejos. Existen diversos paradigmas de programación, por ejemplo:

* Programación estructurada: Se basa en estructuras de control de flujo de programa. No existen saltos entre rutinas, el flujo es continuo, haciendo que los programas sean más fáciles de entender.
* Programación funcional: Se programa con funciones que son llamadas bajo demanda. La programación de pequeños segmentos de código, funciones, promueve la reutilización de código y simplifica el entendimiento de las funciones. 
* Programación orienta a objetos: Los programas trabajan con base en unidades llamadas objetos. Todo esto se explica más adelante.

La programación orientada a objetos esta basada bajo el concepto de clases y objetos. Este tipo de programación se utiliza para estructurar un programa de software en piezas simples y reutilizables, en ello entran las clases las cuales serán el modelo o planos que definen el objetivo y comportamiento de cada sección de código. El objetivo de esta forma de programar es pensar objetos, su construcción, su funcionalidad y su interacción junto con otros objetos.

El paradigma orienta a objetos es útil cuando el sistema se modela de forma casi análoga a la realidad, porque así se simplifica el diseño de alto nivel. Esta analogía permite que los programadores tengan más claro cuál es el papel de cada porción del código y de los datos, lo que facilita la creación y el mantenimiento del sistema, promueve la reutilización de código por ejemplo en la implementación de clases abstractas (esto se verá mas adelante).

Para comprender la programación orientada a objetos se debe de entender los conceptos bajo los cuales esta pensado, el primero de ellos son las clases y lo objetos.

---
## Clases y objetos

Las clases y los objetos son el eje central del paradigma orientado a objetos, debemos entender lo que son y sus diferencias para no utilizar estos conceptos de manera indistinta.

### Objeto

Lo primero es hablar acerca de objetos, un objeto es una entidad que puede ser tangible o no, cada objeto tiene un conjunto de características y funciones, con ello sabemos que los objetos tendrán estados y comportamientos.

* _Estado_: Son los datos asociados con el objeto que nos indican la situación en la que se encuentra, como por ejemplo un objeto puede tener una velocidad, capacidad de carga, volumen, calificación, color, encontrarse encendido/apagado, etc.
* _Comportamiento_: Es la manera en la que el objeto responde a estímulos, como por ejemplo lo que sucede cuando un botón es presionado, cuando la gasolina de un automóvil se ha terminado o lo que sucede cuando se hace un retiro de una cuenta bancaria.

En este punto nos acercamos al concepto de clases por lo que ahora debemos comprender que las características de un objeto las llamaremos ahora ___atributos___ y sus funciones o comportamientos pasaremos a conocerlos como ___métodos___.

Para entender estos conceptos vamos a utilizar un ejemplo que intente dejar claro lo que son. Partiremos desde el hecho de que un coche es un objeto y si deseamos abstraerlo para poder hacer una representación informática de el, debemos observar las características y funcionalidades que tiene o puede tener un coche. Para empezar con este ejemplo observemos el siguiente coche:

![[coche_rojo.jpg | center | 350]]

Algunas de las características del coche anterior son las siguientes:

```mermaid
flowchart
	A([Coche])
	B([Rojo])
	C([4])
	D([2])
	E([160 Km/Hr])
	F([De piel])

	A -- Color --> B
	A -- No. de llantas --> C
	A -- No. de faros delanteros --> D
	A -- Max. Velocidad --> E
	A -- Material de los asientos --> F
	
```

Comencemos mencionando que las característica del color es completamente observable, sin embargo el no. de llantas es algo intuitivo ya que no observamos en su totalidad las 4 y en un hipotético caso tal vez nos encontremos un auto con más llantas. Algo más por resaltar son las características de máxima velocidad y el material de los asientos, si bien no son características que observemos en la imagen o exista algún indicio, supondremos que estas son las características de este coche en particular.

![[coches.jpg | center | 300]]

Ahora si observamos la imagen anterior vemos un tipo de coche distinto, y cada uno de diferente color, tal vez sus interiores sean distintos al coche rojo que vimos anteriormente, sin embargo todos cumplen la condición de que son coches, automóviles. En este punto debemos comenzar a hablar de lo que es una clase.

### Clase

Con el concepto de objeto logramos observar que cada objeto tiene características particulares, sin embargo como objetos podemos encontrar características comunes. Si bien en el ejemplo de objetos se limito a mencionar características el mismo concepto se extiende a sus funcionalidades, retomando el ejemplo de un coche podemos mencionar que una función de un coche puede ser el acelerar o el bajar las ventanas, sin embargo podemos observar que distintos objetos coche tienen la misma funcionalidad.

Entonces ¿qué pasa cuando tenemos características y funcionalidades que distintos objetos tienen en común? en este punto toma relevancia el concepto de clase.




### Atributos y métodos

### Constructores y destructores

---
## Modularidad


---
## Jerarquía


---
## Tipificación


---
## Sobrecarga


---
## Agregación


---
## Herencia


---
## Clases abstractas


---
## Upcasting y downcasting


---
## Polimorfismo


---
## Encapsulamiento
