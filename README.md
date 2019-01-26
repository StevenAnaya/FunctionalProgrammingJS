# Functional Programming with JS

La programación funcional es un paradigma de programación, una forma diferente de entender el código. Buscamos explicar qué hay que hacer, en vez de cómo lo debemos hacer. Tampoco utilizamos clases o constructores, más bien declaramos objetos con propiedades que actualizamos sin modificar los objetos iniciales (funciones puras e inmutabilidad).


´// POO
classPerson{
        constructor(name, age) {
                this.name = name
                this.age = age
        }
        getOld() {
                this.age += 1
        }
}

let person = new Person('JuanDC', 15)
person.getOld() // 16

// Functional
const juandc = {
        name: 'JuanDC',
        age: 15
}

const getOld = person => Object.assign(
    {},
    person,
    { age: person.age + 1}
)
getOld(juandc) // 16´

´// Imperative
let array = [1,2,3]
let array2 = []

for (let i = 0; i<array.length;i++) {
        array2.push(array[i]*2) // [2,4,6]
}

// Declarative
let array = [1,2,3]
let array2 = array.map(item => item*2) // [2,4,6]´

## Funciones Algebraicas y Funciones en JavaScript

Por lo general, utilizamos las funciones de JavaScript para realizar algunas operaciones o una secuencia de procedimientos. También podemos convertir una función algebraica a una función pura de JavaScript: definimos una función F que recibe el parámetro X y devuelve el valor de X*2.

F(x) = 2 * x

Por supuesto, la manera de utilizar este tipo de funciones es asignando el valor de X:

F(2) = 2 * 2
F(2) = 4

Estas funciones siempre van a devolver el mismo resultado, es decir, si entregamos el parámetro 2, siempre vamos a recibir un 4, si entregamos el parámetro 3, siempre vamos a recibir 6 y así sucesivamente.

Las funciones en JavaScript funcionan de la misma manera:

´const double = (x) => x*2
double(2) // 4´

## Funciones Puras

Las funciones puras siempre devuelven el mismo resultado cuando reciben los mismos parámetros. En cambio, otras funciones que dependen de factores externos (como el tiempo o una petición HTTP) no siempre pueden devolver el mismo resultado aunque reciban los mismos parámetros, incluso, pueden no necesitar recibir parámetros para ejecutarse correctamente.

Ejemplos de funciones puras:

`const double = x => x*2
double(2) // siempre es 4
double(3) // siempre es 6

const isGreaterThan = (value, comparison) => value > comparison
isGreaterThan(5, 6) // siempre devuelve false
isGreaterThan(8, 6) // siempre devuelve true`

Ejemplos de funciones no puras: 

`const time = () => new Data().toLocalTimeString()
time() // siempre devuelve un resultado diferente`

## Objetos y Tipos de Memoria en JavaScript

Un objeto es una referencia a un espacio en memoria, cada vez que creamos un objeto, este se guarda en la memoria (no sabemos exactamente dónde) y podemos acceder a su valor gracias a las coordenadas.

Existen dos tipos de memoria: Stack y Heap.
La memoria Stack es mucho más rápida y nos permite almacenar los datos de forma ““ordenada”” y en JavaScript la utilizamos para guardar datos primitivos, como booleanos, números o strings. En cambio, los objetos utilizan la memoria Heap, una memoria que nos permite guardar grandes cantidades de información pero con un poco menos de velocidad.

Estos dos conceptos nos van a ayudar mucho a la hora de copiar objetos cuando utilizamos la programación funcional.

Otra forma de ver es, los tipos de datos primitivos son accedidos por su valor, es por eso que cuando se crea una constante de tipo primitivo su valor no puede ser modificado, pero por otro lado cuando creamos una constante de tipo Objeto podemos modificar todas sus propiedades ya que su valor no modificable es su *Pointer* o *Referencia*.


## Copiar y modificar Objetos en JavaScript

En JavaScript tenemos diferentes formas de copiar y modificar elementos o variables, normalmente, basta con asignar dos variables e indicar que la segunda es igual a la primera:

`let a = 1
let b = a

console.log(a, b) // 1, 1`

De esta forma podemos copiar el valor de otra variable y realizar modificaciones más adelante:

`let a = 1
let b = a
b += 1

console.log(a, b) // 1, 2`

Sin embargo, todo esto cambia cuando trabajamos con objetos. Así como aprendimos en la clase anterior, los objetos se comportan distinto al resto de datos primitivos dentro de JavaScript.

Cuando asignamos el valor de una variable de tipo objeto a otras variables, en realidad, estamos copiando la referencia al objeto inicial. Esto quiere decir que, a pesar de que modifiquemos la copia de nuestras variables de tipo objeto, en realidad, estamos modificando el objeto original y, por lo tanto, todas las variables con la referencia a este objeto que acabamos de modificar:

