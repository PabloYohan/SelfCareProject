# Modelagem do Banco de Dados

Este documento descreve a modelagem inicial do banco de dados do projeto **SelfCare Finance**.

O banco de dados será utilizado para armazenar informações relacionadas a usuários, transações financeiras, categorias, atividades de rotina, registros de rotina e metas.

## Banco de dados previsto

O projeto utilizará um banco de dados relacional.

Banco previsto:

```txt
PostgreSQL
```

O acesso ao banco será realizado pelo back-end em **ASP.NET Core**, utilizando **Entity Framework Core** como ORM.

---

# 1. Visão geral da modelagem

A modelagem inicial contempla as seguintes entidades principais:

* User
* FinancialCategory
* FinancialTransaction
* RoutineActivity
* RoutineRecord
* Goal

Essas entidades representam os principais módulos do sistema:

* Autenticação e usuário
* Controle financeiro
* Controle de rotina
* Metas

---

# 2. Convenções gerais

## 2.1 Identificadores

As entidades principais devem utilizar identificadores únicos.

Tipo sugerido:

```txt
UUID / GUID
```

Exemplo em C#:

```csharp
public Guid Id { get; set; }
```

Exemplo em PostgreSQL:

```sql
id UUID PRIMARY KEY
```

## 2.2 Datas de controle

As entidades principais devem possuir campos de controle de criação e atualização.

Campos padrão:

```txt
created_at
updated_at
```

Significado:

* `created_at`: data e hora de criação do registro.
* `updated_at`: data e hora da última atualização do registro.

## 2.3 Exclusão de registros

Inicialmente, o sistema pode utilizar exclusão física dos registros.

Em versões futuras, poderá ser adotada exclusão lógica com o campo:

```txt
deleted_at
```

ou:

```txt
is_deleted
```

## 2.4 Relacionamento com usuário

As entidades privadas do sistema devem estar vinculadas a um usuário.

Entidades vinculadas ao usuário:

* FinancialCategory
* FinancialTransaction
* RoutineActivity
* RoutineRecord
* Goal

Esse vínculo garante que cada usuário acesse apenas seus próprios dados.

---

# 3. Diagrama lógico simplificado

```txt
users
  ├── financial_categories
  │       └── financial_transactions
  │
  ├── routine_activities
  │       └── routine_records
  │
  └── goals
```

Representação com relacionamentos:

```txt
users 1 ─── N financial_categories
users 1 ─── N financial_transactions
financial_categories 1 ─── N financial_transactions

users 1 ─── N routine_activities
users 1 ─── N routine_records
routine_activities 1 ─── N routine_records

users 1 ─── N goals
```

---

# 4. Entidade: users

A tabela `users` armazena os dados dos usuários cadastrados no sistema.

## 4.1 Campos

```txt
users
- id
- name
- email
- password_hash
- created_at
- updated_at
```

## 4.2 Descrição dos campos

| Campo         | Tipo sugerido | Obrigatório | Descrição                              |
| ------------- | ------------: | ----------: | -------------------------------------- |
| id            |          UUID |         Sim | Identificador único do usuário.        |
| name          |  VARCHAR(150) |         Sim | Nome do usuário.                       |
| email         |  VARCHAR(255) |         Sim | E-mail utilizado para login.           |
| password_hash |          TEXT |         Sim | Hash da senha do usuário.              |
| created_at    |     TIMESTAMP |         Sim | Data de criação do usuário.            |
| updated_at    |     TIMESTAMP |         Sim | Data da última atualização do usuário. |

## 4.3 Restrições

* O campo `email` deve ser único.
* O campo `email` deve ser obrigatório.
* O campo `password_hash` deve ser obrigatório.
* A senha não deve ser armazenada em texto puro.

## 4.4 Relacionamentos

Um usuário pode possuir:

* Muitas categorias financeiras.
* Muitas transações financeiras.
* Muitas atividades de rotina.
* Muitos registros de rotina.
* Muitas metas.

