# Seção 6 - SQL - Guia Básico

## Aula 45 - Introdução

- SQL (Structured Query Language): linguagem estruturada de consulta
- Tornou-se o padrão para bancos de dados relacionais
- Grupos:
  - DDL: Data Definition Language (definição dos dados), com `CREATE`, `ALTER` e `DROP`
  - DML: Data Manipulation Language (manipulação dos dados), com `INSERT`, `UPDATE` e `DELETE`
  - DCL: Data Control Language (controle de acesso), com `GRANT` e `REVOKE`
  - DTL: Data Transaction Language (transações), com `COMMIT` e `ROLLBACK`
  - DQL: Data Query Language (consulta), com `SELECT`

## Aula 46 - DDL - CREATE TABLE

```sql
CREATE TABLE nome_tabela (
    nome_atributo tipo [restrição] PRIMARY KEY,
    nome_atributo2 tipo [restrição],
    FOREIGN KEY (nome_atributo2) REFERENCES nome_tabela2 (nome_atributo3)
);
```

- Restrições comuns: `NOT NULL`, `UNIQUE`, `DEFAULT` e `CHECK`

## Aula 47 - DDL - ALTER TABLE

- Cada comando abaixo é uma alteração possível na tabela:

```sql
ALTER TABLE nome_tabela ADD PRIMARY KEY (nome_atributo);
ALTER TABLE nome_tabela ADD FOREIGN KEY (nome_atributo) REFERENCES nome_tabela2 (nome_atributo);
ALTER TABLE nome_tabela ADD COLUMN nome_atributo tipo;
ALTER TABLE nome_tabela ALTER COLUMN nome_atributo TYPE tipo;
ALTER TABLE nome_tabela RENAME COLUMN nome_atributo TO novo_nome;
ALTER TABLE nome_tabela DROP COLUMN nome_atributo;
```

## Aula 48 - DDL - DROP TABLE

```sql
DROP TABLE nome_tabela;
```

- `DROP TABLE IF EXISTS nome_tabela` evita erro caso a tabela não exista

## Aula 54 - DML - INSERT

```sql
INSERT INTO nome_tabela (nome_atributo, nome_atributo2) VALUES (valor1, valor2);
```

## Aula 55 - DML - UPDATE

```sql
UPDATE nome_tabela SET nome_atributo = novo_valor, nome_atributo2 = novo_valor WHERE condição;
```

- O `WHERE` é opcional, mas sem ele todas as linhas da tabela são atualizadas

## Aula 56 - DML - DELETE

```sql
DELETE FROM nome_tabela WHERE condição;
```

- O `WHERE` é opcional, mas sem ele todas as linhas da tabela são apagadas

```sql
TRUNCATE TABLE nome_tabela;
```

- Semelhante ao `DELETE`, mas apaga todas as linhas da tabela de uma só vez, sem filtro e normalmente de forma mais rápida

## Aula 58 - DQL - SELECT - Seleção e ordenação

```sql
SELECT nome_atributo, nome_atributo2 FROM nome_tabela;
SELECT * FROM nome_tabela;
SELECT * FROM nome_tabela ORDER BY nome_atributo [ASC | DESC];
```

- `*`: seleciona todos os atributos
- `ORDER BY`: ordena os resultados com base em um atributo, podendo ser crescente (`ASC`, o padrão) ou decrescente (`DESC`)

## Aula 59 - DQL - SELECT - Filtrando dados

```sql
SELECT atributo FROM tabela WHERE condição;
```

- Operadores:
  - `=`: define igualdade
  - `<>`: define diferença
  - `>`: define comparação maior
  - `<`: define comparação menor
  - `>=` e `<=`: maior ou igual e menor ou igual

## Aula 60 - DQL - SELECT - Filtrando dados (IN)

```sql
SELECT atributo FROM tabela WHERE elemento IN (lista);
SELECT atributo FROM tabela WHERE elemento NOT IN (lista);
```

- Filtra todos os elementos contidos (ou não contidos, com `NOT`) em uma lista de elementos

## Aula 61 - DQL - SELECT - Filtrando dados (BETWEEN)

```sql
SELECT atributo FROM tabela WHERE elemento BETWEEN elemento1 AND elemento2;
SELECT atributo FROM tabela WHERE elemento NOT BETWEEN elemento1 AND elemento2;
```

- Filtra todos os elementos contidos em um intervalo, incluindo os extremos

## Aula 62 - DQL - SELECT - Filtrando dados (LIKE)

```sql
SELECT atributo FROM tabela WHERE elemento LIKE '%a%';
```

- Busca semelhança entre os elementos, sem exigir um valor exato
- `%` é um coringa que representa qualquer sequência de caracteres:
  - `'a%'`: começa com "a"
  - `'%a'`: termina com "a"
  - `'%a%'`: contém "a" em qualquer posição
- `_` representa exatamente um caractere

