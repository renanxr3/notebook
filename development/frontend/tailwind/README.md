# Tailwind CSS

## Tags Corpo

| Elemento           | Classe Principal                      | Descrição                                                                 |
|--------------------|---------------------------------------|--------------------------------------------------------------------------|
| Fundo da Página    | `bg-white dark:bg-gray-900`           | Cor de fundo da página com suporte a modo escuro                         |
| Texto Padrão       | `text-gray-800 dark:text-gray-100`    | Cor de texto padrão com suporte a modo escuro                            |
| Corpo              | `font-sans antialiased`               | Fonte padrão sem serifa com suavização de texto                          |
| Altura Mínima      | `min-h-screen`                        | Altura mínima de 100% da viewport                                        |
| Padding Global     | `p-4 md:p-8`                          | Padding responsivo para todo o corpo                                     |
| Espaçamento        | `leading-relaxed`                     | Espaçamento entre linhas para melhor legibilidade                        |
| Cor de Fundo       | `bg-gradient-to-br from-blue-50 to-indigo-100` | Gradiente de fundo                                                       |
| Largura Máxima     | `max-w-screen-xl mx-auto`             | Limita a largura máxima e centraliza                                     |
| Overflow           | `overflow-x-hidden`                   | Remove barra de scroll horizontal                                        |
| Transição          | `transition duration-300`             | Animação suave de transição                                              |

## Tags Estilo

| Elemento           | Classe Principal                      | Descrição                                                                 |
|--------------------|---------------------------------------|--------------------------------------------------------------------------|
| Sombra             | `shadow shadow-lg shadow-xl`          | Adiciona sombra ao elemento com diferentes intensidades                  |
| Borda              | `border border-gray-300`              | Adiciona borda com cor customizável                                      |
| Raio da Borda      | `rounded rounded-lg rounded-full`     | Arredonda as bordas com diferentes raios                                 |
| Opacidade          | `opacity-50 opacity-75 opacity-100`   | Controla a transparência do elemento                                     |
| Display            | `flex block inline grid`              | Define o tipo de exibição do elemento                                    |
| Posição            | `absolute relative fixed sticky`     | Controla o posicionamento do elemento                                    |
| Z-index            | `z-10 z-20 z-50`                     | Define a ordem de sobreposição dos elementos                             |
| Escala             | `scale-75 scale-100 scale-125`       | Amplia ou reduz o tamanho do elemento                                    |
| Rotação            | `rotate-0 rotate-45 rotate-90`       | Rotaciona o elemento em graus                                            |
| Filtro             | `blur grayscale sepia`                | Aplica efeitos visuais ao elemento                                       |
| Cursor             | `cursor-pointer cursor-default`      | Define o tipo de cursor ao passar sobre o elemento                       |
| Hover              | `hover:bg-blue-600 hover:text-white` | Aplica estilos quando o elemento sofre hover                             |
| Focus              | `focus:outline focus:ring`            | Aplica estilos quando o elemento recebe foco                             |
| Active             | `active:scale-95`                     | Aplica estilos quando o elemento está ativo                              |

## Tags de Texto

| Elemento           | Classe Principal                              | Descrição                                                         |
|--------------------|-----------------------------------------------|-------------------------------------------------------------------|
| Tamanho do Texto   | `text-xs text-sm text-base text-lg text-xl`    | Define o tamanho da fonte                                         |
| Peso da Fonte      | `font-light font-normal font-medium font-bold` | Controla o peso (negrito) do texto                                |
| Cor do Texto       | `text-gray-700 text-red-500 text-blue-600`     | Define a cor do texto                                             |
| Alinhamento        | `text-left text-center text-right`             | Alinha o texto                                                    |
| Espaçamento Letras | `tracking-tight tracking-normal tracking-wide` | Ajusta o espaçamento entre letras                                 |
| Espaçamento Linhas | `leading-tight leading-normal leading-loose`   | Ajusta o espaçamento entre linhas                                 |
| Estilo da Fonte    | `italic not-italic`                           | Aplica ou remove itálico                                          |
| Transformação      | `uppercase lowercase capitalize`               | Transforma o texto em maiúsculas, minúsculas ou capitalizado      |
| Quebra de Texto    | `break-normal break-words break-all`           | Controla como o texto é quebrado                                  |
| Decoração          | `underline line-through no-underline`          | Aplica sublinhado, tachado ou remove decoração                    |
| Truncamento        | `truncate`                                     | Limita o texto a uma linha com reticências                        |
| Overflow Texto     | `overflow-ellipsis overflow-clip`              | Controla overflow do texto com reticências ou corte               |

