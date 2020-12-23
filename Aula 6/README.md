# Aula 6 - Treinamento - JavaScript e ReactJS

Como vimos ao longo das aulas anterioes, o Javascript é uma linguagem bastante dinâmica e flexível. No entando, essa liberdade tem um custo: não se tem controle sobre o fluxo dos dados.

Um alternativa para nos ajudar na tarefa de deixar o código Javascript mais compreensível e auto-documentável é o Typescript.

## Assuntos Abordados

- Introdução ao Typescript
  * Tipos básicos e suas operações
  * Interfaces
  * Funções
  * Classes
  * Generics
- Typescript com React
  * Criando componentes tipados
  * Tipos com Hooks
  * Criando modelos de dados
  * Tipos com Axios
- Encerramento
  * Revisão Geral
  * Encaminhamentos e fontes de estudo

## Tipos Básicos

### Boolean
```ts
let isDone: boolean = false;
```

### Number
```ts
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
let big: bigint = 100n;
```

### String
```ts
let color: string = "blue";
color = 'red';
```

### Array
```ts
let list: number[] = [1, 2, 3];

let list: Array<number> = [1, 2, 3];
```

### Tuple
```ts
let x: [string, number];
// Initialize it
x = ["hello", 10]; // OK
// Initialize it incorrectly
x = [10, "hello"]; // Error
```

### Enum
```ts
enum Color {
  Red,
  Green,
  Blue,
}
let c: Color = Color.Green;
```

### Unknown
```ts
let notSure: unknown = 4;
notSure = "maybe a string instead";

// OK, definitely a boolean
notSure = false;
```

### Any
```ts
let looselyTyped: any = 4;
// OK, ifItExists might exist at runtime
looselyTyped.ifItExists();
// OK, toFixed exists (but the compiler doesn't check)
looselyTyped.toFixed();

let strictlyTyped: unknown = 4;
strictlyTyped.toFixed();
// Object is of type 'unknown'.
```

### Void
```js
function warnUser(): void {
  console.log("This is my warning message");
}
```

### Never
```js
// Function returning never must not have a reachable end point
function error(message: string): never {
  throw new Error(message);
}

// Inferred return type is never
function fail() {
  return error("Something failed");
}

// Function returning never must not have a reachable end point
function infiniteLoop(): never {
  while (true) {}
}
```

## Interfaces

```ts
interface LabeledValue {
  label: string;
}

function printLabel(labeledObj: LabeledValue) {
  console.log(labeledObj.label);
}

let myObj = { size: 10, label: "Size 10 Object" };
printLabel(myObj);
```

### Optional Properties
```ts
interface SquareConfig {
  color?: string;
  width?: number;
}
```

### Readonly properties
```ts
interface Point {
  readonly x: number;
  readonly y: number;
}

let p1: Point = { x: 10, y: 20 };
p1.x = 5; // error!
```

### Function Types
```ts
interface SearchFunc {
  (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;

mySearch = function (source: string, subString: string) {
  let result = source.search(subString);
  return result > -1;
};
```

### Class Types
```ts
interface ClockInterface {
  currentTime: Date;
  setTime(d: Date): void;
}

class Clock implements ClockInterface {
  currentTime: Date = new Date();
  setTime(d: Date) {
    this.currentTime = d;
  }
  constructor(h: number, m: number) {}
}
```


## Functions
```ts
function add(a: number, b: number): number {
  return a + b
}
```

### Rest Params
```ts
function buildName(firstName: string, ...restOfName: string[]) {
  return firstName + " " + restOfName.join(" ");
}
```

## Union and Intersection Types

```ts
/**
 * Takes a string and adds "padding" to the left.
 * If 'padding' is a string, then 'padding' is appended to the left side.
 * If 'padding' is a number, then that number of spaces is added to the left side.
 */
function padLeft(value: string, padding: string | number) {
  // ...
}
```