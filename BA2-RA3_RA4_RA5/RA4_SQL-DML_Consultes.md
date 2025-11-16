# Continguts

- [Instrucció SQL SELECT](#instrucció-SQL-SELECT)
- [Instrucció SQL WHERE](#instrucció-SQL-WHERE)
- [Instrucció SQL ORDER BY](#instrucció-SQL-ORDER-BY)
- [SQL DATE FUNCTIONS](#SQL-DATE-FUNCTIONS)
- [SQL AGGREGATE FUNCTIONS](#SQL-AGGREGATE-FUNCTIONS)
- [SQL JOINS](#SQL-JOINS)

<br>

## Instrucció SQL SELECT

L'instrucció **SELECT** és la comanda més utilitzada en el llenguatge SQL (Structured Query Language). Serveix per accedir als registres d'una o més taules o vistes d'una base de dades. També permet recuperar dades específiques que compleixin les condicions definides.

Mitjançant aquesta comanda, també podem accedir a un registre concret d'una columna específica d'una taula. La taula que emmagatzema els registres retornats per la instrucció **SELECT** s'anomena **taula de resultats**.

### Sintaxi de l'instrucció SELECT en SQL

```sql
SELECT Nom_Columna_1, Nom_Columna_2, ..., Nom_Columna_N FROM Nom_Taula;
```

En aquesta sintaxi:
- **Nom_Columna_1**, **Nom_Columna_2**, ..., **Nom_Columna_N** són els noms de les columnes de la taula de les quals volem llegir les dades.
- **Nom_Taula** és el nom de la taula on es troben les dades.

Si voleu accedir a totes les files i columnes d'una taula, podeu utilitzar la següent sintaxi amb l'asterisc `*`:

```sql
SELECT * FROM Nom_Taula;
```

Aquesta instrucció retornarà totes les dades disponibles de totes les columnes de la taula especificada.

#### Exemple de l'ús de SELECT

Imagina que tenim una taula amb 6 columnes i 7 registres en total. La taula s'anomena `Student_Records`. 

Si nosaltres executem el següent codi: 

```sql
SELECT * FROM Student_Records;
```

Obtindrem **tots** els registres d'aquesta taula: 

| **Student_ID** | **First_Name** | **Address**   | **Age** | **Percentage** | **Grade** |
|-----------------|---------------|---------------|---------|----------------|-----------|
| 201             | Akash         | Delhi         | 18      | 89             | A2        |
| 202             | Bhavesh       | Kanpur        | 19      | 93             | A1        |
| 203             | Yash          | Delhi         | 20      | 89             | A2        |
| 204             | Bhavna        | Delhi         | 19      | 78             | B1        |
| 205             | Yatin         | Lucknow       | 20      | 75             | B1        |
| 206             | Ishika        | Ghaziabad     | 19      | 91             | C1        |
| 207             | Vivek         | Goa           | 20      | 80             | B2        |

D'altra banda, si nosaltres especifiquem unes columnes en particular, només obtindrem les columnes especificades:  

```sql
SELECT Student_Id, Age, Percentage, Grade FROM Employee;
```

Obtindrem:

| **Student_ID**  | **Age** | **Percentage** | **Grade** |
|-----------------|---------|----------------|-----------|
| 201             | 18      | 89             | A2        |
| 202             | 19      | 93             | A1        |
| 203             | 20      | 89             | A2        |
| 204             | 19      | 78             | B1        |
| 205             | 20      | 75             | B1        |
| 206             | 19      | 91             | C1        |
| 207             | 20      | 80             | B2        |

### Sintaxi de la instrucció SELECT utilitzant la clàusula DISTINCT

La instrucció **SQL DISTINCT** s'utilitza amb la paraula clau **SELECT** per recuperar només dades úniques o distintes.

En una taula, pot existir la possibilitat que hi hagi valors duplicats, i en alguns casos volem recuperar només els valors únics. Per a aquests escenaris, s'utilitza la instrucció **SQL SELECT DISTINCT**.

```sql
SELECT DISTINCT column_name1 ,column_name2  
FROM  table_name;
```

#### Exemple de l'ús de SELECT utilitzant la clàusula DISTINCT

Imaginem que tenim la següent taula de dades: 

```sql
SELECT * FROM Student_Information;
```

| **Student_Name**    | **Gender** | **Mobile_Number** | **HOME_TOWN** |
|----------------------|------------|--------------------|---------------|
| Rahul Ojha          | Male       | 7503896532         | Lucknow       |
| Disha Rai           | Female     | 9270568893         | Varanasi      |
| Sonoo Jaiswal       | Male       | 9990449935         | Lucknow       |


Volem extreure la informació referida a les diferents poblacions de les quals hi ha estudiants: 

```sql
SELECT DISTINCT home_town  
FROM students;
```

Obtindrem:

| **HOME_TOWN** |
|---------------|
| Lucknow       |
| Varanasi      |

<br>

## Instrucció SQL WHERE

La clàusula **WHERE** en SQL és una instrucció del llenguatge de manipulació de dades (**DML**, Data Manipulation Language).

Tot i que no és obligatori incloure una clàusula **WHERE** en les instruccions DML, s’utilitza sovint per limitar el nombre de files afectades per una instrucció SQL (com **UPDATE** o **DELETE**) o retornades per una consulta (**SELECT**).

La clàusula **WHERE** serveix per aplicar condicions específiques als registres. Només retorna o afecta aquells registres que compleixen les condicions especificades.

La clàusula **WHERE** es pot utilitzar amb les instruccions **SELECT**, **UPDATE**, **DELETE**, entre d’altres.

### Sintaxi de l'instrucció WHERE en SQL

```sql
SELECT column1, column2, ... columnN  
FROM table_name  
WHERE [conditions]
```

La clàusula WHERE utilitza una selecció de condicions: 

| Operador | Significat                      |
|----------|---------------------------------|
| =        | Igual                          |
| >        | Major que                      |
| <        | Menor que                      |
| >=       | Major o igual que              |
| <=       | Menor o igual que              |
| <>       | Diferent de (no igual a)       |

A més, per combinar l'ús de diferents condicions s'han d'utilitzar els operadors lógics `AND` i `OR`.

```sql
SELECT column1, column2, ... columnN  
FROM table_name  
WHERE [condition1] AND [condition2] AND [condition3] AND ... [conditionN]
```

O una altra opció:

```sql
SELECT column1, column2, ... columnN  
FROM table_name  
WHERE ([condition1] AND [condition2]) OR [condition3]...
```

Explorem aquest tema amb més detall mitjançant exemples. Suposem que tenim una taula anomenada `customers` amb els següents registres:

| ID  | NAME               | AGE | ADDRESS      | SALARY |
|-----|--------------------|-----|--------------|--------|
| 1   | Himani Gupta       | 21  | Modinagar    | 22000  |
| 2   | Shiva Tiwari       | 22  | Bhopal       | 21000  |
| 3   | Ajeet Bhargav      | 45  | Meerut       | 65000  |
| 4   | Ritesh Yadav       | 36  | Azamgarh     | 26000  |
| 5   | Balwant Singh      | 45  | Varanasi     | 36000  |
| 6   | Mahesh Sharma      | 26  | Mathura      | 22000  |
| 7   | Rohit Shrivastav   | 19  | Ahemdabad    | 38000  |
| 8   | Neeru Sharma       | 29  | Pune         | 40000  |
| 9   | Aakash Yadav       | 32  | Mumbai       | 43500  |
| 10  | Sahil Sheikh       | 35  | Aurangabad   | 68800  |

#### Exemple 1

Escriviu una consulta per filtrar els clients que tenen menys de 40 anys i que tenen un salari superior o igual a 38000: 

```sql
SELECT *
FROM customers  
WHERE Age < 40 AND Salary >= 38000
```

Obtindriem els següents resultats: 

| ID  | NAME               | AGE | ADDRESS      | SALARY |
|-----|--------------------|-----|--------------|--------|
| 7   | Rohit Shrivastav   | 19  | Ahemdabad    | 38000  |
| 8   | Neeru Sharma       | 29  | Pune         | 40000  |
| 9   | Aakash Yadav       | 32  | Mumbai       | 43500  |
| 10  | Sahil Sheikh       | 35  | Aurangabad   | 68800  |


#### Exemple 2

Utilitzem la condició lògica `OR`.

Escriviu una consulta per filtrar els clients que o bé el seu nom comença per "A" o bé tenen un salari inferior o igual a 22000.

> [!NOTE]  
> Per filtrar els noms que comencen per "A" hem d'utilitzar la clàusula **LIKE**. La clàusula **LIKE** en SQL s'utilitza per cercar patrons específics en columnes de text (no és sensible a majúscules i minúscules en la majoria de SGBDs). El símbol **%** substitueix qualsevol nombre de caràcters (incloent cap) dins del patró. Per exemple:
> - `WHERE name LIKE 'A%'` retorna noms que comencen amb "A".
> - `WHERE name LIKE '%sh%'` retorna noms que contenen "sh".
> - `WHERE name LIKE '%A'` retorna noms que acaba amb "A".

Per tant, la consulta quedaria tal que: 

```sql
SELECT *
FROM customers  
WHERE Name LIKE 'A%' OR Salary <= 22000
```

| ID  | NAME               | AGE | ADDRESS      | SALARY |
|-----|--------------------|-----|--------------|--------|
| 1   | Himani Gupta       | 21  | Modinagar    | 22000  |
| 3   | Ajeet Bhargav      | 45  | Meerut       | 65000  |
| 6   | Mahesh Sharma      | 26  | Mathura      | 22000  |
| 9   | Aakash Yadav       | 32  | Mumbai       | 43500  |

#### Exemple 3

Escriviu una consula per filtrar eles clients que viuen a Mumbai i Modinagar.

Una possible solució seria: 

```sql
SELECT *
FROM customers  
WHERE Address = 'Mumbai' OR Salary Address = 'Modinagar'
```

Però per no repetir dues vegades `Address`, podem utilitzar la clàusula `IN`. 

> [!TIP]
> La clàusula `IN` en SQL s'utilitza per especificar una llista de valors i verificar si una columna conté algun d’aquests valors. És una alternativa més compacta i llegible a múltiples condicions amb `OR`.

```sql
SELECT *
FROM customers  
WHERE Address IN ('Mumbai', 'Modinagar')
```

Obtenint d'aquesta manera els següents resultats: 

| ID  | NAME               | AGE | ADDRESS      | SALARY |
|-----|--------------------|-----|--------------|--------|
| 1   | Himani Gupta       | 21  | Modinagar    | 22000  |
| 9   | Aakash Yadav       | 32  | Mumbai       | 43500  |

<br>

## Instrucció SQL ORDER BY

Quan volem ordenar els registres basant-nos en les columnes emmagatzemades a les taules d’una base de dades SQL, fem servir la clàusula **ORDER BY**.

La clàusula **ORDER BY** en SQL ens permet ordenar els registres segons una columna específica d’una taula. Això vol dir que tots els valors emmagatzemats en la columna sobre la qual apliquem la clàusula ORDER BY seran ordenats, i els valors corresponents de les altres columnes es mostraran seguint la seqüència definida en aquest procés.

Amb la clàusula **ORDER BY**, podem ordenar els registres en ordre ascendent o descendent, segons les nostres necessitats.

- Si utilitzem la paraula clau **ASC**, els registres s’ordenaran en **ordre ascendent**.
- Si utilitzem la paraula clau **DESC**, s’ordenaran en **ordre descendent**.

> [!IMPORTANT] 
> Si no especifiquem cap paraula clau després de la columna sobre la qual volem ordenar els registres, l’ordre per defecte serà ascendent.

Abans d’escriure les consultes per ordenar els registres, vegem la **sintaxi**.

### Sintaxi de l'instrucció SELECT en ORDER BY

Sintaxi per ordenar els registres en ordre ascendent:

```sql
SELECT ColumnName1,...,ColumnNameN
FROM TableName
ORDER BY ColumnName1 ASC;
```

Sintaxi per ordenar els registres en ordre descendent:

```sql
SELECT ColumnName1,...,ColumnNameN
FROM TableName
ORDER BY ColumnName1 DESC;
```

Sintaxi per ordenar els registres en ordre ascendent sense utilitzar la paraula clau ASC:

```sql
SELECT ColumnName1,...,ColumnNameN
FROM TableName
ORDER BY ColumnName1; 
```

Explorem aquest tema amb més detall mitjançant exemples. Suposem que tenim una taula anomenada `customers` amb els següents registres:

| ID  | NAME               | AGE | ADDRESS      | SALARY |
|-----|--------------------|-----|--------------|--------|
| 1   | Himani Gupta       | 21  | Modinagar    | 22000  |
| 2   | Shiva Tiwari       | 22  | Bhopal       | 21000  |
| 3   | Ajeet Bhargav      | 45  | Meerut       | 65000  |
| 4   | Ritesh Yadav       | 36  | Azamgarh     | 26000  |
| 5   | Balwant Singh      | 45  | Varanasi     | 36000  |
| 6   | Mahesh Sharma      | 26  | Mathura      | 22000  |
| 7   | Rohit Shrivastav   | 19  | Ahemdabad    | 38000  |
| 8   | Neeru Sharma       | 29  | Pune         | 40000  |
| 9   | Aakash Yadav       | 32  | Mumbai       | 43500  |
| 10  | Sahil Sheikh       | 35  | Aurangabad   | 68800  |

#### Exemple 1: 

Escriviu una consulta per ordenar els registres en **ordre ascendent** dels noms de clients emmagatzemats a la taula `customers`.

```sql
SELECT * FROM customers ORDER BY Name ASC;  
```

S'aplica una clàusula `ORDER BY` a la columna `Name` per ordenar els registres. La paraula clau `ASC` ordenarà els registres en ordre ascendent. 

> [!IMPORTANT]  
> En el cas de ser una columna de tipus `varchar()`, els registres s'ordenen per ordre alfabètic.
> 
> També funcionaria:
> ```sql
> SELECT *
> FROM customers
> ORDER BY Name;
> ```

Obtindreu la següent sortida:

| ID  | NAME               | AGE | ADDRESS      | SALARY |
|-----|--------------------|-----|--------------|--------|
| 9   | Aakash Yadav       | 32  | Mumbai       | 43500  |
| 3   | Ajeet Bhargav      | 45  | Meerut       | 65000  |
| 5   | Balwant Singh      | 45  | Varanasi     | 36000  |
| 1   | Himani Gupta       | 21  | Modinagar    | 22000  |
| 6   | Mahesh Sharma      | 26  | Mathura      | 22000  |
| 8   | Neeru Sharma       | 29  | Pune         | 40000  |
| 4   | Ritesh Yadav       | 36  | Azamgarh     | 26000  |
| 7   | Rohit Shrivastav   | 19  | Ahemdabad    | 38000  |
| 10  | Sahil Sheikh       | 35  | Aurangabad   | 68800  |
| 2   | Shiva Tiwari       | 22  | Bhopal       | 21000  |

#### Exemple 2: 

Escriu una consulta per ordenar els registres en ordre descendent de les adreces emmagatzemades a la taula `customers`.

```sql
SELECT *
FROM customers
ORDER BY Address DESC;  
```

S'aplica una clàusula `ORDER BY` a la columna `Address` per ordenar els registres. La paraula clau `DESC` ordenarà els registres en ordre descendent. 

Obtenint la següent sortida: 

| ID  | NAME               | AGE | ADDRESS      | SALARY |
|-----|--------------------|-----|--------------|--------|
| 7   | Rohit Shrivastav   | 19  | Ahemdabad    | 38000  |
| 10  | Sahil Sheikh       | 35  | Aurangabad   | 68800  |
| 4   | Ritesh Yadav       | 36  | Azamgarh     | 26000  |
| 2   | Shiva Tiwari       | 22  | Bhopal       | 21000  |
| 6   | Mahesh Sharma      | 26  | Mathura      | 22000  |
| 3   | Ajeet Bhargav      | 45  | Meerut       | 65000  |
| 1   | Himani Gupta       | 21  | Modinagar    | 22000  |
| 9   | Aakash Yadav       | 32  | Mumbai       | 43500  |
| 8   | Neeru Sharma       | 29  | Pune         | 40000  |
| 5   | Balwant Singh      | 45  | Varanasi     | 36000  |

#### Exemple 3: 

Escriviu una consulta per ordenar els registres en ordre descendent del salari del client emmagatzemat a la taula `customers`.

```sql
SELECT *
FROM customers
ORDER BY Salary DESC;   
```

S'aplica una clàusula `ORDER BY` a la columna `Salary` per ordenar els registres. La paraula clau `DESC` ordenarà els registres en ordre descendent. 

Obtenint la següent sortida: 

| ID  | NAME               | AGE | ADDRESS      | SALARY |
|-----|--------------------|-----|--------------|--------|
| 10  | Sahil Sheikh       | 35  | Aurangabad   | 68800  |
| 3   | Ajeet Bhargav      | 45  | Meerut       | 65000  |
| 9   | Aakash Yadav       | 32  | Mumbai       | 43500  |
| 8   | Neeru Sharma       | 29  | Pune         | 40000  |
| 7   | Rohit Shrivastav   | 19  | Ahemdabad    | 38000  |
| 5   | Balwant Singh      | 45  | Varanasi     | 36000  |
| 4   | Ritesh Yadav       | 36  | Azamgarh     | 26000  |
| 6   | Mahesh Sharma      | 26  | Mathura      | 22000  |
| 1   | Himani Gupta       | 21  | Modinagar    | 22000  |
| 2   | Shiva Tiwari       | 22  | Bhopal       | 21000  |

#### Exemple 4: 
Escriviu una consulta per ordenar els registres en ordre descendent del salari i l'edat del client emmagatzemat a la taula `customers`.

```sql
SELECT *
FROM customers
ORDER BY SALARY ASC, AGE ASC; 
```

- Els registres es classifiquen primer pel `Salary` en ordre ascendent.
- Si hi ha registres amb el mateix `Salary`, s'utilitza el segon camp `Age` per determinar l'ordre, també en ascendent.

Com a resultat obtenim: 

| ID  | NAME               | AGE | ADDRESS      | SALARY |
|-----|--------------------|-----|--------------|--------|
| 2   | Shiva Tiwari       | 22  | Bhopal       | 21000  |
| 1   | Himani Gupta       | 21  | Modinagar    | 22000  |
| 6   | Mahesh Sharma      | 26  | Mathura      | 22000  |
| 4   | Ritesh Yadav       | 36  | Azamgarh     | 26000  |
| 5   | Balwant Singh      | 45  | Varanasi     | 36000  |
| 7   | Rohit Shrivastav   | 19  | Ahemdabad    | 38000  |
| 8   | Neeru Sharma       | 29  | Pune         | 40000  |
| 9   | Aakash Yadav       | 32  | Mumbai       | 43500  |
| 3   | Ajeet Bhargav      | 45  | Meerut       | 65000  |
| 10  | Sahil Sheikh       | 35  | Aurangabad   | 68800  |

<br>

## SQL DATE FUNCTIONS

Les funcions de temps en SQL permeten treballar amb dates i hores. A continuació tenim alguns exemples de funcions de temps bastant utilitzades, tingues en compte, que existeixen moltes funcions de data i temps més.

### CURRENT_DATE()

Retorna la data actual del sistema (sense l'hora). 

```sql
SELECT CURRENT_DATE();
```

**Format de sortida**: `YYYY-MM-DD`

### NOW()

Retorna la data i hora actuals del sistema.

```sql
SELECT NOW();
```

**Format de sortida**: `YYYY-MM-DD HH:MM:SS`

### DATE_ADD()

Afegeix un interval de temps a una data determinada.

**Sintaxi:**

```sql
DATE_ADD(data, INTERVAL valor tipus)
```

**Exemple:** Afegeix 10 dies a la data actual.

```sql
SELECT DATE_ADD(CURRENT_DATE(), INTERVAL 10 DAY);
```

**Resultat:** `2025-01-15` (tenint en compte que estem a dia 2025-01-05)

### DATE_SUB()

Resta un interval de temps d'una data determinada.

**Sintaxi:**

```sql
DATE_SUB(data, INTERVAL valor tipus)
```

**Exemple:** Resta 2 mesos de la data actual.

```sql
SELECT DATE_SUB(CURRENT_DATE(), INTERVAL 2 MONTH);
```

**Resultat:** `2024-11-05` (tenint en compte que estem a dia 2025-01-05)

### Altres funcions de data

- `DAY()`: Retorna el número del dia del mes de la data especificada.
- `WEEK()`: Retorna el número de setmana de l'any de la data especificada.
- `MONTH()`: Retorna el número del mes (1-12) de la data especificada.
- `YEAR()`: Retorna l'any de la data especificada.

**Exemples:**

**Filtrar dates més grans o més petites que una data concreta**

Podem fer servir les funcions `YEAR()`, `MONTH()` i `DAY()` quan necessitem comparar només una part de la data.

**Totes les dates posteriors a l'any 2023:**

```sql
SELECT *
FROM taula
WHERE YEAR(data) > 2023;
```

**Totes les dates anteriors al mes de juny independentment de l’any:**

```sql
SELECT *
FROM taula
WHERE MONTH(data) < 6;
```

**Registres on el dia del mes sigui superior a 15:**

```sql
SELECT *
FROM taula
WHERE DAY(data) > 15;
```

**Registres entre 2024 i 2025:**

```sql
SELECT *
FROM taula
WHERE YEAR(data) BETWEEN 2024 AND 2025;
```

**Registres del primer trimestre (gener–març) de qualsevol any:**

```sql
SELECT *
FROM taula
WHERE MONTH(data) BETWEEN 1 AND 3;
```

**Registres entre els dies 10 i 20 de qualsevol mes:**

```sql
SELECT *
FROM taula
WHERE DAY(data) BETWEEN 10 AND 20;
```

<br>

## SQL AGGREGATE FUNCTIONS

Les funcions agregades són funcions que processen grups de files per retornar un únic valor. 

- `COUNT()`: Retorna el nombre de files.
- `SUM()`: Retorna la suma dels valors d'una columna.
- `AVG()`: Retorna la mitjana dels valors d'una columna.
- `MAX()`: Retorna el valor màxim d'una columna.
- `MIN()`: Retorna el valor mínim d'una columna.

> [!IMPORTANT]  
> Es necessari utilitzar `GROUP BY` en les següents situacions:
> - Comptar registres per grup.
> - Filtrar resultats agrupats amb `HAVING`.
> - Trobar màxim o mínim per grup.

**Exemples d'ús de la clàusula `GROUP BY`**

Taula `sales`:

| ID  | CATEGORY    | AMOUNT |
|-----|-------------|--------|
| 1   | Electronics | 1000   |
| 2   | Furniture   | 500    |
| 3   | Electronics | 700    |
| 4   | Furniture   | 400    |
| 5   | Groceries   | 300    |
| 6   | Electronics | 1200   |

Si utilitzem una funció agregada sense `GROUP BY`, s'aplicarà a totes les files: 

```sql
SELECT SUM(AMOUNT) AS Total_Amount
FROM sales;
```

Resultat: 

| Total_Amount |
|--------------|
| 4100         |

Amb `GROUP BY`:

Si volem sumar `Amount` per categoria: 

```sql
SELECT CATEGORY, SUM(AMOUNT) AS Total_Amount
FROM sales
GROUP BY CATEGORY;
```

Resultat: 

| CATEGORY    | Total_Amount |
|-------------|--------------|
| Electronics | 2900         |
| Furniture   | 900          |
| Groceries   | 300          |

Aquí, `GROUP BY` agrupa les files per `Category`, i la funció agregada `SUM()` calcula la suma per a cada grup.

Volem comptar els productes que hi ha a la taula `products` de cada categoria: 

```sql
SELECT CATEGORY, COUNT(*) AS Product_Count
FROM sales
GROUP BY CATEGORY;
```

Volem trobar categories on la suma total és superior a 100: 

```sql
SELECT CATEGORY, SUM(AMOUNT) AS Total_Amount
FROM sales
GROUP BY CATEGORY
HAVING SUM(AMOUNT) > 1000;
```

Volem trobar la categoria amb la quantitat més alta de vendes: 

```sql
SELECT CATEGORY, MAX(AMOUNT) AS Max_Sale
FROM sales
GROUP BY CATEGORY;
```

**Exemples d'ús de la clàusula `HAVING`**

La clàusula `HAVING` filtra els resultats després de l'agrupació.

Imagina que, seguint amb la taula `sales` volem mostrar les categories amb una suma total superior a 500. 

```sql
SELECT CATEGORY, SUM(AMOUNT) AS Total_Amount
FROM sales
GROUP BY CATEGORY
HAVING SUM(AMOUNT) > 500;
```

Obtindrem com a resultat: 

| CATEGORY    | Total_Amount |
|-------------|--------------|
| Electronics | 2900         |
| Furniture   | 900          |

> [!IMPORTANT]  
> `GROUP BY` s'ha de especificar sempre **abans** de `HAVING`.

<br>

## SQL JOINS

Els **JOINS** en SQL s’utilitzen per combinar dades de dues o més taules basant-se en una condició comuna entre elles. 

Es poden definir un total de cinc tipus diferents de JOINS: 
- `INNER JOIN`
- `LEFT JOIN`
- `RIGHT JOIN`
- `FULL OUTER JOIN`
- `CROSS JOIN`

> [!NOTE]  
> Tingueu en compte que `LEFT OUTER JOIN` es lo mateix que `LEFT JOIN`, `RIGHT OUTER JOIN` és lo mateix que `RIGHT JOIN` i `INNER JOIN` és lo mateix que `JOIN`.

Imaginem que tenim dues taules: 

Taula `customers`:

| ID  | NAME           | CITY       |
|-----|----------------|------------|
| 1   | Himani Gupta   | Modinagar  |
| 2   | Shiva Tiwari   | Bhopal     |
| 3   | Ajeet Bhargav  | Meerut     |

Taula `orders`:

| ORDER_ID | CUSTOMER_ID | PRODUCT      |
|----------|-------------|--------------|
| 101      | 1           | Laptop       |
| 102      | 3           | Smartphone   |
| 103      | 4           | Tablet       |

---

### INNER JOIN (o JOIN)

Només retorna les files que compleixen la condició en ambdues taules. Es pot expressar a través d'un diagrama de Venn: 

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/Bases-de-Dades/blob/main/images/innerjoin.png" alt="INNER JOIN" width="260" height="auto"/>
  </div>

Utilitzant les dues taules anteriors i la següent sentencia SQL: 

```sql
SELECT customers.NAME, orders.PRODUCT
FROM customers
INNER JOIN orders
ON customers.ID = orders.CUSTOMER_ID;
```

Obtindriem el següent resultat: 

| NAME           | PRODUCT      |
|----------------|--------------|
| Himani Gupta   | Laptop       |
| Ajeet Bhargav  | Smartphone   |

### LEFT JOIN (o LEFT OUTER JOIN)

Retorna totes les files de la taula esquerra i les que coincideixen de la taula dreta. Si no hi ha coincidència, mostra `NULL` per les columnes de la taula dreta. Es pot expressar a través d'un diagrama de Venn: 

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/Bases-de-Dades/blob/main/images/leftjoin.png" alt="LEFT JOIN" width="260" height="auto"/>
  </div>

Utilitzant les dues taules anteriors i la següent sentencia SQL: 

```sql
SELECT customers.NAME, orders.PRODUCT
FROM customers
LEFT JOIN orders
ON customers.ID = orders.CUSTOMER_ID;
```

Obtindriem el següent resultat: 

| NAME           | PRODUCT      |
|----------------|--------------|
| Himani Gupta   | Laptop       |
| Shiva Tiwari   | NULL         |
| Ajeet Bhargav  | Smartphone   |

### RIGHT JOIN (o RIGHT OUTER JOIN)

És similar al **LEFT JOIN**, però retorna totes les files de la taula dreta i només les que coincideixen de la taula esquerra. Es pot expressar a través d'un diagrama de Venn: 

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/Bases-de-Dades/blob/main/images/rightjoin.png" alt="RIGHT JOIN" width="260" height="auto"/>
  </div>

Utilitzant les dues taules anteriors i la següent sentencia SQL: 

```sql
SELECT customers.NAME, orders.PRODUCT
FROM customers
RIGHT JOIN orders
ON customers.ID = orders.CUSTOMER_ID;
```

Obtindriem el següent resultat:

| NAME           | PRODUCT      |
|----------------|--------------|
| Himani Gupta   | Laptop       |
| Ajeet Bhargav  | Smartphone   |
| NULL           | Tablet       |

### FULL OUTER JOIN

Retorna totes les files de les dues taules, i si no coincideixen, omple amb `NULL`. Es pot expressar a través d'un diagrama de Venn: 

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/Bases-de-Dades/blob/main/images/fullouterjoin.png" alt="FULL OUTER JOIN" width="260" height="auto"/>
  </div>

Utilitzant les dues taules anteriors i la següent sentencia SQL: 

```sql
SELECT customers.NAME, orders.PRODUCT
FROM customers
FULL OUTER JOIN orders
ON customers.ID = orders.CUSTOMER_ID;
```

Obtindriem el següent resultat:

| NAME           | PRODUCT      |
|----------------|--------------|
| Himani Gupta   | Laptop       |
| Shiva Tiwari   | NULL         |
| Ajeet Bhargav  | Smartphone   |
| NULL           | Tablet       |

### CROSS JOIN

Combina totes les files de la primera taula amb totes les de la segona, creant el producte cartesià. Es pot expressar a través d'un diagrama de Venn: 

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/Bases-de-Dades/blob/main/images/crossjoin.png" alt="CROSS JOIN" width="260" height="auto"/>
  </div>

Utilitzant les dues taules anteriors i la següent sentencia SQL: 

```sql
SELECT customers.NAME, orders.PRODUCT
FROM customers
CROSS JOIN orders;
```

Obtindriem el següent resultat:

| NAME           | PRODUCT      |
|----------------|--------------|
| Himani Gupta   | Laptop       |
| Himani Gupta   | Smartphone   |
| Himani Gupta   | Tablet       |
| Shiva Tiwari   | Laptop       |
| Shiva Tiwari   | Smartphone   |
| Shiva Tiwari   | Tablet       |
| Ajeet Bhargav  | Laptop       |
| Ajeet Bhargav  | Smartphone   |
| Ajeet Bhargav  | Tablet       |
