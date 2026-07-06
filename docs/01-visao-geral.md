# Visão Geral do Projeto

## Nome do projeto

SelfCare Finance

## Descrição

O **SelfCare Finance** é uma aplicação web voltada para o controle financeiro pessoal e o acompanhamento de rotina. O sistema tem como objetivo centralizar, em uma única plataforma, informações relacionadas a receitas, despesas, hábitos, atividades recorrentes e frequência em compromissos pessoais, como academia, estudos e outras práticas de autocuidado.

A proposta do projeto é permitir que o usuário tenha uma visão mais clara sobre sua organização financeira e sua disciplina pessoal, facilitando o acompanhamento mensal de gastos, receitas e atividades realizadas ao longo do tempo.

O sistema será desenvolvido utilizando **ASP.NET Core** no back-end e **Angular** no front-end, com comunicação entre as aplicações por meio de uma API REST.

## Contexto

Muitas pessoas utilizam ferramentas separadas para controlar diferentes áreas da vida pessoal. Por exemplo, uma planilha para gastos, um aplicativo para hábitos, anotações para academia e lembretes em calendários.

Essa separação pode dificultar o acompanhamento geral da rotina, pois as informações ficam espalhadas em diferentes lugares. Além disso, ferramentas muito complexas podem acabar dificultando o uso contínuo, especialmente quando o objetivo é apenas registrar informações simples do dia a dia.

Dessa forma, o **SelfCare Finance** surge como uma proposta de sistema simples, centralizado e extensível, permitindo o controle de finanças pessoais e de hábitos de rotina em uma mesma aplicação.

## Problema

O problema principal que o projeto busca resolver é a dificuldade de acompanhar, de forma organizada, informações financeiras e atividades pessoais recorrentes.

Entre os principais pontos observados, destacam-se:

* Falta de centralização entre finanças e rotina.
* Dificuldade em visualizar gastos mensais de forma simples.
* Ausência de um histórico organizado de atividades pessoais.
* Dificuldade em acompanhar frequência na academia ou em outros hábitos.
* Uso de ferramentas separadas, como planilhas, aplicativos de notas e calendários.
* Falta de indicadores simples para acompanhar evolução mensal.

## Solução proposta

A solução proposta é uma aplicação web que permita ao usuário registrar e consultar informações financeiras e de rotina em um único ambiente.

Inicialmente, o sistema deve permitir o cadastro de receitas, despesas, categorias financeiras, atividades de rotina e registros diários de atividades realizadas. Além disso, deve apresentar resumos mensais e semanais, permitindo uma visualização rápida da situação financeira e da consistência da rotina do usuário.

A aplicação será dividida em dois projetos principais:

```txt
selfcare-finance/
├── selfcare-finance-api/
└── selfcare-finance-web/
```

O projeto `selfcare-finance-api` será responsável pelo back-end, incluindo regras de negócio, autenticação, persistência de dados e exposição dos endpoints REST.

O projeto `selfcare-finance-web` será responsável pelo front-end, incluindo telas, formulários, navegação, dashboards e comunicação com a API.

## Objetivos

## Objetivo geral

Desenvolver uma aplicação web para controle financeiro pessoal e acompanhamento de rotina, permitindo que o usuário registre, organize e visualize suas informações de forma simples e centralizada.

## Objetivos específicos

* Permitir o cadastro e autenticação de usuários.
* Permitir o cadastro de receitas e despesas.
* Permitir a organização de movimentações financeiras por categorias.
* Permitir a visualização de saldo mensal.
* Permitir o acompanhamento de gastos por período.
* Permitir o cadastro de atividades de rotina.
* Permitir o registro de dias em que uma atividade foi realizada.
* Permitir o acompanhamento de frequência na academia.
* Apresentar um dashboard com indicadores financeiros e de rotina.
* Organizar o projeto em uma arquitetura separada entre front-end e back-end.
* Servir como projeto de estudo e portfólio utilizando .NET e Angular.

## Público-alvo

O público-alvo do sistema são pessoas que desejam organizar melhor suas finanças pessoais e acompanhar hábitos ou atividades recorrentes da rotina.

O projeto é especialmente útil para usuários que desejam:

* Controlar gastos mensais.
* Registrar receitas e despesas.
* Acompanhar frequência na academia.
* Monitorar hábitos de estudo, leitura ou exercícios.
* Ter uma visão geral da própria rotina.
* Substituir planilhas simples por uma aplicação web personalizada.

## Escopo inicial

O escopo inicial do projeto será focado em funcionalidades essenciais para a criação de um MVP.

As principais áreas contempladas serão:

