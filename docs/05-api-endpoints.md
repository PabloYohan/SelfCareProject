# API Endpoints

Este documento descreve os endpoints previstos para a API do projeto **SelfCare Finance**.

A API será desenvolvida em **ASP.NET Core** e será responsável por fornecer os recursos necessários para autenticação, controle financeiro, controle de rotina, dashboard e metas.

## Convenções gerais

## Base URL

Durante o desenvolvimento local, a API poderá utilizar uma URL semelhante a:

```txt
https://localhost:5001
```

ou:

```txt
http://localhost:5000
```

Em produção, a URL será definida conforme o ambiente de deploy.

## Prefixo da API

Os endpoints devem utilizar o prefixo:

```txt
/api
```

Exemplo:

```txt
/api/auth/login
```

Em uma versão futura, pode ser utilizado versionamento:

```txt
/api/v1/auth/login
```

## Formato de comunicação

A comunicação entre front-end e back-end será feita utilizando JSON.

## Autenticação

Endpoints privados devem exigir autenticação via token JWT.

Formato esperado no header:

```txt
Authorization: Bearer {token}
```

## Padrão de respostas

A API deve utilizar códigos HTTP adequados para cada situação.

Exemplos:

```txt
200 OK
201 Created
204 No Content
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
500 Internal Server Error
```

---

# 1. Autenticação

Endpoints responsáveis por cadastro, login e identificação do usuário autenticado.

## 1.1 Registrar usuário

```txt
POST /api/auth/register
```

Cria uma nova conta de usuário.

## Request

```json
{
  "name": "Pablo Yohan",
  "email": "pablo@email.com",
  "password": "senha123"
}
```

## Response `201 Created`

```json
{
  "id": "uuid",
  "name": "Pablo Yohan",
  "email": "pablo@email.com"
}
```

## Possíveis erros

```txt
400 Bad Request - Dados inválidos
400 Bad Request - E-mail já cadastrado
```

## MVP

Sim.

---

## 1.2 Login

```txt
POST /api/auth/login
```

Realiza login do usuário e retorna um token JWT.

## Request

```json
{
  "email": "pablo@email.com",
  "password": "senha123"
}
```

## Response `200 OK`

```json
{
  "accessToken": "jwt-token",
  "expiresAt": "2026-07-07T23:59:59Z",
  "user": {
    "id": "uuid",
    "name": "Pablo Yohan",
    "email": "pablo@email.com"
  }
}
```

## Possíveis erros

```txt
400 Bad Request - Dados inválidos
401 Unauthorized - E-mail ou senha inválidos
```

## MVP

Sim.

---

## 1.3 Obter usuário autenticado

```txt
GET /api/auth/me
```

Retorna os dados básicos do usuário autenticado.

## Autenticação

Requer token JWT.

## Response `200 OK`

```json
{
  "id": "uuid",
  "name": "Pablo Yohan",
  "email": "pablo@email.com"
}
```

## Possíveis erros

```txt
401 Unauthorized - Usuário não autenticado
```

## MVP

Sim.

---

# 2. Usuário

Endpoints responsáveis pelo gerenciamento dos dados do próprio usuário.

## 2.1 Atualizar dados do usuário

```txt
PUT /api/users/me
```

Atualiza os dados básicos do usuário autenticado.

## Autenticação

Requer token JWT.

## Request

```json
{
  "name": "Pablo Yohan",
  "email": "novo-email@email.com"
}
```

## Response `200 OK`

```json
{
  "id": "uuid",
  "name": "Pablo Yohan",
  "email": "novo-email@email.com"
}
```

## Possíveis erros

```txt
400 Bad Request - Dados inválidos
400 Bad Request - E-mail já utilizado
401 Unauthorized - Usuário não autenticado
```

## MVP

Não.

---

## 2.2 Alterar senha

```txt
PUT /api/users/me/password
```

Altera a senha do usuário autenticado.

## Autenticação

Requer token JWT.

## Request

```json
{
  "currentPassword": "senhaAtual123",
  "newPassword": "novaSenha123",
  "confirmNewPassword": "novaSenha123"
}
```

## Response `204 No Content`

Sem corpo de resposta.

## Possíveis erros

```txt
400 Bad Request - Dados inválidos
400 Bad Request - Senha atual incorreta
401 Unauthorized - Usuário não autenticado
```

