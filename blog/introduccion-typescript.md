# Introduccion a TypeScript

Si bien sabemos que _[todo el software esta roto](https://www.youtube.com/watch?v=0riSpvps4mA)_, podemos utilizar herramientas que nos permitan reducir los errores que podamos cometer a la hora de programar. A lo largo de este post vamos a ver como TypeScript es una herramienta que nos puede ayudar a escribir código "menos roto" en JavaScript.

> Este post es resultado de lo visto durante la primera sesión del ["círculo de aprendizaje"](https://github.com/circulo-aprendizaje/organizacion) de [TypeScript + Programación funcional](https://github.com/circulo-aprendizaje/fp-ts) cuyo video puede encontrarse en [Youtube](https://www.youtube.com/watch?v=SYybejXWP7Q)

## Que es TypeScript?

TypeScript es un lenguaje que compila a JavaScript. Otros lenguajes como [CoffeScript](http://coffeescript.org/) o [Dart](https://www.dartlang.org/) también lo hacen, pero a diferencia de estos, TypeScript se puede considerar como una **extensión del lenguaje** más que un lenguaje nuevo. De la misma manera que podemos considerar que ES6 es una extensión de ES5.

Vamos a tratar de aclarar la definición anterior con la siguiente imagen:

![conjuntos](https://image.ibb.co/gq2jhm/Intro_Ts_Image.png)

Vemos como TypeScript contiene a ES6 y ES5. Esto quiere decir que cualquier funcionalidad que desarollemos en ES6/ES5 será un código de TypeScript válido, porque es _compatible hacia atrás_ con dichas versiones.

## Transpiladores

El lenguaje JavaScript evoluciona para satifacer a las necesidades de los desarrolladores, pero las últimas versiones del estandar no siempre están implementadas en los navegadores o en Node. Es por eso que si queremos usar los últimos features debemos utilizar un transpilador (como [babel](https://github.com/babel/babel)) en una etapa de compilación para convertir nuestro código en algo que funcione en los distintos entornos.

Y qué tiene que ver todo esto con TypeScript? Al ser compatible con ES6, como mínimo se podría con ese propósito. La ventaja que posee sobre otros transpiladores, es que TypeScript toma una postura más conservadora. Sólo estarán disponibles features que estén en una etapa avanzada de aprobación, con lo cual es muy factible que lleguen al estándar.

### Cuál es el valor agregado de TypeScript?

Ya entendimos que una de sus funcionalidades es actuar como un transpilador, pero si sólo hiciera eso, podríamos seguir utilizando babel.

Fundamentalmente, TypeScript le incorpora dos nuevas características a JavaScript:

* Chequeo estático de tipos
* Annotations (ya lo veremos en próximos posts)

## TypeScript en acción

JavaScript es un lenguage débilmente tipado. Esto lo podemos observar de la siguiente manera:

```javascript
> typeof 2
< "number"
> typeof "hola"
< "string"
> typeof {que: "tal"}
< "object"
```

JavaScript entiende que 2 es un número y "hola" un string, pero al no ser un lenguaje fuertemente tipado, no hay nada que nos prohiba hacer esto:

```javascript
var miVariable = 2;
miVariable = "hola";

// o cosas como

fs.readFile(9, function(err, buffer) {
  // ...
});
```

Probablemente, no sea lo que queríamos hacer, y muchas veces resultará en **errores silenciosos**.

Allí es donde los tipos nos pueden ayudar. Los tipos son **contratos** y de la misma manera que necesitamos leyes (contratos sociales) para no vivir en anarquía, los tipos nos permiten mantener la paz en nuestro código.

Veamos otro ejemplo:

```javascript
function sumar(a, b) {
  return a + b;
}
```

Por lo que vimos en los ejemplos anteriores, nada nos impediría ejecutar lo siguiente:

```javascript
sumar("Nombre-", "Apellido");
> "Nombre-Apellido"

sumar("1","2");
> "12"
```

Históricamente tratamos de resolver el inconveniente anterior con **contratos** dentro de los comentarios. En este caso, queremos aclarar la intención de la función es sumar números.

```javascript
/* Suma dos numeros
 * @param number a : el primer numero
 * @param number b : el segundo numero
 * @return number : la suma del primero con el segundo
 */
function sumar(a, b) {
  return a + b;
}
```
Pero este tipo de contrato es muy débil. Es como tener la ley pero sin la 👮‍ que nos ayude a garantizar el orden. Necesitamos TypeScript!

Veamos como quedaría el ejemplo anterior utilizando TypeScript

```javascript
function sumar(a: number, b: number): number {
  return a + b;
}
```

Si intentaramos sumar con valores no numéricos, TypeScript nos avisará que no estamos cumpliendo con las reglas establecidas! Además, nuestro código queda auto-documentado, no necesitamos más comentarios.

Pueden ver el ejemplo haciendo click [acá](<http://www.typescriptlang.org/play/index.html#src=function%20sumar(a%3A%20number%2C%20b%3A%20number)%3A%20number%20%7B%0D%0A%20%20return%20a%20%2B%20b%3B%0D%0A%7D%0D%0A%0D%0A%2F%2Fdescomenten%20esto!%20sumar(%221%22%2C2)>)

### La madre de todos los linters 
Desde la versión 2.3, se puede agregar la opción @ts-check. Con esto, TypeScript no sólo hará un chequeo de tipos, sino que también, funcionará como linter!


## [Tipos Básicos](https://youtu.be/SYybejXWP7Q?t=2072)

TypeScript nos permite declarar los siguientes tipos:

* **boolean**

`let miVarbiable: boolean = true`

* **number**

`let numero: number = 10`

* **string**

`let color: string = "azul"`

* **array**

`let lista: number[] = [1, 2, 3]`

* **enum**

```javascript
enum  Color {Rojo, Verde, Azul};
let c: Color = Color.Green;
```

* **any**

Útil cuando se necesita probar algo rápidamente, o en el proceso de migrar gradualmente código de JavaScript puro a TypeScript.

`let noEstoySeguroDeTuTipo: any`

> Se recomienda el uso de la opcion noImplicitAny para mostrar explicitamente donde hay dudas sobre el tipo a usar

Pueden chequear todos los tipos básicos [aqui](http://www.typescriptlang.org/docs/handbook/basic-types.html)

## [Clases](https://youtu.be/SYybejXWP7Q?t=2260)

TypeScript también nos va a ayudar a evitar errores comunes cuando utilizamos clases. Por ejemplo, nos obliga a declarar las propiedades al inicio, de manera tal que podamos ver facilmente cuales propiedades pueden ser utilizadas. Y además, podemos declarar la **visiblidad** (`public`, `private`) de funciones y propiedades.

```javascript
class Perro {
	public readonly nombre: string
	public edad: number

	constructor(nombre, edad){
	}

	public ladrar(){
		return "woof!"
	}

	private _calcularEdadHumana(){
		return this.edad * 7;
	}
}
```

## [Interfaces](https://youtu.be/SYybejXWP7Q?t=3500)

También podemos tipar las propiedades de los objetos. Esto se logra a través del uso de interfaces.

```javascript
interface IUsuario {
  nombre: string;
  apellido: string;
  edad: number;
  direccion: iDireccion;
}

interface IDireccion {
  calle: string;
  numero: number;
  codigoPostal: number;
}

const usuario: IUsuario = {
  nombre: "Leonel",
  apellido: "Messi",
  edad: 10,
  direccion: {
    calle: "Calle falsa",
    numero: 123,
    codigoPostal: 1234
  }
};
```

Por default todas las propiedades serán obligatorias a menos contengan un signo de pregunta al final de su nombre.

```javascript
interface IUsuario {
  nombre: string;
  apellido: string;
  edad: number;
  direccion: IDireccion;

  hobbies?: string[];
}
```

> Es recomendable utilizar como prefijo el caracter **I** para el nombre de cada interfaz.

## [Funciones](https://youtu.be/SYybejXWP7Q?t=4381)

También se podrán tipar funciones con una sintaxis muy parecida a las **arrow functions**.

```javascript
interface iUsuario {
  nombre: string;
}

type iCb = (err?: string, user?: iUsuario) => void;

function getUser(cb: iCb) {
  //algo asincronico...
  cb(null, { nombre: "Hernan" });
}

getUser(function(err, user) {
  if (err) {
    console.log(err.toUpperCase());
  }
  console.log(user);
});
```

Cabe destacar que dentro de getUser, TypeScript puede **inferir** automaticamente el tipo de err y user, no hace falta explicitarlo nuevamente.

## Inmutabilidad
Desde la incoporacion de **const** en ES6, existen confusiones sobre la mutabilidad de los valores de las variables en objetos/arrays. TypeScript soluciona esto mediante el uso de **readyonly**

```javascript
type Saludo = {
  valor: string,
  readonly tipo: 'amistoso' | 'irrespetuoso'
}


let saludo:Saludo = {valor: 'Hola, cómo estás?', tipo: 'amistoso'};
saludo.valor = 'Buenas'; // me deja
saludo.tipo = 'irrespetuoso'; // no me deja
```

## Conclusiones

A lo largo de este post pudimos ver como TypeScript puede ayudarnos a crear aplicaciones menos rotas mediante el uso de tipos explícitos, linting, inferencia de tipos, etc. Además nos ahorra el uso de babel ya que por ser compatible con ES6/ES5, funciona como un transpilador. Vimos también, como podemos transmitir mejor nuestros contratos y hacer que el código quede auto-documentado.

Si quieren más info sobre TypeScript entren [aquí](http://www.typescriptlang.org/) (en inglés) y además pueden ver la sesión entera del círculo mediante este [link](https://www.youtube.com/watch?v=SYybejXWP7Q).

Dejen sus dudas/sugerencias en los comentarios!
