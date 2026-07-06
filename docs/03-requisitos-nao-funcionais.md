# Requisitos Não Funcionais

Este documento descreve os requisitos não funcionais previstos para o projeto **SelfCare Finance**.

Os requisitos não funcionais definem características de qualidade, segurança, desempenho, organização e manutenção do sistema. Diferente dos requisitos funcionais, eles não descrevem diretamente uma funcionalidade, mas sim como o sistema deve se comportar.

## Categorias contempladas

* Segurança
* Autenticação e autorização
* Privacidade e isolamento de dados
* Desempenho
* Disponibilidade
* Usabilidade
* Responsividade
* Manutenibilidade
* Escalabilidade
* Banco de dados
* API
* Front-end
* Testes
* Versionamento
* Deploy e ambiente

---

# 1. Segurança

## RNF001 - Armazenamento seguro de senhas

O sistema deve armazenar senhas de usuários utilizando algoritmos seguros de hash.

As senhas nunca devem ser salvas em texto puro no banco de dados.

## RNF002 - Comunicação segura em produção

Em ambiente de produção, a comunicação entre usuário, front-end e back-end deve ocorrer por meio de HTTPS.

## RNF003 - Proteção contra acesso indevido

O sistema deve impedir que usuários acessem, alterem ou excluam dados pertencentes a outros usuários.

Esse requisito se aplica a:

* Transações financeiras
* Categorias financeiras
* Atividades de rotina
* Registros de rotina
* Metas
* Dados pessoais do usuário

## RNF004 - Validação de dados de entrada

O sistema deve validar os dados recebidos nas requisições antes de processá-los.

Exemplos de validações:

* Campos obrigatórios
* Formato de e-mail
* Tamanho mínimo de senha
* Valores monetários maiores que zero
* Datas válidas
* Tipos permitidos para transações e registros

## RNF005 - Tratamento de erros

O sistema deve tratar erros de forma controlada, evitando expor detalhes internos da aplicação ao usuário final.

Erros técnicos, como stack traces, não devem ser retornados diretamente nas respostas da API em produção.

## RNF006 - Proteção contra dados sensíveis em logs

O sistema não deve registrar informações sensíveis nos logs.

Exemplos de dados que não devem aparecer em logs:

* Senhas
* Tokens JWT
* Dados completos de autenticação
* Informações sensíveis do usuário

---

# 2. Autenticação e autorização

## RNF007 - Uso de autenticação baseada em token

A API deve utilizar autenticação baseada em token, preferencialmente JWT.

## RNF008 - Expiração do token

O token de autenticação deve possuir tempo de expiração definido.

## RNF009 - Proteção de endpoints privados

Todos os endpoints que manipulam dados do usuário devem exigir autenticação.

Exemplos:

* Transações financeiras
* Categorias financeiras
* Atividades de rotina
* Registros de rotina
* Dashboard
* Metas

## RNF010 - Autorização baseada no usuário autenticado

A API deve utilizar o usuário autenticado como base para consultar e manipular dados.

O usuário não deve conseguir informar manualmente outro `userId` para acessar dados de terceiros.

---

# 3. Privacidade e isolamento de dados

## RNF011 - Isolamento lógico dos dados

Os dados devem ser isolados por usuário no banco de dados.

Cada registro relacionado ao usuário deve possuir vínculo com o identificador do usuário proprietário.

## RNF012 - Consulta restrita ao usuário autenticado

Toda consulta a dados privados deve retornar apenas informações pertencentes ao usuário autenticado.

## RNF013 - Manipulação restrita ao proprietário do dado

Operações de criação, edição e exclusão devem ser restritas ao usuário proprietário do dado.

## RNF014 - Preparação para exclusão de dados

O sistema deve ser projetado considerando a possibilidade de exclusão futura dos dados do usuário.

---

# 4. Desempenho

## RNF015 - Tempo de resposta adequado

A API deve responder às requisições comuns em tempo adequado para uso interativo.

Como referência inicial, endpoints simples devem responder em até poucos segundos em ambiente local ou de produção básica.

## RNF016 - Paginação em listagens

