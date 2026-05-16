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

<table>
<tr>
<td valign="top">

### Resultado

<img src="inner-join/exercicio-01/resultado1.png">

</td>

<td valign="top">

### [Query](inner-join/exercicio-01/query.sql)

```sql id="h2mf8x"
SELECT
    C.NAME AS PAÍS,
    COUNT(*) AS QUANTIDADE_CIDADES_CADASTRADAS
FROM CITY CI
INNER JOIN COUNTRY C
    ON CI.COUNTRYCODE = C.CODE
GROUP BY C.NAME
HAVING COUNT(*) > 50
ORDER BY QUANTIDADE_CIDADES_CADASTRADAS DESC;
```

</td>
</tr>
</table>

---

## Objetivo 2

Listar:

* nome do país
* quantidade de cidades cadastradas
* média da população das cidades

Considerando apenas cidades com população maior que 300000.

Exibir apenas países com mais de 20 cidades cadastradas.

---

<table>
<tr>
<td valign="top">

### Resultado

<img src="inner-join/exercicio-02/resultado2.png">

</td>

<td valign="top">

### [Query](inner-join/exercicio-02/query.sql)

```sql id="m7xq2n"
SELECT
    C.NAME AS PAÍS,
    COUNT(*) AS QTD_CIDADES,
    AVG(CI.POPULATION) AS MEDIA_POPULACAO
FROM CITY CI
INNER JOIN COUNTRY C
    ON CI.COUNTRYCODE = C.CODE
WHERE CI.POPULATION > 300000
GROUP BY C.NAME
HAVING COUNT(*) > 20
ORDER BY MEDIA_POPULACAO DESC;
```

</td>
</tr>
</table>

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

<table>
<tr>
<td valign="top">

### Resultado

<img src="inner-join/exercicio-03/resultado3.png">

</td>

<td valign="top">

### [Query](inner-join/exercicio-03/query.sql)

```sql id="q8v3mn"
SELECT
    C.CONTINENT AS CONTINENTE,
    COUNT(DISTINCT(C.NAME)) AS QTD_PAISES,
    SUM(CI.POPULATION) AS POPULACAO_TOTAL
FROM COUNTRY C
INNER JOIN CITY CI
    ON C.CODE = CI.COUNTRYCODE
WHERE CI.NAME LIKE 'S%'
    AND CI.POPULATION > 200000
GROUP BY C.CONTINENT
HAVING SUM(CI.POPULATION) > 50000000
ORDER BY POPULACAO_TOTAL DESC;
```

</td>
</tr>
</table>

---

# SUBQUERY

## Objetivo 1

Listar:

* nome do país
* população do país

Exibir apenas países com população maior que a média da população de todos os países.

Retornar apenas os 10 primeiros países nessas condições.

---

<table>
<tr>
<td valign="top">

### Resultado

<img src="subqueries/exercicio-01/resultado1.png">

</td>

<td valign="top">

### [Query](subqueries/exercicio-01/query.sql)

```sql id="z9qm2x"
SELECT
    NAME AS PAÍS,
    POPULATION AS POPULACAO
FROM COUNTRY
WHERE POPULATION > (
    SELECT
        AVG(POPULATION)
    FROM COUNTRY
)
ORDER BY POPULACAO DESC
LIMIT 10;
```

</td>
</tr>
</table>

---

## Objetivo 2

Listar:

* nome da cidade
* população da cidade
* nome do país

Exibir apenas cidades com população maior que a média da população de todas as cidades.

Retornar apenas cidades pertencentes a países do continente `Asia`.

Limitar o resultado aos 10 registros com maior população.

---

<table>
<tr>
<td valign="top">

### Resultado

<img src="subqueries/exercicio-02/resultado2.png">

</td>

<td valign="top">

### [Query](subqueries/exercicio-02/query.sql)

```sql id="n4pk8x"
SELECT
    CI.NAME AS CIDADE,
    CI.POPULATION AS POPULACAO,
    C.NAME AS PAIS,
    C.CONTINENT AS CONTINENTE
FROM CITY CI
INNER JOIN COUNTRY C
    ON CI.COUNTRYCODE = C.CODE
WHERE CI.POPULATION > (
    SELECT
        AVG(POPULATION)
    FROM CITY
)
AND C.CONTINENT = 'ASIA'
ORDER BY POPULACAO DESC
LIMIT 10;
```

</td>
</tr>
</table>

---

## Objetivo 3

Listar:

* nome do país
* continente
* população do país

Exibir apenas países que:

* possuem cidades com população maior que 8000000
* e possuem a letra `A` em qualquer posição do nome.

Retornar apenas os 10 países com maior população.

---

<table>
<tr>
<td valign="top">

### Resultado

<img src="subqueries/exercicio-03/resultado3.png">

</td>

<td valign="top">

### [Query](subqueries/exercicio-03/query.sql)

```sql id="k8vn4p"
SELECT
    NAME AS PAIS,
    CONTINENT AS CONTINENTE,
    POPULATION AS POPULACAO
FROM COUNTRY
WHERE CODE IN (
    SELECT
        COUNTRYCODE
    FROM CITY
    WHERE POPULATION > 8000000
)
AND NAME LIKE '%A%'
ORDER BY POPULACAO DESC
LIMIT 10;
```

</td>
</tr>
</table>

---

# PROCEDURE

## Objetivo 1

Criar uma procedure chamada `LISTAR_PAISES_POR_CONTINENTE`.

A procedure deve receber um parâmetro contendo o nome do continente.

Listar:

* nome do país
* continente
* população

Ordenar da maior população para a menor.

Obs: Como boa prática, antes e após a criação da procedure, realizo a alteração do `DELIMITER`.

---

<table>
<tr>
<td valign="top">

### Resultado

<img src="procedures/exercicio-01/resultado1.png">

</td>

<td valign="top">

### [Query](procedures/exercicio-01/query.sql)

```sql id="x7pk3m"
DELIMITER #

CREATE PROCEDURE LISTAR_PAISES_POR_CONTINENTE(IN P_CONTINENTE VARCHAR(30))
BEGIN

    SELECT
        NAME AS PAIS,
        CONTINENT AS CONTINENTE,
        POPULATION AS POPULACAO
    FROM COUNTRY
    WHERE CONTINENT = P_CONTINENTE
    ORDER BY POPULACAO DESC;

END #

DELIMITER ;

CALL LISTAR_PAISES_POR_CONTINENTE('SOUTH AMERICA');
```

</td>
</tr>
</table>

---

