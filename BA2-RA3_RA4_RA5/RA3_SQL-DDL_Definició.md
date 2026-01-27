# Continguts

- [Creació de bases de dades](#creació-de-bases-de-dades)
- [Creació de taules](#creació-de-taules)
- [Restriccions de columnes](#restriccions-de-columnes)
- [Restriccions de clau forana](#restriccions-de-clau-forana)
- [Modificació de taules](#modificació-de-taules)

<br>

## Creació de bases de dades

### 1. Creació d'una base de dades

#### **1.1. Sintaxi bàsica**

Per crear una base de dades en SQL, s'utilitza la instrucció `CREATE DATABASE`:

```sql
CREATE DATABASE nom_de_la_base_de_dades;
```

#### **1.2. Definició de codificació i col·lació**

En molts SGBD, podem definir la codificació de caràcters i la col·lació (ordre de classificació de text). Exemples:

```sql
CREATE DATABASE exemple_db
CHARACTER SET utf8mb4
COLLATE utf8mb4_0900_ai_ci;
```

- `CHARACTER SET utf8mb4`: Defineix el joc de caràcters per suportar emojis i idiomes diversos.
- `COLLATE utf8mb4_general_ci`: Defineix com es comparen i ordenen les dades textuals.

#### **1.3. Comprovar si la base de dades ja existeix**

Per evitar errors si la base de dades ja existeix:

```sql
CREATE DATABASE IF NOT EXISTS exemple_db;
```

Aquesta instrucció només crearà la base de dades si no existeix prèviament.

---

### 2. Eliminació d'una base de dades

#### **2.1. Sintaxi per eliminar una base de dades**

```sql
DROP DATABASE nom_de_la_base_de_dades;
```

Aquesta ordre **esborra completament** la base de dades i totes les seves taules, per tant, cal fer-ho amb precaució.

#### **2.2. Evitar errors si la base de dades no existeix**

```sql
DROP DATABASE IF EXISTS exemple_db;
```

Aquest enfocament evita errors si la base de dades no existeix.

---

### 3. Modificació d'una base de dades

#### **3.1. Modificar la col·lació o codificació**

```sql
ALTER DATABASE exemple_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

Aquesta ordre canvia la codificació i col·lació de la base de dades, però no modifica les taules existents.

---

### 4. Llistat de bases de dades

Per veure les bases de dades disponibles en un SGBD:

```sql
SHOW DATABASES;
```

Exemple de sortida:

```
+--------------------+
| Database          |
+--------------------+
| exemple_db        |
| information_schema |
| mysql             |
| performance_schema |
+--------------------+
```

---

### 5. Seleccionar una base de dades

Abans de treballar amb una base de dades, cal seleccionar-la:

```sql
USE exemple_db;
```

Aquesta ordre estableix `exemple_db` com la base de dades activa per a les operacions posteriors.

---

### 6. Exemple complet

```sql
-- Creació de la base de dades
CREATE DATABASE IF NOT EXISTS exemple_db
CHARACTER SET utf8mb4
COLLATE utf8mb4_general_ci;

-- Selecció de la base de dades
USE exemple_db;

-- Modificació de la codificació
ALTER DATABASE exemple_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

-- Eliminació segura de la base de dades
DROP DATABASE IF EXISTS exemple_db;
```

<br>

## Creació de taules

### 1. Sintaxi bàsica

La sintaxi general per crear una taula és:

```sql
CREATE TABLE nom_de_la_taula (
    columna1 TIPUS_DE_DADA CONSTRAINTS,
    columna2 TIPUS_DE_DADA CONSTRAINTS,
    ...
);
```

Exemple:

```sql
CREATE TABLE clients (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nom VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    telefon VARCHAR(15),
    data_registre TIMESTAMP DEFAULT TIMESTAMP
);
```

Aquest exemple defineix una taula `clients` amb:
- Un identificador únic `id` amb autoincrement.
- Un camp `nom` obligatori (`NOT NULL`).
- Un camp `email` únic per evitar duplicats.
- Un camp `telefon` opcional.
- Un camp `data_registre` amb valor per defecte a la data actual.

---

### 2. Tipus de dades

Seleccionar el tipus de dada correcte és essencial per a un bon disseny de taula. Alguns dels tipus més comuns són:

| Tipus de Dada | Descripció |
|--------------|------------|
| `INT` | Enter, útil per a identificadors |
| `VARCHAR(n)` | Text de longitud variable `n` |
| `TEXT` | Text llarg, per a descripcions |
| `DATE` | Data en format `AAAA-MM-DD` |
| `DATETIME` | Data i hora |
| `BOOLEAN` | Valors `TRUE` o `FALSE` |
| `DECIMAL(m, d)` | Número decimal amb `m` dígits i `d` decimals |

**Exemple d'ús:**
```sql
CREATE TABLE productes (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nom VARCHAR(100) NOT NULL,
    preu DECIMAL(10,2) CHECK (preu > 0),
    stock INT DEFAULT 0
);
```

Aquest disseny assegura que el `preu` sempre sigui positiu i estableix un valor per defecte de `0` per `stock`.

---

### 3. Restriccions (*Constraints*)

Les restriccions ajuden a garantir la integritat de les dades.

| Restriccions | Descripció |
|---------------|------------|
| `PRIMARY KEY` | Identifica un registre únic en una taula |
| `FOREIGN KEY` | Crea una relació entre dues taules |
| `UNIQUE` | Garanteix que els valors d'una columna siguin únics |
| `NOT NULL` | Evita valors nuls |
| `CHECK` | Defineix una condició per a una columna |
| `DEFAULT` | Assigna un valor per defecte |
| `AUTO_INCREMENT` | Incrementa automàticament una columna (per a claus primàries numèriques) |

**Exemple amb diverses restriccions:**

```sql
CREATE TABLE comandes (
    id INT PRIMARY KEY AUTO_INCREMENT,
    client_id INT,
    total DECIMAL(10,2) CHECK (total >= 0),
    data_comanda DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (client_id) REFERENCES clients(id)
);
```

Aquesta estructura:
- Enllaça `client_id` amb `clients(id)` com a clau forana.
- Assegura que `total` no sigui negatiu.
- Assigna la data i hora actual per defecte a `data_comanda`.

---

### 4. Bones pràctiques en la creació de taules

#### **4.1. Utilitzar noms descriptius**
- Evita noms genèrics com `taula1` o `dades`.
- Usa noms en singular (`usuari` en lloc de `usuaris` si cada fila representa un usuari).

#### **4.2. Evitar tipus de dades innecessàriament grans**
- No usar `TEXT` si `VARCHAR(255)` és suficient.
- Evita `BIGINT` si `INT` cobreix les necessitats.

#### **4.3. Definir claus primàries i foranas sempre que sigui possible**
- Les claus primàries ajuden a l'indexació i eficiència de cerques.
- Les claus foranes mantenen la coherència entre taules.

#### **4.4. Utilitzar valors per defecte i restriccions per evitar errors**
- Assignar `DEFAULT` quan tingui sentit.
- Aplicar `NOT NULL` quan la dada sigui obligatòria.

---

### 5. Exemple complet

```sql
CREATE TABLE categories (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nom VARCHAR(50) NOT NULL UNIQUE
);

CREATE TABLE productes (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nom VARCHAR(100) NOT NULL,
    preu DECIMAL(10,2) CHECK (preu > 0),
    stock INT DEFAULT 0,
    categoria_id INT,
    FOREIGN KEY (categoria_id) REFERENCES categories(id)
);
```

Aquest disseny:
- Garanteix que cada categoria tingui un nom únic.
- Enllaça `productes` amb `categories` mitjançant `categoria_id`.
- Limita `preu` perquè només accepti valors positius.

<br>

## Restriccions de columnes

### 1. Introducció

Les **restriccions de columnes** en bases de dades garanteixen la integritat i validesa de les dades, evitant errors i inconsistències. S'apliquen quan es defineix o modifica una taula i ajuden a establir regles específiques sobre els valors que es poden emmagatzemar en cada columna.

---

### 2. Tipus de restriccions

#### **2.1. NOT NULL**

La restricció `NOT NULL` assegura que una columna no pugui contenir valors nuls (`NULL`).

```sql
CREATE TABLE clients (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nom VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    telefon VARCHAR(15)
);
```

En aquest exemple, `nom` és obligatori i no pot ser `NULL`.

Si volem afegir `NOT NULL` a una columna existent:

```sql
ALTER TABLE clients MODIFY COLUMN telefon VARCHAR(15) NOT NULL;
```

Per eliminar la restricció:

```sql
ALTER TABLE clients MODIFY COLUMN telefon VARCHAR(15) NULL;
```

---

#### **2.2. UNIQUE**

La restricció `UNIQUE` garanteix que tots els valors d’una columna siguin únics.

```sql
CREATE TABLE usuaris (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nom VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE
);
```

Si volem afegir `UNIQUE` a una columna existent:

```sql
ALTER TABLE usuaris ADD CONSTRAINT un_email UNIQUE (email);
```

Per eliminar la restricció:

```sql
ALTER TABLE usuaris DROP CONSTRAINT un_email;
```

---

#### **2.3. PRIMARY KEY**

La clau primària (`PRIMARY KEY`) identifica de manera única cada registre d'una taula. Només pot existir una clau primària per taula i no pot tenir valors `NULL`.

```sql
CREATE TABLE comandes (
    id INT PRIMARY KEY AUTO_INCREMENT,
    client_id INT,
    total DECIMAL(10,2) NOT NULL
);
```

Per afegir una clau primària a una taula existent:

```sql
ALTER TABLE comandes ADD PRIMARY KEY (id);
```

Per eliminar-la:

```sql
ALTER TABLE comandes DROP PRIMARY KEY;
```

---

#### **2.4. FOREIGN KEY**

Les claus foranes (`FOREIGN KEY`) estableixen relacions entre taules, garantint la integritat referencial.

```sql
CREATE TABLE comandes (
    id INT PRIMARY KEY AUTO_INCREMENT,
    client_id INT,
    FOREIGN KEY (client_id) REFERENCES clients(id)
);
```

Si volem afegir una clau forana a una taula existent:

```sql
ALTER TABLE comandes ADD CONSTRAINT fk_client FOREIGN KEY (client_id) REFERENCES clients(id);
```

Per eliminar-la:

```sql
ALTER TABLE comandes DROP FOREIGN KEY fk_client;
```

---

#### **2.5. CHECK**

La restricció `CHECK` assegura que els valors d'una columna compleixin una condició específica.

```sql
CREATE TABLE productes (
    id INT PRIMARY KEY AUTO_INCREMENT,
    preu DECIMAL(10,2) CHECK (preu > 0)
);
```

Si volem afegir una restricció `CHECK` a una columna existent:

```sql
ALTER TABLE productes ADD CONSTRAINT chk_preu CHECK (preu > 0);
```

Per eliminar-la:

```sql
ALTER TABLE productes DROP CONSTRAINT chk_preu;
```

---

#### **2.6. DEFAULT**

La restricció `DEFAULT` assigna un valor per defecte a una columna si no s’especifica cap valor en la inserció.

```sql
CREATE TABLE empleats (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nom VARCHAR(100) NOT NULL,
    salari DECIMAL(10,2) DEFAULT 1500.00
);
```

Si volem afegir `DEFAULT` a una columna existent:

```sql
ALTER TABLE empleats ALTER COLUMN salari SET DEFAULT 1800.00;
```

Per eliminar-lo:

```sql
ALTER TABLE empleats ALTER COLUMN salari DROP DEFAULT;
```

---

#### **2.7. AUTO_INCREMENT**

La restricció `AUTO_INCREMENT` permet que una columna generi automàticament valors únics, generalment per a claus primàries.

```sql
CREATE TABLE clients (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nom VARCHAR(50) NOT NULL
);
```

Cada nou registre inserit en `clients` tindrà un `id` que s'incrementarà automàticament.

Si volem afegir `AUTO_INCREMENT` a una columna existent:

```sql
ALTER TABLE clients MODIFY COLUMN id INT AUTO_INCREMENT;
```

Per eliminar `AUTO_INCREMENT`, es pot modificar la columna:

```sql
ALTER TABLE clients MODIFY COLUMN id INT;
```

---

### 3. Bones pràctiques en l'ús de restriccions

#### **3.1. Planificació prèvia**
- Definir `NOT NULL` per a columnes essencials per evitar registres incomplets.
- Usar `UNIQUE` per a dades que han de ser úniques, com correus electrònics o noms d'usuari.

#### **3.2. Evitar restriccions innecessàries**
- No abusar de `CHECK`, ja que pot incrementar la complexitat de la base de dades.
- Usar `DEFAULT` només quan hi hagi un valor per defecte raonable.

#### **3.3. Comprovar dependències**
- No eliminar una clau forana si la taula depèn d'una altra taula per mantenir la integritat de les dades.
- Repassar les relacions entre taules abans d'afegir o eliminar `FOREIGN KEY`.

#### **3.4. Utilitzar noms clars per les restriccions**
- Assignar noms a les restriccions per facilitar la seva gestió, e lloc de confiar en noms automàtics generats pel SGBD:
  ```sql
  ALTER TABLE usuaris ADD CONSTRAINT un_email UNIQUE (email);
  ```

<br>

## Restriccions de Clau Forana

### Definició
Una **clau forana (foreign key)** és un camp o conjunt de camps en una taula que estableix una relació amb la clau primària d'una altra taula. Serveix per garantir la integritat referencial entre les dades de dues taules.

### Objectiu
Les claus foranes asseguren que els valors d'un camp (o camps) en una taula coincideixin amb els valors d'una clau primària en una altra taula. Això evita l'existència de registres "penjats" o referències a dades inexistents.

### Sintaxi Bàsica
```sql
CREATE TABLE comanda (
    id_comanda INT PRIMARY KEY,
    id_client INT,
    FOREIGN KEY (id_client) REFERENCES client(id_client)
);
```

### Comportaments de Clau Forana: ON DELETE i ON UPDATE
Quan es defineix una clau forana, es poden establir accions específiques per determinar què passa quan es modifiquen o s'eliminen els valors en la taula referenciada:

#### 1. ON DELETE
Defineix el comportament que s'executa quan s'elimina un registre de la taula pare que està referenciat per la clau forana de la taula filla.

#### 2. ON UPDATE
Defineix el comportament que s'executa quan es modifica el valor de la clau primària en la taula pare que està referenciat per la clau forana de la taula filla.

### Opcions per ON DELETE i ON UPDATE
Les opcions són les mateixes tant per ON DELETE com per ON UPDATE. A continuació, s'expliquen detalladament:

#### CASCADE
Quan s'elimina o es modifica un registre de la taula pare, automàticament s'eliminen o s'actualitzen tots els registres de la taula filla que el referencien.

Exemple:

```sql
CREATE TABLE comanda (
    id_comanda INT PRIMARY KEY,
    id_client INT,
    FOREIGN KEY (id_client) REFERENCES client(id_client) ON DELETE CASCADE ON UPDATE CASCADE
);
```
Quan s'utilitza?

- Quan vols que en eliminar o modificar un client, també s'eliminen o es modifiquin totes les comandes associades.

#### SET NULL
Quan s'elimina o es modifica un registre de la taula pare, els camps de la clau forana en la taula filla es posen a NULL.

Exemple:

```sql
CREATE TABLE comanda (
    id_comanda INT PRIMARY KEY,
    id_client INT,
    FOREIGN KEY (id_client) REFERENCES client(id_client) ON DELETE SET NULL ON UPDATE SET NULL
);
```

Quan s'utilitza?

- Quan vols mantenir els registres de la taula filla però deixar el camp de la clau forana sense valor (NULL).

#### SET DEFAULT
Quan s'elimina o es modifica un registre de la taula pare, els camps de la clau forana en la taula filla es posen al valor per defecte definit.

Exemple:

```sql
CREATE TABLE comanda (
    id_comanda INT PRIMARY KEY,
    id_client INT DEFAULT 1,
    FOREIGN KEY (id_client) REFERENCES client(id_client) ON DELETE SET DEFAULT ON UPDATE SET DEFAULT
);
```

Quan s'utilitza?

- Quan vols que, en eliminar o modificar la clau primària, el valor de la clau forana prengui un valor per defecte.

#### RESTRICT
Impedeix l'eliminació o modificació d'un registre de la taula pare si hi ha registres en la taula filla que el referencien.

Exemple:

```sql
CREATE TABLE comanda (
    id_comanda INT PRIMARY KEY,
    id_client INT,
    FOREIGN KEY (id_client) REFERENCES client(id_client) ON DELETE RESTRICT ON UPDATE RESTRICT
);
```
Quan s'utilitza?

- Quan vols impedir que s'elimini o es modifiqui un registre pare si encara està relacionat amb registres a la taula filla.

#### NO ACTION
És similar a RESTRICT. No permet eliminar o modificar el registre pare si hi ha referències a la taula filla. Aquesta és la restricció per defecte si no s'indica cap altra opció.

Exemple:

```sql
CREATE TABLE comanda (
    id_comanda INT PRIMARY KEY,
    id_client INT,
    FOREIGN KEY (id_client) REFERENCES client(id_client) ON DELETE NO ACTION ON UPDATE NO ACTION
);
```

Quan s'utilitza?

- Quan vols mantenir l'estricte compliment de la integritat referencial i no vols accions automàtiques.

### Resum Taula

| Opció              | Comportament (DELETE/UPDATE)                                    | Quan s'utilitza                                        |
|---------------------|-----------------------------------------------------------------|----------------------------------------------------------|
| CASCADE             | Elimina o actualitza els registres relacionats                  | Quan vols eliminar o actualitzar dades lligades automàticament   |
| SET NULL            | Assigna NULL als camps relacionats                              | Quan vols mantenir els registres però sense associació   |
| SET DEFAULT         | Assigna el valor per defecte als camps relacionats              | Quan vols posar un valor per defecte si es modifica la relació |
| RESTRICT            | Impedeix l'eliminació o modificació si hi ha referències        | Quan vols evitar eliminar o modificar dades amb relacions actives    |
| NO ACTION           | Igual que RESTRICT (comportament per defecte)                   | Quan vols integritat estricta per defecte                |

<br>

## Modificació de taules

### 1. Afegir columnes

És possible afegir noves columnes a una taula existent mitjançant `ALTER TABLE ... ADD COLUMN`.

```sql
ALTER TABLE nom_de_la_taula ADD COLUMN nom_columna TIPUS_DE_DADA CONSTRAINTS;
```

Exemple:

```sql
ALTER TABLE clients ADD COLUMN edat INT NOT NULL DEFAULT 18;
```

Aquest exemple afegeix una columna `edat` de tipus `INT` amb un valor per defecte de `18`.

---

### 2. Modificar columnes existents

Podem canviar el tipus de dada, la mida o altres propietats d’una columna amb `MODIFY COLUMN` o `CHANGE COLUMN`.

```sql
ALTER TABLE nom_de_la_taula MODIFY COLUMN nom_columna NOU_TIPUS_DE_DADA;
```

Exemple:

```sql
ALTER TABLE clients MODIFY COLUMN edat SMALLINT;
```

Aquesta ordre redueix la mida de la columna `edat` de `INT` a `SMALLINT`.

Si també volem canviar el nom de la columna:

```sql
ALTER TABLE nom_de_la_taula CHANGE COLUMN nom_actual nou_nom TIPUS_DE_DADA;
```

Exemple:

```sql
ALTER TABLE clients CHANGE COLUMN edat anys SMALLINT;
```

Aquest canvi renombra `edat` a `anys` i ajusta el tipus de dada.

---

### 3. Eliminar columnes

Per eliminar una columna de la taula, fem servir `DROP COLUMN`.

```sql
ALTER TABLE nom_de_la_taula DROP COLUMN nom_columna;
```

Exemple:

```sql
ALTER TABLE clients DROP COLUMN telefon;
```

Aquesta ordre elimina la columna `telefon` de la taula `clients`.

---

### 4. Afegir i eliminar restriccions

Podem modificar les restriccions d'una taula afegint o eliminant claus primàries, claus foranes i altres restriccions.

#### **4.1. Afegir una clau primària**

```sql
ALTER TABLE nom_de_la_taula ADD PRIMARY KEY (nom_columna);
```

Exemple:

```sql
ALTER TABLE clients ADD PRIMARY KEY (id);
```

#### **4.2. Afegir una clau forana**

```sql
ALTER TABLE nom_de_la_taula ADD CONSTRAINT nom_clau FOREIGN KEY (columna) REFERENCES altra_taula(columna);
```

Exemple:

```sql
ALTER TABLE comandes ADD CONSTRAINT fk_client FOREIGN KEY (client_id) REFERENCES clients(id);
```

#### **4.3. Eliminar una clau forana**

```sql
ALTER TABLE nom_de_la_taula DROP FOREIGN KEY nom_clau;
```

Exemple:

```sql
ALTER TABLE comandes DROP FOREIGN KEY fk_client;
```

#### **4.4. Eliminar una clau primària**

```sql
ALTER TABLE nom_de_la_taula DROP PRIMARY KEY;
```

Exemple:

```sql
ALTER TABLE clients DROP PRIMARY KEY;
```

---

### 5. Renombrar taules

Per canviar el nom d'una taula, s'utilitza `RENAME TO`.

```sql
ALTER TABLE nom_actual RENAME TO nou_nom;
```

Exemple:

```sql
ALTER TABLE clients RENAME TO usuaris;
```

Aquest canvi modifica el nom de la taula `clients` a `usuaris`.

---

### 6. Bones pràctiques en la modificació de taules

#### **6.1. Planificar els canvis**
- Abans de modificar una taula, assegura't que el canvi no afectarà altres taules relacionades.
- Fes una còpia de seguretat de la base de dades en cas que sigui necessari restaurar dades.

#### **6.2. Comprovar dependències**
- Revisa si la columna a eliminar està sent usada en altres parts de la base de dades (claus foranes, vistes, procediments).

#### **6.3. Realitzar canvis de forma escalonada**
- Si cal modificar moltes columnes, és recomanable fer-ho en diverses etapes per evitar problemes de compatibilitat.

#### **6.4. Evitar canvis innecessaris**
- No modificar columnes massa sovint, ja que pot afectar el rendiment de la base de dades.

---

### 7. Exemple complet

```sql
-- Afegir una nova columna
ALTER TABLE clients ADD COLUMN data_naixement DATE;

-- Modificar el tipus de dada d'una columna existent
ALTER TABLE clients MODIFY COLUMN data_naixement DATETIME;

-- Canviar el nom d'una columna
ALTER TABLE clients CHANGE COLUMN data_naixement data_neix DATETIME;

-- Afegir una clau forana
ALTER TABLE comandes ADD CONSTRAINT fk_client FOREIGN KEY (client_id) REFERENCES clients(id);

-- Eliminar una clau forana
ALTER TABLE comandes DROP FOREIGN KEY fk_client;

-- Eliminar una columna
ALTER TABLE clients DROP COLUMN telefon;

-- Canviar el nom d'una taula
ALTER TABLE clients RENAME TO usuaris;
```

<br>

