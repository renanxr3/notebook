# React Router

Gerenciamento de rotas e navegação na aplicação.

- BrowserRouter: Utiliza a API history do HTML5 para manter a UI em sincronia com a URL.
- HashRouter: Utiliza o símbolo # na URL (útil para suportar navegadores antigos ou ambientes sem configuração de servidor).
- MemoryRouter: Mantém o histórico em memória (não altera a URL no navegador); ideal para testes e componentes isolados.
  Parâmetros e URL

  Exemplo anotado de rota:

  ```
  <Route path="/course/:slug" component={Course} />
  ```

  Para uma URL como:

  ```
  myapp.com/course/clean-code?module=3
  ```

  - props.match.params.slug --> Captura "clean-code"
  - location.query --> Captura os parâmetros de busca (ex: module=3)
  - location.pathname --> Caminho total (/course/clean-code)
