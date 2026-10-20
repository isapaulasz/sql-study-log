# SQL Lesson 8: A short note on NULLs

- É sempre bom reduzir a possibilidade de valores NULL em bancos de dados, pois eles exigem atenção especial quando construindo consultas, constraints (funções se comportam de maneira diferente com valores nulos) e processando resultados
- Uma alternativa para valores nulos é possuir valores padrão apropriados ao tipo de dado
    - Exemplo: 0 para valores numericos, string vazia para dados de texto, etc.
- Se seu banco de dados precisa armazenar dados incompletos, então valores NULL podem ser apropriados
- As vezes não é possível evitar valores NULL, quando for feito outer-join em duas tabelas com dados assimétricos
- Nesse caso, você pode testar a coluna para valores NULL com WHERE usando **IS NULL** ou **IS NOT NULL**

```
SELECT column, another_column, …
FROM mytable
WHERE column IS/IS NOT NULL
AND/OR another_condition
AND/OR …;
```

# Exercício

- Contexto: Usaremos uma nova tabela que armazena dados fictícios sobre funcionários no estúdio de cinema e seus edifícios de escritórios designados.

>- **Table**: buildings
>- **Columns**: building_name; capacity;

>- **Table**: employees
>- **Columns**: role; name; building; years_employed;

<details>
<summary><b> 1. Encontre o nome e a função de todos os funcionários que não foram designados para um edifício</b></summary>
SELECT name, role 
FROM employees
WHERE building IS NULL;</details>

<details>
<summary><b> 2. Encontre os nomes dos edifícios que não abrigam funcionários</b></summary>
SELECT building_name 
FROM buildings 
    LEFT JOIN employees
        ON buildings.building_name = employees.building
WHERE name IS NULL;
</details>