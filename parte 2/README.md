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

```javascript
class AppDrawer extends HTMLElement {...}

window.customElements.define('app-drawer', AppDrawer);
```

```html
<div>
    <app-drawer></app-drawer>
</div>
```

### React Components
```javascript
class Elemento extends React.Component {
    render() {
        return React.createElement(
            'div',
            {
                className: 'classe-element'
            },
            `Hello ${this.props.title}`
        );
    }
}
```