-----

## Instalação

### Next.js + Tailwind CSS

1. **Instalação do Next.js**:
   ```bash
   npx create-next-app my-project
   cd my-project
   ```

2. **Instalação do Tailwind CSS**:
   ```bash
   npm install -D tailwindcss postcss autoprefixer
   npx tailwindcss init -p
   ```

3. **Configuração do Tailwind CSS**:
   Abra o arquivo `tailwind.config.js` e adicione os paths para seus arquivos:
   ```javascript
   module.exports = {
     content: [
       './pages/**/*.{js,ts,jsx,tsx}',
       './components/**/*.{js,ts,jsx,tsx}',
     ],
     theme: {
       extend: {},
     },
     plugins: [],
   }
   ```

4. **Configuração de estilos globais**:
   No arquivo `styles/globals.css`, adicione as diretivas do Tailwind CSS:
   ```css
   @tailwind base;
   @tailwind components;
   @tailwind utilities;
   ```

5. **Utilização do Tailwind CSS**:
   Agora você pode usar as classes utilitárias do Tailwind CSS nos seus componentes. Por exemplo, em `pages/index.js`:
   ```jsx
   export default function Home() {
     return (
       <div className="flex items-center justify-center h-screen">
         <h1 className="text-4xl font-bold">Hello, Next.js with Tailwind CSS!</h1>
       </div>
     )
   }
   ```

---

### React.js + Tailwind CSS

1. **Criação do Projeto React.js**:
   ```bash
   npx create-react-app my-project
   cd my-project
   ```

2. **Instalação do Tailwind CSS**:
   ```bash
   npm install -D tailwindcss postcss autoprefixer
   npx tailwindcss init -p
   ```

3. **Configuração do Tailwind CSS**:
   Abra o arquivo `tailwind.config.js` e adicione os paths para seus arquivos:
   ```javascript
   module.exports = {
     content: [
       './src/**/*.{js,jsx,ts,tsx}',
     ],
     theme: {
       extend: {},
     },
     plugins: [],
   }
   ```

4. **Configuração de estilos globais**:
   No arquivo `src/index.css`, adicione as diretivas do Tailwind CSS:
   ```css
   @tailwind base;
   @tailwind components;
   @tailwind utilities;
   ```

5. **Utilização do Tailwind CSS**:
   Agora você pode usar as classes utilitárias do Tailwind CSS nos seus componentes. Por exemplo, no arquivo `src/App.js`:
   ```jsx
   function App() {
     return (
       <div className="flex items-center justify-center h-screen">
         <h1 className="text-4xl font-bold">Hello, React with Tailwind CSS!</h1>
       </div>
     )
   }

   export default App;
   ```

---

### Vite + Tailwind CSS

1. **Criação do Projeto Vite com React.js**:
   ```bash
   npm create vite@latest my-project --template react
   cd my-project
   npm install
   ```

2. **Instalação do Tailwind CSS**:
   ```bash
   npm install -D tailwindcss postcss autoprefixer
   npx tailwindcss init -p
   ```

3. **Configuração do Tailwind CSS**:
   Abra o arquivo `tailwind.config.js` e adicione os paths para seus arquivos:
   ```javascript
   module.exports = {
     content: [
       './index.html',
       './src/**/*.{js,jsx,ts,tsx}',
     ],
     theme: {
       extend: {},
     },
     plugins: [],
   }
   ```

4. **Configuração de estilos globais**:
   No arquivo `src/index.css`, adicione as diretivas do Tailwind CSS:
   ```css
   @tailwind base;
   @tailwind components;
   @tailwind utilities;
   ```

5. **Utilização do Tailwind CSS**:
   Agora você pode usar as classes utilitárias do Tailwind CSS nos seus componentes. Por exemplo, no arquivo `src/App.jsx`:
   ```jsx
   function App() {
     return (
       <div className="flex items-center justify-center h-screen">
         <h1 className="text-4xl font-bold">Hello, React with Tailwind CSS!</h1>
       </div>
     )
   }

   export default App;
   ```



---

