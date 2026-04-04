# React - Auth0

Embora o conteúdo seja focado em conceitos de Autenticação e Autorização (OAuth 2.0 e OIDC), esses tópicos são fundamentais no ecossistema React para gerenciar o login e a proteção de rotas.

- Auth providers:
  - Auth0
  - OKTA
- OAuth 2.0 = Protocol (Protocolo)
- Scopes = Authorization (Autorização)

OAuth Roles (Papéis)

- Resource Owner = User (Usuário)
- Client = App (Sua aplicação React, por exemplo)
- Auth Server = Autentica e fornece Access Tokens
- Resource Server = User Data (API) — Onde os dados residem.

> "ACCESS TOKEN": O token usado para acessar os recursos.

## Flow (Fluxo)

- Auth Req --> Auth Grant --> Resource Owner (Pedido e concessão de autorização)
- Auth Grant --> Access Token --> Auth Server (Troca da concessão pelo token)
- Access Token --> Data --> Resource Server (Uso do token para obter dados)

## OpenID Connect (OIDC) = Authentication

Focado na identidade do usuário (quem ele é). Site de referência: openid.net

Adds (O que ele adiciona ao OAuth):

- ID Token (JWT)
- UserInfo endpoint
- Standard scopes = Permissions (Permissões)
  > "IDENTITY TOKEN": Token de identidade.
- Identity Providers:
  - FB (Facebook)
  - Google
  - Twitter
- Relying Party: Your app (Sua aplicação React)

## JWT - JSON Web Token

Site de referência: jwt.io | Formato: Base64

Estrutura do JWT:

- Header: Type, Hash Alg, Key ID
- Body (Payload): User identity, Claims
- Signature: Verify sender (Verificação do remetente)

  > Dica para React:
  > Ao implementar isso no seu projeto, você geralmente usará bibliotecas como @auth0/auth0-react ou oidc-client-ts. O Access Token costuma ser armazenado em memória (estado do React) ou em um cookie seguro para chamadas de API, enquanto o ID Token é usado para exibir o nome e a foto do perfil do usuário na UI.
