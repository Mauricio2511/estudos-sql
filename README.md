# Estudos SQL

Repositório dedicado aos meus estudos práticos em SQL avançado utilizando MySQL.

## Conteúdos estudados

* INNER JOIN
* LEFT JOIN
* RIGHT JOIN
* FULL JOIN
* Subqueries
* EXISTS
* CTE (Common Table Expressions)
* Window Functions
* Views
* Procedures
* Triggers

## Base de dados utilizada

* World Database (base de exemplo oficial do MySQL).

##

# INNER JOIN

## Objetivo 1

Listar o nome do país e a quantidade de cidades cadastradas,
considerando apenas países com mais de 50 cidades.

---

## Query

[Visualizar Query](inner-join/exercicio-01/query.sql)

## Resultado

![Resultado Exercício 1](inner-join/exercicio-01/resultado1.png)

---

## Objetivo 2

Listar:

* nome do país
* quantidade de cidades cadastradas
* média da população das cidades

Considerando apenas cidades com população maior que 300000.

Exibir apenas países com mais de 20 cidades cadastradas.

---

## Query

[Visualizar Query](inner-join/exercicio-02/query.sql)

## Resultado

![Resultado Exercício 2](inner-join/exercicio-02/resultado2.png)

---

## Objetivo 3

Listar:

* continente
* quantidade de países distintos
* soma da população das cidades

Considerando apenas cidades:

* com nome iniciado pela letra `S`
* e população maior que 200000.

Exibir apenas continentes com soma da população das cidades maior que 50000000.

---

## Query

[Visualizar Query](inner-join/exercicio-03/query.sql)

## Resultado

![Resultado Exercício 3](inner-join/exercicio-03/resultado3.png)

---


