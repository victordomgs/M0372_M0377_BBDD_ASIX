# Continguts

- [Transaccions en SQL](#transaccions-en-SQL)
- [Instrucció SQL INSERT](#instrucció-SQL-INSERT)
- [Inserir dades amb SELECT](#inserir-dades-amb-SELECT)
- [Inserir dades amb claus autogenerades](#inserir-dades-amb-claus-autogenerades)
- [Instrucció SQL UPDATE](#instrucció-SQL-UPDATE)
- [Instrucció SQL DELETE](#instrucció-SQL-DELETE)
- [SQL JOINS](#SQL-JOINS)

<br>

## Transaccions en SQL

### Què és una transacció?

Una **transacció** és una unitat d'execució de treball en una base de dades que es tracta com una única operació lògica. Les transaccions garanteixen que un conjunt d'operacions en la base de dades es realitzin de manera completa i consistent.

Les transaccions segueixen les propietats **ACID**:

1. **Atomicitat**: Una transacció es realitza completament o no es realitza en absolut.
2. **Consistència**: La base de dades ha d'estar en un estat consistent abans i després de la transacció.
3. **Aïllament**: Les operacions de diferents transaccions no interfereixen entre elles.
4. **Durabilitat**: Els canvis realitzats per una transacció confirmada es conserven fins i tot en cas de fallades del sistema.

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/Bases-de-Dades/blob/main/images/acid.png" alt="ACID" width="480" height="auto"/><p><em>Figura 1: ACID. Fuente: Gravitar.biz</em></p>
  </div>

### Inici i finalització de transaccions

Les transaccions en SQL es poden gestionar amb els següents comandos:

#### Iniciar una transacció
```sql
START TRANSACTION;
```
Aquest comando indica l'inici d'una transacció.

#### Confirmar una transacció
```sql
COMMIT;
```
Aquest comando confirma la transacció i fa permanents els canvis a la base de dades.

#### Desfer una transacció
```sql
ROLLBACK;
```
Aquest comando desfà tots els canvis realitzats des de l'inici de la transacció.

### Exemples pràctics

#### Exemple 1: Transacció senzilla
```sql
START TRANSACTION;

UPDATE comptes SET saldo = saldo - 100 WHERE id = 1;
UPDATE comptes SET saldo = saldo + 100 WHERE id = 2;

COMMIT;
```
En aquest exemple, es transfereixen 100 unitats del compte 1 al compte 2. La transacció es confirma amb `COMMIT`.

#### Exemple 2: Desfer canvis
```sql
START TRANSACTION;

UPDATE productes SET estoc = estoc - 10 WHERE id = 5;

ROLLBACK;
```
Aquest exemple desfà l'actualització de l'estoc del producte amb id 5.

### Control de transaccions automàtiques

#### Mode autocommit
Molts sistemes de bases de dades utilitzen el mode **autocommit** per defecte. Això significa que cada instrucció SQL es tracta com una transacció independent.

Per desactivar l'autocommit:
```sql
SET autocommit = 0;
```
Per activar-lo de nou:
```sql
SET autocommit = 1;
```

### Punts de recuperació

SQL permet establir **punts de recuperació** (savepoints) dins d'una transacció per tal de desfer només una part dels canvis:

#### Crear un punt de recuperació
```sql
SAVEPOINT punt1;
```

#### Tornar a un punt de recuperació
```sql
ROLLBACK TO punt1;
```

#### Eliminar un punt de recuperació
```sql
RELEASE SAVEPOINT punt1;
```

### Errors comuns

- **Oblidar el `COMMIT`:** Si no es confirma una transacció, els canvis no es reflectiran a la base de dades.
- **Conflictes d'aïllament:** Pot ocórrer quan dues transaccions intenten modificar les mateixes dades simultàniament. Això es pot gestionar amb diferents nivells d'aïllament.

### Nivells d'aïllament

SQL ofereix diferents nivells d'aïllament per controlar el comportament de les transaccions en entorns amb concurrència:

1. **Read Uncommitted:** Permet llegir dades no confirmades.
2. **Read Committed:** Només permet llegir dades confirmades.
3. **Repeatable Read:** Garanteix que les dades llegides no canviaran durant la transacció.
4. **Serializable:** El nivell més alt d'aïllament; garanteix que les transaccions es processen seqüencialment.

Configurar el nivell d'aïllament:
```sql
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;
```

<br>

## Instrucció SQL INSERT

La funció `INSERT` s'utilitza en SQL per afegir noves files (registres) a una taula d'una base de dades.

### **Sintaxi bàsica**

```sql
INSERT INTO nom_taula (columna1, columna2, columna3, ...)
VALUES (valor1, valor2, valor3, ...);
```

- **`nom_taula`**: Nom de la taula on es volen inserir les dades.
- **`columna1, columna2, columna3`**: Noms de les columnes en les quals es volen inserir els valors.
- **`valor1, valor2, valor3`**: Els valors que es volen afegir a cada columna.

### **Exemple bàsic**

Suposem que tenim una taula anomenada `usuaris` amb les següent columnes:

```sql
CREATE TABLE Usuaris (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nom VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    edat INT NOT NULL
);
```

| id | nom     | correu                | edat |
|----|---------|-----------------------|------|
| 1  | Anna    | anna@example.com      | 25   |
| 2  | Marc    | marc@example.com      | 30   |

Volem afegir un nou usuari a aquesta taula. La consulta seria:

```sql
INSERT INTO usuaris (nom, correu, edat)
VALUES ('Joan', 'joan@example.com', 35);
```

Després d'executar aquesta consulta, la taula quedarà:

| id | nom     | correu                | edat |
|----|---------|-----------------------|------|
| 1  | Anna    | anna@example.com      | 25   |
| 2  | Marc    | marc@example.com      | 30   |
| 3  | Joan    | joan@example.com      | 35   |

### **Inserir dades a totes les columnes**
Si vols inserir dades a **totes les columnes** de la taula, pots ometre els noms de les columnes. Però l'ordre dels valors ha de coincidir amb l'ordre de les columnes de la taula.

```sql
INSERT INTO usuaris
VALUES (4, 'Maria', 'maria@example.com', 28);
```

### **Insercions multiples**
També pots inserir diverses files alhora utilitzant una única consulta `INSERT`:

```sql
INSERT INTO usuaris (nom, correu, edat)
VALUES
  ('Pau', 'pau@example.com', 22),
  ('Laura', 'laura@example.com', 27),
  ('Carla', 'carla@example.com', 32);
```

Aquest codi inserirà tres nous registres en la taula `usuaris`.

### **Consideracions**
1. **Valors nuls (`NULL`)**:
   - Si una columna permet valors nuls, pots inserir el valor `NULL` per indicar que la dada no està disponible.
   - Exemple:
     ```sql
     INSERT INTO usuaris (nom, correu, edat)
     VALUES ('Guillem', NULL, 29);
     ```
> [!IMPORTANT]  
> No podem inserir valors `NULL` en algun d'aquests supòsits:
> 1. Restricció `NOT NULL`. Si una columna té la restricció `NOT NULL`, no se li permeten valors `NULL`.
> 2. Les claus primàries no poden contenir valors `NULL`. Això és perquè una clau primària ha de ser única i no pot estar buida per identificar de manera unívoca un registre en una taula.

2. **Tipus de dades**:
   - Assegura't que els valors inserits coincideixen amb els tipus de dades definits per cada columna (per exemple, text, enter, data, etc.).

3. **Claus primàries**:
   - Si la taula té una clau primària, assegura't que els valors inserits en aquesta columna són únics.

### **Errors comuns**
1. **Omplir columnes obligatòries**:
   - Si una columna està definida com a `NOT NULL`, has d'inserir-hi un valor.
   - Exemple d'error:
     ```sql
     INSERT INTO usuaris (nom, correu)
     VALUES ('Anna', 'anna@example.com');
     -- Error: falta la columna "edat"
     ```

2. **Duplicats en claus primàries**:
   - Si intentes inserir un valor duplicat en una columna amb clau primària, rebràs un error.
   - Exemple:
     ```sql
     INSERT INTO usuaris (id, nom, correu, edat)
     VALUES (1, 'Pere', 'pere@example.com', 40);
     -- Error: valor duplicat a la clau primària "id"
     ```

<br>

## Inserir dades amb SELECT

En SQL, pots inserir dades en una taula utilitzant una sentència `INSERT INTO ... SELECT`. Aquesta tècnica és útil quan necessites copiar dades d'una altra taula o generar dades basades en una selecció.

### Estructura general

La sintaxi bàsica per inserir dades mitjançant una selecció és:

```sql
INSERT INTO taula_destinació (columna1, columna2, ...)
SELECT valor1, valor2, ...
FROM taula_origen
WHERE condició;
```

#### Components:
- **`taula_destinació`**: La taula on s'inseriran les dades.
- **`columnes`**: Llista de columnes de la taula destinació on es col·locaran els valors.
- **`SELECT`**: La selecció que genera les dades a inserir.
- **`taula_origen`**: La taula d'on provenen les dades (pot ser una taula existent o una selecció més complexa).
- **`WHERE`** (opcional): Condició per filtrar les dades de la taula origen.

### Exemple pràctic

#### Taules inicials

Suposem que tenim dues taules: 

##### Taula `empleats`
```sql
CREATE TABLE empleats (
    id INT PRIMARY KEY,
    nom VARCHAR(100),
    departament_id INT
);
```

##### Taula `nous_empleats`
```sql
CREATE TABLE nous_empleats (
    id INT PRIMARY KEY,
    nom VARCHAR(100),
    departament_id INT
);
```

Volem inserir dades de la taula `nous_empleats` a la taula `empleats`.

#### Inserir dades

##### Inserir directament de la taula origen

Podem copiar les dades de la taula `nous_empleats` a `empleats` utilitzant una sentència `SELECT`:

```sql
INSERT INTO empleats (id, nom, departament_id)
SELECT id, nom, departament_id
FROM nous_empleats;
```

##### Explicació
- **`INSERT INTO empleats`**: Especifica que inserirem dades a la taula `empleats`.
- **`columnes`**: `id`, `nom`, `departament_id` són les columnes on col·locarem els valors.
- **`SELECT`**: Selecciona dades de la taula `nous_empleats`.

### Inserir dades amb transformacions

#### Inserir amb dades calculades

També pots transformar o calcular els valors durant la selecció abans d'inserir-los. Per exemple, suposem que volem afegir un prefix al nom dels empleats abans d'inserir-los:

```sql
INSERT INTO empleats (id, nom, departament_id)
SELECT id, CONCAT('Nou-', nom), departament_id
FROM nous_empleats;
```

##### Explicació
- **`CONCAT`**: S'utilitza per concatenar una cadena de text amb el valor de la columna `nom`.
- El nom dels empleats inserits tindrà el prefix `Nou-`.

#### Inserir amb valors fixos

Si necessites inserir un valor fix per a una columna, pots especificar-lo directament en la sentència `SELECT`:

```sql
INSERT INTO empleats (id, nom, departament_id)
SELECT id, nom, 1
FROM nous_empleats;
```

##### Explicació
- En lloc de copiar el valor de la columna `departament_id` de la taula origen, s'insereix el valor fix `1` a totes les files.

### Inserir amb condicions

Pots utilitzar una condició per filtrar quines dades s'inseriran a la taula destinació. Per exemple, només volem inserir els empleats que pertanyen al departament 2:

```sql
INSERT INTO empleats (id, nom, departament_id)
SELECT id, nom, departament_id
FROM nous_empleats
WHERE departament_id = 2;
```

##### Explicació
- **`WHERE departament_id = 2`**: Només les files de la taula `nous_empleats` amb `departament_id = 2` s'inseriran a `empleats`.

> [!IMPORTANT]  
> Quan treballes amb dades a inserir, assegura't que:
> 1. Els tipus de dades de les columnes seleccionades coincideixin amb els de les columnes de la taula destinació.
> 2. Les restriccions de la taula destinació (com claus primàries o valors únics) no es violin. Per exemple, si `id` és una clau primària a la taula `empleats`, no pots inserir dues files amb el mateix `id`.

<br>

## Inserir dades amb claus autogenerades

En SQL, les claus autogenerades són valors únics que es creen automàticament per la base de dades quan s'insereix una nova fila. 

### Què són les claus autogenerades?

Les claus autogenerades són valors generats automàticament per la base de dades mitjançant una funcionalitat interna, com ara:

1. **Auto-increment (`AUTO_INCREMENT`)**: En bases de dades com MySQL, es pot definir una columna com a `AUTO_INCREMENT` perquè es generi un nou valor seqüencial cada vegada que s'insereix una fila.

2. **Sequences**: En bases de dades com PostgreSQL i Oracle, es poden utilitzar seqüències per generar valors únics.

3. **UUID**: Algunes bases de dades permeten generar claus úniques utilitzant identificadors únics universals (UUID).

Aquestes claus garanteixen que cada fila tingui un identificador únic, evitant conflictes de claus duplicades.

### Crear una taula amb claus autogenerades

#### Exemple amb `AUTO_INCREMENT` (MySQL)

```sql
CREATE TABLE empleats (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nom VARCHAR(100),
    departament_id INT
);
```

- **`AUTO_INCREMENT`**: Indica que la columna `id` serà autogenerada amb un valor seqüencial.
- **`PRIMARY KEY`**: Defineix que `id` és la clau primària.

#### Exemple amb sequences (PostgreSQL)

```sql
CREATE TABLE empleats (
    id SERIAL PRIMARY KEY,
    nom VARCHAR(100),
    departament_id INT
);
```

- **`SERIAL`**: Genera automàticament valors seqüencials per a la columna `id`.
- Això és similar a `AUTO_INCREMENT` en MySQL.

### Inserir dades amb claus autogenerades

Quan una columna està configurada per generar claus automàticament, no cal incloure-la en la sentència `INSERT INTO`. La base de dades s'encarrega de generar el valor per a aquesta columna.

#### Exemple bàsic d'inserció

##### MySQL o PostgreSQL
```sql
INSERT INTO empleats (nom, departament_id)
VALUES ('Anna', 2);
```

- En aquest cas, la base de dades genera automàticament el valor de la columna `id`.

##### Resultat:
| id  | nom  | departament_id |
|-----|------|----------------|
| 1   | Anna | 2              |

### Recuperar el valor generat

Després d'inserir una fila amb una clau autogenerada, pots recuperar el valor generat utilitzant una funció o una instrucció específica, depenent del sistema de gestió de bases de dades:

#### MySQL
```sql
SELECT LAST_INSERT_ID();
```

- Això retorna el valor de l'última clau autogenerada en la sessió actual.

#### PostgreSQL
```sql
INSERT INTO empleats (nom, departament_id)
VALUES ('Bernat', 3)
RETURNING id;
```

- La clausula `RETURNING` retorna directament el valor generat per la columna `id`.

### Claus autogenerades i `INSERT INTO ... SELECT`

També pots inserir dades en una taula amb claus autogenerades utilitzant una sentència `SELECT`.

#### Exemple
Suposem que tenim una taula `nous_empleats` amb les següents dades:

```sql
CREATE TABLE nous_empleats (
    id AUTO_INCREMENT PRIMARY KEY,
    nom VARCHAR(100),
    departament_id INT
);
```

| id  | nom    | departament_id |
|-----|--------|----------------|
| 1   | Clara  | 1              |
| 2   | David  | 2              |

Podem copiar aquestes dades a la taula `empleats` amb una clau autogenerada per a cada registre:

```sql
INSERT INTO empleats (nom, departament_id)
SELECT nom, departament_id
FROM nous_empleats;
```

- El valor de `id` es genera automàticament com un valor únic.

##### Resultat:
| id  | nom    | departament_id |
|-----|--------|----------------|
| 1   | Anna   | 2              |
| 2   | Clara  | 1              |
| 3   | David  | 2              |

### Claus autogenerades amb UUID

En lloc de valors seqüencials, pots utilitzar identificadors únics universals (`UUID`) per a generar claus primàries. Aquesta tècnica és útil quan necessites claus úniques globalment i no només dins d'una taula específica.

#### Exemple amb UUID (MySQL o PostgreSQL)

##### Crear una taula
```sql
CREATE TABLE empleats (
    id UUID PRIMARY KEY DEFAULT (UUID_GENERATE_V4()),
    nom VARCHAR(100),
    departament_id INT
);
```

##### Inserir dades
```sql
INSERT INTO empleats (nom, departament_id)
VALUES ('Eva', 3);
```

- El valor de `id` es genera automàticament com un UUID.

<br>

## Instrucció SQL UPDATE

La sentència `UPDATE` s'utilitza per modificar els registres existents en una taula d'una base de dades. Amb aquesta instrucció, podem canviar el valor d'una o més columnes en una o diverses files que compleixin una condició determinada.

### Sintaxi bàsica

```sql
UPDATE nom_taula
SET columna1 = valor1, columna2 = valor2, ...
WHERE condició;
```

#### Explicació dels elements:
- **`nom_taula`**: Nom de la taula que volem modificar.
- **`SET`**: Clausula que indica les columnes i els nous valors que volem assignar.
- **`columna1 = valor1`**: Assigna un nou valor a una columna especificada.
- **`WHERE`**: Opcional, especifica una condició per determinar quines files seran actualitzades. Si no s'especifica, totes les files seran modificades.

### Exemple senzill

Suposem que tenim una taula anomenada `Empleats` amb la següent estructura:

| ID  | Nom         | Departament | Sou  |
|------|-------------|-------------|-------|
| 1    | Anna        | Vendes      | 3000  |
| 2    | Joan        | IT          | 3500  |
| 3    | Maria       | Vendes      | 2800  |

#### Actualitzar el sou d'un empleat
Si volem augmentar el sou de Joan a 4000, farem:

```sql
UPDATE Empleats
SET Sou = 4000
WHERE Nom = 'Joan';
```

Resultat:

| ID  | Nom         | Departament | Sou  |
|------|-------------|-------------|-------|
| 1    | Anna        | Vendes      | 3000  |
| 2    | Joan        | IT          | 4000  |
| 3    | Maria       | Vendes      | 2800  |

#### Actualitzar diverses columnes
També podem modificar més d'una columna alhora. Per exemple, canviem el departament de Maria a `IT` i augmentem el seu sou a 3200:

```sql
UPDATE Empleats
SET Departament = 'IT', Sou = 3200
WHERE Nom = 'Maria';
```

Resultat:

| ID  | Nom         | Departament | Sou  |
|------|-------------|-------------|-------|
| 1    | Anna        | Vendes      | 3000  |
| 2    | Joan        | IT          | 4000  |
| 3    | Maria       | IT          | 3200  |

### Actualitzacions massives
Si no s'utilitza la clausula `WHERE`, totes les files de la taula seran modificades. Per exemple:

```sql
UPDATE Empleats
SET Departament = 'General';
```

Resultat:

| ID  | Nom         | Departament | Sou  |
|------|-------------|-------------|-------|
| 1    | Anna        | General     | 3000  |
| 2    | Joan        | General     | 4000  |
| 3    | Maria       | General     | 3200  |

> [!IMPORTANT]  
> **Usar sempre la clausula `WHERE` amb cura**: Sense una condició específica, es poden modificar totes les files de la taula, provocant resultats inesperats.
> 
> **Provar amb una consulta `SELECT` primer**: Executar una consulta `SELECT` amb la mateixa condició que el `WHERE` per verificar quines files seran afectades.

### Exemple amb subconsulta
També es poden utilitzar subconsultes per calcular els valors a assignar. Suposem que volem augmentar el sou de tots els empleats al mateix nivell que el sou més alt de la taula:

```sql
UPDATE Empleats
SET Sou = (SELECT MAX(Sou) FROM Empleats);
```

Resultat:

| ID  | Nom         | Departament | Sou  |
|------|-------------|-------------|-------|
| 1    | Anna        | General     | 4000  |
| 2    | Joan        | General     | 4000  |
| 3    | Maria       | General     | 4000  |

<br>

## Instrucció SQL DELETE

La sentència `DELETE` s'utilitza per eliminar registres d'una taula en una base de dades. Aquesta operació es realitza basant-se en una condició especificada amb la clausula `WHERE`. Sense aquesta condició, tots els registres de la taula seran eliminats.

### Sintaxi bàsica

```sql
DELETE FROM nom_taula
WHERE condició;
```

#### Explicació dels elements:
- **`nom_taula`**: Nom de la taula d'on es volen eliminar els registres.
- **`WHERE`**: Clausula opcional que especifica quins registres s'eliminaran. Si no s'inclou, s'eliminaran tots els registres de la taula.

### Exemple senzill

Suposem que tenim una taula anomenada `Empleats` amb la següent estructura:

| ID  | Nom         | Departament | Sou  |
|------|-------------|-------------|-------|
| 1    | Anna        | Vendes      | 3000  |
| 2    | Joan        | IT          | 3500  |
| 3    | Maria       | Vendes      | 2800  |

#### Eliminar un registre específic
Per eliminar l'empleat amb nom "Maria":

```sql
DELETE FROM Empleats
WHERE Nom = 'Maria';
```

Resultat:

| ID  | Nom         | Departament | Sou  |
|------|-------------|-------------|-------|
| 1    | Anna        | Vendes      | 3000  |
| 2    | Joan        | IT          | 3500  |

#### Eliminar registres basats en una condició
Si volem eliminar tots els empleats del departament de `Vendes`:

```sql
DELETE FROM Empleats
WHERE Departament = 'Vendes';
```

Resultat:

| ID  | Nom         | Departament | Sou  |
|------|-------------|-------------|-------|
| 2    | Joan        | IT          | 3500  |

### Eliminar tots els registres
Si volem buidar completament la taula, ho podem fer ometent la clausula `WHERE`. Això eliminarà totes les files:

```sql
DELETE FROM Empleats;
```

Després d'executar aquesta sentència, la taula quedarà buida:

| ID  | Nom         | Departament | Sou  |
|------|-------------|-------------|-------|
|      |             |             |       |

### Recomanacions importants

> [!IMPORTANT]  
> **Utilitza sempre la clausula `WHERE` amb cura**: Si no especifiques una condició, tots els registres de la taula seran eliminats.
> 
> **Prova amb una consulta `SELECT` prèvia**: Utilitza una consulta `SELECT` amb la mateixa condició per assegurar-te que eliminaràs els registres correctes.
> 
> **Coneix la diferència amb `TRUNCATE`**: La sentència `TRUNCATE` també elimina tots els registres d'una taula, però és més ràpida i no permet especificar condicions. També reinicia els valors autoincrementats.

### Exemple amb subconsulta
També podem utilitzar subconsultes dins d'una sentència `DELETE`. Suposem que volem eliminar tots els empleats amb un sou inferior al sou mitjà de la taula:

```sql
DELETE FROM Empleats
WHERE Sou < (SELECT AVG(Sou) FROM Empleats);
```

Aquesta sentència eliminarà els empleats que tenen un sou inferior a la mitjana.
