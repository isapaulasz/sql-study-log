# Seção 9 - Tratamento de dados

## Aula 35 - Conversão de unidades

- Tipos de conversão:

| Método | Aplicação |
| ------- | ----------- |
| Operador `::` | `x::type` |
| `CAST` | `CAST(x AS type)` |

- O `CAST` faz parte do padrão SQL e funciona em outros bancos; o operador `::` é específico do PostgreSQL
- Às vezes é preciso explicitar o tipo de dado
  - ex: `'2021-10-01'::date`

```sql
-- (Exemplo 1) Conversão de texto em data
-- Corrija a query abaixo utilizando o operador ::
-- SELECT '2021-10-01' - '2021-02-01'
SELECT '2021-10-01'::date - '2021-02-01'::date;

-- (Exemplo 2) Conversão de texto em número
-- Corrija a query abaixo utilizando o operador ::
-- SELECT '100' - '10'
SELECT '100'::numeric - '10'::numeric;

-- (Exemplo 3) Conversão de número em texto
-- Corrija a query abaixo utilizando o operador ::
-- SELECT REPLACE(112122, '1', 'A')
SELECT REPLACE(112122::text, '1', 'A');

-- (Exemplo 4) Conversão de texto em data
-- Corrija a query abaixo utilizando a função CAST
-- SELECT '2021-10-01' - '2021-02-01'
SELECT CAST('2021-10-01' AS date) - CAST('2021-02-01' AS date);
```

## Aula 36 - Tratamento geral

- `CASE WHEN`: utilizado para criar respostas específicas para diferentes condições, sendo muito utilizado para fazer agrupamento de dados
- `COALESCE()`: utilizado para preencher campos nulos com o primeiro valor não nulo de uma sequência de valores

```sql
-- (Exemplo 1) Agrupamento de dados com CASE WHEN
-- Calcule o nº de clientes que ganham abaixo de 5k, entre 5k e 10k, entre 10k e
-- 15k e acima de 15k
WITH faixa_de_renda AS (
    SELECT
        income,
        CASE
            WHEN income < 5000 THEN '0-5000'
            WHEN income >= 5000 AND income < 10000 THEN '5000-10000'
            WHEN income >= 10000 AND income < 15000 THEN '10000-15000'
            ELSE '15000+'
        END AS faixa_renda
    FROM sales.customers
)

SELECT faixa_renda, COUNT(*)
FROM faixa_de_renda
GROUP BY faixa_renda;

-- (Exemplo 2) Tratamento de dados nulos com COALESCE
-- Crie uma coluna chamada populacao_ajustada na tabela temp_tables.regions e
-- preencha com os dados da coluna population, mas caso esse campo esteja nulo,
-- preencha com a população média (geral) das cidades do Brasil
SELECT * FROM temp_tables.regions LIMIT 10;

-- Opção 1
SELECT
    *,
    CASE
        WHEN population IS NOT NULL THEN population
        ELSE (SELECT AVG(population) FROM temp_tables.regions)
    END AS populacao_ajustada
FROM temp_tables.regions;

-- Opção 2
SELECT
    *,
    COALESCE(population, (SELECT AVG(population) FROM temp_tables.regions)) AS populacao_ajustada
FROM temp_tables.regions;
```

- O `CASE WHEN` avalia as condições em ordem e retorna o resultado da primeira verdadeira; o `ELSE` cobre os demais casos
- As opções 1 e 2 do exemplo 2 geram o mesmo resultado, sendo o `COALESCE` a forma mais curta

## Aula 37 - Tratamento de texto

- `LOWER()`: transforma todas as letras em minúsculo
- `UPPER()`: transforma todas as letras em maiúsculo
- `TRIM()`: remove espaços das extremidades do texto
- `REPLACE()`: substitui uma string por outra

```sql
-- (Exemplo 1) Corrija o primeiro elemento das queries abaixo utilizando os comandos
-- de tratamento de texto para que o resultado seja sempre TRUE

-- Original: retorna FALSE
SELECT 'São Paulo' = 'SÃO PAULO';
-- Corrigida: retorna TRUE
SELECT UPPER('São Paulo') = 'SÃO PAULO';

-- Original: retorna FALSE
SELECT 'São Paulo' = 'são paulo';
-- Corrigida: retorna TRUE
SELECT LOWER('São Paulo') = 'são paulo';

-- Original: retorna FALSE
SELECT 'SÃO PAULO     ' = 'SÃO PAULO';
-- Corrigida: retorna TRUE
SELECT TRIM('SÃO PAULO     ') = 'SÃO PAULO';

-- Original: retorna FALSE
SELECT 'SAO PAULO' = 'SÃO PAULO';
-- Corrigida: retorna TRUE
SELECT REPLACE('SAO PAULO', 'SAO', 'SÃO') = 'SÃO PAULO';
```