---

# 5. Entidade: financial_categories

A tabela `financial_categories` armazena as categorias utilizadas nas transações financeiras.

Exemplos:

* Alimentação
* Transporte
* Saúde
* Academia
* Educação
* Lazer
* Salário
* Freelance

## 5.1 Campos

```txt
financial_categories
- id
- user_id
- name
- type
- color
- created_at
- updated_at
```

## 5.2 Descrição dos campos

| Campo      | Tipo sugerido | Obrigatório | Descrição                                 |
| ---------- | ------------: | ----------: | ----------------------------------------- |
| id         |          UUID |         Sim | Identificador único da categoria.         |
| user_id    |          UUID |         Sim | Usuário proprietário da categoria.        |
| name       |  VARCHAR(100) |         Sim | Nome da categoria.                        |
| type       |   VARCHAR(20) |         Sim | Tipo da categoria.                        |
| color      |   VARCHAR(20) |         Não | Cor utilizada para exibição no front-end. |
| created_at |     TIMESTAMP |         Sim | Data de criação da categoria.             |
| updated_at |     TIMESTAMP |         Sim | Data da última atualização da categoria.  |

## 5.3 Valores possíveis para `type`

```txt
Income
Expense
Both
```

Descrição:

* `Income`: categoria utilizada para receitas.
* `Expense`: categoria utilizada para despesas.
* `Both`: categoria que pode ser utilizada em receitas e despesas.

## 5.4 Restrições

* O campo `name` deve ser obrigatório.
* O campo `type` deve aceitar apenas valores válidos.
* Cada categoria deve pertencer a um usuário.
* Um usuário não deve possuir categorias duplicadas com o mesmo nome e tipo.

## 5.5 Relacionamentos

* Uma categoria pertence a um usuário.
* Uma categoria pode estar vinculada a várias transações financeiras.

---

# 6. Entidade: financial_transactions

A tabela `financial_transactions` armazena as movimentações financeiras do usuário.

Exemplos:

* Salário
* Compra no mercado
* Mensalidade da academia
* Conta de internet
* Gasto com transporte
* Compra de livro

## 6.1 Campos

```txt
financial_transactions
- id
- user_id
- category_id
- description
- amount
- type
- transaction_date
- payment_method
- is_fixed
- notes
- created_at
- updated_at
```

## 6.2 Descrição dos campos

| Campo            | Tipo sugerido | Obrigatório | Descrição                                |
| ---------------- | ------------: | ----------: | ---------------------------------------- |
| id               |          UUID |         Sim | Identificador único da transação.        |
| user_id          |          UUID |         Sim | Usuário proprietário da transação.       |
| category_id      |          UUID |         Sim | Categoria da transação.                  |
| description      |  VARCHAR(200) |         Sim | Descrição da transação.                  |
| amount           | DECIMAL(12,2) |         Sim | Valor da transação.                      |
| type             |   VARCHAR(20) |         Sim | Tipo da transação.                       |
| transaction_date |          DATE |         Sim | Data em que a transação ocorreu.         |
| payment_method   |   VARCHAR(30) |         Não | Forma de pagamento utilizada.            |
| is_fixed         |       BOOLEAN |         Sim | Indica se a transação é fixa.            |
| notes            |          TEXT |         Não | Observações adicionais.                  |
| created_at       |     TIMESTAMP |         Sim | Data de criação da transação.            |
| updated_at       |     TIMESTAMP |         Sim | Data da última atualização da transação. |

## 6.3 Valores possíveis para `type`

```txt
Income
Expense
```

Descrição:

* `Income`: receita.
* `Expense`: despesa.

## 6.4 Valores possíveis para `payment_method`

```txt
Cash
Pix
DebitCard
CreditCard
BankTransfer
BankSlip
Other
```

Descrição:

* `Cash`: dinheiro.
* `Pix`: Pix.
* `DebitCard`: cartão de débito.
* `CreditCard`: cartão de crédito.
* `BankTransfer`: transferência bancária.
* `BankSlip`: boleto.
* `Other`: outro método.

