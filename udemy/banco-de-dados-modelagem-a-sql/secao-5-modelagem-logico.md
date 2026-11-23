# Seção 5 - Modelagem Lógica

## Aula 38 - Introdução ao modelo lógico

- O modelo lógico oferece à equipe de desenvolvimento uma visão abstrata do banco
- O modelo lógico deve levar em consideração o banco de dados relacional utilizado
- Para o modelo lógico ser eficiente, além de estar normalizado, é necessário ter as chaves estrangeiras bem definidas

## Aula 39 - Chave estrangeira

- É um ou mais atributos de uma tabela que fazem referência a um ou mais atributos de outra tabela
- É importante para que o SGBDR garanta a integridade referencial
- O modelo lógico não possui tipos de relacionamento: o relacionamento é garantido pelas chaves estrangeiras
- A criação da chave estrangeira depende da cardinalidade existente entre as entidades do modelo conceitual:
  - 1:1: a chave estrangeira fica em uma das tabelas (com valor único)
  - 1:N: a chave estrangeira fica na tabela do lado N
  - N:N: é criada uma tabela associativa, com duas chaves estrangeiras (uma para cada tabela)

## Aula 43 - Tipos de dados mais comuns

- `CHAR(n)`: caracteres com tamanho fixo
- `VARCHAR(n)`: caracteres de tamanho variável
- `NUMBER(p,e)`: números decimais de ponto fixo, com precisão de `p` dígitos no total e escala de `e` dígitos após a vírgula
  - em outros bancos, como o PostgreSQL, o equivalente é `NUMERIC(p,e)` ou `DECIMAL(p,e)`
- `INTEGER`: números inteiros
- `DATE`: data
- `TIME`: hora
- `TIMESTAMP`: data e hora
