# Seção 4 - Operadores

## Aula 16 - Operadores aritméticos

- Servem para executar operações matemáticas
- Muito utilizados para criar colunas calculadas
- Tipos:

| Símbolo | Significado |
| ------- | ----------- |
| `+` | Adição |
| `-` | Subtração |
| `*` | Multiplicação |
| `/` | Divisão |
| `^` | Potenciação |
| `%` | Módulo (resto da divisão) |
| `\|\|` | Concatenação de strings |

- O SQL possui a função `current_date`, que retorna a data atual
- Usamos aspas duplas para nomes de colunas (aliases), principalmente quando têm espaços
- A subtração entre duas datas retorna o número de dias; dividir por 365 dá uma idade aproximada, pois ignora anos bissextos

```sql
-- (Exemplo 1) Criação de coluna calculada
-- Crie uma coluna contendo a idade do cliente da tabela sales.customers
SELECT
    cpf,
    first_name,
    birth_date,
    (current_date - birth_date) / 365 AS "client age"
FROM sales.customers;

-- (Exemplo 2) Utilização da coluna calculada nas queries
-- Liste os 10 clientes mais novos da tabela customers
SELECT
    cpf,
    first_name,
    birth_date,
    (current_date - birth_date) / 365 AS "client age"
FROM sales.customers
ORDER BY "client age"
LIMIT 10;

-- (Exemplo 3) Criação de coluna calculada com strings
-- Crie a coluna "full name" contendo o nome completo do cliente
SELECT
    first_name,
    last_name,
    (first_name || ' ' || last_name) AS "full name"
FROM sales.customers;
```

## Aula 17 - Operadores de comparação

- Servem para comparar dois valores, retornando `TRUE` ou `FALSE`
- Muito utilizados em conjunto com o `WHERE`

| Símbolo | Significado |
| ------- | ----------- |
| `=` | Igual |
| `>` | Maior |
| `<` | Menor |
| `>=` | Maior ou igual |
| `<=` | Menor ou igual |
| `<>` | Diferente |

```sql
-- (Exemplo 1) Uso de operadores como flag
-- Crie uma coluna que retorne TRUE sempre que um cliente for um profissional clt
SELECT
    customer_id,
    first_name,
    professional_status,
    (professional_status = 'clt') AS cliente_clt
FROM sales.customers;
```

## Aula 18 - Operadores lógicos

- São usados para unir expressões simples em uma expressão composta

| Termo | Significado |
| ------- | ----------- |
| `AND` | Verifica se duas comparações são simultaneamente verdadeiras |
| `OR` | Verifica se uma ou outra comparação é verdadeira |
| `NOT` | Retorna o oposto, a negativa do comando |
| `BETWEEN` | Verifica quais valores estão dentro do intervalo definido (inclusive os extremos) |
| `IN` | Funciona como múltiplos `OR` |
| `LIKE` | Compara textos e é sempre utilizado em conjunto com o caractere `%`, que funciona como um coringa, indicando que qualquer texto pode aparecer no lugar |
| `ILIKE` | Igual ao `LIKE`, mas ignora letras maiúsculas e minúsculas na comparação (específico do PostgreSQL) |
| `IS NULL` | Verifica se o campo é nulo |

- O caractere `_` também pode ser usado no `LIKE`, representando exatamente um caractere
- `IS NOT NULL` verifica se o campo não é nulo

```sql
-- (Exemplo 1) Uso do comando BETWEEN
-- Selecione veículos que custam entre 100k e 200k na tabela products
SELECT *
FROM sales.products
WHERE price BETWEEN 100000 AND 200000;

-- (Exemplo 2) Uso do comando NOT
-- Selecione veículos que custam abaixo de 100k ou acima de 200k
SELECT *
FROM sales.products
WHERE price NOT BETWEEN 100000 AND 200000;

-- (Exemplo 3) Uso do comando IN
-- Selecione produtos que sejam da marca HONDA, TOYOTA ou RENAULT
SELECT *
FROM sales.products
WHERE brand IN ('HONDA', 'TOYOTA', 'RENAULT');

-- (Exemplo 4) Uso do comando LIKE (correspondências parciais)
-- Selecione os primeiros nomes distintos da tabela customers que começam
-- com as iniciais ANA
SELECT DISTINCT first_name
FROM sales.customers
WHERE first_name LIKE 'ANA%';

-- (Exemplo 5) Uso do comando ILIKE (ignora letras maiúsculas e minúsculas)
-- Selecione os primeiros nomes distintos com iniciais 'ana'
SELECT DISTINCT first_name
FROM sales.customers
WHERE first_name ILIKE 'ANA%';

-- (Exemplo 6) Uso do comando IS NULL
-- Selecione apenas as linhas que contêm nulo no campo "population" na tabela
-- temp_tables.regions
SELECT *
FROM temp_tables.regions
WHERE population IS NULL;
```

## Aula 19 - Desafio

```sql
-- (Exercício 1) Calcule quantos salários mínimos ganha cada cliente da tabela
-- sales.customers. Selecione as colunas de: email, income e a coluna calculada "salários mínimos"
-- Considere o salário mínimo igual a R$1200
SELECT
    email,
    income,
    (income / 1200) AS "salarios minimos"
FROM sales.customers;

-- (Exercício 2) Na query anterior acrescente uma coluna informando TRUE se o cliente
-- ganha acima de 4 salários mínimos e FALSE se ganha 4 salários ou menos.
-- Chame a nova coluna de "acima de 4 salários"
SELECT
    email,
    income,
    (income / 1200) AS "salarios minimos",
    ((income / 1200) > 4) AS "acima de 4 salarios"
FROM sales.customers;

-- (Exercício 3) Na query anterior filtre apenas os clientes que ganham entre
-- 4 e 5 salários mínimos. Utilize o comando BETWEEN
SELECT
    email,
    income,
    (income / 1200) AS "salarios minimos",
    ((income / 1200) > 4) AS "acima de 4 salarios"
FROM sales.customers
WHERE (income / 1200) BETWEEN 4 AND 5;

-- (Exercício 4) Selecione o email, cidade e estado dos clientes que moram no estado de
-- Minas Gerais e Mato Grosso
SELECT
    email,
    city,
    state
FROM sales.customers
WHERE state IN ('MT', 'MG');

-- (Exercício 5) Selecione o email, cidade e estado dos clientes que não
-- moram no estado de São Paulo
SELECT
    email,
    city,
    state
FROM sales.customers
WHERE state NOT IN ('SP');

-- (Exercício 6) Selecione os nomes das cidades que começam com a letra Z.
-- Dados da tabela temp_tables.regions
SELECT
    city
FROM temp_tables.regions
WHERE city ILIKE 'Z%';
```
