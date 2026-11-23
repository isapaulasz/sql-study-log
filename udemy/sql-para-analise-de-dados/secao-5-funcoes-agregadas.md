# Seção 5 - Funções Agregadas

## Aula 20 - Funções Agregadas

- Servem para executar operações aritméticas nos registros de uma coluna

| Função | Significado |
| ------- | ----------- |
| `COUNT()` | Contagem |
| `SUM()` | Soma |
| `MIN()` | Valor mínimo |
| `MAX()` | Valor máximo |
| `AVG()` | Média |

- Funções agregadas não computam células vazias (`NULL`)
- `COUNT(*)` é a exceção: conta todas as linhas, inclusive as que têm valores nulos
- `COUNT(DISTINCT coluna)` conta apenas valores exclusivos

```sql
-- (Exemplo 1) Contagem de todas as linhas de uma tabela
-- Conte todas as visitas realizadas ao site da empresa fictícia
SELECT COUNT(*) FROM sales.funnel;

-- (Exemplo 2) Contagem das linhas de uma coluna
-- Conte todos os pagamentos registrados na tabela sales.funnel
SELECT COUNT(paid_date) FROM sales.funnel;

-- (Exemplo 3) Contagem distinta de uma coluna
-- Conte todos os produtos distintos visitados em jan/21
SELECT COUNT(DISTINCT product_id)
FROM sales.funnel
WHERE visit_page_date BETWEEN '20210101' AND '20210131';

-- (Exemplo 4) Calcule o preço mínimo, máximo e médio dos produtos da tabela products
SELECT MIN(price), MAX(price), AVG(price) FROM sales.products;

-- (Exemplo 5) Informe qual é o preço do veículo mais caro da tabela products
SELECT MAX(price) FROM sales.products;
```

## Aula 21 - GROUP BY

- Serve para agrupar registros semelhantes de uma coluna
- Normalmente utilizado em conjunto com as funções de agregação
- Podemos referenciar a coluna a ser agrupada pela posição ordinal, porém não é uma boa prática
  - ex: `GROUP BY 1, 2, 3`
- O `GROUP BY` sozinho funciona como um `DISTINCT`

```sql
-- (Exemplo 1) Contagem agrupada de uma coluna
-- Calcule o nº de clientes da tabela customers por estado
SELECT state, COUNT(*) AS customer_per_state
FROM sales.customers
GROUP BY state
ORDER BY customer_per_state DESC;

-- (Exemplo 2) Contagem agrupada de várias colunas
-- Calcule o nº de clientes por estado e status profissional
SELECT state, professional_status, COUNT(*) AS customer_per_state
FROM sales.customers
GROUP BY state, professional_status
ORDER BY state, customer_per_state DESC;

-- (Exemplo 3) Seleção de valores distintos
-- Selecione os estados distintos na tabela customers utilizando o GROUP BY
SELECT state
FROM sales.customers
GROUP BY state;
```

## Aula 22 - HAVING

- Serve para filtrar linhas da seleção a partir do resultado de uma função agregada ou de uma coluna agrupada
- O `WHERE` não aceita funções agregadas, pois filtra as linhas antes do agrupamento; o `HAVING` filtra depois

```sql
-- (Exemplo 1) Seleção com filtro no HAVING
-- Calcule o nº de clientes por estado filtrando apenas estados acima de 100 clientes
SELECT
    state,
    COUNT(*)
FROM sales.customers
GROUP BY state
HAVING COUNT(*) > 100;
```

## Aula 23 - Desafio

```sql
-- (Exercício 1) Conte quantos clientes da tabela sales.customers têm menos de 30 anos
SELECT COUNT(*)
FROM sales.customers
WHERE ((current_date - birth_date) / 365) < 30;

-- (Exercício 2) Informe a idade do cliente mais velho e mais novo da tabela sales.customers
SELECT
    MAX((current_date - birth_date) / 365) AS mais_velho,
    MIN((current_date - birth_date) / 365) AS mais_novo
FROM sales.customers;

-- (Exercício 3) Selecione todas as informações do cliente mais rico da tabela sales.customers
-- (possivelmente a resposta contém mais de um cliente)
SELECT *
FROM sales.customers
WHERE income = (SELECT MAX(income) FROM sales.customers);

-- (Exercício 4) Conte quantos veículos de cada marca têm registrados na tabela sales.products
-- Ordene o resultado pelo nome da marca
SELECT brand, COUNT(*)
FROM sales.products
GROUP BY brand
ORDER BY brand;

-- (Exercício 5) Conte quantos veículos existem registrados na tabela sales.products
-- por marca e ano do modelo. Ordene pelo nome da marca e pelo ano do veículo
SELECT brand, model_year, COUNT(*)
FROM sales.products
GROUP BY brand, model_year
ORDER BY brand, model_year;

-- (Exercício 6) Conte quantos veículos de cada marca têm registrados na tabela sales.products
-- e mostre apenas as marcas que contêm mais de 10 veículos registrados
SELECT brand, COUNT(product_id)
FROM sales.products
GROUP BY brand
HAVING COUNT(*) > 10;
```