## MVP

Não.

---

## 2.3 Excluir conta

```txt
DELETE /api/users/me
```

Exclui a conta do usuário autenticado.

## Autenticação

Requer token JWT.

## Response `204 No Content`

Sem corpo de resposta.

## Possíveis erros

```txt
401 Unauthorized - Usuário não autenticado
```

## MVP

Não.

---

# 3. Categorias financeiras

Endpoints responsáveis pelo gerenciamento das categorias financeiras do usuário.

## 3.1 Listar categorias financeiras

```txt
GET /api/financial-categories
```

Lista as categorias financeiras do usuário autenticado.

## Autenticação

Requer token JWT.

## Query parameters

```txt
type
```

Parâmetros opcionais:

| Parâmetro | Tipo   | Descrição                     |
| --------- | ------ | ----------------------------- |
| type      | string | Filtra por tipo da categoria. |

Valores possíveis para `type`:

```txt
Income
Expense
Both
```

## Response `200 OK`

```json
[
  {
    "id": "uuid",
    "name": "Alimentação",
    "type": "Expense",
    "color": "#FF9800"
  },
  {
    "id": "uuid",
    "name": "Salário",
    "type": "Income",
    "color": "#4CAF50"
  }
]
```

## MVP

Sim.

---

## 3.2 Consultar categoria financeira por ID

```txt
GET /api/financial-categories/{id}
```

Retorna os detalhes de uma categoria financeira específica.

## Autenticação

Requer token JWT.

## Response `200 OK`

```json
{
  "id": "uuid",
  "name": "Alimentação",
  "type": "Expense",
  "color": "#FF9800",
  "createdAt": "2026-07-07T10:00:00Z",
  "updatedAt": "2026-07-07T10:00:00Z"
}
```

## Possíveis erros

```txt
401 Unauthorized - Usuário não autenticado
404 Not Found - Categoria não encontrada
```

## MVP

Sim.

---

## 3.3 Criar categoria financeira

```txt
POST /api/financial-categories
```

Cria uma nova categoria financeira.

## Autenticação

Requer token JWT.

## Request

```json
{
  "name": "Academia",
  "type": "Expense",
  "color": "#2196F3"
}
```

## Response `201 Created`

```json
{
  "id": "uuid",
  "name": "Academia",
  "type": "Expense",
  "color": "#2196F3"
}
```

## Possíveis erros

```txt
400 Bad Request - Dados inválidos
400 Bad Request - Categoria já cadastrada
401 Unauthorized - Usuário não autenticado
```

## MVP

Sim.

---

## 3.4 Atualizar categoria financeira

```txt
PUT /api/financial-categories/{id}
```

Atualiza uma categoria financeira existente.

## Autenticação

Requer token JWT.

## Request

```json
{
  "name": "Saúde e Academia",
  "type": "Expense",
  "color": "#2196F3"
}
```

## Response `200 OK`

```json
{
  "id": "uuid",
  "name": "Saúde e Academia",
  "type": "Expense",
  "color": "#2196F3"
}
```

## Possíveis erros

```txt
400 Bad Request - Dados inválidos
401 Unauthorized - Usuário não autenticado
404 Not Found - Categoria não encontrada
```

## MVP

Sim.

---

## 3.5 Excluir categoria financeira

```txt
DELETE /api/financial-categories/{id}
```

Exclui uma categoria financeira existente.

## Autenticação

Requer token JWT.

## Response `204 No Content`

Sem corpo de resposta.

## Possíveis erros

```txt
400 Bad Request - Categoria possui transações vinculadas
401 Unauthorized - Usuário não autenticado
404 Not Found - Categoria não encontrada
```

## MVP

Sim.

---

# 4. Transações financeiras

Endpoints responsáveis pelo gerenciamento das receitas e despesas do usuário.

## 4.1 Listar transações financeiras

```txt
GET /api/financial-transactions
```

Lista as transações financeiras do usuário autenticado.

## Autenticação

Requer token JWT.

## Query parameters

| Parâmetro     | Tipo   | Descrição                       |
| ------------- | ------ | ------------------------------- |
| month         | int    | Filtra pelo mês da transação.   |
| year          | int    | Filtra pelo ano da transação.   |
| startDate     | date   | Filtra pela data inicial.       |
| endDate       | date   | Filtra pela data final.         |
| type          | string | Filtra por tipo da transação.   |
| categoryId    | uuid   | Filtra por categoria.           |
| paymentMethod | string | Filtra por forma de pagamento.  |
| page          | int    | Página da consulta.             |
| pageSize      | int    | Quantidade de itens por página. |

