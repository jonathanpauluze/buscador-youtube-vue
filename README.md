<h1 align="center">
  Buscador do YouTube | Teste Frontend
</h1>

## Sobre o projeto
Projeto criado como parte de um teste para vaga de frontend. O objetivo era criar uma aplicação que faça a busca de vídeos na API do YouTube e então liste os resultados com thumbnail, título, descrição e um botão de detalhes que direciona para uma nova tela que contém o vídeo para ser reproduzido (com título e descrição) e informações estatísticas (visualizações, curtidas e descurtidas).
- Optei por fazer a paginação com infinite scroll, pois oferece uma melhor experiência ao usuário. Para carregar mais resultados, basta rolar até o final da página.
- Optei mostrar os detalhes do vídeo em um modal, pensando também em uma melhor experiência ao usuário, para não precisar redirecionar o usuário para outras páginas.

Hospedei a aplicação no Netlify e ela pode ser visualizada [clicando aqui](https://busca-youtube.netlify.com).

### Requisitos
Para rodar esta projeto, é necessário ter instalado em sua máquina:
- [Git](https://git-scm.com/)
- [NodeJS](https://nodejs.org/en/)
- [Yarn](https://yarnpkg.com/) (Opcional)

### Baixando projeto
Clone o projeto em sua máquina:
```
git clone https://github.com/jonathanpauluze/vertex-test-frontend.git vertex-test-frontend
```

Abra o terminal a pasta do projeto
```
cd vertex-test-frontend
```

### Configurando projeto
Com o terminal aberto na pasta do projeto e execute o comando:
```
npm install
```
ou
```
yarn install
```

### Compilar e ativar hot-reloads para desenvolvimento
Com o terminal aberto na pasta do projeto, execute:
```
npm run serve
```
ou
```
yarn serve
```

### Visualizar a aplicação
O console informará em qual porta a aplicação está rodando (por padrão é na 8080), basta acessar o [localhost](localhost:8080).
