# Jogo da Velha com React.js

Um exercício simples para entendermos os conceitos básicos do React.js

Esse exercício pode ser encontrado na página de [tutoriais do site oficial do React](https://pt-br.reactjs.org/tutorial/tutorial.html).

A seguir, vamos seguir esse passo a passo traduzido e simplificado:

Reactjs passo a passo

## 1. Criar um app react default e acessá-lo:

``` sh
npx create-react-app jogo-da-velha
cd jogo-da-velha
npm start
```

## 2. Apagar todos os arquivos da pasta src
  

## 3. Criar um arquivo index.css na pasta src
  

## 4. Criar um arquivo index.js na pasta src
  

## 5. Incluindo o código inicial nos arquivos

Colar os respectivos códigos css e js do [CodePen](https://codepen.io/gaearon/pen/oWWQNa?editors=0100).
  

## 6. Importando recursos

Importar o React, ReactDOM e o nosso estilo adicionando as seguintes linhas ao topo do arquivo index.js:

``` js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
```

## 7. Extensão Babel JS para VS COde

Vale também instalar a extensão Babel JavaScript no VS Code para facilitar a identificação dos elementos no código.

## 8. Restartando

Vamos derrubar o npm (Control C no terminal) e subir novamente ( `npm start` ).

## 9. Props

No index.js, no método renderSquare(), vamos passar o valor **i** para o componente Square através de uma `prop` chamada value (semelhante a um atributo de tag HTML):

``` js
renderSquare(i) {
    return <Square value = {
        i
    }
    />;
}
```

_Repare que o valor está sendo declarado entre chaves, e não entre aspas._

## 10. Renderizando props

Vamos renderizar as `props` do Square, substituindo `{ / * TODO * / }` por `{this.props.value}` (dentro do método render() do Square). Ficará assim:

``` js
class Square extends React.Component {
    render() {
        return ( <
            button className = "square" > {
                this.props.value
            } <
            /button>
        );
    }
}
```

Ao visitar o app, o jogo da velha deve ter cada quadrado preenchido com seu índice (0 - 8).

Perceba que o **i** foi definido na class Board mas utilizado na class Square. Transferimos esse valor através das `props` .

Repare ainda que dentro de Board, definimos um método que renderiza o componente Square - `renderSquare()` - passando a `prop` **i**.

Ao chamar esse método (dentro do `render()` do Square), estamos passando justamente o valor de **i** para cada quadrado.

Por fim, na definição da classe Square, 'printamos' o valor da `prop` entre as tags de abertura e fechamento do `button` .

## 11. Adicionando interatividade

Vamos adicionar um evento do tipo clique no `button` (Square):

``` js
onClick = {
    function() {
        alert('click');
    }
}
```

Ficará assim:

``` js
class Square extends React.Component {
    render() {
        return ( <
            button className = "square"
            onClick = {
                function() {
                    alert('click');
                }
            } > {
                this.props.value
            } <
            /button>
        );
    }
}
```

Agora, se clicar num quadrado, verá justamente um `alert()` ser disparado com o texto 'click'. Vamos transformar essa função em uma `arrow function` .

> Obs.: repare que estamos passando uma função como `prop` . Se declarássemos apenas o `alert()` , ele seria disparado a cada renderização.

## 12. Salvando valores com state

> Nota: hoje há como usarmos `hooks` para manipularmos o `state` , mas nessa prática faremos da maneira mais 'roots', seguindo o tutorial do próprio React, por se tratar de um primeiro contato com essa lib/framework.

Para isso adicionaremos um construtor à classe Square para inicializar o state (lembram-se das aulas de POO?). Logo após declararmos a classe Square, vamos colar o seguinte trecho de código:

``` js
constructor(props) {
    super(props);
    this.state = {
        value: null,
    };
}
```

Com isso estamos dizendo que o state começa com o valor `null` .

> O trecho `super(props)` serve para herdarmos as `props` do React. Component, já que a classe Square extende a classe React. Component.

Vamos mudar o método `render()` para exibirmos o `state` ao invés da `prop` . Então o 'miolo' do nosso `button` agora ficará assim:

``` js
{
    this.state.value
}
```

E no nosso onClick, vamos substituir o `alert()` por um método para 'setarmos' nosso state (mediante o clique):

``` js
onClick = {
    () => this.setState({
        value: 'X'
    })
}
```

Depois dessas alterações, ao clicarmos em um quadrado ( `button` definido em `Square` ), o mesmo deve ser preenchido com seu `state` , um 'X'.

> Dica: é comum quebrarmos a linha de acordo com as `props` que temos, no caso `className` e `onClick` .

## 13. Dica Extra: Extensão do React para Chrome ou Firefox

Podemos usar uma extensão chamada React DevTools, que nos permite visualizar, por exemplo, os componentes no devtools. Basta instalarmos e acessarmos a aba Components.

## 14. Movendo o `state` para o Board

Vamos guardar o `state` de cada Square no próprio Board, que definirá se deverá aparecer um 'X' ou um 'O' mediante o clique no Square.

Para isso teremos de compartilhar o `state` entre o Board e o Square.

Então vamos criar um construtor para o Board onde passaremos um array com as 9 posições preenchidas (a princípio com valor `null` ):

``` js
constructor(props) {
    super(props);
    this.state = {
        squares: Array(9).fill(null),
    };
}
```

## 15. Fazendo o Square enxergar o seu `state` a partir do Board

Antes o Square enxergava a `prop` . Depois mudamos para enxergar o `state` e preencher com um 'X'.

Agora precisamos fazer com que ele enxergue sua posição no array através do método `renderSquare()` , definindo seu valor a partir do `state` :

``` js
renderSquare(i) {
    return <Square value = { this.state.squares[i] } />;
}
```

## 16. E precisamos fazer o array em Board enxergar as mudanças de `state` do Square

Para isso construiremos um método que lide com o clique no Square. Esse método vai se chamar `handleClick()` e deve receber a posição do Square (**i**). Primeiro vamos atrelar o método ao evento do tipo clique, dentro do método `renderSquare()` :

``` js
onClick = {
    () => this.handleClick(i)
}
```

Então o método `renderSquare()` (dentro do Board) ficará assim:

``` js
renderSquare(i) {
    return (
        <Square
            value = { this.state.squares[i] }
            onClick = { () => this.handleClick(i) }
        />
    );
}
```

> Perceba que incluímos um parenteses engoblando todo o return. Sabemos que o React transforma nosso código em JS puro. Sem esses parenteses, teríamos um '; ' adicionado ao final de return (como se fosse um return false).

Para concluirmos esse passo, precisamos atualizar o método `render()` do nosso Square novamente, já que o `state` agora está no Board, e não nele. Então precisamos:

* Trocar o `this.state.value` por `this.props.value` ; 
* Trocar `this.setState()` por `this.props.onClick()` ; 
* Remoer nosso construtor do Square.

Então nosso Square ficará assim:

``` js
class Square extends React.Component {
    render() {
        return (
            <button
                className="square"
                onClick={
                    () => this.props.onClick()
                }
            >
                {this.props.value}
            </button>
        );
    }
}
```

> **Recapitulando:**
> A `prop` onClick diz ao React para criar um event listener.
> Ao clicarmos, acessaremos a função recebida através da propriedade `onClick` criada no Board (`this.props.onClick()`).
> No Board, definimos que, ao receber o clique, chamaremos o método `handleClick(i)`, passado do Board para o Square.
> Agora definiremos a função `handleClick()`.

## 17. Método `handleClick()`

Dentro do Board, após seu método construtor (e antes do `renderSquare()`), definiremos a função da seguinte maneira:

``` js
handleClick(i) {
    const squares = this.state.squares.slice();
    squares[i] = 'X';
    this.setState({ squares: squares });
}
```

Com esse método, agora nosso Square é um componente controlado, pois quem controla o `state` é o Board, não Square. E a cada vez que o `state` for atualizado, cada componente Square será renderizado novamente.

Vamos entender o que acontece nesse método:

Criamos uma const chamada 'squares' que é uma cópia do array original (por isso usamos o método `slice()`).
Então salvamos o valor 'X' naquela posição ('i').
Por fim, atualizamos o `state` (com o método `setState()`), atrelando o valor do array copiado ao original.

> Ao darmos essa 'volta', estamos evitando modificarmos o valor diretamente, estamos evitando a **mutação**, o que nos permitirá implementar as aplicações de modo mais simples e ainda nos dará acesso ao histórico do `state`. Além disso, ao evitarmos a mutação, conseguiremos detectar mudanças de forma mais simples. E, por fim, a **imutabilidade** nos dá melhores condições de sabermos quando um componente deve ser re-renderizado (através da detecção das mudanças). Mais informações sobre [Performance em React](https://pt-br.reactjs.org/docs/optimizing-performance.html#examples).

## 18. Componente de Função

Vamos transformar nosso Square em um componente de função - que não possuem `state` próprio, apenas o método `render()`, tornando-o mais simples:

``` js
function Square(props) {
    return (
        <button className="square" onClick={props.onClick}>
            {props.value}
        </button>
    );
}
```

Repare que substituímos `this.props` por `props` e que o valor da prop 'onClick' agora está mais simples também.

## 19. Alternando entre 'X' e 'O'

Por enquanto, só exibimos 'X' quando clicamos no Square, independentemente do 'turno' (do jogador). Vamos ajustar isso.

Então vamos atualizar nosso `state` inicial para que a primeira jogada marque um 'X' adicionando `xIsNext: true,` ao método construtor do Board, que é quem possui o controle do `state`:

``` js
constructor(props) {
    super(props);
    this.state = {
        squares: Array(9).fill(null),
        xIsNext: true,
    };
}
```
E vamos incrementar o método `handleClick()` para alternarmos o valor booleano do `xIsNext`:

``` js
handleClick(i) {
    const squares = this.state.squares.slice();
    squares[i] = this.state.xIsNext ? 'X' : 'O';
    this.setState({
        squares: squares,
        xIsNext: !this.state.xIsNext,
    });
}
```

Também precisamos atualizar o status para informar qual o próximo jogador ('X' ou 'O'). Precisamos atualizar a função render do Board:

``` js
const status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
```

## 20. Temos um vencedor!

Para determinarmos o vencedor (e o fim do jogo), vamos usar a seguinte função auxiliar (devemos colá-la no final do arquivo):

``` js
function calculateWinner(squares) {
    const lines = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
    ];
    for (let i = 0; i < lines.length; i++) {
        const [a, b, c] = lines[i];
        if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
            return squares[a];
        }
    }
    return null;
}
```

Estamos quase lá! Precisamos atualizar a função `render()` do Board incluindo nossa nova função auxiliar. Então antes do `return` vamos substituir o status por:

``` js
const winner = calculateWinner(this.state.squares);
let status;
if (winner) {
    status = 'Winner: ' + winner;
} else {
    status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
}
```

E, no método `handleClick()`, vamos adicionar mais um trecho para desabilitar o evento relacionado ao clique, caso haja um vencedor:

``` js
handleClick(i) {
    const squares = this.state.squares.slice();
    if (calculateWinner(squares) || squares[i]) {
        return;
    }
    squares[i] = this.state.xIsNext ? 'X' : 'O';
    this.setState({
        squares: squares,
        xIsNext: !this.state.xIsNext,
    });
}
```

## Próximas etapas:

No [tutorial do React](https://pt-br.reactjs.org/tutorial/tutorial.html), há [mais alguns passos](](https://pt-br.reactjs.org/tutorial/tutorial.html)), para criarmos o histórico das jogadas e podermos 'voltar no tempo'.

  
___
  
  

# Notas originais do 'boilerplate' do react-app

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

## Available Scripts

In the project directory, you can run:

### `yarn start`

Runs the app in the development mode.<br />
Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

The page will reload if you make edits.<br />
You will also see any lint errors in the console.

### `yarn test`

Launches the test runner in the interactive watch mode.<br />
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

### `yarn build`

Builds the app for production to the `build` folder.<br />
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.<br />
Your app is ready to be deployed!

See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.

### `yarn eject`

**Note: this is a one-way operation. Once you `eject` , you can’t go back!**

If you aren’t satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single build dependency from your project.

Instead, it will copy all the configuration files and the transitive dependencies (webpack, Babel, ESLint, etc) right into your project so you have full control over them. All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them. At this point you’re on your own.

You don’t have to ever use `eject` . The curated feature set is suitable for small and middle deployments, and you shouldn’t feel obligated to use this feature. However we understand that this tool wouldn’t be useful if you couldn’t customize it when you are ready for it.

## Learn More

You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).

To learn React, check out the [React documentation](https://reactjs.org/).

### Code Splitting

This section has moved here: https://facebook.github.io/create-react-app/docs/code-splitting

### Analyzing the Bundle Size

This section has moved here: https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size

### Making a Progressive Web App

This section has moved here: https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app

### Advanced Configuration

This section has moved here: https://facebook.github.io/create-react-app/docs/advanced-configuration

### Deployment

This section has moved here: https://facebook.github.io/create-react-app/docs/deployment

### `yarn build` fails to minify

This section has moved here: https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify
