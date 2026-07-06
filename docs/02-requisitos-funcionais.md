# Requisitos Funcionais

Este documento descreve os requisitos funcionais previstos para o projeto **SelfCare Finance**.

Os requisitos funcionais representam as funcionalidades que o sistema deve oferecer ao usuário, organizadas por módulo.

## Módulos contemplados

* Autenticação
* Usuário
* Controle financeiro
* Categorias financeiras
* Controle de rotina
* Registros de rotina
* Dashboard
* Metas
* Administração dos próprios dados

---

# 1. Autenticação

## RF001 - Cadastro de usuário

O sistema deve permitir que um novo usuário crie uma conta informando, no mínimo:

* Nome
* E-mail
* Senha

A senha deve ser armazenada de forma segura no back-end, nunca em texto puro.

## RF002 - Login de usuário

O sistema deve permitir que o usuário realize login utilizando e-mail e senha.

Ao realizar login com sucesso, o sistema deve retornar um token de autenticação para permitir o acesso às funcionalidades protegidas.

## RF003 - Logout de usuário

O sistema deve permitir que o usuário realize logout da aplicação.

No front-end, o logout deve remover os dados de autenticação armazenados localmente.

## RF004 - Proteção de rotas autenticadas

O sistema deve impedir que usuários não autenticados acessem funcionalidades internas da aplicação.

Exemplos de áreas protegidas:

* Dashboard
* Transações financeiras
* Categorias financeiras
* Atividades de rotina
* Registros de rotina
* Metas

## RF005 - Consulta do usuário autenticado

O sistema deve permitir consultar os dados básicos do usuário autenticado.

Dados previstos:

* Identificador do usuário
* Nome
* E-mail

---

# 2. Usuário

## RF006 - Atualização dos dados do usuário

O sistema deve permitir que o usuário atualize seus dados básicos.

Dados editáveis previstos:

* Nome
* E-mail

## RF007 - Alteração de senha

O sistema deve permitir que o usuário altere sua senha.

Para isso, poderá ser necessário informar:

* Senha atual
* Nova senha
* Confirmação da nova senha

## RF008 - Exclusão da conta

O sistema deve permitir que o usuário exclua sua própria conta.

A exclusão da conta deve remover ou inutilizar os dados vinculados ao usuário, de acordo com a estratégia definida nas regras de negócio.

---

# 3. Controle financeiro

## RF009 - Cadastro de transação financeira

O sistema deve permitir que o usuário cadastre uma transação financeira.

A transação deve conter, no mínimo:

* Descrição
* Valor
* Tipo da transação
* Categoria
* Data da transação

Campos adicionais previstos:

* Forma de pagamento
* Observação
* Indicação se é uma transação fixa
* Data de criação
* Data de atualização

Tipos de transação previstos:

* Receita
* Despesa

## RF010 - Listagem de transações financeiras

O sistema deve permitir que o usuário visualize suas transações financeiras cadastradas.

A listagem deve exibir, no mínimo:

* Descrição
* Valor
* Tipo
* Categoria
* Data da transação

## RF011 - Consulta de transação por identificador

O sistema deve permitir que o usuário consulte os detalhes de uma transação financeira específica.

A consulta deve retornar apenas transações pertencentes ao usuário autenticado.

## RF012 - Atualização de transação financeira

O sistema deve permitir que o usuário atualize uma transação financeira cadastrada anteriormente.

Campos editáveis previstos:

* Descrição
* Valor
* Tipo
* Categoria
* Data da transação
* Forma de pagamento
* Observação
* Indicação se é fixa

## RF013 - Exclusão de transação financeira

O sistema deve permitir que o usuário exclua uma transação financeira cadastrada anteriormente.

A exclusão deve afetar apenas transações pertencentes ao usuário autenticado.

## RF014 - Filtro de transações por período

O sistema deve permitir que o usuário filtre transações financeiras por período.

Filtros previstos:

* Mês
* Ano
* Data inicial
* Data final

## RF015 - Filtro de transações por tipo

O sistema deve permitir que o usuário filtre transações financeiras por tipo.

Tipos previstos:

* Receita
* Despesa

## RF016 - Filtro de transações por categoria

O sistema deve permitir que o usuário filtre transações financeiras por categoria.

## RF017 - Filtro de transações por forma de pagamento

O sistema deve permitir que o usuário filtre transações financeiras por forma de pagamento.