Exemplo:

```txt
GET /api/financial-transactions?month=7&year=2026&type=Expense
```

## Response `200 OK`

```json
{
  "items": [
    {
      "id": "uuid",
      "description": "Academia",
      "amount": 120.00,
      "type": "Expense",
      "category": {
        "id": "uuid",
        "name": "Saúde"
      },
      "transactionDate": "2026-07-07",
      "paymentMethod": "Pix",
      "isFixed": true
    }
  ],
  "page": 1,
  "pageSize": 10,
  "totalItems": 1,
  "totalPages": 1
}
```

## MVP

Sim.

---

## 4.2 Consultar transação por ID

```txt
GET /api/financial-transactions/{id}
```

Retorna os detalhes de uma transação financeira específica.

## Autenticação

Requer token JWT.

## Response `200 OK`

```json
{
  "id": "uuid",
  "description": "Academia",
  "amount": 120.00,
  "type": "Expense",
  "category": {
    "id": "uuid",
    "name": "Saúde"
  },
  "transactionDate": "2026-07-07",
  "paymentMethod": "Pix",
  "isFixed": true,
  "notes": "Mensalidade da academia",
  "createdAt": "2026-07-07T10:00:00Z",
  "updatedAt": "2026-07-07T10:00:00Z"
}
```

## Possíveis erros

```txt
401 Unauthorized - Usuário não autenticado
404 Not Found - Transação não encontrada
```

## MVP

Sim.

---

## 4.3 Criar transação financeira

```txt
POST /api/financial-transactions
```

Cria uma nova transação financeira.

## Autenticação

Requer token JWT.

## Request

```json
{
  "description": "Academia",
  "amount": 120.00,
  "type": "Expense",
  "categoryId": "uuid",
  "transactionDate": "2026-07-07",
  "paymentMethod": "Pix",
  "isFixed": true,
  "notes": "Mensalidade da academia"
}
```

## Response `201 Created`

```json
{
  "id": "uuid",
  "description": "Academia",
  "amount": 120.00,
  "type": "Expense",
  "categoryId": "uuid",
  "transactionDate": "2026-07-07",
  "paymentMethod": "Pix",
  "isFixed": true,
  "notes": "Mensalidade da academia"
}
```

## Possíveis erros

```txt
400 Bad Request - Dados inválidos
400 Bad Request - Categoria inválida
401 Unauthorized - Usuário não autenticado
```

## MVP

Sim.

---

## 4.4 Atualizar transação financeira

```txt
PUT /api/financial-transactions/{id}
```

Atualiza uma transação financeira existente.

## Autenticação

Requer token JWT.

## Request

```json
{
  "description": "Academia",
  "amount": 130.00,
  "type": "Expense",
  "categoryId": "uuid",
  "transactionDate": "2026-07-07",
  "paymentMethod": "Pix",
  "isFixed": true,
  "notes": "Mensalidade reajustada"
}
```

## Response `200 OK`

```json
{
  "id": "uuid",
  "description": "Academia",
  "amount": 130.00,
  "type": "Expense",
  "categoryId": "uuid",
  "transactionDate": "2026-07-07",
  "paymentMethod": "Pix",
  "isFixed": true,
  "notes": "Mensalidade reajustada"
}
```

## Possíveis erros

```txt
400 Bad Request - Dados inválidos
400 Bad Request - Categoria inválida
401 Unauthorized - Usuário não autenticado
404 Not Found - Transação não encontrada
```

## MVP

Sim.

---

## 4.5 Excluir transação financeira

```txt
DELETE /api/financial-transactions/{id}
```

Exclui uma transação financeira existente.

## Autenticação

Requer token JWT.

## Response `204 No Content`

Sem corpo de resposta.

## Possíveis erros

```txt
401 Unauthorized - Usuário não autenticado
404 Not Found - Transação não encontrada
```

## MVP

Sim.

---

## 4.6 Obter resumo financeiro mensal

```txt
GET /api/financial-transactions/monthly-summary
```

Retorna o resumo financeiro mensal do usuário autenticado.

