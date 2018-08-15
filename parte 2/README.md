# Workshop - JavaScript e ReactJS - Parte II
Este arquivo é dedicado ao auxílio da aula referente a parte II do Workshop - JavaScript e ReactJS. Tenha acesso a parte I [aqui](../parte%201/README.md).


## Assuntos Abordados
Todos os assuntos abaixo listados serão introduzidos a partir de um certo conhecimento de javascript, podendo ser obtido na Parte I do Workshop.

**Será abordado:**

* O que o React tem demais?
* Web Components
* React Components
* Como o React funciona?
    - O ambiente de desenvolvimento
    - Webpack, Babel e Browserify
* JSX
* Virtual DOM
* Renderizando Componentes
* Props
* Statefull e Stateless components
* Lists e keys
* Handling Events
* Styling
* Ref
* Introdução sobre o React Native

## Snippets de Códigos

### O que o React tem demais?

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
    alert() {
        alert(this.props.description)
    }

    render() {
        <button onClick={this.alert.bind(this)}>
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

### Web Components
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

### Como o React funciona?

#### Webpack ####

![Webpack](https://webpack.github.io/assets/what-is-webpack.png)

#### Babel ####

![Babel](babel.jpg)

### JSX
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

### Virtual DOM

1) Cria uma representação do DOM na linguagem do Virtual DOM;
2) Manda o Virtual DOM gerar o DOM real;
3) Quando houver alteração no model, em vez de atualizar a DOM real você simplesmente manda regerar toda a representação Virtual DOM passando o novo estado do model como parâmetro;
4) Então você usa o mecanismo de comparação para obter as diferenças entre a representação do Virtual DOM e o DOM real;
5) E usa o mecanismo de patch que vai atualizar o DOM real conforme as diferenças observadas.

![Virtual DOM](https://cdn-images-1.medium.com/max/800/1*CqdIWZy0NMPQhYx2rKzo9g.png)