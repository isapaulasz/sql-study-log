# Seção 6 - JOINs

## Aula 25 - Tipos de JOIN

- Servem para combinar colunas de duas ou mais tabelas, a partir de uma coluna em comum
- Sintaxe geral:

```sql
SELECT t1.coluna, t2.coluna
FROM schema.tabela_1 AS t1
LEFT JOIN schema.tabela_2 AS t2
    ON t1.chave = t2.chave;
```

- `ON`: define a condição de ligação entre as tabelas
- `LEFT JOIN`: inclui todos os dados da tabela à esquerda e somente os dados da tabela à direita que têm correspondência na tabela à esquerda
- `INNER JOIN`: inclui somente os dados presentes em ambas as tabelas
- `RIGHT JOIN`: inclui todos os dados da tabela à direita e somente os dados da tabela à esquerda que têm correspondência na tabela à direita
- `FULL JOIN`: inclui todos os dados de ambas as tabelas, tendo correspondência ou não
- Quando não há correspondência, as colunas da outra tabela são preenchidas com `NULL`

```sql
-- (Exemplo 1) Utilize o LEFT JOIN para fazer join entre as tabelas
-- temp_tables.tabela_1 e temp_tables.tabela_2
SELECT t1.cpf, t1.name, t2.state
FROM temp_tables.tabela_1 AS t1
LEFT JOIN temp_tables.tabela_2 AS t2
    ON t1.cpf = t2.cpf;

-- (Exemplo 2) Utilize o INNER JOIN para fazer join entre as tabelas
-- temp_tables.tabela_1 e temp_tables.tabela_2
SELECT t1.cpf, t1.name, t2.state
FROM temp_tables.tabela_1 AS t1
INNER JOIN temp_tables.tabela_2 AS t2
    ON t1.cpf = t2.cpf;

-- (Exemplo 3) Utilize o RIGHT JOIN para fazer join entre as tabelas
-- temp_tables.tabela_1 e temp_tables.tabela_2
SELECT t1.cpf, t1.name, t2.state
FROM temp_tables.tabela_1 AS t1
RIGHT JOIN temp_tables.tabela_2 AS t2
    ON t1.cpf = t2.cpf;

-- (Exemplo 4) Utilize o FULL JOIN para fazer join entre as tabelas
-- temp_tables.tabela_1 e temp_tables.tabela_2
SELECT t1.cpf, t1.name, t2.state
FROM temp_tables.tabela_1 AS t1
FULL JOIN temp_tables.tabela_2 AS t2
    ON t1.cpf = t2.cpf;
```

## Exemplos

```sql
-- (Exemplo 1) Identifique qual é o status profissional mais frequente nos clientes
-- que compraram automóveis no site
SELECT
    cus.professional_status,
    COUNT(fun.paid_date) AS pagamentos
FROM sales.funnel AS fun
LEFT JOIN sales.customers AS cus
    ON fun.customer_id = cus.customer_id
GROUP BY cus.professional_status
ORDER BY pagamentos DESC;

-- (Exemplo 2) Identifique qual é o gênero mais frequente nos clientes que compraram
-- automóveis no site. Obs: utilizar a tabela temp_tables.ibge_genders
SELECT
    ibge.gender,
    COUNT(fun.paid_date)
FROM sales.funnel AS fun
LEFT JOIN sales.customers AS cus
    ON fun.customer_id = cus.customer_id
LEFT JOIN temp_tables.ibge_genders AS ibge
    ON LOWER(cus.first_name) = ibge.first_name
GROUP BY ibge.gender;

-- (Exemplo 3) Identifique de quais regiões são os clientes que mais visitam o site
-- Obs: utilizar a tabela temp_tables.regions
SELECT
    reg.region,
    COUNT(fun.visit_page_date) AS visits
FROM sales.funnel AS fun
LEFT JOIN sales.customers AS cus
    ON fun.customer_id = cus.customer_id
LEFT JOIN temp_tables.regions AS reg
    ON LOWER(cus.city) = LOWER(reg.city)
    AND LOWER(cus.state) = LOWER(reg.state)
GROUP BY reg.region
ORDER BY visits DESC;
```

- No exemplo 2, o `LOWER` é usado porque a tabela `ibge_genders` guarda os nomes em letras minúsculas
- No exemplo 3, o `JOIN` usa duas condições (cidade e estado), pois há cidades com o mesmo nome em estados diferentes

## Desafio