## Autenticação

Requer token JWT.

## Query parameters

| Parâmetro | Tipo | Obrigatório | Descrição     |
| --------- | ---- | ----------: | ------------- |
| month     | int  |         Sim | Mês desejado. |
| year      | int  |         Sim | Ano desejado. |

Exemplo:

```txt
GET /api/financial-transactions/monthly-summary?month=7&year=2026
```

## Response `200 OK`

```json
{
  "month": 7,
  "year": 2026,
  "totalIncome": 4500.00,
  "totalExpense": 2300.00,
  "balance": 2200.00,
  "fixedExpenses": 900.00,
  "variableExpenses": 1400.00,
  "transactionCount": 28
}
```

## MVP

Sim.

---

## 4.7 Obter despesas por categoria

```txt
GET /api/financial-transactions/expenses-by-category
```

Retorna o total de despesas agrupadas por categoria.

## Autenticação

Requer token JWT.

## Query parameters

| Parâmetro | Tipo | Obrigatório | Descrição     |
| --------- | ---- | ----------: | ------------- |
| month     | int  |         Não | Mês desejado. |
| year      | int  |         Não | Ano desejado. |
| startDate | date |         Não | Data inicial. |
| endDate   | date |         Não | Data final.   |

## Response `200 OK`

```json
[
  {
    "categoryId": "uuid",
    "categoryName": "Alimentação",
    "total": 850.00
  },
  {
    "categoryId": "uuid",
    "categoryName": "Transporte",
    "total": 300.00
  }
]
```

## MVP

Não.

---

## 4.8 Obter receitas por categoria

```txt
GET /api/financial-transactions/income-by-category
```

Retorna o total de receitas agrupadas por categoria.

## Autenticação

Requer token JWT.

## Query parameters

| Parâmetro | Tipo | Obrigatório | Descrição     |
| --------- | ---- | ----------: | ------------- |
| month     | int  |         Não | Mês desejado. |
| year      | int  |         Não | Ano desejado. |
| startDate | date |         Não | Data inicial. |
| endDate   | date |         Não | Data final.   |

## Response `200 OK`

```json
[
  {
    "categoryId": "uuid",
    "categoryName": "Salário",
    "total": 4500.00
  },
  {
    "categoryId": "uuid",
    "categoryName": "Freelance",
    "total": 800.00
  }
]
```

## MVP

Não.

---

# 5. Atividades de rotina

Endpoints responsáveis pelo cadastro e gerenciamento das atividades de rotina.

## 5.1 Listar atividades de rotina

```txt
GET /api/routine-activities
```

Lista as atividades de rotina do usuário autenticado.

## Autenticação

Requer token JWT.

## Query parameters

| Parâmetro    | Tipo   | Descrição                                 |
| ------------ | ------ | ----------------------------------------- |
| activityType | string | Filtra por tipo da atividade.             |
| isActive     | bool   | Filtra por atividades ativas ou inativas. |

## Response `200 OK`

```json
[
  {
    "id": "uuid",
    "name": "Academia",
    "description": "Treino de musculação",
    "activityType": "Gym",
    "targetFrequencyPerWeek": 5,
    "isActive": true
  },
  {
    "id": "uuid",
    "name": "Japonês",
    "description": "Estudo de vocabulário e gramática",
    "activityType": "Study",
    "targetFrequencyPerWeek": 5,
    "isActive": true
  }
]
```

## MVP

Sim.

---

## 5.2 Consultar atividade de rotina por ID

```txt
GET /api/routine-activities/{id}
```

Retorna os detalhes de uma atividade de rotina específica.

## Autenticação

Requer token JWT.

## Response `200 OK`

```json
{
  "id": "uuid",
  "name": "Academia",
  "description": "Treino de musculação",
  "activityType": "Gym",
  "targetFrequencyPerWeek": 5,
  "isActive": true,
  "createdAt": "2026-07-07T10:00:00Z",
  "updatedAt": "2026-07-07T10:00:00Z"
}
```

## Possíveis erros

```txt
401 Unauthorized - Usuário não autenticado
404 Not Found - Atividade não encontrada
```

## MVP

Sim.

---

## 5.3 Criar atividade de rotina

```txt
POST /api/routine-activities
```

Cria uma nova atividade de rotina.

## Autenticação

Requer token JWT.

