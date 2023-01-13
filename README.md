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