## 6.5 Restrições

* O campo `description` deve ser obrigatório.
* O campo `amount` deve ser maior que zero.
* O campo `type` deve aceitar apenas valores válidos.
* O campo `transaction_date` deve ser obrigatório.
* A transação deve pertencer a um usuário.
* A categoria informada deve pertencer ao mesmo usuário da transação.
* O campo `is_fixed` deve possuir valor padrão `false`.

## 6.6 Relacionamentos

* Uma transação pertence a um usuário.
* Uma transação pertence a uma categoria financeira.

---

# 7. Entidade: routine_activities

A tabela `routine_activities` armazena as atividades que o usuário deseja acompanhar.

Exemplos:

* Academia
* Cardio
* Inglês
* Japonês
* Programação
* Leitura
* Sono adequado

## 7.1 Campos

```txt
routine_activities
- id
- user_id
- name
- description
- activity_type
- target_frequency_per_week
- is_active
- created_at
- updated_at
```

## 7.2 Descrição dos campos

| Campo                     | Tipo sugerido | Obrigatório | Descrição                                |
| ------------------------- | ------------: | ----------: | ---------------------------------------- |
| id                        |          UUID |         Sim | Identificador único da atividade.        |
| user_id                   |          UUID |         Sim | Usuário proprietário da atividade.       |
| name                      |  VARCHAR(100) |         Sim | Nome da atividade.                       |
| description               |          TEXT |         Não | Descrição da atividade.                  |
| activity_type             |   VARCHAR(30) |         Sim | Tipo da atividade.                       |
| target_frequency_per_week |       INTEGER |         Não | Frequência semanal desejada.             |
| is_active                 |       BOOLEAN |         Sim | Indica se a atividade está ativa.        |
| created_at                |     TIMESTAMP |         Sim | Data de criação da atividade.            |
| updated_at                |     TIMESTAMP |         Sim | Data da última atualização da atividade. |

## 7.3 Valores possíveis para `activity_type`

```txt
Gym
Study
Health
Reading
Work
Personal
Other
```

Descrição:

* `Gym`: academia.
* `Study`: estudos.
* `Health`: saúde.
* `Reading`: leitura.
* `Work`: trabalho ou desenvolvimento profissional.
* `Personal`: atividade pessoal.
* `Other`: outro tipo.

## 7.4 Restrições

* O campo `name` deve ser obrigatório.
* O campo `activity_type` deve aceitar apenas valores válidos.
* A atividade deve pertencer a um usuário.
* O campo `target_frequency_per_week`, quando informado, deve estar entre 1 e 7.
* O campo `is_active` deve possuir valor padrão `true`.

## 7.5 Relacionamentos

* Uma atividade pertence a um usuário.
* Uma atividade pode possuir vários registros de rotina.

---

# 8. Entidade: routine_records

A tabela `routine_records` armazena os registros de atividades realizadas pelo usuário em determinadas datas.

Exemplos:

* Academia realizada em 2026-07-07.
* Estudo de japonês realizado em 2026-07-07.
* Leitura parcial em 2026-07-08.

## 8.1 Campos

```txt
routine_records
- id
- user_id
- activity_id
- record_date
- status
- notes
- created_at
- updated_at
```

## 8.2 Descrição dos campos

| Campo       | Tipo sugerido | Obrigatório | Descrição                                |
| ----------- | ------------: | ----------: | ---------------------------------------- |
| id          |          UUID |         Sim | Identificador único do registro.         |
| user_id     |          UUID |         Sim | Usuário proprietário do registro.        |
| activity_id |          UUID |         Sim | Atividade vinculada ao registro.         |
| record_date |          DATE |         Sim | Data em que a atividade foi registrada.  |
| status      |   VARCHAR(20) |         Sim | Status da atividade no dia.              |
| notes       |          TEXT |         Não | Observações adicionais sobre o registro. |
| created_at  |     TIMESTAMP |         Sim | Data de criação do registro.             |
| updated_at  |     TIMESTAMP |         Sim | Data da última atualização do registro.  |

