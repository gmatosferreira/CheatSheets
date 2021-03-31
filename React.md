# React

> Based on [freeCodeCamp online course](https://www.youtube.com/watch?v=DLX62G4lc44) and personal experience

O React veio alterar o paradigma do desenvolvimento do *front-end* das aplicações.

- Através da [virtualização do DOM](https://www.youtube.com/watch?v=BYbgopx44vo) o React gere de forma rápida e automatizada as alterações realizadas na *view*.

- Permite a criação de componentes simples e reutilizáveis;
- É mantido e desenvolvido pelo Facebook.



## Criar ambiente de desenvolvimento local

Para criar um ambiente local de desenvolvimento é necessário ter instalada uma versão atualizada no **Node.js**.

De seguida, deve ser criada uma aplicação React.

```bash
$ npx create-react-app <myApp>
```

Este comando vai criar *single page app* React. 

De seguida, basta iniciar o servidor de produção.

```bash
$ cd <myApp>
$ npm start
```

Uma vez criada a aplicação, devem ser eliminados todos os ficheiros na pasta `/src`.

> Depois desta operação o servidor local deve ser reiniciado.

De seguida, devem ser criados nesta pasta os ficheiros que vão servir de base ao projeto: `index.html`, `index.js` e `style.css`.



## ReactDOM



### index.html

Este ficheiro define uma estrutura HTML normal, na qual deve ter uma `div` com id `root`.

```html
<div id="root"></div>
```

Esta `div` será onde todo o conteúdo gerado pelo React será inserido. Podemos vê-la como um *container*.



### index.js

No início deste ficheiro *javascript* começamos pode fazer algumas importações.



#### Importações

```react
import React from "react"
// Enables JSX syntax
import ReactDOM from "react-dom"
// Allows interactions with DOM
```



#### Renderizar o primeiro elemento

Para **renderizar** um elemento na página, recorre-se ao método `.render()` do `ReactDOM`.

> **ReactDOM.render()** recebe dois argumentos
>
> 1. o elemento **JSX** que vai ser renderizado;
> 2. Seletor que define **onde** vai ser renderizado.

Este método aceita como primeiro argumento elementos escritos na linguagem **JSX**. Esta é uma linguagem bastante similar ao HTML, que será explorada mais em detalhe durante os próximos passos.



#### Renderizar múltiplos elementos

O método `ReactDOM.render()` não permite a introdução de vários elementos JSX na mesma hierarquia através de um único comando. Para tal, estes devem ser agrupados numa `div`.

```react
ReactDOM.render(
    <div>
        <h1>Hello world!</h1>
        <p>Loren ipsum...</p>
    </div>
    , 
    document.getElementById("root")
);
```



## Componentes funcionais

Agora que conseguimos renderizar algum conteúdo no DOM, como será que vamos criar vários componentes? Todos no mesmo método `render()`?

Uma das razões que distinguem o React é o facto de permitirem a criação de **componentes reutilizáveis**, implementados através de **componentes funcionais**.

O seu nome deve-se ao facto de serem definidos por funções, que retornam código JSX.

```react
// Same as
// function MyComponent() {}
// But with JS6 (arrow function)
const MyComponent = () => {
    return (<h1>Hello world</h1>);
}

// Usage
ReactDOM.render(
	<MyComponent />,
    document.getElemetById("root")
);
```

Para serem renderizados recorre-se à função `ReactDOM.render()`, fazendo referência à função que define o componente através de um marcador HTMl *self-closing*.

> É uma boa prática a utilização de uma nomenclatura *camel-case* nos componentes, com a primeira letra maiúscula.



### Cada componente no seu ficheiro

À medida que vamos desenvolvendo a nossa página o número de componentes vai crescer cada vez mais e o `index.js` vai tornar-se cada vez mais confuso.

Para evitar que isto aconteça, podemos criar um ficheiro para cada componente. Este ficheiro deve começar por importar o `React`, uma vez que para renderizar um componente é preciso escrever JSX.

Deve de seguida definir a função do componente e terminar por exportá-lo, através do comando `export`.

```react
import React from "react"

const MyComponent = () => { ... }

export default MyComponent
```

Para o utilizar noutro ficheiro, basta importá-lo através da definição da sua localização relativa.

```react
import MyComponent from "./Components/MyComponent.js"
// The .js extension is optional!
```



### Props

Não faz sentido utilizarmos componentes funcionais se não podermos <u>personalizar o seu conteúdo</u>. 

Para tal, utilizamos atributos quando definimos os marcadores na renderização.

```react
<MyComponent title="Hello world" />
```

Na função que define ao componente acedemos a este atributos através do argumento `props`, que é um objeto Javascript.

```react
const MyComponent = (props) => {
    return <h1>{props.title}</h1>
}
```

Em vez de marcadores convencionais, podemos ainda optar por passar um objeto Javascript diretamente.

```react
<MyComponent data={{title: "Hello world"}} />

// Usage:
function MyComponent(props) {
    return <h1>{props.data.title}</h1>
}
```

Em alternativa à variável `props`, pode fazer-se um mapeamento direto entre os atributos e as variáveis.

```react
<MyComponent title={"Hello world"} />

// Usage:
function MyComponent({title}) {
    return <h1>{title}</h1>
}
```

Este mapeamento é direto. A variável `props` pode ser definida para agrupar os restantes atributos não mapeados.

```react
<MyComponent title={"Hello world"} description={"Loren ipsum..."} />
// Usage:
function MyComponent({title, ...props}) {
    return (
    	<div>
        	<h1>{title}</h1>
            <p>{props.description}</p>
        </div>
    )
}
```



### Criar vários componentes a partir de arrays de objetos

Todos a informação mostrada num *site* estava antes armazenada numa base de dados. A sua consulta geralmente retorna objetos ou *arrays* de objetos, a partir dos quais os componentes podem ser gerados.

Para tal, basta mapear o *array* de objetos para um *array* de componentes.

```react
const myComponents = myArray.map(myObj => <MyComponent key={myObj.id} title={myObj.title} />)
```

> Todos os componentes de um *array*, para serem renderizados têm de tem um atributo `key`, que funciona como um identificador e por <u>tem de ser único</u>.

De seguida basta inserir este *array* no JSX a retornar.

```react
return (
	<div className="header">
    	{myComponents}
    </div>
)
```



### Estado

Contrariamente às `props` que são **imutáveis**, o estado é **dinâmico** e pode ser alterado para cada componente.

O estado deve ser definido com o método React `useState()`. Este método define o valor inicial do estado, que pode ser de qualquer tipo. 

Na sua inicialização é definida a variável para o estado e o seu setter.

```react
import React, { useState } from "react";

const MyComponent = () => {
    const [varName, setVarName] = useState(true);
}
```



### Gestão de eventos

A gestão de eventos em React é bastante semelhante à do Javascript, sendo feita através de marcadores que definem qual a função que vai gerir o evento. Distinguem-se apenas porque o marcador é escrito em *camel-case*.

```react
<button onClick={function() {console.log("Clicked me!");}}></button>
```

> Ver lista completa de eventos suportados [aqui](https://reactjs.org/docs/events.html#supported-events)



## Renderização condicional

Há situações em que dependendo do estado podemos querer renderizar elementos diferentes do componente.

> Por exemplo enquanto fazemos uma chamada à API podemos querer mostrar um GIF a informar de que a página está a carregar e só depois de termos os dados mostrar o componente.

Para tal, basta utilizar uma estrutura condicional dentro do método `render()`.

```react
render() {
    if (this.state.isLoading) {
        return(<h1>Loaing...</h1>)
    }
    return(<MyComponent />)
}
```



### Operador ternário

Pode ainda ser utilizado o operador ternário caso a renderização condicional se aplique apenas a parte do componente.

```react
render() {
    return(
    	<div>
        	<h1>My page</h1>
            { this.state.isLoading ? <h1>Loading...</h1> : <MyComponent />}
        </div>
    )
}
```



### Conjunção

Na conjunção de elementos através do símbolo `&&`, o que o Javascript faz é verificar se o primeiro tem valor de verdade `true` e nesse caso retornar o segundo. Retorna `false` caso o primeiro tenha valor de verdade `false`.

Assim, caso haja necessidade de retornar um elemento apenas no caso de uma condição ser verdade (ou seja, retornar nada caso não seja verdade), pode recorre-se a este operador lógico.

```react
return (
	{!this.state.isLoading && <h1>Hello world!</h1>}
)
```

> Para renderizar vários elementos com a mesma condição, basta colocá-los entre `<>` e `</>`.
>
> ```react
> return (
> 	{
>         !this.state.isLoading && 
>         <>
>         	<h1>Hello world!</h1>
>         	<p>Loren ipsum...</p>
>         </>
>     }
> )
> ```
>
> 



## Obter dados de uma API

As consultas a APIs são feitas depois da renderização dos componentes, através do método `useEffect()`.

Este método recebe como argumento um <u>método a executar</u> e uma <u>lista de variáveis desencadeadoras</u>. O método é executado sempre que o valor de algumas das variáveis da lista mude, ou uma vez assim que o componente é renderizado, caso a lista seja vazia.

```react
import React, { useEffect } from "react";

const MyComponent = () => {
    
    useEffect(() => {
		// Make API call
     }, []);
}
```



### fetch

> [Documentação](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch)

Esta é uma ferramenta embutida no Javascript que permite fazer pedidos HTTP baseados em promessas.

```react
componentDidMount() {
    fetch("<APIurl>")
        .then(response => response.json())
    	.then(functionToHandleRequest())
}
```



## Formulários

> [Documentação](https://reactjs.org/docs/forms.html)
>
> Biblioteca que facilita criação de forms: [Formik](https://formik.org/docs/overview)



### Input de texto

```react
<form>
	<input 
        type="text" 
        name="firstName" 
        onChange={this.formUpdated} 
    />
</form>
```

Para acompanhar as alterações feitas nos inputs de texto do formulário podemos recorrer à gestão de eventos.

> A função que vai fazer a gestão do evento recebe como argumento `event`, que tem como atributo `.target` o elemento JSX que gerou o evento e cujos atributos podem ser acedidos.

```react
formUpdated(event) {
    this.setState({
        [event.target.name]: event.target.value
    })
}
```

> A chave é um array porque em Javascript não pode ser definido o nome de um atributo de um objeto como o valor de uma variável de outra forma.

De forma a facilitar a obtenção do nome e valor do atributo, pode ainda ser seguida outra abordagem.

```react
formUpdated(event) {
    const {name, value} = event.target;
    this.setState({
        [name]: value
    })
}
```



### Componentes controlados

Para além de atualizarmos o estado quando o valor do input é alterado, podemos ainda garantir o contrário. Ou seja, que quando o estado é alterado, o valor do input também o é. Basta para isso adicionar um atributo ao input.

```react
value={this.state.attrName}
```



### Outros inputs

#### `<textarea />`

Em JSX, este é um elemento ***self-closing*** e permite a definição de atributos tal como os restantes elementos.

```react
<textarea 
    name="message" 
    value={this.state.message}
    onChange={this.formUpdated}
></textarea>
```



#### `<input type="checkbox">`

Para atualizar o estado de uma `checkbox` não vamos utilizar a propriedade `value`, mas sim a `checked`. Precisamos então de alterar a função que atualiza o estado do formulário, de forma a ter um comportamento diferente caso se trate de um elemento deste tipo.

```react
formUpdated(event) {
    const {name, value, type, checked} = event.target;
    this.setState({
        [name]: type==="checkbox" ? checked : value,
    })
}
```



#### `<input type="radio">`

Os `radio` são uma junção dos elementos abordados anteriormente, uma vez que apresentam os atributos `value` e `checked` em simultâneo.

A sua implementação deve passar por atualizar o valor `onChange` e determinar a propriedade `checked` com base nesse valor.

```react
<label>
    <input
        type="radio"
        name="radioBtn"
        value="Two"
        onChange={this.formChange} // Updates value
        checked={this.state.radioBtn == "Two"}
        ></input>
    Two
</label>
```



#### `<select>`

De forma bastante semelhante aos `input` de texto, com a variante de ter de se definir o `value` no `select` e nas `option`.

```react
<select
    name="favColor"
    onChange={this.formChange}
    value={this.favColor}
>
    <option value="Red">Red</option>
    <option value="Blue">Blue</option>
    <option value="Green">Green</option>
</select>
```



### Submeter formulário

Dentro de um formulário HTML, um elemento `button` é equivalente a ter um `<input type="submit">`.

De forma a gerir a submissão do formulário, podemos criar um *event listener* no formulário.

```react
<form onSubmit={this.formSubmit}></form>
```

A função responsável pela gestão deste evento deve prevenir o comportamento por defeito e fazer a sua gestão da submissão.

```react
formSubmit(event) {
	event.preventDefault();
    // Costum form processing
} 
```



## Atualizações ao React

Sendo uma tecnologia recente e desenvolvida em parte pelo Facebook, esta está em constante atualização e é comum a introdução de novos conceitos.



### Outras novidades a considerar

[Official React Context API](https://reactjs.org/docs/context.html)

Error Boundaries

Render props

Higher Order Components

React Router

> Permite a criação de *single-page-apps* de forma declarativa e bastante simples.

React Hooks

> Permite incluir e modificar estado num componente funcional

React lazy, memo and Suspense

> Suspense tem a ver com carregamento assíncrono



## Notas adicionais



#### Usar caminhos com base na pasta `/public`

No HTML usar `%PUBLIC_URL%`.

```html
<img src="%PUBLIC_URL%/images/logo.png" />
```

No Javascript, usar `process.env.PUBLIC_URL`.

```react
<img className="title" src={process.env.PUBLIC_URL + '/images/titles/about.png'} />
```



#### Inserir HTML através de uma variável

Quando um elemento HTML está armazenado na forma de uma String numa variável, será renderizado forma estrita (sem ser convertido para um elemento HTML).

A solução passar por utilizar o atributo `dangerouslySetInnerHTML`.

```react
<div dangerouslySetInnerHTML={{__html: <HTMLVAR>}} ></div>
```



#### onClick href

```react
onClick={function() {window.open('https://facebook.com', '_blank')}}
```



#### Obter parâmetros do URL (GET)

https://stackoverflow.com/a/66238013/10735382



## Deploy

https://dev.to/crishanks/deploy-host-your-react-app-with-cpanel-in-under-5-minutes-4mf6



### Variáveis de ambiente

Para facilitar o *deploy* deve recorrer-se a variáveis de ambiente para guardar URL de serviços consumidos, assim como eventuais dados de acesso, de forma a que alterações futuras se limitem à alteração da variável e não ao conteúdo de vários ficheiros, com potencial para esquecimentos e quebra da versão em produção.

> [Tutorial](https://betterprogramming.pub/using-environment-variables-in-reactjs-9ad9c5322408)

Para tal basta criar um ficheiro `.env` na raiz do projeto (mesma pasta que o `/src`) e declarar as variáveis sempre com o sufixo `REACT_APP_`.

```text
REACT_APP_API="https://myapi.com"
```

Para lhes aceder no código, recorre-se ao `process.env`.

```react
console.log(process.env.REACT_APP_API);
```

> Pode ser necessário reiniciar a aplicação para as alterações realizadas às variáveis sejam visíveis!