## Aula 38 - Tratamento de datas

- `INTERVAL`: usado para somar ou subtrair períodos de tempo na unidade desejada; ao somar um número a uma data, a unidade padrão é dias
- `DATE_TRUNC`: utilizado para truncar uma data no início do período (mês, ano, etc)
- `EXTRACT`: utilizado para extrair unidades de uma data ou timestamp
- `DATEDIFF`: calcula a diferença entre datas; não existe nativamente no PostgreSQL, por isso é criada como função na próxima aula

```sql
-- (Exemplo 1) Soma de datas utilizando INTERVAL
-- Calcule a data de hoje mais 10 unidades (dias, semanas, meses, horas)
SELECT current_date + 10;
SELECT (current_date + INTERVAL '10 weeks')::date;
SELECT (current_date + INTERVAL '10 months')::date;
SELECT current_date + INTERVAL '10 hours';

-- (Exemplo 2) Truncagem de datas utilizando DATE_TRUNC
-- Calcule quantas visitas ocorreram por mês no site da empresa

-- Visitas por dia
SELECT visit_page_date, COUNT(*)
FROM sales.funnel
GROUP BY visit_page_date
ORDER BY visit_page_date DESC;

-- Visitas por mês
SELECT
    DATE_TRUNC('month', visit_page_date)::date AS visit_page_month,
    COUNT(*)
FROM sales.funnel
GROUP BY visit_page_month
ORDER BY visit_page_month DESC;

-- (Exemplo 3) Extração de unidades de uma data utilizando EXTRACT
-- Calcule qual é o dia da semana que mais recebe visitas ao site
SELECT
    EXTRACT('dow' FROM visit_page_date) AS dia_da_semana,
    COUNT(*)
FROM sales.funnel
GROUP BY dia_da_semana
ORDER BY dia_da_semana;
```

- `dow` (day of week) retorna o dia da semana de 0 (domingo) a 6 (sábado)
- Para ver qual dia recebe mais visitas, é possível ordenar por `COUNT(*) DESC`

## Aula 39 - Funções

- Servem para criar comandos personalizados a partir de scripts usados recorrentemente
- Uma vez que a função foi criada, o script não precisa ser rodado novamente: ela fica salva no banco de dados e continua disponível quando o pgAdmin for aberto novamente
- É possível apagar uma função usando `DROP FUNCTION`
- `CREATE OR REPLACE FUNCTION` permite recriar uma função que já existe

```sql
-- (Exemplo 1) Crie uma função chamada DATEDIFF para calcular a diferença entre
-- duas datas em dias, semanas, meses, anos

-- Cálculos manuais, que a função vai automatizar
SELECT (current_date - '2018-06-01');
SELECT (current_date - '2018-06-01') / 7;
SELECT (current_date - '2018-06-01') / 30;
SELECT (current_date - '2018-06-01') / 365;

-- Criação da função
CREATE FUNCTION datediff(unidade VARCHAR, data_inicial DATE, data_final DATE)
RETURNS INTEGER
LANGUAGE SQL

AS

$$

    SELECT
        CASE
            WHEN unidade IN ('d', 'day', 'days') THEN (data_final - data_inicial)
            WHEN unidade IN ('w', 'week', 'weeks') THEN (data_final - data_inicial) / 7
            WHEN unidade IN ('m', 'month', 'months') THEN (data_final - data_inicial) / 30
            WHEN unidade IN ('y', 'year', 'years') THEN (data_final - data_inicial) / 365
        END AS diferenca

$$;

-- Teste da função
SELECT datediff('weeks', '2018-06-01', current_date);
SELECT datediff('years', '2021-02-04', current_date);

-- (Exemplo 2) Delete a função DATEDIFF criada no exercício anterior
DROP FUNCTION datediff;
```

- A função recebe a unidade (`unidade`), a data inicial e a data final, e usa o `CASE WHEN` para escolher o cálculo
- Os cálculos são aproximados: usam 30 dias por mês e 365 dias por ano
- Se existirem funções com o mesmo nome e parâmetros diferentes, o `DROP FUNCTION` exige informar os tipos, como `DROP FUNCTION datediff(VARCHAR, DATE, DATE);`
