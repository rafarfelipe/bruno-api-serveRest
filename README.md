# Bruno API ServeRest

[![Bruno](https://img.shields.io/badge/Bruno-API%20Testing-1E1E1E?logo=bruno&logoColor=white)](https://www.usebruno.com/) [![GitHub repo size](https://img.shields.io/github/repo-size/rafarfelipe/bruno-api-serveRest)](https://github.com/rafarfelipe/bruno-api-serveRest) [![GitHub last commit](https://img.shields.io/github/last-commit/rafarfelipe/bruno-api-serveRest)](https://github.com/rafarfelipe/bruno-api-serveRest/commits/main) [![GitHub language count](https://img.shields.io/github/languages/count/rafarfelipe/bruno-api-serveRest)](https://github.com/rafarfelipe/bruno-api-serveRest) [![CI](https://github.com/rafarfelipe/bruno-api-serveRest/actions/workflows/api-tests.yml/badge.svg?branch=main)](https://github.com/rafarfelipe/bruno-api-serveRest/actions/workflows/api-tests.yml?query=branch%3Amain)

Este repositório reúne uma suíte de testes automatizados de API para a aplicação [ServeRest](https://github.com/ServeRest/serve-rest), desenvolvida com [Bruno](https://www.usebruno.com/). O objetivo é validar o comportamento de autenticação, usuários, produtos e carrinhos, além de garantir a qualidade das regras de negócio por meio de testes automatizados e execução contínua em CI/CD.

## Visão geral

A estrutura do projeto organiza as requisições por domínio funcional:

- Login
- Produtos
- Usuários
- Carrinhos
- App

Cada arquivo `.yml` representa uma requisição HTTP, cenário de teste ou fluxo de API, com validações de status, payload e respostas esperadas.

## Stack

- Bruno — execução e organização de testes de API
- YAML — definição de coleções e ambientes
- GitHub Actions — execução automatizada em CI
- ServeRest — API de referência usada para validação
- Node.js — suporte para CLI do Bruno

## Quick start

> Projeto de automação de testes de API com Bruno + ServeRest.

### Subir a API localmente

```bash
docker run -d --name serverest -p 3000:3000 paulogoncalvesbh/serverest:latest
```

### Instalar o Bruno CLI

```bash
npm install -g @usebruno/cli
```

### Executar a suíte

```bash
cd collection
bru run --env local
```

### Gerar relatório em XML

```bash
cd collection
bru run --env local --reporter-junit ../reports/results.xml
```

## Endpoints principais

| Módulo    | Método | Endpoint           | Objetivo                 |
| --------- | ------ | ------------------ | ------------------------ |
| Login     | POST   | `/login`           | Autenticação do usuário  |
| Usuários  | GET    | `/usuarios`        | Listagem de usuários     |
| Usuários  | POST   | `/usuarios`        | Cadastro de usuário      |
| Usuários  | PUT    | `/usuarios/:id`    | Atualização de usuário   |
| Usuários  | DELETE | `/usuarios/:id`    | Exclusão de usuário      |
| Produtos  | GET    | `/produtos`        | Listagem de produtos     |
| Produtos  | POST   | `/produtos`        | Cadastro de produto      |
| Produtos  | PUT    | `/produtos/:id`    | Atualização do produto   |
| Produtos  | DELETE | `/produtos/:id`    | Exclusão do produto      |
| Carrinhos | POST   | `/carrinhos`       | Criação do carrinho      |
| Carrinhos | GET    | `/carrinhos/:id`   | Consulta de carrinho     |
| Carrinhos | DELETE | `/carrinhos/:id`   | Cancelamento do carrinho |
| Compras   | POST   | `/concluir-compra` | Finalização da compra    |

## Regras de negócio avaliadas

- usuário deve ser criado com dados válidos
- login deve retornar token de autorização
- preços de produtos respeitam regras mínimas e válidas
- carrinho exige produtos válidos e quantidades corretas
- ações restritas exigem autenticação adequada
- respostas de erro e sucesso são validadas por assertions do Bruno

## Arquitetura

A arquitetura do projeto é orientada por coleções e cenários de teste, organizados por contexto funcional:

```text
[Bruno Collection]
        │
        └── collection/
            ├── Login
            │   ├── Login Admin
            │   └── Login Cliente
        │
            ├── Usuarios
            │   ├── Criar Usuario
            │   ├── Listar Usuarios
            │   ├── Alterar Usuario
            │   └── Excluir Usuario
        │
            ├── Produtos
            │   ├── Cadastrar Produtos
            │   ├── Buscar Produto Por Nome
            │   └── Listar produtos
        │
            ├── carrinhos
            │   ├── Criar carrinho
            │   ├── Concluir Compra
            │   └── Listar Carrinhos
        │
            └── environments/
                └── local.yml

        └── reports/
```

### Fluxo de execução

1. A API ServeRest é iniciada localmente ou por container.
2. O ambiente `local` define a URL base e variáveis globais.
3. Cada request do Bruno valida status, payload e regras de negócio.
4. Os resultados são exportados em relatórios HTML/JSON/XML.
5. A pipeline do GitHub Actions executa a suíte automaticamente em pull requests.

## Repositório

```text
.
├── .github/
│   └── workflows/
│       └── api-tests.yml
├── collection/
│   ├── Login/
│   ├── Produtos/
│   ├── Usuarios/
│   ├── carrinhos/
│   ├── environments/
│   └── opencollection.yml
├── reports/
├── .gitignore
├── README.md
└── ...
```

## Pré-requisitos

Antes de executar os testes localmente, certifique-se de ter instalado:

- Node.js 18+
- npm
- Docker
- Bruno CLI

## Configuração local

### 1) Subir a API de testes

```bash
docker run -d --name serverest -p 3000:3000 paulogoncalvesbh/serverest:latest
```

A API ficará disponível em:

```text
http://localhost:3000
```

### 2) Instalar o Bruno CLI

```bash
npm install -g @usebruno/cli
```

### 3) Executar a coleção

```bash
cd collection
bru run --env local
```

Para gerar relatório em XML:

```bash
cd collection
bru run --env local --reporter-junit ../reports/results.xml
```

## Ambientes

O ambiente padrão configurado é `local`, definido em:

- `collection/environments/local.yml`

Ele define a URL base da API e as variáveis de autenticação e sessão.

## Fluxos cobertos

A suíte inclui testes para:

- autenticação
- cadastro e consulta de usuários
- atualização e exclusão de usuários
- cadastro e listagem de produtos
- filtros e buscas por produto
- criação e manipulação de carrinhos
- fluxo de compra e conclusão

## CI/CD

O projeto já possui pipeline no GitHub Actions em:

- `.github/workflows/api-tests.yml`

A workflow:

- inicia a API ServeRest em um container
- aguarda a aplicação ficar disponível
- instala o Bruno CLI
- executa a coleção de testes
- publica o relatório em artefato JUnit

## Relatórios

Os arquivos de resultado gerados em `reports/` são artefatos de execução e ficam ignorados no versionamento para evitar ruído no repositório. O projeto já inclui regra no `.gitignore` para esse comportamento.

## Troubleshooting

### A API não responde

Verifique se o container do ServeRest foi iniciado corretamente e se a porta 3000 está disponível.

```bash
docker ps
curl http://localhost:3000/produtos
```

### Bruno não encontra o ambiente

Confirme se o arquivo `collection/environments/local.yml` existe e se o comando foi executado com o ambiente correto:

```bash
bru run --env local
```

### Token ausente ou inválido

Revise a etapa de login e valide se o `authorization` foi capturado corretamente no `after-response` script.

### Erros de status 400 ou 401

Verifique payload, token, permissões e regras de negócio aplicadas ao endpoint acessado.

## Contribuição

Para contribuir com melhorias na suíte:

1. clone o repositório
2. crie uma branch
3. adicione ou ajuste cenários de teste
4. valide a execução localmente
5. abra um pull request com descrição clara

## Autor

- Rafael Felipe
- [![LinkedIn](https://img.shields.io/badge/LinkedIn-Rafael%20Felipe-0A66C2?logo=linkedin&logoColor=white)](https://www.linkedin.com/in/rafaelrfelipe/)
