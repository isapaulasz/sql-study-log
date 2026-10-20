# SQL Lesson 6: Multi-table queries with JOINs

- ```JOIN```: Permite combinar linhas de dados através de duas tabelas separadas usando uma chave (primária/estrangeira)
- ```INNER JOIN```: Combina linhas da primeira e segunda tabela que tem a mesma chave para criar uma linha resultado com as colunas combinadas de ambas as tabelas
    - ```ON```: Define a chave

> Você pode ver consultas onde INNER JOIN é escrito simplesmente como JOIN . Esses dois são equivalentes, mas continuaremos a nos referir a essas JOINs como INNER JOIN.

```
SELECT column, another_table_column, …
FROM mytable
INNER JOIN another_table 
    ON mytable.id = another_table.id
WHERE condition(s)
ORDER BY column, … ASC/DESC
LIMIT num_limit OFFSET num_offset;
```

# Exercício

- Contexto: Usaremos um banco de dados com dados sobre alguns filmes clássicos da Pixar. Adicionamos uma nova tabela ao banco de dados da Pixar para que você possa tentar praticar algumas junções.

>- **Table**: movies
>- **Columns**: id; title; director; year; length_minutes

>- **Table**: boxoffice
>- **Columns**: movie_id; rating; domestic_sales; international_sales;

<details>
<summary><b> 1. Encontre as vendas nacionais e internacionais de cada filme</b></summary>
SELECT 
    movies.title, 
    boxoffice.domestic_sales, 
    boxoffice.international_sales 
FROM movies 
    INNER JOIN boxoffice 
        ON movies.id = boxoffice.movie_id;
</details>

<details>
<summary><b> 2. Mostre os números de vendas de cada filme que teve melhor desempenho internacional do que nacionalmente</b></summary>
SELECT 
    movies.title, 
    boxoffice.domestic_sales, 
    boxoffice.international_sales 
FROM movies 
    INNER JOIN boxoffice 
        ON movies.id = boxoffice.movie_id 
WHERE boxoffice.international_sales > boxoffice.domestic_sales;
</details>

<details>
<summary><b> 3. Liste todos os filmes por suas classificações em ordem decrescente</b></summary>
SELECT 
    movies.title, 
    boxoffice.rating 
FROM movies 
    INNER JOIN boxoffice 
        ON movies.id = boxoffice.movie_id 
ORDER BY boxoffice.rating DESC;
</details>