# Typescript

## PropTypes

Utilizado para validar o tipo de dado que um componente recebe.
Tipos comuns listados:

- array
- arrayOf(...)
- bool
- func
- number
- object
- string
- .isRequired (Modificador para tornar a propriedade obrigatória)

  > Exemplo de uso: Component.propTypes = { name: PropTypes.string.isRequired };
  >
  > Default Props
  > Define valores padrão para as propriedades caso elas não sejam passadas pelo componente pai.

- Sintaxe: Component.defaultProps = { errors: {} };
  - Neste caso, se a prop errors não for enviada, ela assumirá um objeto vazio como valor inicial.
