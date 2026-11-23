# Seção 7 - Unions

## Aula 29 - Tipos de UNION

- Serve para colar uma tabela sobre a outra (empilhar os resultados de duas consultas)
- É necessário que as duas consultas tenham a mesma quantidade de colunas e que as colunas tenham tipos de dados compatíveis
- As colunas são combinadas pela posição, e não pelo nome
- `UNION` e `UNION ALL`:
  - `UNION ALL` não remove linhas duplicadas
  - `UNION` remove linhas duplicadas

## Aula 30 - Exemplo

```sql
-- (Exemplo 1) União simples de duas tabelas
-- Una a tabela sales.products com a tabela temp_tables.products_2
SELECT * FROM sales.products
UNION ALL
SELECT * FROM temp_tables.products_2;

-- (Exemplo 2) União removendo linhas duplicadas
-- Una as mesmas tabelas, mantendo apenas uma ocorrência de cada linha repetida
SELECT * FROM sales.products
UNION
SELECT * FROM temp_tables.products_2;
```