## Request

```json
{
  "name": "Academia",
  "description": "Treino de musculação",
  "activityType": "Gym",
  "targetFrequencyPerWeek": 5
}
```

## Response `201 Created`

```json
{
  "id": "uuid",
  "name": "Academia",
  "description": "Treino de musculação",
  "activityType": "Gym",
  "targetFrequencyPerWeek": 5,
  "isActive": true
}
```

## Possíveis erros

```txt
400 Bad Request - Dados inválidos
401 Unauthorized - Usuário não autenticado
```

## MVP

Sim.

---

## 5.4 Atualizar atividade de rotina

```txt
PUT /api/routine-activities/{id}
```

Atualiza uma atividade de rotina existente.

## Autenticação

Requer token JWT.

## Request

```json
{
  "name": "Academia",
  "description": "Treino de musculação e cardio",
  "activityType": "Gym",
  "targetFrequencyPerWeek": 4,
  "isActive": true
}
```

## Response `200 OK`

```json
{
  "id": "uuid",
  "name": "Academia",
  "description": "Treino de musculação e cardio",
  "activityType": "Gym",
  "targetFrequencyPerWeek": 4,
  "isActive": true
}
```

## Possíveis erros

```txt
400 Bad Request - Dados inválidos
401 Unauthorized - Usuário não autenticado
404 Not Found - Atividade não encontrada
```

## MVP

Sim.

---

## 5.5 Excluir atividade de rotina

```txt
DELETE /api/routine-activities/{id}
```

Exclui uma atividade de rotina.

## Autenticação

Requer token JWT.

## Response `204 No Content`

Sem corpo de resposta.

## Possíveis erros

```txt
400 Bad Request - Atividade possui registros vinculados
401 Unauthorized - Usuário não autenticado
404 Not Found - Atividade não encontrada
```

## MVP

Sim.

---

## 5.6 Ativar ou desativar atividade de rotina

```txt
PATCH /api/routine-activities/{id}/status
```

Altera o status de uma atividade para ativa ou inativa.

## Autenticação

Requer token JWT.

## Request

```json
{
  "isActive": false
}
```

## Response `200 OK`

```json
{
  "id": "uuid",
  "name": "Academia",
  "isActive": false
}
```

## Possíveis erros

```txt
400 Bad Request - Dados inválidos
401 Unauthorized - Usuário não autenticado
404 Not Found - Atividade não encontrada
```

## MVP

Não.

---

# 6. Registros de rotina

Endpoints responsáveis pelo registro das atividades realizadas pelo usuário.

## 6.1 Listar registros de rotina

```txt
GET /api/routine-records
```

Lista os registros de rotina do usuário autenticado.

## Autenticação

Requer token JWT.

## Query parameters

| Parâmetro  | Tipo   | Descrição                       |
| ---------- | ------ | ------------------------------- |
| month      | int    | Filtra pelo mês.                |
| year       | int    | Filtra pelo ano.                |
| week       | int    | Filtra pela semana.             |
| startDate  | date   | Filtra pela data inicial.       |
| endDate    | date   | Filtra pela data final.         |
| activityId | uuid   | Filtra por atividade.           |
| status     | string | Filtra por status.              |
| page       | int    | Página da consulta.             |
| pageSize   | int    | Quantidade de itens por página. |

Exemplo:

```txt
GET /api/routine-records?month=7&year=2026
```

## Response `200 OK`

```json
{
  "items": [
    {
      "id": "uuid",
      "activity": {
        "id": "uuid",
        "name": "Academia",
        "activityType": "Gym"
      },
      "recordDate": "2026-07-07",
      "status": "Completed",
      "notes": "Treino de pernas"
    }
  ],
  "page": 1,
  "pageSize": 10,
  "totalItems": 1,
  "totalPages": 1
}
```

## MVP

Sim.

---

## 6.2 Consultar registro de rotina por ID

```txt
GET /api/routine-records/{id}
```

Retorna os detalhes de um registro de rotina específico.

## Autenticação

Requer token JWT.

## Response `200 OK`

```json
{
  "id": "uuid",
  "activity": {
    "id": "uuid",
    "name": "Academia",
    "activityType": "Gym"
  },
  "recordDate": "2026-07-07",
  "status": "Completed",
  "notes": "Treino de pernas",
  "createdAt": "2026-07-07T10:00:00Z",
  "updatedAt": "2026-07-07T10:00:00Z"
}
```