## Aula 63 - DQL - SELECT - Filtrando dados (IS NULL)

```sql
SELECT atributo FROM tabela WHERE elemento IS NULL;
```

- Procura elementos nulos
- `IS NOT NULL` procura elementos que não são nulos
- Não se usa `= NULL`, pois a comparação com nulo nunca é verdadeira

## Aula 64 - DQL - SELECT - Filtrando dados (AND e OR)

```sql
SELECT atributo FROM tabela WHERE condição AND condição;
SELECT atributo FROM tabela WHERE condição OR condição;
```

- `AND`: busca registros em que as duas condições são verdadeiras
- `OR`: busca registros em que pelo menos uma condição é verdadeira

## Aula 65 - DQL - SELECT - Manipulando saída - Parte 1

```sql
SELECT atributo || atributo2 || atributo3 FROM tabela WHERE condição AND condição;
```

- O operador `||` une os resultados de diversos atributos em uma única coluna (concatenação)

## Aula 66 - DQL - SELECT - Manipulando saída - Parte 2

```sql
SELECT atributo + atributo2 FROM tabela;
SELECT atributo - atributo2 FROM tabela;
SELECT atributo * 10 FROM tabela;
SELECT atributo / 10 FROM tabela;
```

- Manipula o valor da saída usando operadores numéricos

## Aula 67 - DQL - SELECT - Regras de precedência

| Ordem | Regra |
| ----- | ----- |
| 1 | Expressões entre parênteses `(...)` |
| 2 | Todos os operadores de manipulação |
| 3 | Todos os operadores de comparação |
| 4 | Operador lógico `AND` |
| 5 | Operador lógico `OR` |

- Como o `AND` é avaliado antes do `OR`, use parênteses ao combinar os dois

## Aula 68 - DQL - SELECT - Funções de agregação - Parte 1

```sql
SELECT MAX(atributo) FROM tabela;
SELECT MIN(atributo) FROM tabela;
SELECT COUNT(atributo) FROM tabela;
SELECT SUM(atributo) FROM tabela;
SELECT AVG(atributo) FROM tabela;
```

- `MAX`: valor máximo
- `MIN`: valor mínimo
- `COUNT`: conta a quantidade de valores (não nulos)
- `SUM`: soma os valores
- `AVG`: média dos valores

## Aula 69 - DQL - SELECT - Funções de agregação - Parte 2

```sql
SELECT atributo, COUNT(*) FROM tabela GROUP BY atributo;
SELECT atributo, COUNT(*) FROM tabela GROUP BY atributo HAVING COUNT(*) > 1;
```

- `GROUP BY`: agrupa os registros que têm o mesmo valor no atributo, geralmente para aplicar funções de agregação em cada grupo
- `HAVING`: filtra os grupos a partir do resultado de uma função de agregação (o `WHERE` filtra as linhas antes do agrupamento)

## Aula 70 - DQL - SELECT - Valores distintos

```sql
SELECT DISTINCT atributo FROM tabela;
```

- Remove os valores repetidos do resultado

## Aula 71 - DQL - SELECT - Junções - Parte 1

- Junção interna: somente os valores da interseção de `tabela1` e `tabela2`

```sql
SELECT atributo FROM tabela1 INNER JOIN tabela2 ON tabela1.chave = tabela2.chave;
```

## Aula 72 - DQL - SELECT - Junções - Parte 2

- Junção externa à esquerda: todos os valores de `tabela1` e somente a interseção de `tabela2`

```sql
SELECT atributo FROM tabela1 LEFT OUTER JOIN tabela2 ON tabela1.chave = tabela2.chave;
```

- Junção externa à direita: todos os valores de `tabela2` e somente a interseção de `tabela1`

```sql
SELECT atributo FROM tabela1 RIGHT OUTER JOIN tabela2 ON tabela1.chave = tabela2.chave;
```

- Junção externa completa: todos os valores de `tabela1` e `tabela2`

```sql
SELECT atributo FROM tabela1 FULL OUTER JOIN tabela2 ON tabela1.chave = tabela2.chave;
```

## Aula 73 - DQL - SELECT - Junções - Parte 3

- Incluir `IS NULL` na condição permite excluir a interseção, mantendo apenas os valores sem correspondência na outra tabela
  - esquerda: valores que existem só em `tabela1`
  - direita: valores que existem só em `tabela2`
  - completa: valores que existem em apenas uma das duas tabelas

```sql
SELECT atributo FROM tabela1 LEFT OUTER JOIN tabela2 ON chave1 = chave2 WHERE chave2 IS NULL;
SELECT atributo FROM tabela1 RIGHT OUTER JOIN tabela2 ON chave1 = chave2 WHERE chave1 IS NULL;
SELECT atributo FROM tabela1 FULL OUTER JOIN tabela2 ON chave1 = chave2 WHERE chave1 IS NULL OR chave2 IS NULL;
```
