# SQL Lesson 2: Queries with constraints (Pt. 1)

- ```WHERE```: Permite filtrar resultados. 
- ```AND/OR```: Permite construir cláusulas mais complexas, unindo operadores lógicos. 

```
SELECT column, another_column, …
FROM mytable
WHERE condition
    AND/OR another_condition
    AND/OR …;
```

- Operadores úteis: 

| Operador | Condição | Exemplo |
| -------- | -------- | ------- |
| =, !=, <, <=, >, >= | Operadores numéricos padrão | col_name!= 4 |
| BETWEEN … AND … | O número está dentro do intervalo de dois valores (inclusive) | col_name BETWEEN 1.5 AND 10.5 |
| NOT BETWEEN … AND … | O número não está dentro do intervalo de dois valores (inclusive) | col_name NOT BETWEEN 1 AND 10 |
| IN (…) | Número existe em uma lista | col_name IN (2, 4, 6) |
| NOT IN (…) | Número não existe em uma lista | col_name NOT IN (1, 3, 5) |

# Exercício

- Contexto: Usaremos um banco de dados com dados sobre alguns filmes clássicos da Pixar. Este primeiro exercício envolverá apenas a tabela Filmes. 
- **Table**: movies
- **Columns**: id; title; director; year; length_minutes

<details>
<summary><b> 1. Encontre o filme com uma coluna id de valor 6</b></summary>
SELECT * FROM movies WHERE id = 6; 
</details>

<details>
<summary><b> 2. Encontre os filmes lançados nosyear anos 2000 e 2010</b></summary>
SELECT * FROM movies WHERE year BETWEEN 2000 AND 2010; 
</details>

<details>
<summary><b> 3. Encontre os filmes não lançados nosyear anos 2000 e 2010</b></summary>
SELECT * FROM movies WHERE year NOT BETWEEN 2000 AND 2010; 
</details>

<details>
<summary><b> 4. Encontre os primeiros 5 filmes da Pixar e seu lançamento year</b></summary>
SELECT title, year FROM movies WHERE year <= 2003;
</details>



