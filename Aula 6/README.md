# Aula 6 - Treinamento - JavaScript e ReactJS

Como vimos ao longo das aulas anterioes, o Javascript é uma linguagem bastante dinâmica e flexível. No entando, essa liberdade tem um custo: não se tem controle sobre o fluxo dos dados.

Um alternativa para nos ajudar na tarefa de deixar o código Javascript mais compreensível e auto-documentável é o Typescript.

## Referências

- [Typescript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html)

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

## Classes
```ts
class Greeter {
  greeting: string;

  constructor(message: string) {
    this.greeting = message;
  }

  greet() {
    return "Hello, " + this.greeting;
  }
}

let greeter = new Greeter("world");
```

### Private methods and attributes
```ts
class Animal {
  private name: string;

  constructor(theName: string) {
    this.name = theName;
  }
}

new Animal("Cat").name;
```
> Property 'name' is private and only accessible within class 'Animal'.


### Abstract Classes
```ts
abstract class Department {
  constructor(public name: string) {}

  printName(): void {
    console.log("Department name: " + this.name);
  }

  abstract printMeeting(): void; // must be implemented in derived classes
}
```

## Generics

Sem o `generics`, nós podemos ou definir a função identidade com um tipo específico:

```ts
function identity(arg: number): number {
  return arg;
}
```

Ou, podemos descrevê-la usando o tipo `any`:
```ts
function identity(arg: any): any {
  return arg;
}
```

Porém, com `generics` é possível criar uma *variável de tipo* que permite que os tipos sejam repassados pelo usuário:
```ts
function identity<T>(arg: T): T {
  return arg;
}
```

### Typed Array Operations
```ts
interface Array<T> {
  length: number;

  map<U>(callbackfn: (value: T, index: number, array: T[]) => U, thisArg?: any): U[];

  filter<S extends T>(predicate: (value: T, index: number, array: T[]) => value is S, thisArg?: any): S[];

  some(predicate: (value: T, index: number, array: T[]) => unknown, thisArg?: any): boolean;
}
```

### Generic Classes
```ts
class GenericNumber<T> {
  zeroValue: T;
  add: (x: T, y: T) => T;
}
```

## Typescript com React

### Criando um App com Typescript

```sh
npx create-react-app my-app --template typescript
```

### Componentes

```ts
import React, { FunctionComponent } from 'react'

const MyComponent: FunctionComponent = () => {
  return ( ... )
}
```

Ou

```ts
import React, { FC } from 'react'

const MyComponent: FC = () => {
  return ( ... )
}
```

### Definindo tipos de propriedades

```ts
import React, { FunctionComponent } from 'react'

export interface MyComponentsProps {
  title: string,
  description: string,
  amount: number,
  createdAt: Date
}

const MyComponent: FunctionComponent<MyComponentsProps> = ({ title, description, amount, createdAt }) => {
  return ( ... )
}
```

### Hooks com tipos

```ts
import React, { useState } from 'react'

interface User {
  id: number,
  name: string,
  email: string,
  pictureUrl: string,
  birthdate: Date
}

const UsersPage: FC = () => {
  const [users, setUsers] = useState<User[]>([])

  return ( ... )
}
```

```ts
const inputEl = useRef<HTMLInputElement>(null);
```

```ts
type Theme = 'light' | 'dark';
const ThemeContext = createContext<Theme>('dark');
```

### Redux

```ts
export interface Message {
  user: string
  message: string
  timestamp: number
}

export interface ChatState {
  messages: Message[]
}
```

```ts
// src/store/chat/types.ts
export const SEND_MESSAGE = 'SEND_MESSAGE'
export const DELETE_MESSAGE = 'DELETE_MESSAGE'

interface SendMessageAction {
  type: typeof SEND_MESSAGE
  payload: Message
}

interface DeleteMessageAction {
  type: typeof DELETE_MESSAGE
  meta: {
    timestamp: number
  }
}

export type ChatActionTypes = SendMessageAction | DeleteMessageAction
```

```ts
// src/store/chat/reducers.ts

import {
  ChatState,
  ChatActionTypes,
  SEND_MESSAGE,
  DELETE_MESSAGE
} from './types'

const initialState: ChatState = {
  messages: []
}

export function chatReducer(
  state = initialState,
  action: ChatActionTypes
): ChatState {
  switch (action.type) {
    case SEND_MESSAGE:
      return {
        messages: [...state.messages, action.payload]
      }
    case DELETE_MESSAGE:
      return {
        messages: state.messages.filter(
          message => message.timestamp !== action.meta.timestamp
        )
      }
    default:
      return state
  }
}
```

## Arquivo `tsconfig.json`

```json
{
  "compilerOptions": {
    "target": "es5",
    "lib": [
      "dom",
      "dom.iterable",
      "esnext"
    ],
    "allowJs": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "preserve"
  },
  "include": [
    "src"
  ]
}
```