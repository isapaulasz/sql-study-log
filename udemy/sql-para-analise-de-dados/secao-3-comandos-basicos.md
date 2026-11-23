# Seção 3 - Comandos Básicos

## Aula 9 - SELECT

- Serve para selecionar colunas de tabelas

```sql
SELECT coluna_1, coluna_2 FROM schema_1.tabela_1;
```

- Colunas são separadas por vírgula
- O asterisco (`*`) é usado para selecionar todas as colunas da tabela

- Exemplos

```sql
-- (Exemplo 1) Seleção de uma coluna de uma tabela
-- Liste os e-mails dos clientes da tabela sales.customers
SELECT email FROM sales.customers;

-- (Exemplo 2) Seleção de mais de uma coluna de uma tabela
-- Liste os e-mails e nomes dos clientes da tabela sales.customers
SELECT first_name, last_name, email FROM sales.customers;

-- (Exemplo 3) Seleção de todas as colunas de uma tabela
-- Liste todas as informações dos clientes da tabela sales.customers
SELECT * FROM sales.customers;
```

## Aula 10 - DISTINCT

- Serve para remover linhas duplicadas e mostrar apenas valores distintos

```sql
SELECT DISTINCT coluna_1 FROM schema.tabela;
```

- Ao usar o `DISTINCT` com duas colunas, o comando encontra todas as combinações distintas das duas colunas

```sql
-- (Exemplo 1) Seleção de uma coluna sem DISTINCT
-- Liste as marcas de carro que constam na tabela products
SELECT brand FROM sales.products;

-- (Exemplo 2) Seleção de uma coluna com DISTINCT
-- Liste as marcas de carro distintas que constam na tabela products
SELECT DISTINCT brand FROM sales.products;

-- (Exemplo 3) Seleção de mais de uma coluna com DISTINCT
-- Liste as marcas e anos de modelo distintos que constam na tabela products
SELECT DISTINCT brand, model_year FROM sales.products;
```

## Aula 11 - WHERE

- Serve para filtrar as linhas de acordo com uma condição

```sql
SELECT coluna_1 FROM schema.tabela WHERE condicao;
```

- É importante checar o tipo de dado e como ele está registrado antes de fazer a busca
- A comparação de textos diferencia maiúsculas de minúsculas, e não se atentar a isso pode afetar a busca
- Valores de texto usam aspas simples (`'SC'`); as aspas duplas no PostgreSQL servem para nomes de objetos, como colunas
- Datas podem ser escritas no formato `'AAAA-MM-DD'` ou no formato `'AAAAMMDD'`
- Quando `AND` e `OR` são usados juntos, o `AND` é avaliado primeiro; use parênteses para garantir a lógica desejada

```sql
-- (Exemplo 1) Filtro com condição única
-- Liste os e-mails dos clientes da nossa base que moram no estado de Santa Catarina
SELECT email FROM sales.customers WHERE customers.state = 'SC';

-- (Exemplo 2) Filtro com mais de uma condição
-- Liste os e-mails dos clientes da nossa base que moram no estado de Santa Catarina
-- ou Mato Grosso do Sul
SELECT email FROM sales.customers WHERE customers.state = 'SC' OR customers.state = 'MS';

-- (Exemplo 3) Filtro de condição com data
-- Liste os e-mails dos clientes da nossa base que moram no estado de Santa Catarina
-- ou Mato Grosso do Sul e que têm mais de 30 anos
SELECT email
FROM sales.customers
WHERE (customers.state = 'SC' OR customers.state = 'MS')
    AND customers.birth_date < '1994-11-09';
```

- No exemplo 3, a data `'1994-11-09'` equivale a 30 anos atrás na data em que a aula foi feita; para outra data, ajuste o valor

## Aula 12 - ORDER BY

- Serve para ordenar a seleção de acordo com uma regra

```sql
SELECT coluna FROM tabela WHERE condicao ORDER BY coluna;
```

- O `ORDER BY` por padrão ordena na ordem crescente (`ASC`)
- Para ordenar na ordem decrescente usamos `DESC`
- Textos são ordenados em ordem alfabética

```sql
-- (Exemplo 1) Ordenação de valores numéricos
-- Liste produtos da tabela products na ordem crescente com base no preço
SELECT * FROM sales.products ORDER BY price;

-- (Exemplo 2) Ordenação de texto
-- Liste os estados distintos da tabela customers na ordem crescente
SELECT DISTINCT state FROM sales.customers ORDER BY state;
```

## Aula 13 - LIMIT

- Serve para limitar o número de linhas retornado pela consulta
- Útil para trabalhar com grandes volumes de dados

```sql
SELECT coluna
FROM tabela
WHERE condicao
ORDER BY coluna
LIMIT n;
```

- Muito utilizado em conjunto com o comando `ORDER BY`

```sql
-- (Exemplo 1) Seleção das N primeiras linhas usando LIMIT
-- Liste as 10 primeiras linhas da tabela funnel utilizando o LIMIT
SELECT * FROM sales.funnel LIMIT 10;

-- (Exemplo 2) Seleção das N primeiras linhas usando LIMIT e ORDER BY
-- Liste os 10 produtos mais caros da tabela products com o comando LIMIT
SELECT * FROM sales.products ORDER BY price DESC LIMIT 10;
```

## Aula 14 - Desafio

```sql
-- (Exercício 1) Selecione os nomes de cidades distintas que existem no estado de
-- Minas Gerais em ordem alfabética (dados da tabela sales.customers)
SELECT DISTINCT city FROM sales.customers WHERE state = 'MG' ORDER BY city;

-- (Exercício 2) Selecione o visit_id das 10 compras mais recentes efetuadas
-- (dados da tabela sales.funnel)
SELECT visit_id FROM sales.funnel WHERE paid_date IS NOT NULL ORDER BY paid_date DESC LIMIT 10;

-- (Exercício 3) Selecione todos os dados dos 10 clientes com maior score nascidos
-- após 01/01/2000 (dados da tabela sales.customers)
SELECT * FROM sales.customers WHERE birth_date > '20000101' ORDER BY score DESC LIMIT 10;
```
