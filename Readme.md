# SelfCare Finance

Aplicação web para controle financeiro pessoal e acompanhamento de rotina, desenvolvida com **ASP.NET Core** no back-end e **Angular** no front-end.

O objetivo do projeto é centralizar, em uma única plataforma, o registro de receitas, despesas, hábitos e atividades recorrentes, permitindo que o usuário acompanhe sua organização financeira e sua disciplina pessoal ao longo do tempo.

## Visão geral

O **SelfCare Finance** foi pensado como uma aplicação pessoal para auxiliar no acompanhamento de duas áreas importantes da rotina: finanças e autocuidado.

A aplicação permitirá que o usuário registre suas movimentações financeiras, acompanhe seus gastos por categoria, visualize resumos mensais e também registre atividades de rotina, como dias em que foi à academia, estudos, leitura, cardio ou outros hábitos recorrentes.

## Objetivos do projeto

* Controlar receitas e despesas pessoais.
* Organizar movimentações financeiras por categorias.
* Acompanhar saldo mensal.
* Registrar hábitos e atividades de rotina.
* Controlar frequência na academia.
* Visualizar resumos financeiros e de rotina.
* Servir como projeto de estudo e portfólio utilizando .NET e Angular.

## Estrutura do repositório

```txt
selfcare-finance/
├── docs/
├── selfcare-finance-api/
└── selfcare-finance-web/
```

### `docs/`

Contém a documentação geral do projeto, incluindo requisitos, modelagem do banco de dados, endpoints da API, regras de negócio, roadmap e checklist de implementação.

### `selfcare-finance-api/`

Projeto back-end da aplicação, responsável pela API REST, regras de negócio, autenticação, persistência dos dados e integração com o banco de dados.

### `selfcare-finance-web/`

Projeto front-end da aplicação, responsável pela interface web, navegação, formulários, dashboards e comunicação com a API.

## Tecnologias previstas

### Back-end

* ASP.NET Core
* Entity Framework Core
* PostgreSQL
* JWT para autenticação
* Swagger/OpenAPI para documentação da API

### Front-end

* Angular
* TypeScript
* Angular Router
* Reactive Forms
* HTTP Client

### Infraestrutura e ferramentas

* Git e GitHub
* Docker
* Docker Compose
* PostgreSQL
* Visual Studio Code

## Módulos principais

## Autenticação

Módulo responsável pelo cadastro, login e controle de acesso dos usuários.

Funcionalidades previstas:

* Cadastro de usuário.
* Login com e-mail e senha.
* Geração de token JWT.
* Proteção de rotas autenticadas.
* Recuperação dos dados do usuário logado.

## Controle financeiro

Módulo responsável pelo registro e acompanhamento das movimentações financeiras.

Funcionalidades previstas:

* Cadastro de receitas.
* Cadastro de despesas.
* Cadastro de categorias financeiras.
* Classificação por tipo de transação.
* Filtro por mês, ano, categoria e tipo.
* Visualização do saldo mensal.
* Resumo de receitas e despesas.

## Controle de rotina

Módulo responsável pelo acompanhamento de atividades recorrentes e hábitos pessoais.

Funcionalidades previstas:

* Cadastro de atividades de rotina.
* Registro de atividades realizadas por data.
* Controle de frequência na academia.
* Acompanhamento de hábitos semanais e mensais.
* Histórico de atividades realizadas.

## Dashboard

Módulo responsável por apresentar uma visão resumida das informações do usuário.

Funcionalidades previstas:

* Resumo financeiro mensal.
* Total de receitas.
* Total de despesas.
* Saldo do mês.
* Quantidade de dias de academia no mês.
* Hábitos concluídos na semana.
* Indicadores gerais de rotina.

## MVP

A primeira versão funcional do projeto deve conter apenas as funcionalidades essenciais.

### Funcionalidades incluídas no MVP

* Cadastro de usuário.
* Login de usuário.
* Cadastro de categorias financeiras.
* Cadastro de receitas e despesas.
* Listagem de transações financeiras.
* Filtro de transações por mês.
* Resumo financeiro mensal.
* Cadastro de atividades de rotina.
* Registro de dias de academia.
* Resumo básico de rotina.

### Funcionalidades fora do MVP

* Controle de cartão de crédito.
* Parcelamento de compras.
* Importação de extratos bancários.
* Notificações.
* Integração com bancos.
* Controle detalhado de treinos.
* Aplicativo mobile.
* Relatórios avançados.

## Comunicação entre os projetos

A comunicação entre front-end e back-end será feita por meio de requisições HTTP utilizando JSON.

```txt
Angular Web
    |
    | HTTP/JSON
    v
ASP.NET Core API
    |
    | Entity Framework Core
    v
PostgreSQL
```

Exemplo de endpoint:

```txt
GET /api/financial-transactions?month=7&year=2026
```

Exemplo de resposta:

```json
[
  {
    "id": "uuid",
    "description": "Academia",
    "amount": 120.00,
    "type": "Expense",
    "category": "Saúde",
    "transactionDate": "2026-07-06"
  }
]
```

## Organização da documentação

A documentação do projeto será mantida na pasta `docs/`.

Estrutura inicial prevista:

```txt
docs/
├── 01-visao-geral.md
├── 02-requisitos-funcionais.md
├── 03-requisitos-nao-funcionais.md
├── 04-modelagem-banco.md
├── 05-api-endpoints.md
├── 06-regras-de-negocio.md
├── 07-checklist-implementacao.md
└── 08-roadmap.md
```

## Autor

Pablo Yohan
