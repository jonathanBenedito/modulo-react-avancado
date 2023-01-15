# Índice das Aulas React Avançado
📄 Link de acesso aos <a href="https://cord-delivery-7eb.notion.site/React-Avan-ado-0dd7bebfaf364c1f8544098923b060e5">resumos</a>. 

🖼 Link de acesso a <a href="https://jonathanbenedito.github.io/modulo-react-avancado/">página</a>.

<!-- TABLE OF CONTENTS -->
## Conteúdo
<ul>
    <li>
        <a href="#aula-01---estado-no-react">Aula 01 - Estado no React</a>
    </li>
    <li>
        <a href="#aula-02---ciclos-de-vida">Aula 02 - Ciclos de Vida</a>
    </li>
    <li>
        <a href="#aula-03---hooks">Aula 03 - Hooks</a>
    </li>
    <li>
        <a href="#aula-04---renderização-condicional">Aula 04 - Renderização Condicional</a>
    </li>
    <li>
        <a href="#aula-05---formulários-no-react">Aula 05 - Formulários no React</a>
    </li>
    <li>
        <a href="#aula-06---trabalhando-com-vários-inputs">Aula 06 - Trabalhando com vários inputs</a>
    </li>
    <li>
        <a href="#aula-07---rotas-no-react">Aula 07 - Rotas no React</a>
    </li>
    <li>
        <a href="#aula-08---styled-components">Aula 08 - Styled Components</a>
    </li>
</ul>

## Aula 01 - Estado no React

Faz com que o aplicativo re-renderize um elemento, atualizando por valores atualizados. 

```jsx
import { Component } from 'react'
import './panel.css'

class Panel extends Component{
    constructor(){
        super()
        this.state = {
            title: 'Título do painel'
        }
    }

    render(){
        return(
            <section className='panel' onClick={() => this.setState({title: 'Título novo'})}>
                <h2>{this.state.title}</h2>
            </section>
        )
    }
}

export default Panel
```

## Aula 02 - Ciclos de Vida

```jsx
import { Component } from "react";

async function createDeck(){
    const response = await fetch('https://deckofcardsapi.com/api/deck/new/shuffle/?deck_count=1')
    const deck = await response.json()
    return deck.deck_id
}

async function getCards(deckId){
    const response = await fetch(`https://deckofcardsapi.com/api/deck/${deckId}/draw/?count=52`)
    return await response.json()
}

class DeckOfCards extends Component {
    constructor(){
        super()
        this.state = {
            cards: []
        }
    }

    async componentDidMount(){
        const deckId = await createDeck()
        const data = await getCards(deckId)

        this.setState({
            cards: data.cards
        })
    }

    render(){
        return(
            <section>
                <ul>
                    {
                        this.state.cards.map((card, index) => {
                            return (
                                <li key={index}>
                                    <img src={card.image} alt={card.value} />
                                </li>
                            )
                        })
                    }
                </ul>
            </section>
        )
    }
}

