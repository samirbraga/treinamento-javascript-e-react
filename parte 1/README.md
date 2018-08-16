# Workshop - JavaScript e ReactJS - Parte I
Este arquivo é dedicado ao auxílio da aula referente a parte I do Workshop - JavaScript e ReactJS. Tenha acesso a parte II [aqui](../parte%202/README.md).

## Assuntos abordados
Todos os assuntos abaixo listados serão introduzidos com uma comparação frequente entre as versões **ES5** e **ES6/7**. Os temas priorizam o que será necessário para adentrar no universo do ReactJs na parte II do Workshop.

**Será abordado:**

* [Declaração de Variáveis](#user-content-declaração-de-variáveis-)
* [Tipos de Dados](#user-content-tipos-de-dados-)
* [Arrays](#user-content-arrays-)
* [Objetos](#user-content-objetos-)
* [Manipulação de Arrays (Iterador e Métodos)](#user-content-manipulação-de-arrays-)
* [Manipulação de Objetos](#user-content-manipulação-de-objetos-)
* [Funções e Parâmentros](#user-content-funções-e-parâmentros-)
* [Spread Operator e Rest Params](#user-content-spread-operator-e-rest-params-)
* [Assincronicidade](#user-content-assincronicidade-)
* [Classes e Herança](#user-content-classes-e-herança-)
* [Manipulação do DOM](#user-content-manipulação-do-dom-)
    - O que é DOM?
    - Eventos
    - Seleção e manipulação de elementos
    - Estilos dinâmicos
    - CSS Transform
* [**HANDSON**](http://jsfiddle.net/SamirChaves/cfgwzs94/embedded/result/#Result)

## Snippets de Códigos

### Declaração de variáveis [⇧](#assuntos-abordados)

```javascript

// ES5
var name = "Bia";
window.name; // "Bia"

// ES6
const name = "Bia";
let name = "Bia";
window.name; // undefined


// Atribuição múltipla
let [name, idade] = ["Bia", 19];
name;  // Bia
idade; // 19

// Atribuição com objetos
let obj = { prop: 'value' };
let { prop } = obj;
prop; // "value"
```

```javascript
// Uso do "var"
for (var i = 0; i < 10; i++) {
    setTimeout(function() {
        console.log(i);
    }, 1000);
}

// Uso do "let"
for (let i = 0; i < 10; i++) {
    setTimeout(function() {
        console.log(i);
    }, 1000);
}
```


### Tipos de Dados [⇧](#assuntos-abordados)

```javascript
let name = "Joana";                  // string
let age = 25;                        // number
let hasBoyfriend = false;            // boolean
let props = { employed: true };      // object
let getProps = () => props;          // function
let birthDay = new Date(1994, 2, 3); // object, instância de Date
let secretProp = Symbol("password"); // symbol
let unknowValue = null;              // null
let unassignedValue;                 // undefined
```

### Arrays [⇧](#assuntos-abordados)

```javascript
const arr = [1, 2, 3, 4];            // [1, 2, 3, 4]
const mixedArr = [1, "2", {}, []];   // [1, "2", {}, []]
const emptyArr = Array(10);          // [ <10 empty items> ]
const filledArr = Array(1, 2, 3, 4); // [1, 2, 3, 4]
```

### Objetos [⇧](#assuntos-abordados)

```javascript
const properties = {
    fisrtname: "Maria",
    lastname: "Carmelita",
    skills: ['javascript', 'node.js', 'react', 'webdesign'],
    getDescription: function() {
        return `${this.firstname} ${this.lastname}, ${this.age} anos`;
    }
};

properties.firstname; // "Maria"
properties['lastname']; // "Carmelita"

properties.age = 20;

// ES6
let newProp = 'married';

newProperties = {
    ...properties,
    [newProp]: false
};

newProperties.married; // false
```

### Manipulação de Arrays [⇧](#assuntos-abordados)

```javascript
let nums = [1, 2, 3, 4, 5, 6];

nums.forEach(num => {
    console.log(num);
});

let squares = nums.map(num => num * num);
squares; // [ 1, 4, 9, 16, 25, 36 ]

let even = nums.filter(num => num % 2 == 0);
even; // [ 2, 4, 6 ]
```

### Manipulação de Objetos [⇧](#assuntos-abordados)

```javascript
let pessoa = { name: "Marcela", age: 29 };
Object.keys(pessoa); // [ "name", "age" ]
Object.values(pessoa); // [ "Marcela", "29" ]

// Shorthand

// ES5
function makeUser(name, age) {
  return {
    name: name,
    age: age
    // ...other properties
  };
}

let user = makeUser("John", 30);
user.name; // John


// ES6
function makeUser(name, age) {
  return {
    name, // same as name: name
    age   // same as age: age
  };
}
```

### Funções e Parâmentros [⇧](#assuntos-abordados)

```javascript
function print(text) {
    console.log(text);
}

print("Hello World"); // "Hello World"

/*
 * Arrow Functions
**/

// ES5
var print = function(text) {
    console.log(text);
}

// ES6
const print = (text) => {
    console.log(text);
}

// ou
const print = text => {
    console.log(text);
}

// ou ainda
const print = text => console.log(text);
```

```javascript
// Simulando parâmetros nomeados com objetos
let vehicle = {
    kind: 'car',
    capacity: 4,
    horses: 6,
    year: 2015
};

const getVehicleDescription = ({ kind, capacity, horses, year }) => (
    `This ${kind} with capacity for ${capacity} people has ${horses} horses and is ${year - new Date().getFullYear()} years old.`
);

getVehicleDescription(vehicle);
// This car with capacity for 4 people has 6 horses and are 3 years old.
```

### Spread Operator e Rest Params [⇧](#assuntos-abordados)

```javascript
/*
 * Spread Operator
**/

// ARRAYS
const names = ["Carla", "José", "Antônio", "Maria"];
console.log(names); // [ 'Carla', 'José', 'Antônio', 'Maria' ]

// ES5
console.log(names[0], names[1], names[2], names[3]); //  Carla José Antônio Maria
// ou
console.log.apply(console, names); // Carla José Antônio Maria

// ES6
console.log(...names); // Carla José Antônio Maria


// OBJETOS

// ES5
let part1 = { name: 'Samir', age: 2 };
let part2 = { age: 31 };

let pessoa = Object.assign(part1, part2);
pessoa; // {"name":"Samir","age":31}

// ES6
let part1 = { name: 'Samir', age: 2 };
let part2 = { age: 31 };

let pessoa = { ...part1, ...part2 };
pessoa; // {"name":"Samir","age":31}
```

```javascript
/*
 * Rest Params
**/

// ES5
function join() {
    var initialString = "";
    for (var i = 0; i < arguments.length; i++) {
        initialString += arguments[i];
    }

    return initialString;
}


// ES6
function join(...strings) {
    var initialString = "";
    for (let str of strings) {
        initialString += arguments[i];
    }

    return initialString;
}
```

### Assincronicidade [⇧](#assuntos-abordados)

```javascript
console.log("Primeiro");

setTimeout(function() {
  console.log("Terceiro");
}, 1000);

console.log("Segundo");
```

### Classes e Herança [⇧](#assuntos-abordados)

```javascript
// ES5
var Pessoa = function(name, age) {
    this.name = name;
    this.age = age;
    this.getDescription = function() {
        return `${this.name}, ${this.age} anos`;
    };
};

var Camila = new Pessoa('Camila', 34);
Camila.getDescription(); // 'Camila, 34 anos';

// ES6
class Pessoa {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    getDescription() {
        return `${this.name}, ${this.age} anos`;
    };
};

const Camila = new Pessoa('Camila', 34);
Camila.getDescription(); // 'Camila, 34 anos';
```

```javascript
// Herança
class Action {
    constructor() {
        this.done = true;
    }

    undo() {
        this.done = false;
    }
}

class Draw extends Action {
    constructor() {
        super();
    }
}

const myDraw = new Draw();

myDraw.done; // true
myDraw.undo();
myDraw.done; // false
```

### Manipulação do DOM [⇧](#assuntos-abordados)

```javascript
// Selecionando
let div = document.querySelector("div");

// Estilizando
div.style.opacity = 0;

// Adicionando Eventos
div.addEventListener("click", e => {
    console.log("Clicado!");
});
```
