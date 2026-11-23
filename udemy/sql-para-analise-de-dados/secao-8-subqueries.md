# Seção 8 - Subqueries

## Aula 31 - Tipos de Subqueries

- Servem para realizar uma query dentro de outra e utilizar o resultado de uma subconsulta em uma consulta

| Tipo de Subquery | Significado |
| ------- | ----------- |
| Subquery no `WHERE` | Filtra as linhas usando o resultado de outra consulta |
| Subquery com `WITH` | Cria uma tabela temporária, usada na consulta principal |
| Subquery no `FROM` | Usa o resultado de uma consulta como se fosse uma tabela |
| Subquery no `SELECT` | Cria uma coluna a partir do resultado de outra consulta |

- Para que subqueries no `WHERE` (com operadores de comparação) e no `SELECT` funcionem, elas devem retornar apenas um único valor
- Subqueries no `WHERE` com o operador `IN` podem retornar vários valores
- Apesar de existir a subquery no `FROM`, é preferível o uso da query com `WITH`, por ser mais legível

```sql
-- (Exemplo 1) Subquery no WHERE
-- Informe qual é o veículo mais barato da tabela products
SELECT *
FROM sales.products
WHERE price = (SELECT MIN(price) FROM sales.products);

-- (Exemplo 2) Subquery com WITH
-- Calcule a idade média dos clientes por status profissional
WITH tabela_nova AS (
    SELECT
        professional_status,
        (current_date - birth_date) / 365 AS idade
    FROM sales.customers
)

SELECT
    professional_status,
    AVG(idade) AS idade_media
FROM tabela_nova
GROUP BY professional_status;

-- (Exemplo 3) Subquery no FROM
-- Calcule a média de idades dos clientes por status profissional
SELECT
    professional_status,
    AVG(idade) AS idade_media
FROM (
    SELECT
        professional_status,
        (current_date - birth_date) / 365 AS idade
    FROM sales.customers
) AS tabela_nova
GROUP BY professional_status;

-- (Exemplo 4) Subquery no SELECT
-- Na tabela sales.funnel crie uma coluna que informe o nº de visitas acumuladas
-- que a loja visitada recebeu até o momento
SELECT
    fun.visit_id,
    fun.visit_page_date,
    sto.store_name,
    (
        SELECT COUNT(*)
        FROM sales.funnel AS fun2
        WHERE fun2.visit_page_date <= fun.visit_page_date
            AND fun2.store_id = fun.store_id
    ) AS visitas_acumuladas
FROM sales.funnel AS fun
LEFT JOIN sales.stores AS sto
    ON fun.store_id = sto.store_id
ORDER BY sto.store_name, fun.visit_page_date;
```

- Os exemplos 2 e 3 retornam o mesmo resultado, escritos de formas diferentes

## Exemplos

```sql
-- (Exemplo 1) Análise de recorrência dos leads
-- Calcule o volume de visitas por dia ao site separado por 1ª visita e demais visitas
WITH primeira_visita AS (
    SELECT
        customer_id,
        MIN(visit_page_date) AS visita_1
    FROM sales.funnel
    GROUP BY customer_id
)

SELECT
    fun.visit_page_date,
    (fun.visit_page_date <> primeira_visita.visita_1) AS lead_recorrente,
    COUNT(*)
FROM sales.funnel AS fun
LEFT JOIN primeira_visita
    ON fun.customer_id = primeira_visita.customer_id
GROUP BY fun.visit_page_date, lead_recorrente
ORDER BY fun.visit_page_date DESC;

-- (Exemplo 2) Análise do preço versus o preço médio
-- Calcule, para cada visita ao site, quanto o preço do veículo visitado pelo cliente
-- estava acima ou abaixo do preço médio dos veículos daquela marca
-- (levar em consideração o desconto dado no veículo)
WITH preco_medio AS (
    SELECT
        brand,
        AVG(price) AS preco_medio_marca
    FROM sales.products
    GROUP BY brand
)

SELECT
    fun.visit_id,
    fun.visit_page_date,
    pro.brand,
    (pro.price * (1 + fun.discount)) AS preco_final,
    preco_medio.preco_medio_marca,
    (pro.price * (1 + fun.discount)) - preco_medio.preco_medio_marca AS preco_vs_media
FROM sales.funnel AS fun
LEFT JOIN sales.products AS pro
    ON fun.product_id = pro.product_id
LEFT JOIN preco_medio
    ON pro.brand = preco_medio.brand;
```

- No exemplo 1, a `CTE` `primeira_visita` guarda a data da primeira visita de cada cliente, e a comparação com a data de cada visita indica se o lead é recorrente
- No exemplo 2, a `CTE` `preco_medio` calcula o preço médio por marca, que depois é ligado a cada visita por meio do `LEFT JOIN`

## Desafio