Formas de pagamento previstas:

* Dinheiro
* Pix
* Cartão de débito
* Cartão de crédito
* Transferência
* Boleto
* Outro

## RF018 - Identificação de transações fixas

O sistema deve permitir que o usuário marque uma transação como fixa.

Exemplos de transações fixas:

* Aluguel
* Mensalidade da academia
* Internet
* Assinaturas
* Plano de celular

## RF019 - Resumo financeiro mensal

O sistema deve permitir que o usuário visualize um resumo financeiro mensal contendo:

* Total de receitas
* Total de despesas
* Saldo do mês
* Quantidade de transações
* Total de despesas fixas
* Total de despesas variáveis

## RF020 - Consulta de despesas por categoria

O sistema deve permitir que o usuário visualize o total de despesas agrupadas por categoria em determinado período.

Exemplos:

* Alimentação
* Transporte
* Saúde
* Lazer
* Academia
* Educação

## RF021 - Consulta de receitas por categoria

O sistema deve permitir que o usuário visualize o total de receitas agrupadas por categoria em determinado período.

Exemplos:

* Salário
* Freelance
* Reembolso
* Investimentos
* Outros

---

# 4. Categorias financeiras

## RF022 - Cadastro de categoria financeira

O sistema deve permitir que o usuário cadastre categorias financeiras.

A categoria deve conter, no mínimo:

* Nome
* Tipo
* Cor opcional

Tipos previstos:

* Receita
* Despesa
* Ambos

## RF023 - Listagem de categorias financeiras

O sistema deve permitir que o usuário visualize suas categorias financeiras cadastradas.

## RF024 - Consulta de categoria por identificador

O sistema deve permitir que o usuário consulte os detalhes de uma categoria financeira específica.

## RF025 - Atualização de categoria financeira

O sistema deve permitir que o usuário atualize uma categoria financeira cadastrada anteriormente.

Campos editáveis previstos:

* Nome
* Tipo
* Cor

## RF026 - Exclusão de categoria financeira

O sistema deve permitir que o usuário exclua uma categoria financeira cadastrada anteriormente.

A exclusão deve respeitar as regras de negócio relacionadas a transações vinculadas.

## RF027 - Criação de categorias financeiras padrão

O sistema pode criar categorias financeiras padrão para novos usuários.

Categorias de despesa sugeridas:

* Alimentação
* Transporte
* Moradia
* Saúde
* Educação
* Academia
* Lazer
* Assinaturas
* Outros

Categorias de receita sugeridas:

* Salário
* Freelance
* Reembolso
* Investimentos
* Outros

---

# 5. Controle de rotina

## RF028 - Cadastro de atividade de rotina

O sistema deve permitir que o usuário cadastre uma atividade de rotina.

A atividade deve conter, no mínimo:

* Nome
* Tipo
* Descrição opcional
* Frequência semanal desejada

Exemplos de atividades:

* Academia
* Cardio
* Estudo de inglês
* Estudo de japonês
* Programação
* Leitura
* Sono adequado

## RF029 - Listagem de atividades de rotina

O sistema deve permitir que o usuário visualize suas atividades de rotina cadastradas.

## RF030 - Consulta de atividade por identificador

O sistema deve permitir que o usuário consulte os detalhes de uma atividade de rotina específica.

## RF031 - Atualização de atividade de rotina

O sistema deve permitir que o usuário atualize uma atividade de rotina cadastrada anteriormente.

Campos editáveis previstos:

* Nome
* Tipo
* Descrição
* Frequência semanal desejada
* Status da atividade

## RF032 - Exclusão de atividade de rotina

O sistema deve permitir que o usuário exclua uma atividade de rotina cadastrada anteriormente.

A exclusão deve respeitar as regras relacionadas aos registros vinculados à atividade.

## RF033 - Ativação e desativação de atividade de rotina

O sistema deve permitir que o usuário ative ou desative uma atividade de rotina.

Atividades desativadas não devem aparecer como prioridade nas telas principais, mas podem continuar existindo no histórico.

---

# 6. Registros de rotina

## RF034 - Registro de atividade realizada

O sistema deve permitir que o usuário registre uma atividade realizada em determinada data.

O registro deve conter, no mínimo:

* Atividade
* Data
* Status

Status previstos:

* Concluída
* Parcial
* Não realizada

Campos adicionais previstos:

* Observação
* Data de criação
* Data de atualização