Listagens que podem crescer com o tempo devem possuir suporte a paginação.

Exemplos:

* Transações financeiras
* Registros de rotina
* Metas
* Histórico de atividades

## RNF017 - Filtros eficientes

Consultas com filtros por período, tipo, categoria ou atividade devem ser implementadas de forma eficiente no banco de dados.

## RNF018 - Evitar carregamento desnecessário de dados

A API deve retornar apenas os dados necessários para cada tela ou funcionalidade.

Dados muito detalhados devem ser consultados apenas quando necessário.

---

# 5. Disponibilidade

## RNF019 - Execução em ambiente local

O sistema deve ser capaz de executar em ambiente local de desenvolvimento.

O ambiente local deve permitir subir, no mínimo:

* API
* Front-end
* Banco de dados

## RNF020 - Uso de Docker para dependências

O banco de dados e possíveis serviços auxiliares devem poder ser executados por meio de Docker Compose.

## RNF021 - Configuração por ambiente

O sistema deve permitir configurações diferentes para ambientes distintos.

Ambientes previstos:

* Desenvolvimento
* Testes
* Produção

---

# 6. Usabilidade

## RNF022 - Interface simples e objetiva

A interface deve ser simples e objetiva, facilitando o registro rápido de informações financeiras e de rotina.

## RNF023 - Feedback ao usuário

O sistema deve apresentar feedback visual para ações realizadas pelo usuário.

Exemplos:

* Cadastro realizado com sucesso
* Erro ao salvar
* Campo obrigatório não preenchido
* Login inválido
* Registro excluído com sucesso

## RNF024 - Mensagens de erro compreensíveis

As mensagens de erro exibidas ao usuário devem ser claras e compreensíveis.

Sempre que possível, devem evitar termos técnicos.

## RNF025 - Navegação intuitiva

O front-end deve possuir navegação organizada entre os principais módulos da aplicação.

Módulos principais:

* Dashboard
* Finanças
* Rotina
* Metas
* Perfil

---

# 7. Responsividade

## RNF026 - Suporte a diferentes tamanhos de tela

A aplicação web deve ser responsiva, permitindo uso adequado em:

* Desktop
* Notebook
* Tablet
* Celular

## RNF027 - Layout adaptável

Os componentes visuais devem se adaptar ao tamanho da tela.

Exemplos:

* Menus
* Cards
* Tabelas
* Formulários
* Gráficos

## RNF028 - Tabelas utilizáveis em telas menores

Tabelas com muitas informações devem ser adaptadas para uso em telas menores.

Possíveis soluções:

* Scroll horizontal
* Cards responsivos
* Redução de colunas
* Filtros para refinar os dados exibidos

---

# 8. Manutenibilidade

## RNF029 - Separação entre front-end e back-end

O projeto deve manter separação clara entre a aplicação front-end e a aplicação back-end.

Estrutura prevista:

```txt
selfcare-finance/
├── selfcare-finance-api/
└── selfcare-finance-web/
```

## RNF030 - Organização em camadas no back-end

O back-end deve ser organizado de forma a separar responsabilidades.

Estrutura inicial sugerida:

* Controllers
* Services
* Repositories
* DTOs
* Entities
* Enums
* Data

## RNF031 - Organização por features no front-end

O front-end deve ser organizado por funcionalidades ou módulos.

Estrutura inicial sugerida:

* Auth
* Dashboard
* Finance
* Routine
* Goals
* Shared
* Core

## RNF032 - Código legível

O código deve priorizar clareza, legibilidade e nomes descritivos.

## RNF033 - Baixo acoplamento

Sempre que possível, as regras de negócio devem ficar separadas de detalhes de infraestrutura, interface e persistência.

## RNF034 - Facilidade de evolução

A estrutura do projeto deve permitir a adição futura de novos módulos sem exigir grandes alterações na base existente.

---

# 9. Escalabilidade

## RNF035 - Estrutura preparada para crescimento

O sistema deve ser desenvolvido de forma que permita crescimento gradual.

Exemplos de crescimento futuro:

* Controle de cartão de crédito
* Parcelamento de compras
* Relatórios avançados
* Notificações
* Aplicativo mobile
* Integração com bancos

