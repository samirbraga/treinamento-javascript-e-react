# Aula 3 - Treinamento - JavaScript e ReactJS

**Refêrencias** 
  - [Protocolo HTTP](https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Overview)
  - [APIs Públicas](https://github.com/public-apis/public-apis)
  - [Axios](https://github.com/axios/axios)
  - [hoppscotch.io](https://hoppscotch.io/pt-br)
  - [React Router](https://reactrouter.com/)

## Comunicação com Servidores e Roteamento
* Rest APIs, fetch API e Axios
    * [Entendendo o padrão de comunicação síncrono](#comunicacao-sincrona)
    * Protocolo HTTP
    * Realizando requisições e tratando respostas e exceções com o Axios
    * Organização de um projeto extenso
* Consumindo Streams de Dados
    * [Entendendo o padrão de comunicação assíncrono](#comunicacao-assincrona)
    * WebSockets
* [Trabalhando com Várias Páginas na Mesma Aplicação](#single-page-applications)
    * Conhecendo uma SPA (Single Page Application)
    * React Router

## Comunicação Síncrona

![HTTP](https://www.researchgate.net/profile/Hussein_Suleman/publication/267954448/figure/fig1/AS:670019910385671@1536756603995/HTTP-communication-example.png)

## Protocolo HTTP

### Requisição

![Request](https://mdn.mozillademos.org/files/13687/HTTP_Request.png)

### Resposta

![Response](https://mdn.mozillademos.org/files/13691/HTTP_Response.png)

## Axios

[**Breaking Bad API**](https://breakingbadapi.com/documentation)

```js
import { useEffect, useState } from 'react';
import axios from 'axios'

const api = axios.create({
  baseURL: 'https://www.breakingbadapi.com/api'
});

function App() {
  const [characters, setCharacters] = useState([])

  useEffect(() => {
    api.get("/characters")
    .then(response => {
      setCharacters(response.data)
    }).catch(err => {
      console.error(err)
    })
  }, [])

  return (
    <div className="App">
      <ul>
        {characters.map(char => (
          <li>
            <strong>Nome</strong>: {char.name}
          </li>
        ))}
      </ul>
    </div>
  );
}

export default App;
```

## Comunicação Assíncrona

![Assíncrono vs síncrono](https://lh3.googleusercontent.com/proxy/O69HddZPfQ6PgZrpO84R-hMVF_Kqhlzs02HOrEHZpfQvgNq742RWdZ1fnecb1tLLUta_3lMKBO0bu6vyy6NHkUOLzds-4tKVUL5MzouRgVUJsI-208zwphRHK3NDBeiQdL85Sl4QUgLSTtcEagQ)

### Websocket

![Websocket](https://images.techhive.com/images/article/2016/06/websockets-100668229-orig.jpg)

## Single Page Applications

```jsx
import React from "react";
import {
  BrowserRouter as Router,
  Switch,
  Route,
  Link
} from "react-router-dom";

export default function BasicExample() {
  return (
    <Router>
      <div>
        <ul>
          <li>
            <Link to="/">Home</Link>
          </li>
          <li>
            <Link to="/about">About</Link>
          </li>
          <li>
            <Link to="/dashboard">Dashboard</Link>
          </li>
        </ul>

        <hr />
  
        <Switch>
          <Route exact path="/">
            <Home />
          </Route>
          <Route path="/about">
            <About />
          </Route>
          <Route path="/dashboard">
            <Dashboard />
          </Route>
        </Switch>
      </div>
    </Router>
  );
}

function Home() {
  return (
    <div>
      <h2>Home</h2>
    </div>
  );
}

function About() {
  return (
    <div>
      <h2>About</h2>
    </div>
  );
}

function Dashboard() {
  return (
    <div>
      <h2>Dashboard</h2>
    </div>
  );
}

```