`let car = {
        color: 'red',
        year: 2019,
        km: 0,
}

let car2 = car
car2.color = 'blue'


console.log(car, car2) // ambos objetos tienen color azul, no solo car2`

En vez de copiar los valores de nuestros objetos, cuando utilizamos el = lo que copiamos es la referencia al objeto con sus respectivos valores. Esto lo podemos solucionar utilizando la función Object.assign:

`let car = {
        color: 'red',
        year: 2019,
        km: 0,
}

let car2 = Object.assign({} , car)
car2.color = 'blue'


console.log(car, car2) // car es de color rojo y car2 de color azul`

Sin embargo, este método no es suficiente para copiar y modificar objetos con subobjetos por el mismo problema de las referencias. La mejor manera copiar los valores de nuestros objetos en vez de sus referencias es utilizando las funciones JSON.parse y JSON.stringify:

`let car = {
        color: 'red',
        year: 2019,
        km: 0,
        owner: {
                name: 'David',
                age: 25
        }
}

let car2 = JSON.parse(JSON.stringify(car))
car2.owner.age += 1


console.log(car, car2) // el dueño de car2 es un año mayor al dueño de car`

## Utilizando Inmutabilidad en nuestras funciones

Otra característica de las funciones puras es la inmutabilidad. Si necesitamos modificar el valor de los parámetros que reciben nuestras funciones, debemos copiar el valor de los argumentos y modificar estas nuevas variables, así evitamos modificar innecesariamente variables con las que nuestras funciones puras no tienen nada que ver.

Ejemplo:

`// Con mutaciones
const addToList = (list, item, quantity) => {
    list.push({ // modificamos el argumento `list`
        item,
        quantity
    })
    return list
}

//  Sin mutaciones (inmutabilidad)
const addToList = (list, item, quantity) => {
    const newList = JSON.parse(JSON.stringify(list))
    newList.push({ // modificamos la copia del argumento
        item,
        quantity
    })


<span class="hljs-keyword">return</span> newList
}`

## Estado compartido o Share State

Shared State significa que diferentes métodos trabajan a partir de una misma variable. y, así como aprendimos en clases anteriores, cuando modificamos variables con el mismo objeto de referencia podemos encontrarnos con algunos problemas y obtener resultados inesperados a pesar de ejecutar el mismo código y recibir los mismos parámetros:

`// Intento #1
const a = {
        value: 2
}

const addOne = () => a.value += 1
const timesTwo = () => a.value *= 2


addOne()
timesTwo()


console.log(a.value) // 6


// Sin embargo, si ejecutamos las mismas funciones en orden invertido
// obtenemos resultados diferentes


timesTwo()
addOne()


console.log(a.value) // 5 !??`

Para resolver este tipo de problemas debemos utilizar la programación funcional, en vez de modificar la variable original, nuestras funciones deben copiar y modificar sus argumentos:

`const b = {
        value: 2
}

const addOne = x => Object.assign({}, x, { value: x.value + 1 })
const timesTwo = x => Object.assign({}, x, { value: x.value * 2 })


addOne(b)
timesTwo(b)


// El resultado siempre es el mismo a pesar de
// ejecutar las funciones en orden diferente


timesTwo(b)
addOne(b)


console.log(b.value)`

## Funciones Compuestas o Function Composition

Conocemos como Function Composition a las funciones que obtenemos como resultado de combinar otras dos o más funciones, el resultado de cada función es el argumento de la siguiente y así sucesivamente.

es similar a la manera en que se usan los closures. La clave allí es que tag('h1') esta retornando una función, que espera como parámetro el contenido.

`const tag = t => content => `<${t}>${content}</${t}>``

Esta funcion puedo haber sido escrita de una manera mucho mas convencional:

`function tag( t ){
    return function ( content ){
        return`<${t}>${content}</${t}>`
    }
}`

Siguiendo el ejemplo del punto anterior podemos obtener el resultado de 5 o 6 componiendo funciones de la siguiente forma:

`console.log(addOne(timesTwo(2))) // 5`

Debemos tener claro:

* Las funciones se ejecutan de derecha a izquierda según se pasen a la función compone.
* El tipo de dato que resulta de una función, debe ser el mismo que acepta como entrada la siguiente función.


## Closures en programacion funcional

Los Closures son funciones que retornan otras funciones y recuerdan el scope en el que fueron creadas, es decir, son funciones que utilizan principios de la programación funcional, no modifica el valor de variables u objetos externos, más bien, utilizan sus propias variables independientes (a partir de los parámetros que reciban estas funciones) para dar resultados correctos.

`function buildSum(a) {
	return function(b) {
		return a + b
	}
}

const addFive = buildSum(5)
console.log(addFive(5)) //10`