* Autenticação de usuário.
* Controle financeiro básico.
* Controle de rotina básico.
* Dashboard inicial.
* Documentação do projeto.
* Separação entre aplicação front-end e back-end.

## Fora do escopo inicial

Algumas funcionalidades poderão ser adicionadas futuramente, mas não fazem parte da primeira versão do projeto.

Ficam fora do escopo inicial:

* Controle completo de cartão de crédito.
* Parcelamento de compras.
* Importação automática de extratos bancários.
* Integração com instituições financeiras.
* Notificações por e-mail ou push.
* Aplicativo mobile.
* Controle detalhado de treinos com séries, cargas e repetições.
* Integração com dispositivos vestíveis.
* Relatórios financeiros avançados.
* Compartilhamento de dados entre usuários.

## Módulos do sistema

## Autenticação

Módulo responsável pelo controle de acesso à aplicação.

Funcionalidades previstas:

* Cadastro de usuário.
* Login com e-mail e senha.
* Geração de token JWT.
* Proteção de rotas autenticadas.
* Consulta dos dados do usuário logado.

## Controle financeiro

Módulo responsável pelo registro e gerenciamento das movimentações financeiras do usuário.

Funcionalidades previstas:

* Cadastro de receitas.
* Cadastro de despesas.
* Cadastro de categorias financeiras.
* Classificação das transações por tipo.
* Definição de data da transação.
* Definição de forma de pagamento.
* Filtro por mês, ano, categoria e tipo.
* Visualização de resumo financeiro mensal.

## Controle de rotina

Módulo responsável pelo acompanhamento de hábitos e atividades recorrentes.

Funcionalidades previstas:

* Cadastro de atividades de rotina.
* Registro de atividades realizadas por data.
* Controle de frequência na academia.
* Registro de observações sobre a atividade.
* Visualização de histórico de atividades.
* Resumo semanal e mensal de atividades realizadas.

## Dashboard

Módulo responsável por apresentar informações resumidas ao usuário.

Funcionalidades previstas:

* Total de receitas do mês.
* Total de despesas do mês.
* Saldo mensal.
* Quantidade de transações no mês.
* Quantidade de dias com academia no mês.
* Quantidade de hábitos concluídos na semana.
* Indicadores simples de evolução da rotina.

## Arquitetura geral

A arquitetura inicial do sistema será baseada na separação entre front-end, back-end e banco de dados.

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

O front-end será responsável pela interface do usuário e irá consumir os dados disponibilizados pela API.

O back-end será responsável por processar as requisições, aplicar regras de negócio, autenticar usuários e realizar a comunicação com o banco de dados.

O banco de dados será responsável por armazenar usuários, transações financeiras, categorias, atividades de rotina e registros diários.

## Tecnologias previstas

## Back-end

* ASP.NET Core
* C#
* Entity Framework Core
* PostgreSQL
* JWT
* Swagger/OpenAPI

## Front-end

* Angular
* TypeScript
* Angular Router
* Reactive Forms
* HTTP Client

## Banco de dados

* PostgreSQL

## Ferramentas de apoio

* Git
* GitHub
* Docker
* Docker Compose
* Visual Studio Code

## Entidades iniciais previstas

As principais entidades previstas para a primeira versão são:

* User
* FinancialTransaction
* FinancialCategory
* RoutineActivity
* RoutineRecord
* Goal

## MVP

O MVP do projeto deve entregar uma primeira versão funcional da aplicação, permitindo o uso básico do controle financeiro e do controle de rotina.

## Funcionalidades do MVP

* Cadastro de usuário.
* Login de usuário.
* Cadastro de categorias financeiras.
* Cadastro de receitas.
* Cadastro de despesas.
* Listagem de transações financeiras.
* Filtro de transações por mês.
* Resumo financeiro mensal.
* Cadastro de atividades de rotina.
* Registro de dias de academia.
* Registro de atividades realizadas.
* Resumo básico de rotina.
* Dashboard inicial.

## Critérios de sucesso do MVP

O MVP será considerado concluído quando o usuário conseguir:

* Criar uma conta.
* Realizar login.
* Cadastrar receitas e despesas.
* Consultar suas movimentações financeiras.
* Visualizar um resumo financeiro mensal.
* Cadastrar atividades de rotina.
* Registrar dias em que foi à academia.
* Visualizar um resumo simples da sua rotina.

## Organização do projeto

A estrutura inicial do repositório será:

```txt
selfcare-finance/
├── docs/
├── selfcare-finance-api/
└── selfcare-finance-web/
```

## Documentação prevista

A documentação será mantida na pasta `docs/`.

Documentos previstos:

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

## Status

Projeto em fase inicial de documentação e planejamento.

## Autor

Pablo Yohan
