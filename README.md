# Índice das Aulas Javascript Básico
📄 Link de acesso aos <a href="https://cord-delivery-7eb.notion.site/React-Avan-ado-0dd7bebfaf364c1f8544098923b060e5">resumos</a>. 

🖼 Link de acesso a <a href="https://jonathanbenedito.github.io/modulo-react-avancado/">página</a>.

<!-- TABLE OF CONTENTS -->
## Conteúdo
<ul>
    <li>
        <a href="#aula-01---estado-no-react">Aula 01 - Estado no React</a>
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