## 8.3 Valores possíveis para `status`

```txt
Completed
Partial
Skipped
```

Descrição:

* `Completed`: atividade concluída.
* `Partial`: atividade realizada parcialmente.
* `Skipped`: atividade não realizada.

## 8.4 Restrições

* O campo `activity_id` deve ser obrigatório.
* O campo `record_date` deve ser obrigatório.
* O campo `status` deve aceitar apenas valores válidos.
* O registro deve pertencer a um usuário.
* A atividade informada deve pertencer ao mesmo usuário do registro.
* Para uma mesma atividade, deve existir apenas um registro por data para o mesmo usuário.

## 8.5 Relacionamentos

* Um registro pertence a um usuário.
* Um registro pertence a uma atividade de rotina.

---

# 9. Entidade: goals

A tabela `goals` armazena metas financeiras ou pessoais do usuário.

Exemplos:

* Guardar R$ 1.000,00 no mês.
* Ir à academia 16 vezes no mês.
* Estudar japonês 20 dias no mês.
* Ler 10 páginas por dia.

## 9.1 Campos

```txt
goals
- id
- user_id
- title
- description
- goal_type
- target_value
- current_value
- start_date
- end_date
- status
- created_at
- updated_at
```

## 9.2 Descrição dos campos

| Campo         | Tipo sugerido | Obrigatório | Descrição                           |
| ------------- | ------------: | ----------: | ----------------------------------- |
| id            |          UUID |         Sim | Identificador único da meta.        |
| user_id       |          UUID |         Sim | Usuário proprietário da meta.       |
| title         |  VARCHAR(150) |         Sim | Título da meta.                     |
| description   |          TEXT |         Não | Descrição da meta.                  |
| goal_type     |   VARCHAR(30) |         Sim | Tipo da meta.                       |
| target_value  | DECIMAL(12,2) |         Não | Valor ou quantidade alvo da meta.   |
| current_value | DECIMAL(12,2) |         Não | Valor ou quantidade atual da meta.  |
| start_date    |          DATE |         Não | Data inicial da meta.               |
| end_date      |          DATE |         Não | Data final da meta.                 |
| status        |   VARCHAR(20) |         Sim | Status da meta.                     |
| created_at    |     TIMESTAMP |         Sim | Data de criação da meta.            |
| updated_at    |     TIMESTAMP |         Sim | Data da última atualização da meta. |

## 9.3 Valores possíveis para `goal_type`

```txt
Financial
Routine
Gym
Study
Other
```

Descrição:

* `Financial`: meta financeira.
* `Routine`: meta relacionada a hábitos ou rotina.
* `Gym`: meta relacionada à academia.
* `Study`: meta relacionada a estudos.
* `Other`: outro tipo de meta.

## 9.4 Valores possíveis para `status`

```txt
Active
Completed
Canceled
Paused
```

Descrição:

* `Active`: meta ativa.
* `Completed`: meta concluída.
* `Canceled`: meta cancelada.
* `Paused`: meta pausada.

## 9.5 Restrições

* O campo `title` deve ser obrigatório.
* O campo `goal_type` deve aceitar apenas valores válidos.
* O campo `status` deve aceitar apenas valores válidos.
* A meta deve pertencer a um usuário.
* O campo `target_value`, quando informado, deve ser maior que zero.
* O campo `current_value`, quando informado, não deve ser negativo.
* A data final, quando informada, deve ser maior ou igual à data inicial.

## 9.6 Relacionamentos

* Uma meta pertence a um usuário.

---

# 10. Relacionamentos detalhados

## 10.1 users e financial_categories

Um usuário pode possuir várias categorias financeiras.

```txt
users.id 1 ─── N financial_categories.user_id
```

## 10.2 users e financial_transactions

Um usuário pode possuir várias transações financeiras.

