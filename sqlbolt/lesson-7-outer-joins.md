# SQL Lesson 7: OUTER JOINs

- ```LEFT JOIN```: Inclui todas as linhas da **primeira tabela**, independente de terem uma correspondencia com a **segunda tabela**
- ```RIGHT JOIN```: Inclui todas as linhas da **segunda tabela**, independente de terem uma correspondencia com a **primeira tabela**
- ```FULL JOIN```: Todas as linhas de ambas as tabelas sãp mantidas

> Você pode ver consultas com essss JOINs escritas como LEFT OUTER JOIN, RIGHT OUTER JOIN, ou FULL OUTER JOIN , mas o termo OUTER é mantida para compatibilidade com SQL-92 e essas consultas são equivalentes a LEFT JOIN, RIGHT JOIN, e FULL JOIN respectivamente.

# Exercício

- Contexto: Usaremos uma nova tabela que armazena dados fictícios sobre funcionários no estúdio de cinema e seus edifícios de escritórios designados.

>- **Table**: buildings
>- **Columns**: building_name; capacity;

>- **Table**: employees
>- **Columns**: role; name; building; years_employed;

<details>
<summary><b> 1. Encontre a lista de todos os edifícios que têm funcionários</b></summary>
SELECT DISTINCT building FROM employees;
</details>

<details>
<summary><b> 2. Encontre a lista de todos os edifícios e sua capacidade</b></summary>
SELECT * FROM buildings;
</details>

<details>
<summary><b> 3. Liste todos os edifícios e as funções distintas dos funcionários em cada edifício (incluindo edifícios vazios)</b></summary>
SELECT DISTINCT building_name, role 
FROM buildings 
    LEFT JOIN employees 
        ON employees.building = buildings.building_name;
</details>