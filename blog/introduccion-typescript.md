# Introduccion a TypeScript
Si bien sabemos que *[todo el software esta roto](https://www.youtube.com/watch?v=0riSpvps4mA)*, podemos utilizar herramientas que nos permitan reducir los errores que podamos cometer a la hora de programar. A lo largo de este post vamos a ver como TypeScript es una herramienta que nos puede ayudar a escribir código "menos roto" en JavaScript. 

> Este post es resultado de lo visto durante la primera sesión del ["círculo de aprendizaje"](https://github.com/circulo-aprendizaje/organizacion) de [TypeScript + Programación funcional](https://github.com/circulo-aprendizaje/fp-ts) cuyo video puede encontrarse en [Youtube](https://www.youtube.com/watch?v=SYybejXWP7Q)

## Que es TypeScript?
TypeScript es un lenguaje que compila a JavaScript. Otros lenguajes como [CoffeScript](http://coffeescript.org/) o [Dart](https://www.dartlang.org/) también lo hacen, pero a diferencia de estos, TypeScript se puede considerar como una **extensión del lenguaje** más que un lenguaje nuevo. De la misma manera que podemos considerar que ES6 es una extensión de ES5.

Vamos a tratar de aclarar la definición anterior con la siguiente imagen:

![conjuntos](https://image.ibb.co/gq2jhm/Intro_Ts_Image.png)


Vemos como TypeScript contiene a ES6 y ES5. Esto quiere decir que cualquier funcionalidad que desarollemos en ES6/ES5 será un código de TypeScript válido, porque es *compatible hacia atrás* con dichas versiones. 

## Transpiladores
El lenguaje JavaScript evoluciona para satifacer a las necesidades de los desarrolladores, pero las últimas versiones del estandar no siempre están implementadas en los navegadores o en Node. Es por eso que si queremos usar los últimos features debemos utilizar un transpilador (como [babel](https://github.com/babel/babel)) en una etapa de compilación para convertir nuestro código en algo que funcione en los distintos entornos.

Y qué tiene que ver todo esto con TypeScript? Al ser compatible con ES6, como mínimo se podría utilizarlo como un transpilador. La ventaja que posee sobre otros transpiladores, es que TypeScript toma una postura más conservadora. Sólo estarán disponibles features que estén en una etapa avanzada de aprobación, con lo cual es muy factible que lleguen al estándar.


## Entonces.. cuál es el valor agregado de TypeScript?
Ya entendimos que una de sus funcionalidades es actuar como un transpilador, pero si sólo hiciera eso, podríamos seguir utilizando babel. 

Fundamentalmente, TypeScript le incorpora dos nuevas características a JavaScript:

* Chequeo estático de tipos
* Annotations (ya lo veremos en próximos posts)

### Chequeo dinámico vs Chequeo estático
Javascript posee un **chequeo de tipos dinámico**. Esto quiere decir que el motor de JavaScript tratará de hacer las operaciones necesarias con los valores de las variables en tiempo de ejecución sin hacer un chequeo previo. 

Pero esta implementación tiene algunas desventajas. Cuando la cantidad de código en el proyecto empieza a crecer y hay varias personas desarrollando al mismo tiempo, es necesario aclarar de alguna forma cuales son los **contratos** a cumplir para el uso de nuestras funciones. De otra forma, *damos lugar a que se empiecen a infererir según el entendimiento de cada persona*.

Veamos todos estos inconvenientes con una simple funcion **sumar**:

```javascript
function sumar (a,b){
	return a + b;
}
```

El objetivo de nuestra implementacion es sumar dos números. Esto se ve claramente, ustedes qué piensan? Observemosla un poco mejor, de qué otra forma otros desarrolladores podrían usar nuestra función? 

```javascript
sumar(2,2)
> 4
sumar("hola,", "chau!")
> " hola,chau! "
sumar(1,"2")
> "12"

```
Todos los usos anteriores son correctos, ya que están cumpliendo con el contrato que especifica nuestra función "sumar dos elementos que lo puedan hacer a través del operador +"

Cuáles son las alternativas comunes para expresar mejor nuestra **intención**?

```javascript
# Documentacion - Comentarios
/* Suma dos numeros
 * @param number a : el primer numero 
 * @param number b : el segundo numero
 * @return number : la suma del primero y el segundo
 */ 
function sumar (a,b){
	return a + b;
}

# Nombre de variables con prefijo (n = number, s = string , o = object...)
function sumar (nPrimerNumero,nSegundoNumbero){
	return a + b;
}
```

Si bien las soluciones anteriores determinan explicitamente como deberia usarse la función, nada impide que sea utilizadas de manera incorrecta. 

Aquí es donde entra en juego la importancia del uso de TypeScript, ya que agregará un **chequeo de tipos estático**. Ahora en nuestro código deberemos explicitar los tipos de los valores de las variables y si detecta algun error de tipos, veremos el error en tiempo de compilación.

Veamos como queda el ejemplo anterior utilizando TypeScript

```javascript
function sumar (a:number,b:number):number{
	return a + b;
}
```
## Ventajas
A partir de lo visto anteriomente vemos que:

* Transmitimos mucho mejor la intención del código.

* El código se "auto documenta". Ya no tenemos que escribir comentarios para aclarar tipos.

* Podemos capturar bugs en forma temprana.

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
TypeScript también nos va a ayudar a evitar errores comunes cuando utilizamos clases. Por ejemplo, nos obliga a declarar las propiedades al inicio, de manera tal que podamos ver facilmente cuales propiedades pueden ser utilizadas. Y además, podemos declarar la **visiblidad** (`public`, `private`) de funciones y propiedades, asi como también si las propiedades son **inmutables** (`readonly`). 

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
    direccion: iDireccion
}

interface IDireccion { 
    calle: string;
    numero: number;
    codigoPostal: number;
}

const usuario: IUsuario = {
    nombre: "Leonel", 
    apellido: "Messi",
    edad:10,
    direccion: {
        calle: "Calle falsa", 
        numero: 123, 
        codigoPostal: 1234
    }
}
```

Por default todas las propiedades serán obligatorias a menos contengan un signo de pregunta al final de su nombre.

```javascript
interface IUsuario { 
    nombre: string;
    apellido: string;
    edad: number;
    direccion: IDireccion;
    
    hobbies? :string[];
}
```

>  Es recomendable utilizar como prefijo el caracter **I**  para el nombre de cada interfaz.


## [Funciones](https://youtu.be/SYybejXWP7Q?t=4381)
También se podrán tipar funciones con una sintaxis muy parecida a las **arrow functions**.

```javascript
interface iUsuario { 
    nombre: string;
}

type iCb = (err? :string , user? :iUsuario) => void

function getUser(cb: iCb) { 
    //algo asincronico...
    cb(null, { nombre: 'Hernan' });
}

getUser(function (err, user) {
    if (err) { 
        console.log(err.toUpperCase());
    }
    console.log(user)
})
```
Cabe destacar que dentro de getUser, TypeScript puede **inferir** automaticamente el tipo de err y user, no hace falta explicitarlo nuevamente.  


## Conclusiones 
A lo largo de este post pudimos ver como TypeScript puede ayudarnos a crear aplicaciones menos rotas mediante el uso de tipos explícitos, algunas reestricciones en el uso de clases, tipados de funciones y el uso de interfaces. Además nos ahorra el uso de babel ya que por ser compatible con ES6/ES5, funciona como un transpilador. Vimos también, como podemos transmitir mejor nuestros contratos y hacer que el código quede auto-documentado.

Si quieren más info sobre TypeScript entren [aquí](http://www.typescriptlang.org/) (en inglés) y además pueden ver la sesión entera del círculo mediante este [link](https://www.youtube.com/watch?v=SYybejXWP7Q). 

Dejen sus dudas/sugerencias en los comentarios!