```txt
users.id 1 ─── N financial_transactions.user_id
```

## 10.3 financial_categories e financial_transactions

Uma categoria financeira pode estar associada a várias transações.

```txt
financial_categories.id 1 ─── N financial_transactions.category_id
```

## 10.4 users e routine_activities

Um usuário pode possuir várias atividades de rotina.

```txt
users.id 1 ─── N routine_activities.user_id
```

## 10.5 users e routine_records

Um usuário pode possuir vários registros de rotina.

```txt
users.id 1 ─── N routine_records.user_id
```

## 10.6 routine_activities e routine_records

Uma atividade de rotina pode possuir vários registros.

```txt
routine_activities.id 1 ─── N routine_records.activity_id
```

## 10.7 users e goals

Um usuário pode possuir várias metas.

```txt
users.id 1 ─── N goals.user_id
```

---

# 11. Índices recomendados

Para melhorar consultas comuns, alguns índices são recomendados.

## 11.1 users

```txt
email
```

Motivo:

* Login por e-mail.
* Garantia de unicidade do e-mail.

## 11.2 financial_categories

```txt
user_id
user_id, name, type
```

Motivo:

* Listagem de categorias por usuário.
* Evitar duplicidade de categorias para o mesmo usuário.

## 11.3 financial_transactions

```txt
user_id
user_id, transaction_date
user_id, type
user_id, category_id
```

Motivo:

* Listagem de transações por usuário.
* Filtros por período.
* Filtros por tipo.
* Filtros por categoria.
* Resumo financeiro mensal.

## 11.4 routine_activities

```txt
user_id
user_id, name
user_id, activity_type
```

Motivo:

* Listagem de atividades por usuário.
* Filtro por tipo de atividade.
* Evitar duplicidade de atividades, se desejado.

## 11.5 routine_records

```txt
user_id
activity_id
user_id, record_date
user_id, activity_id, record_date
```

Motivo:

* Listagem de registros por usuário.
* Filtros por período.
* Filtros por atividade.
* Garantia de um registro por atividade em uma data.

## 11.6 goals

```txt
user_id
user_id, status
user_id, goal_type
```

Motivo:

* Listagem de metas por usuário.
* Filtros por status.
* Filtros por tipo de meta.

---

# 12. Regras de integridade

## 12.1 Integridade por usuário

Todas as entidades privadas devem ser associadas a um usuário.

A aplicação deve garantir que:

* Uma transação só use categorias do mesmo usuário.
* Um registro de rotina só use atividades do mesmo usuário.
* Um usuário não acesse dados de outro usuário.

## 12.2 Exclusão de categoria financeira

Caso uma categoria possua transações vinculadas, existem algumas opções:

1. Impedir a exclusão da categoria.
2. Permitir exclusão apenas se não houver transações.
3. Marcar a categoria como inativa.
4. Mover transações para uma categoria padrão, como `Outros`.

Para o MVP, a opção recomendada é:

```txt
Impedir a exclusão de categorias que possuam transações vinculadas.
```

## 12.3 Exclusão de atividade de rotina

Caso uma atividade possua registros vinculados, existem algumas opções:

1. Impedir a exclusão da atividade.
2. Permitir exclusão em cascata.
3. Marcar a atividade como inativa.
4. Remover a atividade e manter registros históricos sem vínculo.

Para o MVP, a opção recomendada é:

```txt
Impedir a exclusão de atividades que possuam registros vinculados.
```

ou:

```txt
Marcar atividades como inativas.
```

A segunda opção é mais interessante para preservar histórico.

## 12.4 Exclusão de usuário

A exclusão de usuário deve considerar todos os dados vinculados.

Possíveis estratégias:

1. Excluir todos os dados em cascata.
2. Anonimizar o usuário.
3. Desativar a conta.
4. Marcar registros como excluídos logicamente.

Para o MVP, a exclusão de conta pode ficar fora do escopo inicial.

---

# 13. Modelo SQL inicial sugerido