## RNF036 - API independente do front-end

A API deve ser independente da interface Angular, permitindo que futuramente outros clientes possam consumir os mesmos endpoints.

Exemplos:

* Aplicativo mobile
* Integrações externas
* Painel administrativo

## RNF037 - Banco preparado para novos módulos

A modelagem do banco deve considerar a possibilidade de adicionar novas entidades e relacionamentos no futuro.

---

# 10. Banco de dados

## RNF038 - Uso de banco relacional

O sistema deve utilizar um banco de dados relacional.

Banco previsto:

* PostgreSQL

## RNF039 - Uso de migrations

Alterações na estrutura do banco de dados devem ser controladas por migrations.

No back-end .NET, as migrations devem ser gerenciadas pelo Entity Framework Core.

## RNF040 - Chaves primárias consistentes

As entidades principais devem possuir identificadores únicos.

Preferencialmente, devem ser utilizados identificadores do tipo UUID ou GUID.

## RNF041 - Relacionamentos bem definidos

As tabelas devem possuir relacionamentos bem definidos entre si.

Exemplos:

* Usuário e transações financeiras
* Usuário e categorias
* Usuário e atividades de rotina
* Atividades de rotina e registros
* Usuário e metas

## RNF042 - Controle de data de criação e atualização

As entidades principais devem possuir campos de controle de data.

Campos previstos:

* CreatedAt
* UpdatedAt

## RNF043 - Integridade referencial

O banco de dados deve preservar a integridade referencial entre entidades relacionadas.

---

# 11. API

## RNF044 - API REST

O back-end deve expor uma API no estilo REST.

## RNF045 - Uso de JSON

A comunicação entre front-end e back-end deve utilizar JSON como formato principal de troca de dados.

## RNF046 - Padronização de respostas

A API deve manter um padrão consistente de respostas.

As respostas devem ser previsíveis para:

* Sucesso
* Erro de validação
* Recurso não encontrado
* Erro de autenticação
* Erro interno

## RNF047 - Uso adequado de códigos HTTP

A API deve utilizar códigos HTTP adequados.

Exemplos:

* `200 OK`
* `201 Created`
* `204 No Content`
* `400 Bad Request`
* `401 Unauthorized`
* `403 Forbidden`
* `404 Not Found`
* `500 Internal Server Error`

## RNF048 - Documentação com Swagger/OpenAPI

A API deve possuir documentação acessível por Swagger/OpenAPI durante o desenvolvimento.

## RNF049 - Versionamento futuro da API

A estrutura da API deve considerar a possibilidade de versionamento futuro.

Exemplo:

```txt
/api/v1/financial-transactions
```

---

# 12. Front-end

## RNF050 - Uso de Angular

O front-end deve ser desenvolvido utilizando Angular.

## RNF051 - Uso de TypeScript

O front-end deve utilizar TypeScript como linguagem principal.

## RNF052 - Rotas organizadas

As rotas da aplicação devem ser organizadas por módulo.

Exemplos:

* `/login`
* `/register`
* `/dashboard`
* `/finance`
* `/routine`
* `/goals`

## RNF053 - Proteção de rotas no front-end

Rotas internas devem ser protegidas por guards de autenticação.

## RNF054 - Interceptor para autenticação

O front-end deve utilizar interceptor HTTP para incluir automaticamente o token JWT nas requisições autenticadas.

## RNF055 - Formulários com validação

Os formulários devem possuir validações no front-end.

Exemplos:

* Campo obrigatório
* E-mail válido
* Valor numérico válido
* Data obrigatória
* Senha com tamanho mínimo

## RNF056 - Separação entre serviços e componentes

A comunicação com a API deve ser concentrada em services, evitando chamadas HTTP diretamente dentro dos componentes quando possível.

---

# 13. Testes

## RNF057 - Testes unitários no back-end

O back-end deve possuir testes unitários para regras de negócio principais.

Exemplos:

* Cadastro de transações
* Cálculo de resumo financeiro
* Registro de rotina
* Validação de proprietário do dado

