# Intro Funcional
La implementación de un paradigma a nuestro trabajo nos puede interesar por varios motivos, nos puede facilitar el modelado o diseño de un programa, el entendimiento de código resultante, y también vamos a decir que trae algunas características "gratis" al producto final (no significa que no se puedan implementar construyendo de otra forma). Nos vamos a concentrar en cómo afecta el diseño y código resultante de un programa.

Un programa es una entidad muy compleja, principalmente por el estado, sus relaciones y los quilombos que metemos para intentar controlar esto. La idea con esto es intentar facilitarles la construcción del programa que piensen mediante la aplicación de estos conceptos.

Voy a introducirles algunos conceptos del paradigma funcional y otros que vienen de la mano ya que estamos.

## De qué vamos a hablar
* [Paradigma](https://gist.github.com/NormanPerrin/369c161578cf01710743d15dd72ec869#paradigma)
* [Algunos conceptos para entendernos mejor](https://gist.github.com/NormanPerrin/369c161578cf01710743d15dd72ec869#algunos-conceptos-para-entendernos-mejor)
  * [Abstracción](https://gist.github.com/NormanPerrin/369c161578cf01710743d15dd72ec869#abstracción)
  * [Declaratividad](https://gist.github.com/NormanPerrin/369c161578cf01710743d15dd72ec869#declaratividad)
  * [Expresividad](https://gist.github.com/NormanPerrin/369c161578cf01710743d15dd72ec869#expresividad)
* [Un poquito de matemática (no se asusten por favor)](https://gist.github.com/NormanPerrin/369c161578cf01710743d15dd72ec869#matemática-)
* [Algunos conceptos funcionales](https://gist.github.com/NormanPerrin/369c161578cf01710743d15dd72ec869#conceptos-funcionales-)
  * [Transparencia referencial](https://gist.github.com/NormanPerrin/369c161578cf01710743d15dd72ec869#transparencia-referencial-o-efecto-de-lado)
  * [Inmutabilidad](https://gist.github.com/NormanPerrin/369c161578cf01710743d15dd72ec869#inmutabilidad)
  * [Orden superior](https://gist.github.com/NormanPerrin/369c161578cf01710743d15dd72ec869#orden-superior)
  * [Composición](https://gist.github.com/NormanPerrin/369c161578cf01710743d15dd72ec869#composición)

### [Paradigma](http://wiki.uqbar.org/wiki/articles/paradigma-de-programacion.html)
_Era una agradable tarde de verano en la antigua Grecia..._

![Image:](https://s3-sa-east-1.amazonaws.com/intro-funcional/sheldon.jpg)

Para hacerla corta y entendernos vamos a decir que un paradigma es:
> Un conjunto de ideas que acompañan al programador en la construcción de un programa, definiéndole herramientas conceptuales a utilizar y cómo usarlas.

Otra forma de definirlo puede ser:
> Marco conceptual que vamos a usar en la construcción de un programa.

Quizás la última queda muy genérica, pero nos da una idea de que el paradigma no son más que conceptos que vamos a usar cuando pensemos un programa.

### Algunos conceptos para entendernos mejor
Cuando entremos en el análisis del código, vamos a estar usando algunos conceptos para poder decir cosas como "este código es más lindo que este otro", "se entiende mejor" o "separa mejor sus partes" pero de una manera un poco más técnica.

#### [Abstracción](http://wiki.uqbar.org/wiki/articles/abstraccion.html)
Nosotros en nuestro día a día utilizamos abtracciones, quizás sin conocer una definición exacta del concepto, pero es intuitivo y por eso lo aplicamos, ya que sino no podríamos lograr mucho en el día.

Necesitamos dividir la complejidad de todo lo que queremos hacer en tareas, y esas tareas en más tareas, haciendo éstas más fáciles de entender y llevar adelante.

Un programa no es muy diferente, vamos a decir que también queremos dividir sus relaciones, funciones y estados en partes para entenderlo mejor.

El problema es que como humanos, estamos limitados en nuestra capacidad de entender y retener cosas en nuestra cabeza. Ahí es donde necesitamos agrupar los conceptos, ponerles nombre, abstraerlos. Por estas razones es que no programamos en assembler, sino que usamos lenguajes de [alto nivel](https://es.wikipedia.org/wiki/Lenguaje_de_alto_nivel).

A continuación les muestro algo de código para calmar su ansia y también vean de lo que hablo:
```javascript
const mapearDoble = arr => {
  const arrRetorno = [];
  for  (let i = 0; i < arr.length; i++) {
    arrRetorno.push(arr[i] * 2);
  }
  return arrRetorno;
};
```

Podría implementarle algunas abtracciones para intentar simplificarlo, ahora no me importa cómo recorrer un array, me abstraigo de ese problema y solo me concentro en qué transformación le quiero realizar a esos elementos:
```javascript
const _map = (arr, f) => {
  const arrRetorno = [];
  for (let i = 0; i < arr.length; i++) {
    arrRetorno.push(f(arr[i]));
  }
  return arrRetorno;
};

const mapearDoble = arr => _map( arr, num => num * 2 );
```

Así ya podemos ver cómo separando un problema en partes obtenemos algunos beneficios importantes: puedo reutilizar `_map` para otras transformaciones de array, y la definición de `mapearDoble` queda mucho más sencilla.

#### [Declaratividad](http://wiki.uqbar.org/wiki/articles/declaratividad.html)
![Image:](https://s3-sa-east-1.amazonaws.com/intro-funcional/dignidad.jpg)

Su definición formal es esta:
> Característica de algunas herramientas que permiten o fuerzan la separación entre el conocimiento del dominio de un programa y la manipulación de dicho conocimiento.

Intentando simplificar un poco esta definición, podemos decir que el _conocimiento del dominio_ es el "qué" (declaración) de un programa, mientras que la _manipulación de dicho conocimiento_ es el "cómo" (implementación).

Si volvemos al ejemplo anterior podemos ver cómo una es más declarativa que otra:

No declarativa:
```javascript
const mapearDoble = arr => {
  const arrRetorno = [];
  for  (let i = 0; i < arr.length; i++) {
    arrRetorno.push(arr[i] * 2);
  }
  return arrRetorno;
};
```

Declarativa:
```javascript
const mapearDoble = arr => _map( arr, num => num * 2 );
```

Nos olvidamos del detalle de implementación de cómo recorrer el array, en el segundo ejemplo solo le decimos que el retorno va a ser el resultado de "mapear" el array con nuestra función que obtiene el doble de un número (si la quisiésemos hacer más declarativa, podríamos abstraer esta parte también). Entonces podemos ver cómo el segundo ejemplo está más cerca de ser una definición de "qué" es la función y se aleja del "cómo".

#### [Expresividad](http://wiki.uqbar.org/wiki/articles/expresividad.htm)
Esto ya es más subjetivo, nos sirve para evaluar la facilidad de entiendimiento de una solución. O sea, que tan legible es un cacho de código, o que tan lindo es de leer. No solo va a depender de los nombres que demos y la estructura del código, sino que van a influir los conocimientos y prejuicios que pueda tener el que mire el código.

Para aclarar, podemos volver al ejemplo de la función `mapearDoble` con `for` y la otra con `_map`. Si la persona sabe lo que significa "map" entonces va considerar más expresivo el uso de `_map`, pero si no sabe y ya está acostumbrado al uso de `for` va a entender este ejemplo mucho más rápido.

### Matemática 😱
![Image:](https://s3-sa-east-1.amazonaws.com/intro-funcional/chalmers.jpg)

Vamos a dedicarle unos renglones a esta parte ya que el paradigma funcional tiene sus bases en la matemática.
Esto le da algunas propiedades que vamos a usar para nuestro beneficio. Sus caractersticas:
```
 f(x) = 2 + x
```
* **Existencia**: `cada entrada tiene una salida`, entonces cualquier número que le pase a _f_ va a tener una salida.
* **Unicidad**: `cada entrada tiene una única salida`, si a _f_ le paso un _2_ no puede devolverme varias salidas, solo va a tener 1, y eso debería cumplirse para todos las entradas que le pase.
* **Pueden componerse**: si tuviese una función _g_ podría hacer `f.g(x)` (f "cerito" g), lo que sería lo mismo que decir: `f(g(x))`
* **Pueden llamarse a sí mismas**: dentro de la definición de _f_ podría tener un llamado a _f_.
* **Pueden devolver otras funciones**: si nos acordamos, podemos pensar en, perdón por mencionarlo, las derivadas e integrales, este es un ejemplo de funciones que pueden retornar otras funciones.

Excelente, y? Bueno, ahora vamos a ver cómo estas propiedades se aplican a código y qué beneficios nos traen...

### Conceptos funcionales 🎉
Llegaste!, incluso después que dije _derivadas e integrales_, así que lo prometido es prometido: vamos a explicar los conceptos principales de este paradigma. Te recomiento que ahora te levantes, estires un poco y busques un café, porque estos conceptos puede que no se asimilen de una y vamos a necesitar estar atentos, aunque tampoco es para asustarse, con la práctica se van a consolidar.

#### [Transparencia referencial](http://wiki.uqbar.org/wiki/articles/transparencia-referencial--efecto-de-lado-y-asignacion-destructiva.html)
_Apreciación personal: concepto más importante del paradigma._
> Una expresión tiene transparencia referencial si esta puede ser reemplazada por el valor que genera y no cambia el comportamiento de un programa. No tiene efecto de lado.

Como primer caracterstica, para una entrada no puede generar varias posibles salidas, solo debe generar 1, entonces no debe depender de nada de afuera, porque si algo de afuera cambia, la salida también y eso significaría que hay otra posible salida.

Luego como segunda característica es que tampoco modifica nada de afuera, ya que entonces estaría haciendo más cosas que solo retornar un resultado, y de ser así, no sería lo mismo si se lo reemplaza en su llamado por el valor de retorno.

Vamos a diferenciar 2 tipos de transparencia referencial:
* **Efecto secundario** (salida oculta): cambia el contexto.
* **Causas secundarias** (entrada oculta): depende del contexto.

Al romper con este concepto, dependemos de las asunciones que hizo el programador del contexto, el cual probablemente cambie, cuando pensó esa función que creó.

¿Qué podemos hacer para no tener este tipo de problemas? Declarar todas las entradas y salidas que usa o genera una función.
Cuando hacemos esto, podemos ver que la firma de la función es más grande. Esto no significa que la complejidad haya aumentado, sino que antes estaba oculta.

Algunos beneficios que obtenemos:
* **Predictibilidad**: ahora sabemos que con una serie de entradas me va a generar las mismas salidas, no depende del contexto.
* **Facilidad de testeo**: ahora no tengo que recrear el contexto de nuevo, con pasar las entradas a una función ya se puede ejecutar.
* **Memorizar**: ya que con una serie de entradas, me devuelve las mismas salidas, puedo reemplazar el valor de resultado por el llamado a la función. Mientras más grande el costo de ejecución de esa función que reemplazo, mayor el beneficio al usar este método.
* **Paralelismo**: al no haber dependencia entre funciones, las puedo ejecutar en paralelo.

Para bajar a tierra el concepto veamos un poco de código:
```javascript
const agregarComida = comida => heladera.push(comida)
```

Viendo este ejemplo nos preguntamos... de dónde sale heladera? probablemente es alguna variable global que usamos en nuestro programa "cocina". Por ahora vaya y pase, no parece tan complejo y la verdad que no veo dónde puede romper.

Entonces se empieza a usar la heladera, pero... una función, (llamemosla María) se nos queja porque dice que `heladera.length` le dio un resultado innesperado. El problema es que en otro lugar, fuera del conocimiento de María, otra función le agregó comida y María no se dió cuenta.

Vemos cómo entra en juego el primer beneficio que mencionamos: **predictibilidad**. María no se lo vió venir.

Mientras más grande se haga un programa que muta estado y depende del contexto global, más difícil va a ser razonar sobre qué pasa con el programa en diferentes tiempos.

La **facilidad de testeo** se relaciona con la **predictibilidad**. De la misma forma que María quedó confundida por un valor inesperado, un test puede fallar.

Para poder **memorizar** un valor, debería dar el mismo resultado la función con un mismo input. Esto no pasa. Tampoco se podría implementar el **paralelismo**, ya que las funciones no son independientes entre sí y dependen del contexto.

Ni siquiera cumplimos con la parte de la definición que dice que se debería poder reemplazar su llamado por el valor que genera. Para solucionarlo podemos escribir algo así:
```javascript
const agregarComida = (heladera, comida) => heladera.concat(comida)
```

Genial, con esos pocos cambios ahora cumplimos con todo. Tomense el tiempo para analizar si cumple cada propiedad que mencionamos a ver si no cumple con alguna. Incluso ahora como bonus si tenemos otra heladera en la cocina lo podemos manejar sin tener que hacer ningún cambio.

#### Inmutabilidad
Un concepto bastante sencillo y cercano al anterior. Podemos decir que la inmutabilidad es una restricción que le ponemos a la asignación de variables, todas las variables pasan ahora a ser inmodificables.

Vale la pena mencionar que el uso de `const` no garantiza inmutabilidad, en el caso de objetos estamos complicados, a menos que usemos estructuras de datos de alguna lib tipo [immutable.js](https://facebook.github.io/immutable-js/) o cuidemos el retorno de objetos, devolviendo copias de los mismos y no su [referencia](https://es.stackoverflow.com/questions/1493/cuál-es-la-diferencia-entre-paso-de-variables-por-valor-y-por-referencia/1497) (los arrays también son objetos).

Para devolver copias de objetos u arrays podemos hacer lo siguiente:
```javascript
// objeto (ES6)
const nuevoObj = {...obj}

// objeto
const nuevoObj = Object.assign(obj)

// array 
const nuevoArr = arr.slice()
```

La inmutabilidad garantiza predictibilidad en el flujo de ejecución de un programa:
```javascript
const mascotaEspecial = { tipo: 'tortuga', 'nombre': 'Martin' }
...
...
...
assert(mascotaEspecial.tipo === 'tortuga') // true
```

#### [Orden superior](http://wiki.uqbar.org/wiki/articles/orden-superior.html)
Este concepto también es clave, vamos a ganar un montón de expresividad aplicándolo bien. Así que empecemos con la definición:
> Función que puede recibir y ejecutar internamente otra función o devolver una.

Esto nos importa ya que puedo generar mejores abstracciones para la reutilización de código, declaratividad y todo lo que veníamos viendo hasta ahora, o sea que vamos a estar reutilizando comportamiento.

Cuando un lenguaje permite esto, se dice que sus funciones son de _ciudadanos de primera clase_, lo que significa que no tiene restricciones en su uso, pueden aparecer en cualquier lado del programa.

Las funciones más conocidas de orden superior son: `map`, `filter`, etc. Por lo general las que son para arrays. ¿Y por qué son las más conocidas? nuevamente, lo que tienen todas estas funciones en común es que te podés abstraer de cómo recorrer el array y las personas que conocieron estas funciones notaron sus beneficios, solo te interesa "decirle" qué querés hacer cuando lo recorrés, y aparte, cada una de estas funciones hace algo diferente en el recorrido, por ejemplo, `map` agarra un array y te devuelve otro del mismo tamaño pero aplicándole una transformación, la cual se la pasámos por parámetro. Así, ya tenemos bastantes cosas resueltas.
```javascript
const doble = num => num * 2

const mapearDoble = arr => arr.map(doble)
```

Sabiendo que hace `map`, no te parece más sencillo ahora?

Ya que venimos con envión podemos analizar una función de alto orden importante, en otros lenguajes se lo puede conocer como `fold` o `inject`, en JS es `reduce`. Esta función actúa sobre arrays, obteniendo como parámetro una función que analizaremos luego y una "semilla", esta devuelve una transformación que hace sobre el elemento actual. A diferencia de `map`, el retorno de esta es irrestricto, puede ser cualquier cosa que construyamos durante su ejecucón.

La función a pasarle va a actuar sobre todos los elementos del array en orden y es de la siguiente forma:
```typescript
(previo, actual, indice, array) => A
```

Donde:
* _previo_: valor que retornó la función en la evaluación del anterior elemento (en caso de ser el primer elemento, toma el valor de la semilla)
* _actual_: valor.. actual que se está evaluando del array.
* _indice_: es el índice del elemento actual que se está evaluando del array.
* _array_: es el array sobre el que actúa
* _A_: un tipo genérico cualquiera, puede ser un número, un array, una promesa, ...

Un ejemplo de uso de esta función puede ser el siguiente:
```javascript
[1, 2, 3, 4, 5].reduce( (previo, actual) => previo + actual, 0 ) // devuelve 15
```

En este ejemplo estamos reduciendo el array `[1, 2, 3]` a un valor, el cual es la suma de sus elementos.

Algo medio loco es que con esta función podemos generar todas las otras funciones de alto orden de arrays, ya que el retorno es irrestricto. Eso se los dejo para que jueguen un poco y prueben si les sale con `reduce` implementar las otras funciones de alto orden.

Una vez que sabemos que hacen estas funciones, casi seguro que no volvés a tocar un `for`. Porque entendiste el sentido de cada una y podés ver la ganancia en expresividad que se obtiene.

Si todavía no te quedó claro del todo `reduce` te entiendo, es bastante complicado al principio. Quizás este gráfico te ayude a entender lo que pasa cuando se ejecuta:

![Image:](https://s3-sa-east-1.amazonaws.com/intro-funcional/fold.png)

#### [Composición](http://wiki.uqbar.org/wiki/articles/composicion.html)
![Image:](https://s3-sa-east-1.amazonaws.com/intro-funcional/sinpiernas.jpg)

Como dijimos antes en la parte de matemática: `f.g(x)` es lo mismo que `f(g(x))`

Bajado a una definición escrita podemos decir:
> Una composición es el proceso de combinar dos o más funciones para producir una nueva función. Como en matemática el resultado de una función es pasado como argumento de la siguiente.

Al tener un programa con muchas funciones que hacen cosas simples, esta herramienta se vuelve fundamental para unir de una forma más linda las funciones que tenemos.

Y sí. Aunque javascript no tenga algo lindo como Haskell cuando queremos componer funciones, podemos definir una función que haga eso:
```javascript
const compose = (...fns) =>
  fns.reverse().reduce((prevFn, nextFn) =>
    value => nextFn(prevFn(value)),
    value => value
  );
```

Para poner un ejemplo podríamos volver al ejemplo de `mapearDoble`. Supongamos que ahora queremos una función que sea `mapearDobleDelSiguiente`, con nuestra función podemos implementarla facilmente asi:
```javascript
const doble = num => num * 2
const siguiente = num => num + 1

const mapearDobleDelSiguiente = arr => arr.map( compose(doble, siguiente) )
```

Tengamos en cuenta que en una composición tipo `f.g` primero se aplicara `g` y luego `f`, lo que parece antiintuitivo, ya que se aplica en orden inverso al que leemos. Si lo queremos implementar al revés, podemos reemplazar el `reduce` por un `reduceRight` en el `compose`.

Para comparar, si quisiesemos hacer lo mismo pero sin la función `compose` deberíamos hacer algo del estilo:
```javascript
const doble = num => num * 2
const siguiente = num => num + 1

const mapearDobleDelSiguiente = arr => arr.map( num => doble(siguiente(num)) )
```

Ahora con 2 funciones no hay mucho drama, con una lista ya queda más feo de leer. Así nos ahorramos de definir algunas variables temporales y podemos ver mejor el orden de ejecución.

El tipo de salida de la función sigue siendo predecible, por ejemplo si queremos transformar un string a número y de eso sacar su doble sería:
```javascript
         parseInt            doble
String ----------- Number ----------- Number
```

La función composición quedaría:
```javascript
         composicion
String --------------- Number
```

Otra forma de componer es "encadenando" funciones:
```javascript
const transformacionLoca = str =>
 str
 .toLowerCase()
 .split(' ')
 .map(c => c.trim())
 .reverse()
 .filter(x => x.length > 3)
 .join('');
```

Si lo pensamos cuando encadenamos estamos componiendo también:
```javascript
str.toUpperCase().trim()
```

Es equivalente a escribir:
```javascript
trim(toUpperCase(str))
```

Bastante fácil de seguir no? "encadenar" funciones es una forma bastante linda de componer. Podemos usar este método incluso cuando pareciese que no:
```javascript
const otraFuncionLoca = str => {
 const trimmed = str.trim();
 const number = parseInt(trimmed);
 const nextNumber = number + 1;
 return String.fromCharCode(nextNumber);
}
```

A primera vista parecería que no pudiese componerse encadenando, pero... si lo modifico un poco:
```javascript
const otraFuncionLoca = str =>
 [str]
 .map(str => str.trim())
 .map(parseInt)
 .map(numero => numero + 1)
 .map(siguiente => String.fromCharCode(siguiente))[0];
```

Entonces sí se podía encadenar. Lo que hicimos fue meter los valores en un contenedor, un array en este caso, para poder usar su interfaz e ir transformándolo, pero siempre devolviendo un array. Vemos que no necesitamos variables intermediarias, ya que el estado se va pasando de `map` en `map`.

Incluso podría definir mi propio contenedor con su interfaz, lo me permitiría hacer otras cosas también. Pero eso ya es otro tema que veremos en una segunda parte de esta serie.

### Cerrando
Vimos muchas cosas copadas y que prometen, lamentablemente debo decirles que lo van a olvidar, a menos que... apliquen esto a sus proyectos y [practiquen!](https://github.com/timoxley/functional-javascript-workshop). Voy a estar subiendo más material así que estén atentos 🙂

Mientras tanto pueden investigar más del paradigma con las fuentes que dejo.

### Fuentes
* Wikis:
  * [Uqbar](http://wiki.uqbar.org)
* Guías:
  * [PdeP](http://www.pdep.com.ar/material/apuntes)
  * [Eric Elliot - The Rise and Fall and Rise of Functional Programming](https://medium.com/javascript-scene/the-rise-and-fall-and-rise-of-functional-programming-composable-software-c2d91b424c8c)
  * [Aprende Haskell](http://aprendehaskell.es/main.html)
* Blogs:
  * [Kris Jenkins - What Is Functional Programming?](http://blog.jenkster.com/2015/12/what-is-functional-programming.html)
  * [David Nimmo - Pure functions, and why I like them](https://dev.to/dnimmo/pure-functions-and-why-i-like-them)
  * [Johnny Reina - Intro to the fold function](https://dev.to/jreina/intro-to-the-fold-function-aka-reduce-or-aggregate)
* Videos:
  * [Brian Lonsdorf - Oh Composable World!](https://youtu.be/SfWR3dKnFIo)
  * [Quilombo Driven Deveopment](https://www.youtube.com/channel/UCiAxxHXVLBwlW72EhZweX3Q)