Este modelo representa uma versão inicial das tabelas. Ele pode ser ajustado durante a implementação com Entity Framework Core Migrations.

```sql
CREATE TABLE users (
    id UUID PRIMARY KEY,
    name VARCHAR(150) NOT NULL,
    email VARCHAR(255) NOT NULL UNIQUE,
    password_hash TEXT NOT NULL,
    created_at TIMESTAMP NOT NULL,
    updated_at TIMESTAMP NOT NULL
);

CREATE TABLE financial_categories (
    id UUID PRIMARY KEY,
    user_id UUID NOT NULL,
    name VARCHAR(100) NOT NULL,
    type VARCHAR(20) NOT NULL,
    color VARCHAR(20),
    created_at TIMESTAMP NOT NULL,
    updated_at TIMESTAMP NOT NULL,

    CONSTRAINT fk_financial_categories_user
        FOREIGN KEY (user_id)
        REFERENCES users(id)
);

CREATE TABLE financial_transactions (
    id UUID PRIMARY KEY,
    user_id UUID NOT NULL,
    category_id UUID NOT NULL,
    description VARCHAR(200) NOT NULL,
    amount DECIMAL(12,2) NOT NULL,
    type VARCHAR(20) NOT NULL,
    transaction_date DATE NOT NULL,
    payment_method VARCHAR(30),
    is_fixed BOOLEAN NOT NULL DEFAULT FALSE,
    notes TEXT,
    created_at TIMESTAMP NOT NULL,
    updated_at TIMESTAMP NOT NULL,

    CONSTRAINT fk_financial_transactions_user
        FOREIGN KEY (user_id)
        REFERENCES users(id),

    CONSTRAINT fk_financial_transactions_category
        FOREIGN KEY (category_id)
        REFERENCES financial_categories(id)
);

CREATE TABLE routine_activities (
    id UUID PRIMARY KEY,
    user_id UUID NOT NULL,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    activity_type VARCHAR(30) NOT NULL,
    target_frequency_per_week INTEGER,
    is_active BOOLEAN NOT NULL DEFAULT TRUE,
    created_at TIMESTAMP NOT NULL,
    updated_at TIMESTAMP NOT NULL,

    CONSTRAINT fk_routine_activities_user
        FOREIGN KEY (user_id)
        REFERENCES users(id)
);

CREATE TABLE routine_records (
    id UUID PRIMARY KEY,
    user_id UUID NOT NULL,
    activity_id UUID NOT NULL,
    record_date DATE NOT NULL,
    status VARCHAR(20) NOT NULL,
    notes TEXT,
    created_at TIMESTAMP NOT NULL,
    updated_at TIMESTAMP NOT NULL,

    CONSTRAINT fk_routine_records_user
        FOREIGN KEY (user_id)
        REFERENCES users(id),

    CONSTRAINT fk_routine_records_activity
        FOREIGN KEY (activity_id)
        REFERENCES routine_activities(id)
);

CREATE TABLE goals (
    id UUID PRIMARY KEY,
    user_id UUID NOT NULL,
    title VARCHAR(150) NOT NULL,
    description TEXT,
    goal_type VARCHAR(30) NOT NULL,
    target_value DECIMAL(12,2),
    current_value DECIMAL(12,2),
    start_date DATE,
    end_date DATE,
    status VARCHAR(20) NOT NULL,
    created_at TIMESTAMP NOT NULL,
    updated_at TIMESTAMP NOT NULL,

    CONSTRAINT fk_goals_user
        FOREIGN KEY (user_id)
        REFERENCES users(id)
);
```

---

# 14. Índices SQL sugeridos

