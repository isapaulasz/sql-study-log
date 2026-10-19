# SQL Review: Simple SELECT Queries

No exercício abaixo, você trabalhará com uma tabela diferente. Esta tabela contém informações sobre algumas das cidades mais populosas da América do Norte[1], incluindo sua população e localização geoespacial no mundo.

- **Table**: north_american_cities
- **Columns**: city; country; population; latitude; longitude

<details>
<summary><b> 1. Liste todas as cidades canadenses e suas populações</b></summary>
SELECT * FROM north_american_cities 
WHERE country = "Canada";
</details>

<details>
<summary><b> 2. Ordene todas as cidades dos Estados Unidos pela sua latitude de norte a sul</b></summary>
SELECT * FROM north_american_cities 
WHERE country = "United States" 
ORDER BY latitude DESC;
</details>

<details>
<summary><b> 3. Liste todas as cidades a oeste de Chicago, ordenadas de oeste para leste</b></summary>
SELECT city, longitude FROM north_american_cities 
WHERE longitude < -87.629798 
ORDER BY longitude ASC;
</details>

<details>
<summary><b> 4. Liste as duas maiores cidades do México (por população)</b></summary>
SELECT * FROM north_american_cities 
WHERE country = "Mexico" 
ORDER BY population DESC 
LIMIT 2;
</details>

<details>
<summary><b> 5. Liste a terceira e quarta maiores cidades (por população) dos Estados Unidos e sua população</b></summary>
SELECT * FROM north_american_cities 
WHERE country = "United States" 
ORDER BY population DESC 
LIMIT 2 OFFSET 2;
</details>