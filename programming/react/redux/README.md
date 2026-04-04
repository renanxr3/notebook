## Flux

Arquitetura para fluxo de dados unidirecional (base do Redux).

Exemplo de Action:

```
Action: {
type: "USER_SAVED",
data: { firstName: "Cory" }
}
```

Action Types:

```
export default { LOAD: "LOAD", CREATE: "CREATE" }
```

O Ciclo do Flux (Diagrama)

- ACTION: O que aconteceu (evento).
- DISPATCHER: O "centralizador" que envia a action.
- STORE: Onde o estado (dados) é mantido.
- REACT (View): A interface que reage às mudanças na Store e reinicia o ciclo.

### Aprofundamento no Flux

Detalhes de como o Dispatcher gerencia as Stores e as Actions:
| Método | Descrição | Relacionado a |
|---|---|---|
| register(callback) | O dispatcher chamará por quem? | Store |
| unregister(string id) | Para a ação | Store |
| waitFor(array string id) | Ordem dos callbacks (sincronização) | Store |
| dispatch(object payload) | Chama o dispatcher, avisa as Stores | Action |
| isDispatching | Verificando se já está despachando | - |
Conceito chave anotado:

> "Every payload calls every callback. Every store is notified of every action."
> (Cada payload chama cada callback. Cada Store é notificada de cada ação.)

## Aprofundamento em Redux

Handling Arrays (Manipulação de Arrays)
Para manter a imutabilidade, você deve evitar métodos que alteram o array original:

- Avoid (Evitar): push, pop, reverse. (Se usar, deve clonar o array antes).
- Prefer (Preferir): map, filter, reduce, concat, spread ([...]).
  Handling Immutability (Bibliotecas e Ferramentas)
- Native JS: Object.assign, spread operator.
- Libraries (Bibliotecas): Immer, seamless-immutable, react-addons-update, immutable.js.
- Enforcement (Segurança): redux-immutable-state-invariant (útil para avisar se você mutar o estado por acidente durante o desenvolvimento).
  Reducer
  A regra fundamental: function myReducer(state, action) => newState.
- Forbidden (Proibido):
  - Mutar argumentos (state ou action).
  - Realizar "side effects" (efeitos colaterais como chamadas de API ou logs).
  - Chamar funções não puras (ex: Date.now() ou Math.random()).
- Nota importante: Todos os reducers são chamados em cada dispatch.
  React Redux (A "Cola" entre os dois)
  Conceitos da biblioteca oficial de integração:
- Provider: O componente que envolve sua aplicação e provê a Store.
- Connect: Função que conecta um componente à Store.
  - mapStateToProps: Define quais partes do estado serão expostas como props.
  - mapDispatchToPrors: Define quais actions serão expostas como props.
    Performance e Organização
- Memorize for performance: Uso de técnicas de memorização (como a biblioteca Reselect) para evitar re-renderizações desnecessárias.
- "Normalizing state shape": Recomendação oficial da documentação do Redux para organizar o estado como se fosse um banco de dados (evitando aninhamentos profundos).
  Dica Técnica:
  Você anotou sobre o connect, mapStateToProps e mapDispatchToProps. Embora ainda existam em muitos projetos, o padrão moderno do React Redux hoje utiliza Hooks:
- Em vez de connect, usamos useSelector para pegar dados do estado.
- Em vez de mapDispatchToProps, usamos useDispatch para disparar ações.
  Isso torna o código muito mais limpo e fácil de ler, especialmente com TypeScript, que você já está usando no seu projeto de apostas e na migração do seu site pessoal para Next.js.
