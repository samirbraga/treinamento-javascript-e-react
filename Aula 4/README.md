# Aula 4 - Treinamento - JavaScript e ReactJS

O foco desta aula é entendermos como os componentes da aplicação necessitam, muitas vezes, compartilhar informações entre si.

Esses componentes podem estar "próximos" na árvore do Virtual DOM, o que nos pode levar a tentar realizar esse compartilhamento por meios da propriedades (`props`). No entanto, quando esses componentes estão em contextos e locais diferentes no app, devemos fazer com que ambos se comuniquem com uma mesma entidade, responsável por gerenciar os dados (ou estado) naquele contexto.

## Assuntos Abordados

* Gerenciamento do Estado
  - [Context API](#context-api)
  - Redux Pattern

* Implementação no React
  - React Redux

* Handson

## Context API

**Criando um contexto**

```jsx
import { createContext } from 'react'

const AppearanceContext = createContext({
  primaryColor: 'cornflowerblue',
  setPrimaryColor: () => {}
})

export default AppearanceContext
```

**Provendo um contexto à uma subárvoce do Virtual DOM**

```jsx
const MyComponent = () => {
  ...

  return (
    <AppearanceContext.Provider
      value={{
        primaryColor: ???,
        setPrimaryColor: ???
      }}
    >
      ...
    </AppearanceContext.Provider>
  );
};
```

**Acessando os dados do contexto**
```jsx

const PageContent = () => {
  const { primaryColor } = useContext(AppearanceContext);
  ...
}
```

**Atualizando o Contexto**

```jsx
const AppSection = () => {
  const [primaryColor, setPrimaryColor] = useState("#ff00ff");

  return (
    <AppearanceContext.Provider
      value={{
        primaryColor,
        setPrimaryColor
      }}
    >
    ...
```
```jsx
const ColorPicker = () => {
  const { primaryColor, setPrimaryColor } = useContext(AppearanceContext);

  return (
    <input
      type="color"
      value={primaryColor}
      onChange={(event) => setPrimaryColor(event.target.value)}
    />
  );

};
```

**Reagindo a Alterações no Contexto**

```jsx
const ColorPicker = () => {
  const { primaryColor } = useContext(AppearanceContext);

  useEffect(() => {
    console.log(primaryColor)
  }, [primaryColor])

  ...
```

[**EXEMPLO**](https://codesandbox.io/s/context-api-example-xe055?file=/src/App.js)

## Redux Pattern

### *Store* Global

![Redux Pattern](https://blog.codecentric.de/files/2017/12/Bildschirmfoto-2017-12-01-um-08.53.32.png)

### Visão Geral

![Redux Pattern Components](https://www.dotnetcurry.com/images/reactjs/redux/redux.png)

### [**HANDSON**](https://codesandbox.io/s/black-worker-ljv8u)