## Possíveis erros

```txt
401 Unauthorized - Usuário não autenticado
404 Not Found - Registro não encontrado
```

## MVP

Não.

---

## 6.3 Criar registro de rotina

```txt
POST /api/routine-records
```

Cria um novo registro de rotina.

## Autenticação

Requer token JWT.

## Request

```json
{
  "activityId": "uuid",
  "recordDate": "2026-07-07",
  "status": "Completed",
  "notes": "Treino de pernas"
}
```

## Response `201 Created`

```json
{
  "id": "uuid",
  "activityId": "uuid",
  "recordDate": "2026-07-07",
  "status": "Completed",
  "notes": "Treino de pernas"
}
```

## Possíveis erros

```txt
400 Bad Request - Dados inválidos
400 Bad Request - Já existe registro para essa atividade nessa data
401 Unauthorized - Usuário não autenticado
404 Not Found - Atividade não encontrada
```

## MVP

Sim.

---

## 6.4 Atualizar registro de rotina

```txt
PUT /api/routine-records/{id}
```

Atualiza um registro de rotina existente.

## Autenticação

Requer token JWT.

## Request

```json
{
  "activityId": "uuid",
  "recordDate": "2026-07-07",
  "status": "Partial",
  "notes": "Treino reduzido"
}
```

## Response `200 OK`

```json
{
  "id": "uuid",
  "activityId": "uuid",
  "recordDate": "2026-07-07",
  "status": "Partial",
  "notes": "Treino reduzido"
}
```

## Possíveis erros

```txt
400 Bad Request - Dados inválidos
400 Bad Request - Já existe registro para essa atividade nessa data
401 Unauthorized - Usuário não autenticado
404 Not Found - Registro não encontrado
```

## MVP

Sim.

---

## 6.5 Excluir registro de rotina

```txt
DELETE /api/routine-records/{id}
```

Exclui um registro de rotina.

## Autenticação

Requer token JWT.

## Response `204 No Content`

Sem corpo de resposta.

## Possíveis erros

```txt
401 Unauthorized - Usuário não autenticado
404 Not Found - Registro não encontrado
```

## MVP

Sim.

---

## 6.6 Obter resumo semanal de rotina

```txt
GET /api/routine-records/weekly-summary
```

Retorna um resumo semanal das atividades de rotina.

## Autenticação

Requer token JWT.

## Query parameters

| Parâmetro | Tipo | Obrigatório | Descrição               |
| --------- | ---- | ----------: | ----------------------- |
| startDate | date |         Sim | Data inicial da semana. |
| endDate   | date |         Sim | Data final da semana.   |

Exemplo:

```txt
GET /api/routine-records/weekly-summary?startDate=2026-07-06&endDate=2026-07-12
```

## Response `200 OK`

```json
{
  "startDate": "2026-07-06",
  "endDate": "2026-07-12",
  "completedCount": 8,
  "partialCount": 2,
  "skippedCount": 1,
  "activities": [
    {
      "activityId": "uuid",
      "activityName": "Academia",
      "targetFrequencyPerWeek": 5,
      "completedCount": 4,
      "progressPercentage": 80
    }
  ]
}
```

## MVP

Sim.

---

## 6.7 Obter resumo mensal de rotina

```txt
GET /api/routine-records/monthly-summary
```

Retorna um resumo mensal das atividades de rotina.

## Autenticação

Requer token JWT.

## Query parameters

| Parâmetro | Tipo | Obrigatório | Descrição     |
| --------- | ---- | ----------: | ------------- |
| month     | int  |         Sim | Mês desejado. |
| year      | int  |         Sim | Ano desejado. |

Exemplo:

```txt
GET /api/routine-records/monthly-summary?month=7&year=2026
```

## Response `200 OK`

```json
{
  "month": 7,
  "year": 2026,
  "completedCount": 32,
  "partialCount": 5,
  "skippedCount": 4,
  "gymDaysCount": 16,
  "activities": [
    {
      "activityId": "uuid",
      "activityName": "Academia",
      "activityType": "Gym",
      "completedCount": 16
    },
    {
      "activityId": "uuid",
      "activityName": "Japonês",
      "activityType": "Study",
      "completedCount": 20
    }
  ]
}
```