## RF035 - Registro de ida à academia

O sistema deve permitir que o usuário registre os dias em que foi à academia.

Esse registro pode ser tratado como uma atividade de rotina do tipo academia.

## RF036 - Listagem de registros de rotina

O sistema deve permitir que o usuário visualize os registros de rotina realizados.

A listagem deve exibir, no mínimo:

* Atividade
* Data
* Status
* Observação, quando houver

## RF037 - Consulta de registro por identificador

O sistema deve permitir que o usuário consulte os detalhes de um registro de rotina específico.

## RF038 - Atualização de registro de rotina

O sistema deve permitir que o usuário atualize um registro de rotina cadastrado anteriormente.

Campos editáveis previstos:

* Atividade
* Data
* Status
* Observação

## RF039 - Exclusão de registro de rotina

O sistema deve permitir que o usuário exclua um registro de rotina cadastrado anteriormente.

## RF040 - Filtro de registros de rotina por período

O sistema deve permitir que o usuário filtre registros de rotina por período.

Filtros previstos:

* Mês
* Ano
* Semana
* Data inicial
* Data final

## RF041 - Filtro de registros por atividade

O sistema deve permitir que o usuário filtre registros de rotina por atividade.

## RF042 - Resumo semanal de rotina

O sistema deve permitir que o usuário visualize um resumo semanal de suas atividades.

O resumo deve conter:

* Quantidade de atividades concluídas
* Quantidade de atividades parciais
* Quantidade de atividades não realizadas
* Frequência por atividade
* Comparação com a frequência semanal desejada

## RF043 - Resumo mensal de rotina

O sistema deve permitir que o usuário visualize um resumo mensal de suas atividades.

O resumo deve conter:

* Total de atividades concluídas no mês
* Dias com registro de academia
* Atividades mais realizadas
* Atividades menos realizadas
* Percentual de cumprimento das metas de frequência

## RF044 - Visualização de calendário de rotina

O sistema deve permitir que o usuário visualize registros de rotina em formato de calendário.

Cada dia poderá exibir as atividades registradas e seus respectivos status.

---

# 7. Dashboard

## RF045 - Exibição de dashboard inicial

O sistema deve exibir um dashboard com informações resumidas do usuário autenticado.

## RF046 - Exibição de resumo financeiro no dashboard

O dashboard deve apresentar um resumo financeiro do mês atual.

Informações previstas:

* Total de receitas
* Total de despesas
* Saldo mensal
* Maiores categorias de despesa
* Últimas transações cadastradas

## RF047 - Exibição de resumo de rotina no dashboard

O dashboard deve apresentar um resumo da rotina do usuário.

Informações previstas:

* Dias de academia no mês
* Hábitos concluídos na semana
* Atividades pendentes ou não realizadas
* Frequência das principais atividades

## RF048 - Exibição de últimas transações

O sistema deve exibir no dashboard as últimas transações financeiras cadastradas pelo usuário.

## RF049 - Exibição de últimos registros de rotina

O sistema deve exibir no dashboard os últimos registros de rotina cadastrados pelo usuário.

## RF050 - Filtro do dashboard por período

O sistema deve permitir que o usuário altere o período de visualização do dashboard.

Filtros previstos:

* Mês atual
* Mês anterior
* Ano atual
* Período personalizado

---

# 8. Metas

## RF051 - Cadastro de meta

O sistema deve permitir que o usuário cadastre metas pessoais.

A meta deve conter, no mínimo:

* Título
* Tipo
* Valor alvo ou quantidade alvo
* Data inicial
* Data final
* Status

Tipos de meta previstos:

* Financeira
* Rotina
* Academia
* Estudos
* Outro

## RF052 - Listagem de metas

O sistema deve permitir que o usuário visualize suas metas cadastradas.

## RF053 - Consulta de meta por identificador

O sistema deve permitir que o usuário consulte os detalhes de uma meta específica.

## RF054 - Atualização de meta

O sistema deve permitir que o usuário atualize uma meta cadastrada anteriormente.

Campos editáveis previstos:

* Título
* Descrição
* Tipo
* Valor alvo
* Valor atual
* Data inicial
* Data final
* Status

## RF055 - Exclusão de meta

O sistema deve permitir que o usuário exclua uma meta cadastrada anteriormente.

## RF056 - Acompanhamento de progresso da meta

