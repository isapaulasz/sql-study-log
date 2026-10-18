# SQL Lesson 4: Filtering and sorting Query results

- ```DISTINCT```: Descarta linhas que têm um valor de coluna duplicado.
- ```ORDER BY```: Maneira de classificar seus resultados por uma determinada coluna em ordem crescente ou decrescente
    - ```ASC```: Ascendente
    - ```DESC```: Descendente 
- ```LIMIT```: Reduz a quantidade de linhas retornadas
- ```OFFSET```: Especifica onde começar a retornar as linhas

```
SELECT DISTINCT column, another_column, …
FROM mytable
WHERE condition(s)
ORDER BY column ASC/DESC
LIMIT num_limit OFFSET num_offset;
```

# Exercício

- Contexto: Usaremos um banco de dados com dados sobre alguns filmes clássicos da Pixar. Este primeiro exercício envolverá apenas a tabela Filmes. 
- **Table**: movies
- **Columns**: id; title; director; year; length_minutes

<details>
<summary><b> 1. Liste todos os diretores de filmes da Pixar (em ordem alfabética), sem duplicatas</b></summary>
SELECT DISTINCT director FROM movies ORDER BY director ASC;
</details>

<details>
<summary><b> 2. Liste os últimos quatro filmes da Pixar lançados (ordenados do mais recente ao menos)</b></summary>
SELECT * FROM movies ORDER BY year DESC LIMIT 4;
</details>

<details>
<summary><b> 3. Liste os primeiros cinco filmes da Pixar classificados em ordem alfabética</b></summary>
SELECT * FROM movies ORDER BY title ASC LIMIT 5;
</details>

<details>
<summary><b> 4. Liste os próximos cinco filmes da Pixar classificados em ordem alfabética</b></summary>
SELECT * FROM movies ORDER BY title ASC LIMIT 5 OFFSET 5;
</details>