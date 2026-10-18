# SQL Lesson 3: Queries with constraints (Pt. 2)

- SQL suporta diversos operadores para fazer comparações de strings sem distinção entre maiúsculas e minúsculas e correspondência de padrões coringa.
- Alguns operadores comuns de dados de texto:

| Operador | Condição | Exemplo |
| -------- | -------- | ------- |
| = | Comparação exata de strings com diferenciação entre maiúsculas e minúsculas (observe que o único é igual) | col_name = "abc" |
| != or <> | Comparação exata da desigualdade de strings sensível a casos  col_name != "abcd" |
| LIKE | Comparação exata de strings sem distinção entre maiúsculas e minúsculas | col_name LIKE "ABC" |
| NOT LIKE | Comparação exata de desigualdade de strings sem distinção entre maiúsculas e minúsculas | col_name NOT LIKE "ABCD" |
| % | Usado em qualquer lugar em uma string para corresponder uma sequência de zero ou mais caracteres (somente com LIKE ou NOT LIKE) | col_name LIKE "%AT%" |
| _ | Usado em qualquer lugar em uma string para corresponder um único caractere (somente com LIKE ou NOT LIKE) | col_name LIKE "AN_" |
| IN (…) | String existe em uma lista | col_name IN ("A", "B", "C") |
| NOT IN (…) | String não existe em uma lista | col_name NOT IN ("D", "E", "F") |


# Exercício

- Contexto: Usaremos um banco de dados com dados sobre alguns filmes clássicos da Pixar. Este primeiro exercício envolverá apenas a tabela Filmes. 
- **Table**: movies
- **Columns**: id; title; director; year; length_minutes

<details>
<summary><b> 1. Encontre todos os filmes de Toy Story</b></summary>
SELECT * FROM movies WHERE title LIKE "%Toy Story%";
</details>

<details>
<summary><b> 2. Encontre todos os filmes dirigidos por John Lasseter</b></summary>
SELECT title, director FROM movies WHERE director = "John Lasseter";
SELECT * FROM movies WHERE director LIKE "John Lasseter"; 
</details>

<details>
<summary><b> 3. Encontre todos os filmes (e diretores) não dirigidos por John Lasseter</b></summary>
SELECT title, director FROM movies WHERE director != "John Lasseter";
SELECT * FROM movies WHERE director NOT LIKE "John Lasseter";
</details>

<details>
<summary><b> 4. Encontre todos os filmes WALL-*</b></summary>
SELECT * FROM movies WHERE title LIKE "WALL-_";
SELECT * FROM movies WHERE title LIKE "WALL-%";
</details>