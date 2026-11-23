# Seção 10 - Manipulação de tabelas

## Aula 40 - Tabelas - Criação e deleção

- É possível criar tabelas a partir de uma query usando `INTO` antes do `FROM`
  - alternativa: `CREATE TABLE nome AS SELECT ...`
- Para criar uma tabela do zero, usamos `CREATE TABLE`
- Para inserir valores na tabela usamos `INSERT INTO`
- Para deletar uma tabela usamos `DROP TABLE`
- O exemplo 1 usa a função `datediff`, criada na seção anterior; caso ela tenha sido apagada, é preciso criá-la novamente

```sql
-- (Exemplo 1) Criação de tabela a partir de uma query
-- Crie uma tabela chamada customers_age com o id e a idade dos clientes.
-- Chame-a de temp_tables.customers_age
SELECT
    customer_id,
    datediff('years', birth_date, current_date) AS idade_cliente
INTO temp_tables.customers_age
FROM sales.customers;

SELECT *
FROM temp_tables.customers_age;

-- (Exemplo 2) Criação de tabela a partir do zero
-- Crie uma tabela com a tradução dos status profissionais dos clientes.
-- Chame-a de temp_tables.profissoes
SELECT DISTINCT professional_status
FROM sales.customers;

CREATE TABLE temp_tables.profissoes (
    professional_status VARCHAR,
    status_profissional VARCHAR
);

INSERT INTO temp_tables.profissoes
    (professional_status, status_profissional)
VALUES
    ('freelancer', 'freelancer'),
    ('retired', 'aposentado(a)'),
    ('clt', 'clt'),
    ('self_employed', 'autônomo(a)'),
    ('other', 'outro'),
    ('businessman', 'empresário(a)'),
    ('civil_servant', 'funcionário público(a)'),
    ('student', 'estudante');

SELECT * FROM temp_tables.profissoes;

-- (Exemplo 3) Deleção de tabelas
-- Delete a tabela temp_tables.profissoes
DROP TABLE temp_tables.profissoes;
```

- `DROP TABLE IF EXISTS nome` evita erro caso a tabela não exista

## Aula 41 - Linhas - Inserção, atualização e deleção

- Para inserir linhas usamos `INSERT INTO` e `VALUES`
- Para atualizar as linhas de uma tabela usamos `UPDATE` e `SET`
- Podemos usar `WHERE` para filtrar quais linhas serão atualizadas
- Para deletar linhas usamos `DELETE FROM`
- Cuidado: `UPDATE` e `DELETE FROM` sem `WHERE` afetam todas as linhas da tabela

```sql
-- (Exemplo 1) Inserção de linhas
-- Insira os status 'desempregado(a)' e 'estagiário(a)' na temp_tables.profissoes

-- A tabela foi apagada no exemplo anterior, por isso é criada novamente
CREATE TABLE temp_tables.profissoes (
    professional_status VARCHAR,
    status_profissional VARCHAR
);

INSERT INTO temp_tables.profissoes
    (professional_status, status_profissional)
VALUES
    ('freelancer', 'freelancer'),
    ('retired', 'aposentado(a)'),
    ('clt', 'clt'),
    ('self_employed', 'autônomo(a)'),
    ('other', 'outro'),
    ('businessman', 'empresário(a)'),
    ('civil_servant', 'funcionário público(a)'),
    ('student', 'estudante');

SELECT * FROM temp_tables.profissoes;

-- Obs.: o valor de estagiário foi gravado sem acento ('estagiario(a)'),
-- por isso o UPDATE do exemplo 2 usa essa grafia
INSERT INTO temp_tables.profissoes
    (professional_status, status_profissional)
VALUES
    ('unemployed', 'desempregado(a)'),
    ('trainee', 'estagiario(a)');

-- (Exemplo 2) Atualização de linhas
-- Corrija a tradução de 'estagiário(a)' de 'trainee' para 'intern'
UPDATE temp_tables.profissoes
SET professional_status = 'intern'
WHERE status_profissional = 'estagiario(a)';

-- (Exemplo 3) Deleção de linhas
-- Delete a linha com o status 'unemployed'
DELETE FROM temp_tables.profissoes
WHERE professional_status = 'unemployed';
```

## Aula 42 - Colunas - Inserção, atualização e deleção

- Usamos `ALTER TABLE` para fazer modificações em uma tabela
- Para adicionar colunas usamos `ADD` (ou `ADD COLUMN`)
- Para alterar o tipo de dado da coluna usamos `ALTER COLUMN`
- Para renomear uma coluna utilizamos `RENAME COLUMN`
- Para deletar uma coluna usamos `DROP COLUMN`
- Os exemplos modificam a tabela `sales.customers`; ao final, a coluna criada é removida, voltando a tabela ao formato original

```sql
-- (Exemplo 1) Inserção de colunas
-- Insira uma coluna na tabela sales.customers com a idade do cliente
ALTER TABLE sales.customers
ADD customer_age INT;

SELECT * FROM sales.customers LIMIT 10;

UPDATE sales.customers
SET customer_age = datediff('years', birth_date, current_date)
WHERE true;

-- (Exemplo 2) Alteração do tipo da coluna
-- Altere o tipo da coluna customer_age de inteiro para varchar
ALTER TABLE sales.customers
ALTER COLUMN customer_age TYPE VARCHAR;

SELECT * FROM sales.customers LIMIT 10;

-- (Exemplo 3) Alteração do nome da coluna
-- Renomeie a coluna "customer_age" para "age"
ALTER TABLE sales.customers
RENAME COLUMN customer_age TO age;

SELECT * FROM sales.customers LIMIT 10;

-- (Exemplo 4) Deleção de coluna
-- Delete a coluna "age"
ALTER TABLE sales.customers
DROP COLUMN age;

SELECT * FROM sales.customers LIMIT 10;
```

- O `WHERE true` no `UPDATE` indica, de forma explícita, que todas as linhas da tabela serão atualizadas