O sistema deve permitir que o usuário acompanhe o progresso de uma meta.

Exemplos:

* Guardar R$ 1.000,00 no mês.
* Ir à academia 16 vezes no mês.
* Estudar japonês 20 dias no mês.
* Ler 10 páginas por dia.

---

# 9. Administração dos próprios dados

## RF057 - Isolamento de dados por usuário

O sistema deve garantir que cada usuário visualize e manipule apenas seus próprios dados.

Esse requisito se aplica a:

* Transações financeiras
* Categorias financeiras
* Atividades de rotina
* Registros de rotina
* Metas

## RF058 - Consulta de histórico

O sistema deve permitir que o usuário consulte seu histórico de informações cadastradas.

Históricos previstos:

* Histórico financeiro
* Histórico de rotina
* Histórico de metas

## RF059 - Exportação futura de dados

O sistema poderá permitir que o usuário exporte seus próprios dados em formato externo.

Formatos possíveis:

* CSV
* Excel
* PDF

Este requisito não faz parte obrigatória do MVP.

---

# 10. Requisitos funcionais do MVP

Os requisitos abaixo fazem parte da primeira versão funcional do sistema.

## Autenticação

* RF001 - Cadastro de usuário
* RF002 - Login de usuário
* RF003 - Logout de usuário
* RF004 - Proteção de rotas autenticadas
* RF005 - Consulta do usuário autenticado

## Controle financeiro

* RF009 - Cadastro de transação financeira
* RF010 - Listagem de transações financeiras
* RF011 - Consulta de transação por identificador
* RF012 - Atualização de transação financeira
* RF013 - Exclusão de transação financeira
* RF014 - Filtro de transações por período
* RF015 - Filtro de transações por tipo
* RF016 - Filtro de transações por categoria
* RF019 - Resumo financeiro mensal

## Categorias financeiras

* RF022 - Cadastro de categoria financeira
* RF023 - Listagem de categorias financeiras
* RF024 - Consulta de categoria por identificador
* RF025 - Atualização de categoria financeira
* RF026 - Exclusão de categoria financeira
* RF027 - Criação de categorias financeiras padrão

## Controle de rotina

* RF028 - Cadastro de atividade de rotina
* RF029 - Listagem de atividades de rotina
* RF030 - Consulta de atividade por identificador
* RF031 - Atualização de atividade de rotina
* RF032 - Exclusão de atividade de rotina

## Registros de rotina

* RF034 - Registro de atividade realizada
* RF035 - Registro de ida à academia
* RF036 - Listagem de registros de rotina
* RF038 - Atualização de registro de rotina
* RF039 - Exclusão de registro de rotina
* RF040 - Filtro de registros de rotina por período
* RF041 - Filtro de registros por atividade
* RF042 - Resumo semanal de rotina
* RF043 - Resumo mensal de rotina

## Dashboard

* RF045 - Exibição de dashboard inicial
* RF046 - Exibição de resumo financeiro no dashboard
* RF047 - Exibição de resumo de rotina no dashboard
* RF048 - Exibição de últimas transações
* RF049 - Exibição de últimos registros de rotina

---

# 11. Requisitos funcionais fora do MVP

Os requisitos abaixo poderão ser implementados em versões futuras.

* RF006 - Atualização dos dados do usuário
* RF007 - Alteração de senha
* RF008 - Exclusão da conta
* RF017 - Filtro de transações por forma de pagamento
* RF018 - Identificação de transações fixas
* RF020 - Consulta de despesas por categoria
* RF021 - Consulta de receitas por categoria
* RF033 - Ativação e desativação de atividade de rotina
* RF037 - Consulta de registro por identificador
* RF044 - Visualização de calendário de rotina
* RF050 - Filtro do dashboard por período
* RF051 - Cadastro de meta
* RF052 - Listagem de metas
* RF053 - Consulta de meta por identificador
* RF054 - Atualização de meta
* RF055 - Exclusão de meta
* RF056 - Acompanhamento de progresso da meta
* RF058 - Consulta de histórico
* RF059 - Exportação futura de dados

---

# 12. Observações

Os requisitos descritos neste documento podem ser ajustados ao longo do desenvolvimento do projeto.

Durante a implementação, cada requisito poderá ser relacionado a:

* Uma ou mais entidades do banco de dados.
* Um ou mais endpoints da API.
* Uma ou mais telas do front-end.
* Um ou mais testes automatizados.