export default DeckOfCards
```

## Aula 03 - Hooks
- `useState()`
    
    Usando estado dentro de **componente de classe**.
    
    ```jsx
    class DeckOfCards extends Component {
        constructor(){
            super()
            this.state = {
                cards: []
            }
        }
    
        async componentDidMount(){
            const deckId = await createDeck()
            const data = await getCards(deckId)
    
            this.setState({
                cards: data.cards
            })
        }
    ```
    
    Usando estado dentro de **componente de função**.
    
    ```jsx
    function Example() {
    
    const [count, setCount] = useState(0)
    
    return (
    	<div>
    		<p>You clicked {count} times</p>
    		<button onClick={() => setCount(count + 1)}>
    			Click me
    		</button>
    	</div>
    )
    
    ```
    
- `useEffect()`
    
    Permite você usar efeitos colaterais nos componentes de funções.
    
    ```jsx
    useEffect(() => {
        const fetchData = async () => {
            const deckId = await createDeck()
            const data = await getCards(deckId)
    
            setDeck({
                cards: data.cards
            })
        }
        fetchData()
    }, [])
    ```

## Aula 04 - Renderização Condicional

É simplesmente usar condições como `if` ou operador condicional para renderizar elementos de acordo com seu estado atual.

```jsx
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  if (isLoggedIn) {
    return <button>LogOut</button>;
  }
  return <button>LogIn</button>;
}

return (
  // Try changing to isLoggedIn={true}:
  <Greeting isLoggedIn={false} />,
  document.getElementById('root')
);
```

## Aula 05 - Formulários no React

- `onChange` → É um evento que executa uma função toda vez que seu valor for alterado, isso geralmente é utilizado em formulários para atualizar e registrar o valor do input.
- `onSubmit` → É um evento que executa uma função quando um usuário envia um formulário através do botão de submit, comumente usado com hooks.
```jsx
const Form = (props) => {

    const [inputs, setInputs] = useState({
        image: ''
    })

    const handleInputChange = (event) => {
        setInputs({
            image: event.target.value
        })
    }

    const handleSubmit = (event) => {
        event.preventDefault()
        props.addCard(inputs)
    }

    return (
        <>
            <form onSubmit={handleSubmit}>
                <div>
                    <label htmlFor='image'>Endereço da imagem da carta</label>
                    <input type="text" id="image" name="image" onChange={handleInputChange} value={inputs.image}/>
                </div>
                <input type="submit" />
            </form>
        </>
    )
}
```

## Aula 06 - Trabalhando com vários inputs

Para trabalhar com um objeto com múltiplos valores, nós modificamos a função do handler para ler os valores de forma genérica usando a desestruturação de objetos no event para extrairmos o alvo, nome e seu valor.

```jsx
    const handleInputChange = (event) => {
        const { target } = event
        const { name } = target
        const { value } = target

        setInputs({
            ...inputs,
            [name]: value
        })
    }
```

Código na prática.

```jsx
const Form = (props) => {

    const [inputs, setInputs] = useState({
        image: '',
        value: '',
        suit: ''
    })

    const handleInputChange = (event) => {
        const { target } = event
        const { name } = target
        const { value } = target

        setInputs({
            ...inputs,
            [name]: value
        })
    }

    const handleSubmit = (event) => {
        event.preventDefault()
        props.addCard(inputs)
    }

    return (
        <>
            <form onSubmit={handleSubmit}>
                <div>
                    <label htmlFor='image'>Endereço da imagem da carta</label>
                    <input type="text" id="image" name="image" onChange={handleInputChange} value={inputs.image}/>
                </div>
                <div>
                    <label htmlFor='value'>Nome da Carta</label>
                    <input type="text" id="value" name="value" onChange={handleInputChange} value={inputs.name}/>
                </div>
                <div>
                    <label htmlFor='suit'>Naipe da carta</label>
                    <input type="text" id="suit" name="suit" onChange={handleInputChange} value={inputs.suit}/>
                </div>
                <input type="submit" />
            </form>
        </>
    )
}
```

# Aula 07 - Rotas no React

São caminhos específicos que levam para determinadas páginas, como se fossem links.

```jsx
function App() {
  return (
    <div>
      <AppRoutes/>
    </div>
  );
}
```
```jsx
const AppRoutes = () => (
    <BrowserRouter>
        <Routes>
            <Route exact path='/' element={<Posts />} />
            <Route exact path='/post/:id' element={<Post />} />
        </Routes>
    </BrowserRouter>
)
```
Página padrão para definir as rotas do projeto
```jsx
<Link to='/'>Voltar para os posts</Link>
```
Link simples para acessar o caminho da rota.
```jsx
<Link to={`/post/${post.id}`}>
    <img src={post.image} alt="" />
    <h2>{post.title}</h2>
</Link>

// ----------------------------------------------

// Lembre-se de usar useParams na página onde vai receber os parâmetros

const { id } = useParams()
```
Link para acessar o caminho da rota com parâmetros.

## Aula 08 - Styled Components
É uma biblioteca que possibilita estilizar páginas de forma individual, evitando bugs de estilos e deixando nosso projeto mais dinâmico.

> Estilizando um componente individual
> 

```jsx
const Button = styled.button`
  background: transparent;
  border-radius: 3px;
  border: 2px solid palevioletred;
  color: palevioletred;
  margin: 0 1em;
  padding: 0.25em 1em;
`
```

> Estilizando de forma global
> 

```jsx
import { createGlobalStyle } from 'styled-components'

function App() {
	return (
		<div>
			<GlobalStyle />
			<AppRoutes />
		</div>
	);
}
```

```jsx
const GlobalStyle = createGlobalStyle`
	* {
		margin: 0;
		padding: 0;
	}
`
```

> Estilizando componente de acordo com uma prop.
> 

```jsx
const Section = styled.section`
    background-color: blue;
    ${props => props.red && css `
        background-color: red;
    `}
    padding: 50px;
`
```

```jsx
<Section red>
    <Link to='/'>Voltar para os posts</Link>
    <div>
        <Img src={post.image} alt={post.title} />
        <h2>{post.title}</h2>
        <p>{post.text}</p>
    </div>
</Section>
```

Ao colocar a prop `red`, automaticamente o componente terá fundo vermelho.