```sql
CREATE INDEX idx_financial_categories_user_id
ON financial_categories(user_id);

CREATE UNIQUE INDEX idx_financial_categories_user_name_type
ON financial_categories(user_id, name, type);

CREATE INDEX idx_financial_transactions_user_id
ON financial_transactions(user_id);

CREATE INDEX idx_financial_transactions_user_transaction_date
ON financial_transactions(user_id, transaction_date);

CREATE INDEX idx_financial_transactions_user_type
ON financial_transactions(user_id, type);

CREATE INDEX idx_financial_transactions_user_category
ON financial_transactions(user_id, category_id);

CREATE INDEX idx_routine_activities_user_id
ON routine_activities(user_id);

CREATE INDEX idx_routine_activities_user_type
ON routine_activities(user_id, activity_type);

CREATE INDEX idx_routine_records_user_id
ON routine_records(user_id);

CREATE INDEX idx_routine_records_user_record_date
ON routine_records(user_id, record_date);

CREATE UNIQUE INDEX idx_routine_records_user_activity_date
ON routine_records(user_id, activity_id, record_date);

CREATE INDEX idx_goals_user_id
ON goals(user_id);

CREATE INDEX idx_goals_user_status
ON goals(user_id, status);

CREATE INDEX idx_goals_user_type
ON goals(user_id, goal_type);
```

---

# 15. Entidades prioritárias para o MVP

Para a primeira versão funcional do projeto, as entidades mais importantes são:

```txt
users
financial_categories
financial_transactions
routine_activities
routine_records
```

A entidade `goals` pode ser deixada para uma versão futura, caso o foco inicial seja apenas controle financeiro e rotina.

## 15.1 MVP mínimo recomendado

Para começar o desenvolvimento de forma mais simples, o MVP pode considerar apenas:

```txt
users
financial_categories
financial_transactions
routine_activities
routine_records
```

Com isso, já será possível implementar:

* Cadastro e login.
* Categorias financeiras.
* Receitas e despesas.
* Resumo mensal financeiro.
* Atividades de rotina.
* Registro de dias de academia.
* Resumo semanal ou mensal de rotina.

---

# 16. Observações para implementação com Entity Framework Core

## 16.1 Enums

Os campos abaixo podem ser representados como enums no C#:

```txt
FinancialTransactionType
FinancialCategoryType
PaymentMethod
RoutineActivityType
RoutineRecordStatus
GoalType
GoalStatus
```

Exemplo:

```csharp
public enum FinancialTransactionType
{
    Income = 1,
    Expense = 2
}
```

No banco, é possível armazenar enums como:

* Inteiro
* Texto

Para facilitar leitura direta no banco, pode ser interessante armazenar como texto. Para maior eficiência e controle, pode ser usado inteiro.

Para o MVP, a recomendação é:

```txt
Armazenar enums como texto ou inteiro, mantendo consistência em todo o projeto.
```

## 16.2 Configurações com Fluent API

As configurações de tabelas, campos e relacionamentos podem ser feitas utilizando Fluent API no Entity Framework Core.

Exemplo:

```csharp
builder.Entity<FinancialTransaction>(entity =>
{
    entity.ToTable("financial_transactions");

    entity.HasKey(x => x.Id);

    entity.Property(x => x.Description)
        .IsRequired()
        .HasMaxLength(200);

    entity.Property(x => x.Amount)
        .IsRequired()
        .HasPrecision(12, 2);

    entity.HasOne(x => x.User)
        .WithMany()
        .HasForeignKey(x => x.UserId);
});
```

## 16.3 Migrations

Toda alteração na estrutura do banco deve ser feita por migrations.

Exemplo de comando:

```bash
dotnet ef migrations add InitialCreate
dotnet ef database update
```

---

# 17. Possíveis evoluções futuras

A modelagem poderá ser expandida com novas entidades, como:

* CreditCard
* CreditCardInvoice
* Installment
* Budget
* BudgetCategory
* RecurringTransaction
* Workout
* WorkoutExercise
* BodyMeasurement
* Notification
* UserSettings

Essas entidades não fazem parte do MVP inicial, mas podem ser consideradas em versões futuras.

---

# 18. Status

Modelagem inicial criada para orientar a implementação do projeto.

Esta modelagem poderá ser ajustada conforme novas regras de negócio forem definidas.
