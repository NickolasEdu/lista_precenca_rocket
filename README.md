# Lista Presenca Rocketseat

Um repositório do [Rodrigo](https://github.com/rodrigorgtic) disponibilizado na trilha Especializar do Discover da Rocketseat,
o objetivo de fazer o clone desse projeto foi entender os conceitos de Typescript na prática fazendo uma conversão parcial de um projeto padrão e aplicar o Typescript.
Esse exemplo serviu para mostrar que a importação do Typescript é feito de modo gradual,
não obrigando a typar tudo, mas apenas os pontos que achar necessário e fazer essas mudanças a medida que considerarmos necessárias.

## Passos Iniciais
- git clone
- npm i
- npm run dev
- npm i typescript —save-dev 
- definir tsconfig - padrão da documentação oficial referente as preferencias usadas
- No caso desse repo foi usado React com Vite, então as configs estão referentes a esta preferencia de desenvolvimento.
- Alterar o arquivo do componente main de ‘.jsx’ para ‘.tsx
- Isso irá ocasionar um erro inicial, agora é o momento de começar as alterações no formato TS


Components ⇒ Card ⇒ Main
Agora criado uma typagem para os valores de props no componente.
**Atenção**: Neste caso o próprio React recomendou a instalação de uma dependência do framework,
por questão de que além do TS precisar ser configurado para ler o JSX, o React também precisa ser capaz de ler o TSX.
‘npm i --save-dev @types/react’

Alteração adicionada
Declaração da tipagem e atribuição no argumento da função jsx 
```tsx
export type CardProps = {
  name: string;
  time: string;
}

export function Card(props: CardProps) {
```

Pages ⇒ Home ⇒ Index
No arquivo da página principal os valores recebidos são semelhantes a aqueles no componente principal,
então além da importação dele, também será feita dentro do Card main a exportação do da typagem.
Aletrações adicionadas
Importação da typagem do múdulo main
Passando a typagem na variavel de estado com o modelo  semelhante ao Generic  ‘<>’
E indicando que espera por um tipo de dado vazio, nesse caso array ‘[ ]’

```tsx
import { Card, CardProps } from '../../components/Card';

export function Home() {
  const [studentName, setStudentName] = useState('');
  const [students, setStudents] = useState<CardProps[]>([]);
  const [user, setUser] = useState({ name: '', avatar: '' });
```

### Simplificando a declaração de useState de Users

Está declarado como começando vazio, justamente para evitar que o sistema retorne algum erro.
Porém se esse usuário receber novos parâmetros além de ‘name’ e ‘avatar’ essa função de state ficarará ainda maior.
Para evitar isso podemos declarar uma typagem para o user.

Original
```tsx
const [user, setUser] = useState({ name: '', avatar: '' });
```

Typagem

```tsx
type User = {
  name: string,
  avatar: string,
}

const [user, setUser] = useState<User>({} as User)
```

### Simplificando API Response
Nete caso a API retorna uma grande quantidade de conteúdo, porém estamos usando apenas ‘name’ e ‘avatar’. Então para especificar o uso dos dados para estes dois valores é feito o mesmo método para adcionar typagem no useEffect.

Original
```tsx
useEffect(() => {
    async function fetchData() {
      const response = await fetch('https://api.github.com/users/rodrigorgtic');
      const data = await response.json();
      console.log("DADOS ===> ", data);

      setUser({
        name: data.name,
        avatar: data.avatar_url,
      });
    }

    fetchData();
  }, [])
```

Typagem

```tsx
type ProfileResponse = {
  name: string;
  avatar_url: string;
}

useEffect(() => {
    async function fetchData() {
      const response = await fetch('https://api.github.com/users/rodrigorgtic');
      const data = await response.json() as ProfileResponse

      setUser({
        name: data.name,
        avatar: data.avatar_url,
      });
    }

    fetchData();
  }, [])
```
