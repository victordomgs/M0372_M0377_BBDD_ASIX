# Continguts

- [Instrucció SQL SELECT](#instrucció-SQL-SELECT)
- [Instrucció SQL WHERE](#instrucció-SQL-WHERE)
- [Instrucció SQL ORDER BY](#instrucció-SQL-ORDER-BY)

---

## Instrucció SQL SELECT

L'instrucció **SELECT** és la comanda més utilitzada en el llenguatge SQL (Structured Query Language). Serveix per accedir als registres d'una o més taules o vistes d'una base de dades. També permet recuperar dades específiques que compleixin les condicions definides.

Mitjançant aquesta comanda, també podem accedir a un registre concret d'una columna específica d'una taula. La taula que emmagatzema els registres retornats per la instrucció **SELECT** s'anomena **taula de resultats**.

---

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

---

### Sintaxi de l'instrucció SELECT utilitzant la clàusula WHERE

La clàusula **WHERE** s'utilitza amb la instrucció **SELECT** per retornar només aquelles files de la taula que compleixen la condició especificada a la consulta.

En SQL, la clàusula **WHERE** no només s'utilitza amb la instrucció **SELECT**, sinó també amb altres instruccions com **UPDATE**, **ALTER** i **DELETE**.

```sql
SELECT * FROM Nom_de_Taula WHERE [condició];
```
En aquesta sintaxi, la condició es defineix a la clàusula WHERE mitjançant operadors lògics o de comparació d'SQL.

#### Exemple de l'ús de SELECT utilitzant la clàusula WHERE 

Imaginem que tenim la següent taula de dades: 

```sql
SELECT * FROM Employee_Details;
```

| **Employee_Id** | **Emp_Name** | **Emp_City**  | **Emp_Salary** | **Emp_Panelty** |
|------------------|-------------|---------------|----------------|-----------------|
| 101              | Anuj        | Ghaziabad     | 25000          | 500             |
| 102              | Tushar      | Lucknow       | 29000          | 1000            |
| 103              | Vivek       | Kolkata       | 35000          | 500             |
| 104              | Shivam      | Goa           | 22000          | 500             |

La següent consulta mostra els registres dels empleats de la taula anterior que tenen una penalització (**Emp_Panelty**) igual a 500:

```sql
SELECT * FROM Employee_Details WHERE Emp_Panelty = 500;
```

| **Employee_Id** | **Emp_Name** | **Emp_City**  | **Emp_Salary** | **Emp_Panelty** |
|------------------|-------------|---------------|----------------|-----------------|
| 101              | Anuj        | Ghaziabad     | 25000          | 500             |
| 103              | Vivek       | Kolkata       | 35000          | 500             |
| 104              | Shivam      | Goa           | 22000          | 500             |

---

### Sintaxi de l'instrucció SELECT utilitzant la clàusula GROUP BY

La clàusula **GROUP BY** s'utilitza amb la instrucció **SELECT** per agrupar les dades comunes d'una columna d'una taula. És especialment útil quan es combinen amb funcions agregades com `SUM`, `AVG`, `COUNT`, etc., per obtenir resums o estadístiques de grups de dades.

```sql
SELECT Nom_Columna_1, Nom_Columna_2, ..., Nom_Columna_N, funció_agregada(Nom_Columna2) 
FROM Nom_Taula 
GROUP BY Nom_Columna1;
```

#### Exemple de l'ús de SELECT utilitzant la clàusula GROUP BY

Imaginem que tenim la següent taula de dades: 

```sql
SELECT * FROM Cars_Details; 
```

| **Car_Number** | **Car_Name** | **Car_Amount** | **Car_Price** |
|----------------|--------------|----------------|---------------|
| 2578          | Creta        | 3              | 1000000       |
| 9258          | Audi         | 2              | 900000        |
| 8233          | Venue        | 6              | 900000        |
| 6214          | Nexon        | 7              | 1000000       |

La següent consulta **SELECT** amb la clàusula **GROUP BY** llista el nombre de cotxes que tenen el mateix preu:

```sql
SELECT Car_Price , COUNT(Car_Name) 
FROM Cars_Details 
GROUP BY Car_Price;
```

| **Car_Price** | **COUNT(Car_Name)** |
|----------------|--------------|
| 1000000        | 2        | 
| 900000         | 2       |

---

### Sintaxi de l'instrucció SELECT utilitzant la clàusula HAVING

La clàusula **HAVING** s'utilitza en una instrucció **SELECT** per aplicar condicions sobre els grups creats per la clàusula **GROUP BY**. A diferència de la clàusula **WHERE**, que filtra registres abans d'agrupació, **HAVING** permet filtrar els grups resultants.

```sql
SELECT Nom_Columna_1, Nom_Columna_2, ..., Nom_Columna_N, funció_agregada(Nom_Columna_2) 
FROM Nom_Taula 
GROUP BY Nom_Columna_1 
HAVING [condició];
```

#### Exemple de l'ús de SELECT utilitzant la clàusula HAVING

Imaginem que tenim la següent taula de dades: 

```sql
SELECT * FROM Employee_Having;
```

| **Employee_Id** | **Employee_Name** | **Employee_Salary** | **Employee_City** |
|------------------|-------------------|----------------------|--------------------|
| 201              | Jone             | 20000               | Goa                |
| 202              | Basant           | 40000               | Delhi              |
| 203              | Rashet           | 80000               | Jaipur             |
| 204              | Anuj             | 20000               | Goa                |
| 205              | Sumit            | 50000               | Delhi              |

La següent consulta mostra el salari total dels empleats que tenen un salari superior a 50000, agrupats per ciutat:

```sql
SELECT Employee_City , SUM(Employee_Salary)
FROM Employee_Having 
GROUP BY Employee_City 
HAVING SUM(Employee_Salary) > 50000;
```
|  **Employee_City** | **SUM(Employee_Salary)** |
|---------------------------|--------------------|
| Delhi                   |      90000         |
| Jaipur                    |     80000        |

---

### Sintaxi de l'instrucció SELECT utilitzant la clàusula ORDER BY

La clàusula **ORDER BY** amb la instrucció **SQL SELECT** permet mostrar els registres o files de manera ordenada.

La clàusula **ORDER BY** organitza els valors en ordre **ascendent** o **descendent**. En molts sistemes de bases de dades, els valors de la columna s'ordenen en ordre ascendent per defecte.

```sql
SELECT Column_Name_1, Column_Name_2, ....., column_Name_N
FROM table_name
WHERE [Condition]
ORDER BY [column_Name_1, column_Name_2, ....., column_Name_N asc | desc ];  
```

#### Exemple de l'ús de SELECT utilitzant la clàusula ORDER BY

Imaginem que tenim la següent taula de dades: 

```sql
SELECT * FROM Employee_Order;
```

| **Id** | **FirstName** | **Salary** | **City**  |
|--------|---------------|------------|-----------|
| 201    | Jone          | 20000      | Goa       |
| 202    | Basant        | 15000      | Delhi     |
| 203    | Rashet        | 80000      | Jaipur    |
| 204    | Anuj          | 90000      | Goa       |
| 205    | Sumit         | 50000      | Delhi     |

La següent consulta ordena els salaris dels empleats en ordre descendent a partir de la taula `Employee_Order`:

```sql
SELECT * FROM Employee_Order 
ORDER BY Emp_Salary DESC;
```

| **Emp_Id** | **Emp_Name** | **Emp_Salary** | **Emp_City** |
|------------|--------------|----------------|--------------|
| 204        | Anuj         | 90000          | Goa          |
| 203        | Rashet       | 80000          | Jaipur       |
| 205        | Sumit        | 50000          | Delhi        |
| 201        | Jone         | 20000          | Goa          |
| 202        | Basant       | 15000          | Delhi        |

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

---

## Instrucció SQL WHERE

La clàusula **WHERE** en SQL és una instrucció del llenguatge de manipulació de dades (**DML**, Data Manipulation Language).

Tot i que no és obligatori incloure una clàusula **WHERE** en les instruccions DML, s’utilitza sovint per limitar el nombre de files afectades per una instrucció SQL (com **UPDATE** o **DELETE**) o retornades per una consulta (**SELECT**).

La clàusula **WHERE** serveix per aplicar condicions específiques als registres. Només retorna o afecta aquells registres que compleixen les condicions especificades.

La clàusula **WHERE** es pot utilitzar amb les instruccions **SELECT**, **UPDATE**, **DELETE**, entre d’altres.

---

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

---

## Instrucció SQL ORDER BY

Quan volem ordenar els registres basant-nos en les columnes emmagatzemades a les taules d’una base de dades SQL, fem servir la clàusula **ORDER BY**.

La clàusula **ORDER BY** en SQL ens permet ordenar els registres segons una columna específica d’una taula. Això vol dir que tots els valors emmagatzemats en la columna sobre la qual apliquem la clàusula ORDER BY seran ordenats, i els valors corresponents de les altres columnes es mostraran seguint la seqüència definida en aquest procés.

Amb la clàusula **ORDER BY**, podem ordenar els registres en ordre ascendent o descendent, segons les nostres necessitats.

- Si utilitzem la paraula clau **ASC**, els registres s’ordenaran en **ordre ascendent**.
- Si utilitzem la paraula clau **DESC**, s’ordenaran en **ordre descendent**.

> [!IMPORTANT] 
> Si no especifiquem cap paraula clau després de la columna sobre la qual volem ordenar els registres, l’ordre per defecte serà ascendent.

Abans d’escriure les consultes per ordenar els registres, vegem la **sintaxi**.

---

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