## MVP

Sim.

---

# 7. Dashboard

Endpoints responsáveis por fornecer informações consolidadas para a tela inicial da aplicação.

## 7.1 Obter dashboard mensal

```txt
GET /api/dashboard/monthly-summary
```

Retorna o resumo geral do mês, incluindo dados financeiros e de rotina.

## Autenticação

Requer token JWT.

## Query parameters

| Parâmetro | Tipo | Obrigatório | Descrição     |
| --------- | ---- | ----------: | ------------- |
| month     | int  |         Sim | Mês desejado. |
| year      | int  |         Sim | Ano desejado. |

Exemplo:

```txt
GET /api/dashboard/monthly-summary?month=7&year=2026
```

## Response `200 OK`

```json
{
  "month": 7,
  "year": 2026,
  "finance": {
    "totalIncome": 4500.00,
    "totalExpense": 2300.00,
    "balance": 2200.00,
    "transactionCount": 28
  },
  "routine": {
    "completedCount": 32,
    "partialCount": 5,
    "skippedCount": 4,
    "gymDaysCount": 16
  },
  "latestTransactions": [
    {
      "id": "uuid",
      "description": "Mercado",
      "amount": 180.00,
      "type": "Expense",
      "transactionDate": "2026-07-07"
    }
  ],
  "latestRoutineRecords": [
    {
      "id": "uuid",
      "activityName": "Academia",
      "recordDate": "2026-07-07",
      "status": "Completed"
    }
  ]
}
```

## Possíveis erros

```txt
400 Bad Request - Parâmetros inválidos
401 Unauthorized - Usuário não autenticado
```

## MVP

Sim.

---

# 8. Metas

Endpoints responsáveis pelo gerenciamento de metas financeiras e pessoais.

A implementação desse módulo não é obrigatória para o MVP.

## 8.1 Listar metas

```txt
GET /api/goals
```

Lista as metas do usuário autenticado.

## Autenticação

Requer token JWT.

## Query parameters

| Parâmetro | Tipo   | Descrição                |
| --------- | ------ | ------------------------ |
| goalType  | string | Filtra por tipo de meta. |
| status    | string | Filtra por status.       |

## Response `200 OK`

```json
[
  {
    "id": "uuid",
    "title": "Ir à academia 16 vezes no mês",
    "goalType": "Gym",
    "targetValue": 16,
    "currentValue": 10,
    "status": "Active",
    "startDate": "2026-07-01",
    "endDate": "2026-07-31"
  }
]
```

## MVP

Não.

---

## 8.2 Consultar meta por ID

```txt
GET /api/goals/{id}
```

Retorna os detalhes de uma meta específica.

## Autenticação

Requer token JWT.

## Response `200 OK`

```json
{
  "id": "uuid",
  "title": "Ir à academia 16 vezes no mês",
  "description": "Meta mensal de frequência na academia",
  "goalType": "Gym",
  "targetValue": 16,
  "currentValue": 10,
  "status": "Active",
  "startDate": "2026-07-01",
  "endDate": "2026-07-31",
  "createdAt": "2026-07-07T10:00:00Z",
  "updatedAt": "2026-07-07T10:00:00Z"
}
```

## Possíveis erros

```txt
401 Unauthorized - Usuário não autenticado
404 Not Found - Meta não encontrada
```

## MVP

Não.

---

## 8.3 Criar meta

```txt
POST /api/goals
```

Cria uma nova meta.

## Autenticação

Requer token JWT.

## Request

```json
{
  "title": "Ir à academia 16 vezes no mês",
  "description": "Meta mensal de frequência na academia",
  "goalType": "Gym",
  "targetValue": 16,
  "currentValue": 0,
  "startDate": "2026-07-01",
  "endDate": "2026-07-31",
  "status": "Active"
}
```

## Response `201 Created`

```json
{
  "id": "uuid",
  "title": "Ir à academia 16 vezes no mês",
  "goalType": "Gym",
  "targetValue": 16,
  "currentValue": 0,
  "status": "Active"
}
```

## Possíveis erros

```txt
400 Bad Request - Dados inválidos
401 Unauthorized - Usuário não autenticado
```

## MVP

Não.

---

## 8.4 Atualizar meta

```txt
PUT /api/goals/{id}
```

Atualiza uma meta existente.

