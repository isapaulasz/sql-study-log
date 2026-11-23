# Seção 2 - Configuração do ambiente de trabalho

## Aula 3 - Configuração da máquina

- Baixar e instalar o pgAdmin: software gráfico para administração do PostgreSQL
  - Link: https://www.pgadmin.org/download/
- Baixar e instalar o PostgreSQL: sistema gerenciador de banco de dados (SGBD) que possibilita a consulta aos dados com o uso da linguagem SQL
  - Link: https://www.postgresql.org/download/
- Configurar o banco de dados: vamos rodar um script que cria todas as tabelas que serão utilizadas durante as aulas do curso

## Aula 4 - Overview do pgAdmin

- O pgAdmin já vem com um servidor configurado e um schema criado (`public`)
- Query Tool: na aba Query Editor é onde podemos escrever e executar as consultas

## Aula 5 - Configuração do banco de dados

- O curso irá trabalhar com um banco de dados fictício que simula problemas de um e-commerce de vendas
- O e-commerce se assemelha a lojas como a Magazine Luiza, com um site onde qualquer pessoa pode cadastrar produtos, porém direcionado somente para a venda de automóveis
- O projeto terá 2 schemas: `sales` e `temp_tables`
- `sales`: possui as tabelas `funnel`, `customers`, `stores` e `products`
  - `funnel`: tabela principal, mostra qual foi a ação tomada por cada cliente para cada visita ao site da empresa
  - `customers`: dados dos clientes
  - `products`: dados dos veículos
  - `stores`: dados das lojas
- `temp_tables`: possui tabelas auxiliares, usadas em exemplos e desafios (`ibge_genders`, `regions`, `duplicados`, `tabela_1`, `tabela_2` e `products_2`)
- Iniciamos criando os schemas `sales` e `temp_tables` no pgAdmin:

```sql
CREATE SCHEMA IF NOT EXISTS sales;
CREATE SCHEMA IF NOT EXISTS temp_tables;
```

- Criando as tabelas dos schemas:

> OBS: a query original do curso inclui mais de 100 mil linhas de produtos, que não serão incluídas nas anotações, somente os comandos principais

```sql
CREATE TABLE sales.funnel (
    visit_id VARCHAR PRIMARY KEY,
    customer_id VARCHAR NOT NULL,
    store_id VARCHAR NOT NULL,
    product_id VARCHAR NOT NULL,
    visit_page_date DATE,
    add_to_cart_date DATE,
    start_checkout_date DATE,
    finish_checkout_date DATE,
    paid_date DATE,
    discount NUMERIC
);

CREATE TABLE sales.customers (
    customer_id VARCHAR PRIMARY KEY,
    cpf VARCHAR NOT NULL,
    first_name VARCHAR NOT NULL,
    last_name VARCHAR NOT NULL,
    state VARCHAR NOT NULL,
    city VARCHAR NOT NULL,
    birth_date DATE NOT NULL,
    income NUMERIC NOT NULL,
    score NUMERIC NOT NULL,
    professional_status VARCHAR NOT NULL,
    email VARCHAR NOT NULL,
    mobile VARCHAR NOT NULL
);

CREATE TABLE sales.products (
    product_id VARCHAR PRIMARY KEY,
    brand VARCHAR NOT NULL,
    model VARCHAR NOT NULL,
    model_year VARCHAR NOT NULL,
    price NUMERIC NOT NULL
);

CREATE TABLE sales.stores (
    store_name VARCHAR NOT NULL,
    store_cnpj VARCHAR NOT NULL,
    store_id VARCHAR PRIMARY KEY
);

CREATE TABLE temp_tables.ibge_genders (
    first_name VARCHAR PRIMARY KEY,
    gender VARCHAR NOT NULL
);

CREATE TABLE temp_tables.regions (
    city VARCHAR NOT NULL,
    state VARCHAR NOT NULL,
    region VARCHAR NOT NULL,
    size VARCHAR NOT NULL,
    code VARCHAR NOT NULL,
    population NUMERIC
);

CREATE TABLE temp_tables.duplicados (
    cpf VARCHAR NOT NULL,
    name VARCHAR NOT NULL,
    state VARCHAR NOT NULL
);

CREATE TABLE temp_tables.tabela_1 (
    cpf VARCHAR NOT NULL,
    name VARCHAR NOT NULL
);

CREATE TABLE temp_tables.tabela_2 (
    cpf VARCHAR NOT NULL,
    state VARCHAR NOT NULL
);

CREATE TABLE temp_tables.products_2 (
    product_id VARCHAR PRIMARY KEY,
    brand VARCHAR NOT NULL,
    model VARCHAR NOT NULL,
    model_year VARCHAR NOT NULL,
    price NUMERIC NOT NULL
);
```
