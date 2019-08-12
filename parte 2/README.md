# Workshop - JavaScript e ReactJS - Parte II
Este arquivo é dedicado ao auxílio da aula referente a parte II do Workshop - JavaScript e ReactJS. Tenha acesso a parte I [aqui](../parte%201/README.md).


## Assuntos Abordados
Todos os assuntos abaixo listados serão introduzidos a partir de um certo conhecimento de javascript, podendo ser obtido na Parte I do Workshop.

**Será abordado:**

* [O que o React tem demais?](#o-que-o-react-tem-demais-)
* [Web Components](#web-components-)
* [React Components](#react-components-)
* [Como o React funciona?](#como-o-react-funciona?-)
    - O ambiente de desenvolvimento
    - Webpack, Babel e Browserify
* [JSX](#jsx-)
* [Virtual DOM](#virtual-dom-)
* [Renderizando Componentes](#renderizando-componentes-)
* [Props](#props-)
* [Statefull e Stateless components](#statefull-e-stateless-components-)
* [Lists e keys](#lists-e-keys-)
* [Handling Events](#handling-events-)
* [Ref](#ref-)
* [Styling](#styling-)
* [Curiosidades](#curiosidades-)
* Introdução sobre o React Native
* [**HANDSON**](https://codesandbox.io/s/61q0mzrvlk)

## Snippets de Códigos

### O que o React tem demais? [⇧](#assuntos-abordados)

#### JQUERY ####
```html
<button class="botao" data-description="Eu sou um botão!">
    CLIQUE-ME!
</button>
```

```javascript
// Elemento já existente no DOM
// Manipulação posterior
$('.botao').on('click', function() {
    var self = $(this);
    alert(self.attr('data-description'));
});
```

#### REACT ####
```javascript

// Elemento criado dinamicamente
// Manipulação concorrente
class Botao extends React.Component {
    alert = () => {
        alert(this.props.description)
    }

    render() {
        <button onClick={this.alert}>
            {this.props.children}
        </button>
    } 
}
```

```jsx
<Botao description="Eu sou um botão!">
    CLIQUE-ME!
</Botao>
```

### Web Components [⇧](#assuntos-abordados)
> *Web components are a set of web platform APIs that allow you to create new custom, reusable, encapsulated HTML tags to use in web pages and web apps. Custom components and widgets build on the Web Component standards, will work across modern browsers, and can be used with any JavaScript library or framework that works with HTML.*

[*EXEMPLO DO GOOGLE*](https://developers.google.com/web/fundamentals/web-components/customelements?hl=pt-br)

```javascript
class AppDrawer extends HTMLElement {
  get open() {
    return this.hasAttribute('open');
  }

  set open(val) {
    if (val) {
      this.setAttribute('open', '');
    } else {
      this.removeAttribute('open');
    }
    this.toggleDrawer();
  }

  get disabled() {
    return this.hasAttribute('disabled');
  }

  set disabled(val) {
    if (val) {
      this.setAttribute('disabled', '');
    } else {
      this.removeAttribute('disabled');
    }
  }

  constructor() {
    super();

    this.addEventListener('click', e => {
      if (this.disabled) {
        return;
      }
      this.toggleDrawer();
    });
  }

  toggleDrawer() {
    ...
  }
}

window.customElements.define('app-drawer', AppDrawer);
```

```html
<div>
    <app-drawer></app-drawer>
</div>
```

#### Shadow DOM ####

```javascript
const header = document.createElement('header');
const shadowRoot = header.attachShadow({ mode: 'open' });
shadowRoot.innerHTML = '<h1>Hello Shadow DOM</h1>';
```

### React Components
```javascript
class ListItem extends React.Component {
    render() {
        return React.createElement(
            'li',
            {
                className: 'list-item',
            },
            `ITEM - ${this.props.title}`
        );
    }
}

class List extends React.Component {
    render() {
        return React.createElement(
            'ul',
            {
                className: 'classe-elemento',
                id: 'id-elemento'
            },
            React.createElement(
                ListItem,
                {
                    title: 'TASk TO DO 1',
                }
            ),
            React.createElement(
                ListItem,
                {
                    title: 'TASk TO DO 2',
                },
            )
        );
    }
}
```

### Como o React funciona? [⇧](#assuntos-abordados)

#### Webpack ####

![Webpack](https://webpack.github.io/assets/what-is-webpack.png)

#### Babel ####

![Babel](babel.jpg)

### JSX  [⇧](#assuntos-abordados)
```jsx
// Um novo "tipo" está agora disponível
let div = <div></div>;
```

```jsx
class ListItem extends React.Component {  
    render() {
    	return (
            <li className="list-item">
                ITEM - {this.props.title}
            </li>
        );
    }
}

class List extends React.Component {  
    render() {
    	return (
            <ul>
            	<ListItem title="TASk TO DO 1" />
            	<ListItem title="TASk TO DO 2" />
            </ul>
        );
    }
}
```

```jsx
// React.Fragment
let divs = (
    <React.Fragment>
        <div></div>
        <div></div>
    </React.Fragment>
);
```

### Virtual DOM [⇧](#assuntos-abordados)

1) Cria uma representação do DOM na linguagem do Virtual DOM;
2) Manda o Virtual DOM gerar o DOM real;
3) Quando houver alteração no model, em vez de atualizar a DOM real você simplesmente manda regerar toda a representação Virtual DOM passando o novo estado do model como parâmetro;
4) Então você usa o mecanismo de comparação para obter as diferenças entre a representação do Virtual DOM e o DOM real;
5) E usa o mecanismo de patch que vai atualizar o DOM real conforme as diferenças observadas.

![Virtual DOM](https://cdn-images-1.medium.com/max/800/1*CqdIWZy0NMPQhYx2rKzo9g.png)

### Renderizando Componentes [⇧](#assuntos-abordados)

```jsx
class App extends Component { ... }

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```
```jsx
// Renderização Condicional
class LoginControl extends React.Component {
    ...

    render() {
        const isLoggedIn = this.state.isLoggedIn;
        let button;

        if (isLoggedIn) {
            button = <LogoutButton onClick={this.handleLogoutClick} />;
        } else {
            button = <LoginButton onClick={this.handleLoginClick} />
        }

        return (
            <div>
                ...
                {button}
            </div>
        );
    }
}
```

### Props [⇧](#assuntos-abordados)

#### Atributos ####

```html
<!--
    Tag: div
    Atributos: id, class, data-title 
-->
<div id="div-1" class="container" data-title="example of title" ></div>
```
```javascript
let div = document.getElementById("div-1");

div.getAttribute('data-title'); // example of title
div.getAttribute('class'); // container
```

#### Props ####

```jsx
<App theme="dark">
    ...
</App>
```

```jsx
const App = props => (
    <div className={`app-wrapper ${props.theme}`}>
        ...
    </div>
);
```
ou
```jsx
class App extends Component {
    render() {
        return (
            <div className={`app-wrapper ${this.props.theme}`}>
                ...
            </div>
        );
    }
}
```

### Statefull e Stateless components [⇧](#assuntos-abordados)

#### State ####

```jsx
// Stateless (Sem estado)

const MyButton = props => (
    <button className="my-button" >
        {props.children}
    </button>
);
```

```jsx
// Statefull (Com estado)

class MyButton extends Component {
    constructor(props) {
        super(props);

        this.state = {
            pressed: false
        };

        this.togglePress = this.togglePress.bind(this);
    }

    togglePress() {
        this.setState({
            togglePress: !this.state.togglePress
        });
    }

    render() {
        return (
            <button
                onPress={this.togglePress}
                className={`my-button ${this.state.pressed ? 'pressed' : ''}`}
            >
                {props.children}
            </button>
        )
    }
}
```

#### Life Cicle ####

```js
class MyComponent extends Component {
    UNSAFE_componentWillMount() { ... }
    componentDidMount() { ... }

    UNSAFE_componentWillUnmount() { ... }
    componentDidUnmount() { ... }

    UNSAFE_componentWillReceiveProps(nextProps)

    UNSAFE_componentWillUpdate(nextProps, nextState) { ... }
    componentDidUpdate(prevProps, prevState, snapshot) { ... }

    shouldComponentUpdate(nextProps, nextState) { ... }

    ...
}
```

### Lists e keys [⇧](#assuntos-abordados)

```jsx
// Renderizando Listas
class List extends Component {
    constructor(props) {
        super(props);

        this.state = {
            todos: [
                'Me organizar',
                'Ler o livro de CG',
                'Fazer algum dos items acima'
            ]
        };
    }

    render() {
        return (
            <ul>
                {this.state.todos.map(todo => (
                    <li key={todo} >{todo}</li>
                ))}
            </ul>
        );
    }
}
```

### Handling Events [⇧](#assuntos-abordados)

```jsx
// Capturando eventos
class List extends Component {
  constructor(props) {
    super(props);

    this.state = {
      todos: [
        'Me organizar',
        'Ler o livro de CG',
        'Fazer algum dos items acima'
      ]
    };
  }

  addTodo() {
    this.setState(prevState => {
      let { todos } = prevState;
      todos.push('Uma nova tarefa.');
      return {
        todos
      };
    });
  }

  render() {
    return (
      <div>
        <ul>
          {this.state.todos.map(todo => (
            <li key= { todo } >
              { todo }
            </li>
          ))}
        </ul>
        <button onClick={this.addTodo.bind(this)}>Adicionar</button>
      </div>
    );
  }
}
```

```jsx
// Capturando eventos
class List extends Component {
  constructor(props) {
    super(props);

    this.state = {
      todos: [
        "Me organizar",
        "Ler o livro de CG",
        "Fazer algum dos items acima"
      ],
      newTodo: ""
    };
  }

  handleInput(event) {
    this.setState({
      newTodo: event.target.value
    });
  }

  addTodo() {
    this.setState(prevState => {
      let { todos } = prevState;

      if (this.state.newTodo) {
        todos.push(this.state.newTodo);
      }

      return {
        todos
      };
    });
  }

  render() {
    return (
      <div>
        <ul>{this.state.todos.map(todo => <li key={todo}>{todo}</li>)}</ul>
        <input type="text" onInput={this.handleInput.bind(this)} />
        <button onClick={this.addTodo.bind(this)}>Adicionar</button>
      </div>
    );
  }
}
```

### Ref [⇧](#assuntos-abordados)
```jsx
// Capturando eventos
class List extends Component {
  constructor(props) {
    super(props);

    this.state = {
      todos: [
        "Me organizar",
        "Ler o livro de CG",
        "Fazer algum dos items acima"
      ],
      newTodo: ""
    };

    this.textInput = React.createRef();
  }

  handleInput(event) {
    this.setState({
      newTodo: event.target.value
    });
  }

  addTodo() {
    this.setState(prevState => {
      let { todos } = prevState;

      if (this.state.newTodo) {
        todos.push(this.state.newTodo);
        this.textInput.current.value = "";
      }

      return {
        todos
      };
    });
  }

  render() {
    return (
      <div>
        <ul>{this.state.todos.map(todo => <li key={todo}>{todo}</li>)}</ul>
        <input ref={this.textInput} type="text" onInput={this.handleInput.bind(this)} />
        <button onClick={this.addTodo.bind(this)}>Adicionar</button>
      </div>
    );
  }
}
```

### Styling [⇧](#assuntos-abordados)

```jsx
// Estilizando componentes com objetos
class Element extends Component {
  render() {
    return (
      <div
        style={{
            width: '200px',
            height: '220px',
            backgroundColor: 'cornflowerblue'
        }} />
    );
  }
}
```
---------
#### JSX ####
```jsx
import './style.css';

// Estilizando componentes com classes
class Element extends Component {
  render() {
    return (
      <div className="element" />
    );
  }
}
```

#### style.css ####
```css
.element {
    width: 200px;
    height: 220px;
    background-color: cornflowerblue;
}
```

### Curiosidades [⇧](#assuntos-abordados)

- Server rendering with [Next.js](https://nextjs.org)
- Tests with [Jest](https://jestjs.io/)
- Gerenciando o estado da aplicação com [Redux](https://redux.js.org/)
- [React Hooks](https://pt-br.reactjs.org/docs/hooks-intro.html)
    - https://twitter.com/prchdk/status/1056960391543062528
    - https://medium.com/@dan_abramov/making-sense-of-react-hooks-fdbde8803889
- Desktop application with [Electron](https://electronjs.org/)
    - https://www.freecodecamp.org/news/building-an-electron-application-with-create-react-app-97945861647c/

### HANDSON EM AULA

#### App.js ####

```jsx
import React, { Component } from "react";
import ReactDOM from "react-dom";

import "./style.css";

const ListItem = props => <li className="list-item">{props.text}</li>;

class List extends Component {
  constructor(props) {
    super(props);

    this.state = {
      todos: [
        "Ler o livro de CG",
        "Ler o livro de MN",
        "Descansar",
        "Estudar de novo"
      ],
      currentTodo: ""
    };

    this.textInput = React.createRef();
  }

  addTodo() {
    let { todos } = this.state;
    todos.push(this.state.currentTodo);
    this.setState({
      todos
    });

    this.textInput.current.value = "";
  }

  getNewTodo(event) {
    let input = event.target;
    this.setState({
      currentTodo: input.value
    });
  }

  render() {
    return (
      <div>
        <ul>{this.state.todos.map(todo => <ListItem text={todo} />)}</ul>
        <input
          ref={this.textInput}
          onInput={this.getNewTodo.bind(this)}
          type="text"
        />
        <button onClick={this.addTodo.bind(this)}>Adicionar</button>
        <br />
      </div>
    );
  }
}

class App extends Component {
  render() {
    return <List />;
  }
}

export default App;
```