## Autenticação

Requer token JWT.

## Request

```json
{
  "title": "Ir à academia 18 vezes no mês",
  "description": "Meta mensal de frequência na academia",
  "goalType": "Gym",
  "targetValue": 18,
  "currentValue": 10,
  "startDate": "2026-07-01",
  "endDate": "2026-07-31",
  "status": "Active"
}
```

## Response `200 OK`

```json
{
  "id": "uuid",
  "title": "Ir à academia 18 vezes no mês",
  "goalType": "Gym",
  "targetValue": 18,
  "currentValue": 10,
  "status": "Active"
}
```

## Possíveis erros

```txt
400 Bad Request - Dados inválidos
401 Unauthorized - Usuário não autenticado
404 Not Found - Meta não encontrada
```

## MVP

Não.

---

## 8.5 Excluir meta

```txt
DELETE /api/goals/{id}
```

Exclui uma meta existente.

## Autenticação

Requer token JWT.

## Response `204 No Content`

Sem corpo de resposta.

## Possíveis erros

```txt
401 Unauthorized - Usuário não autenticado
404 Not Found - Meta não encontrada
```

## MVP

Não.

---

# 9. Padrão de erros

A API deve retornar erros em um formato padronizado.

## 9.1 Erro de validação

Exemplo de resposta para erro de validação:

```json
{
  "status": 400,
  "message": "Dados inválidos.",
  "errors": [
    {
      "field": "email",
      "message": "E-mail é obrigatório."
    },
    {
      "field": "password",
      "message": "Senha deve ter no mínimo 6 caracteres."
    }
  ]
}
```

## 9.2 Recurso não encontrado

```json
{
  "status": 404,
  "message": "Recurso não encontrado."
}
```

## 9.3 Usuário não autenticado

```json
{
  "status": 401,
  "message": "Usuário não autenticado."
}
```

## 9.4 Acesso negado

```json
{
  "status": 403,
  "message": "Acesso negado."
}
```

## 9.5 Erro interno

```json
{
  "status": 500,
  "message": "Erro interno no servidor."
}
```

---

# 10. Endpoints prioritários para o MVP

Os endpoints abaixo devem ser priorizados na primeira versão funcional do sistema.

## Autenticação

```txt
POST /api/auth/register
POST /api/auth/login
GET /api/auth/me
```

## Categorias financeiras

```txt
GET /api/financial-categories
GET /api/financial-categories/{id}
POST /api/financial-categories
PUT /api/financial-categories/{id}
DELETE /api/financial-categories/{id}
```

## Transações financeiras

```txt
GET /api/financial-transactions
GET /api/financial-transactions/{id}
POST /api/financial-transactions
PUT /api/financial-transactions/{id}
DELETE /api/financial-transactions/{id}
GET /api/financial-transactions/monthly-summary
```

## Atividades de rotina

```txt
GET /api/routine-activities
GET /api/routine-activities/{id}
POST /api/routine-activities
PUT /api/routine-activities/{id}
DELETE /api/routine-activities/{id}
```

## Registros de rotina

```txt
GET /api/routine-records
POST /api/routine-records
PUT /api/routine-records/{id}
DELETE /api/routine-records/{id}
GET /api/routine-records/weekly-summary
GET /api/routine-records/monthly-summary
```

## Dashboard

```txt
GET /api/dashboard/monthly-summary
```

---

# 11. Endpoints fora do MVP

Os endpoints abaixo podem ser implementados em versões futuras.

## Usuário

```txt
PUT /api/users/me
PUT /api/users/me/password
DELETE /api/users/me
```

## Relatórios financeiros

```txt
GET /api/financial-transactions/expenses-by-category
GET /api/financial-transactions/income-by-category
```

## Atividades de rotina

```txt
PATCH /api/routine-activities/{id}/status
```

## Registros de rotina

```txt
GET /api/routine-records/{id}
```

## Metas

```txt
GET /api/goals
GET /api/goals/{id}
POST /api/goals
PUT /api/goals/{id}
DELETE /api/goals/{id}
```

---

# 12. Observações

Os endpoints descritos neste documento representam uma proposta inicial para a API.

Durante o desenvolvimento, os nomes, parâmetros e formatos de resposta poderão ser ajustados conforme as necessidades do projeto, desde que a documentação seja atualizada junto com a implementação.