## RNF058 - Testes de integração no back-end

O back-end poderá possuir testes de integração para validar o funcionamento dos endpoints e do banco de dados.

## RNF059 - Testes no front-end

O front-end poderá possuir testes para componentes, services e fluxos principais.

## RNF060 - Testes para autenticação

O sistema deve possuir testes para os principais fluxos de autenticação.

Exemplos:

* Cadastro
* Login
* Acesso sem token
* Acesso com token inválido
* Acesso com token válido

---

# 14. Versionamento e organização do código

## RNF061 - Uso de Git

O projeto deve utilizar Git para controle de versão.

## RNF062 - Histórico de commits claro

Os commits devem possuir mensagens claras, indicando a alteração realizada.

Exemplos:

```txt
docs: add project overview
feat: add user authentication
fix: correct transaction validation
refactor: improve finance service
```

## RNF063 - Uso de branches por funcionalidade

O desenvolvimento poderá utilizar branches separadas por funcionalidade.

Exemplos:

```txt
feature/api-auth
feature/api-finance
feature/web-auth
feature/web-dashboard
```

## RNF064 - README atualizado

O README principal deve ser mantido atualizado com informações básicas sobre o projeto.

## RNF065 - Documentação atualizada

A documentação da pasta `docs` deve ser atualizada conforme o projeto evoluir.

---

# 15. Deploy e ambiente

## RNF066 - Variáveis de ambiente

Configurações sensíveis ou dependentes de ambiente devem ser definidas por variáveis de ambiente.

Exemplos:

* String de conexão do banco
* Chave de assinatura JWT
* URL da API
* Configurações de ambiente

## RNF067 - Separação de configurações por ambiente

O sistema deve separar configurações de desenvolvimento, teste e produção.

## RNF068 - Preparação para deploy futuro

O projeto deve ser organizado considerando um possível deploy futuro.

Possibilidades:

* Render
* Railway
* Azure
* AWS
* VPS
* Docker

## RNF069 - Build independente dos projetos

O front-end e o back-end devem poder ser construídos de forma independente.

## RNF070 - Execução local simplificada

O projeto deve buscar facilitar a execução local por meio de instruções documentadas.

Exemplos:

* Como rodar a API
* Como rodar o front-end
* Como subir o banco
* Como executar migrations

---

# 16. Requisitos não funcionais prioritários para o MVP

Os requisitos abaixo devem ser priorizados na primeira versão funcional do sistema.

## Segurança e autenticação

* RNF001 - Armazenamento seguro de senhas
* RNF003 - Proteção contra acesso indevido
* RNF004 - Validação de dados de entrada
* RNF007 - Uso de autenticação baseada em token
* RNF009 - Proteção de endpoints privados
* RNF010 - Autorização baseada no usuário autenticado
* RNF011 - Isolamento lógico dos dados

## Banco de dados

* RNF038 - Uso de banco relacional
* RNF039 - Uso de migrations
* RNF040 - Chaves primárias consistentes
* RNF041 - Relacionamentos bem definidos
* RNF042 - Controle de data de criação e atualização
* RNF043 - Integridade referencial

## API

* RNF044 - API REST
* RNF045 - Uso de JSON
* RNF046 - Padronização de respostas
* RNF047 - Uso adequado de códigos HTTP
* RNF048 - Documentação com Swagger/OpenAPI

## Front-end

* RNF050 - Uso de Angular
* RNF051 - Uso de TypeScript
* RNF052 - Rotas organizadas
* RNF053 - Proteção de rotas no front-end
* RNF054 - Interceptor para autenticação
* RNF055 - Formulários com validação

## Organização

* RNF029 - Separação entre front-end e back-end
* RNF030 - Organização em camadas no back-end
* RNF031 - Organização por features no front-end
* RNF061 - Uso de Git
* RNF064 - README atualizado
* RNF065 - Documentação atualizada

---

# 17. Observações

Os requisitos não funcionais poderão ser ajustados conforme o projeto evoluir.

Durante o desenvolvimento, cada requisito poderá ser relacionado a decisões técnicas, padrões de arquitetura, testes e configurações de ambiente.
