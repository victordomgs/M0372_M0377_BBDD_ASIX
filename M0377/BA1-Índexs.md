# Continguts

- [Introducció als índexs](#introducció-als-índexs)
- [Tipus d'índexs segons la seva funció](#tipus-d-índexs-segons-la-seva-funció)
- [Arquitectura interna: Clustered vs Secondary](#arquitectura-interna-clustered-vs-secondary)
- [Estratègies d'indexació (Simples i Compostos)](#estratègies-d-indexació-simples-i-compostos)
- [Implementació en SQL (Sintaxi)](#implementació-en-sql-sintaxi)
- [Diagnosi amb EXPLAIN i optimització](#diagnosi-amb-explain-i-optimització)
- [Manteniment i fragmentació](#manteniment-i-fragmentació)

<br>

## Introducció als índexs
Un índex en una base de dades és una **estructura de dades addicional** que permet a MySQL trobar files específiques d'una taula de manera molt més ràpida que si hagués de revisar tot el contingut.

### L'analogia del llibre

Per entendre-ho fàcilment, imagina un llibre de text de 500 pàgines sobre "Bases de Dades":

- **Sense índex:** Si vols trobar on es parla de "Deadlocks", hauries de llegir el llibre des de la pàgina 1 fins a la 500 fins a trobar la paraula. Això en bases de dades s'anomena Full Table Scan.

- **Amb índex:** Vas a les últimes pàgines del llibre (l'índex alfabètic), busques la "D", trobes "Deadlock" i veus que apareix a la pàgina 342. Vas directament allà. L'índex és una llista ordenada que apunta a la ubicació real de la dada.

### Com funciona internament?

Quan creem un índex sobre una columna (per exemple, `cognom`), MySQL crea una estructura (normalment un B-Tree) on guarda els cognoms ordenats alfabèticament juntament amb un punter (una adreça física) a la fila real on es troba tota la informació de l'usuari.

> [!NOTE]  
> Més informació sobre B-Tree al següent [enllaç](https://es.wikipedia.org/wiki/%C3%81rbol-B).

### Avantatges i desavantatges

| Avantatges | Desavantatges |
| :--- | :--- |
| **Velocitat de lectura**: Les consultes `SELECT` amb `WHERE`, `ORDER BY` o `GROUP BY` són molt més ràpides. | **Cost d'escriptura**: Cada vegada que fas un `INSERT`, `UPDATE` o `DELETE`, MySQL ha d'actualitzar també l'índex. |
| **Eficiència**: Redueix la càrrega de treball de la CPU i els accessos a disc (I/O). | **Espai en disc**: Els índexs ocupen espai físic addicional al fitxer de dades. |
| **Optimització de JOINs**: Millora dràsticament el rendiment quan unim taules. | **Manteniment**: Els índexs es poden fragmentar amb el temps i perdre eficiència. |

### Quan actua l'Optimitzador?
L'usuari no decideix quan s'utilitza l'índex. És l'**Optimitzador de MySQL** qui analitza la consulta i decideix:
1. Si hi ha un índex disponible per a les columnes del `WHERE`.
2. Si és més ràpid fer servir l'índex o llegir tota la taula (si la taula té 5 files, l'optimitzador probablement farà un *Full Table Scan* perquè és més ràpid que obrir el fitxer de l'índex).

<br>

## Tipus d'índexs segons la seva funció

A MySQL, no tots els índexs serveixen només per anar més ràpid; alguns també s'utilitzen per garantir la integritat de les dades.

### Primary Key (Clau Primària)
És l'índex principal de la taula. 
* **Característiques:** No pot contenir valors nuls (`NOT NULL`) i cada valor ha de ser únic.
* **Particularitat:** A MySQL (motor InnoDB), la taula s'ordena físicament al disc seguint aquest índex (és el que anomenem *Clustered Index*). Només n'hi pot haver un per taula.

### Unique Index (Índex Únic)
S'utilitza per assegurar que els valors d'una columna (o conjunt de columnes) no es repeteixin.
* **Exemple:** El DNI, el correu electrònic o el nom d'usuari.
* **Diferència amb la PK:** Permet valors `NULL` (tret que s'especifiqui el contrari) i pots tenir tants índexs únics com vulguis en una taula.

### Index / Key (Índex Regular)
És l'índex estàndard. La seva única funció és millorar la velocitat de cerca.
* **Característiques:** Permet valors duplicats i valors nuls.
* **Ús:** Es posa en columnes que consultem sovint però que no són identificadors únics (per exemple: `cognom`, `data_naixement`, `categoria`).

### Full-text Index
Un índex especial dissenyat per cercar paraules clau dins de grans blocs de text (columnes `TEXT` o `VARCHAR` llargs).
* **Funció:** Permet fer cerques complexes que un simple `LIKE '%paraula%'` no podria fer de manera eficient.
* **Sintaxi de cerca:** Utilitza funcions específiques com `MATCH()` i `AGAINST()`.

### Spatial Index (Índex Espacial)
S'utilitza exclusivament per a tipus de dades geogràfiques (punts, línies, polígons).
* **Funció:** Permet trobar elements per proximitat geogràfica (ex: "busca els restaurants a menys d'1 km d'aquest punt").
* **Requisit:** Les columnes han de ser de tipus `GEOMETRY` i no poden ser nules.

### Resum comparatiu

| Tipus | Únic? | Permet NULL? | Funció Principal |
| :--- | :--- | :--- | :--- |
| **PRIMARY KEY** | Sí | No | Identificador i ordre físic |
| **UNIQUE** | Sí | Sí | Integritat de dades |
| **INDEX** | No | Sí | Velocitat de cerca |
| **FULLTEXT** | No | Sí | Cerques en text llarg |
| **SPATIAL** | No | No | Cerques geogràfiques |

<br>

## Arquitectura interna: Clustered vs Secondary

Dins del motor **InnoDB** (el motor per defecte de MySQL), els índexs no són només "llistes", sinó que determinen com s'emmagatzemen les dades físicament al disc.

### Índex Agrupat (Clustered Index)
L'índex agrupat **és la taula mateixa**. En aquest tipus d'índex, els nodes fulla de l'estructura contenen les dades reals de la fila (totes les columnes).

* **Només n'hi pot haver un:** Com que les dades només es poden ordenar d'una manera al disc, només pot existir un índex agrupat.
* **Selecció automàtica:** 1. MySQL utilitza la `PRIMARY KEY` com a índex agrupat.
    2. Si no hi ha PK, utilitza el primer `UNIQUE INDEX` que no tingui valors nuls.
    3. Si no n'hi ha cap, MySQL crea una columna interna oculta (RowID) per actuar com a clau.
* **Avantatge:** Recuperar una fila sencera per la seva clau primària és extremadament ràpid perquè no ha de fer cap segona cerca.

### Índexs Secundaris (Secondary Indexes)
Qualsevol índex que no sigui el Clustered Index (és a dir, qualsevol `INDEX` o `UNIQUE` que hagis creat a sobre de columnes normals) és un índex secundari.

* **Estructura:** Els nodes fulla d'un índex secundari **no contenen la fila sencera**. Només contenen el valor de la columna indexada i el valor de la **Clau Primària** que actua com a punter.
* **Funcionament:** Si busques per un índex secundari (ex: cercar pel cognom "Garcia"):
    1. MySQL busca "Garcia" a l'índex secundari.
    2. Troba que "Garcia" té la PK número `50`.
    3. Llavors va a l'índex agrupat (Clustered) a buscar la PK `50` per treure tota la resta de dades (nom, telèfon, adreça). Aquest segon pas s'anomena **Bookmark Lookup**.

### 3.3. L'estructura en B-Tree (Arbre B)
Tant els índexs agrupats com els secundaris utilitzen una estructura d'arbre balancejat anomenada **B-Tree**.

* **Cerca logarítmica:** En lloc de llegir `N` files, MySQL pot trobar qualsevol dada en un grapat de salts (normalment de 3 a 4 nivells, fins i tot en taules de milions de registres).
* **Ordre:** Les dades es mantenen sempre ordenades, cosa que permet cerques de rang (ex: `WHERE preu BETWEEN 10 AND 20`).

<br>

## Estratègies d'indexació (Simples i Compostos)

No totes les cerques es fan sobre una única columna. Saber quan utilitzar un índex simple o un de compost és la diferència entre una base de dades professional i una de principiant.

### Índexs simples
Són índexs creats sobre una **única columna**. 
* **Ús ideal:** Columnes que apareixen soles freqüentment en el `WHERE` o que tenen una alta varietat de valors (alta cardinalitat).
* **Exemple:** `CREATE INDEX idx_cognom ON empleats(cognom);`

### Índexs compostos (o multicolumna)
Són índexs creats sobre **dues o més columnes** alhora. MySQL guarda els valors combinats com una única entrada.
* **Sintaxi:** `CREATE INDEX idx_nom_cognom ON alumnes(cognom, nom);`
* **Ordre de les columnes:** És CRÍTIC. MySQL llegeix l'índex d'esquerra a dreta. 
    * L'índex anterior serveix per a:
        1. Cerques per `cognom` i `nom`.
        2. Cerques només per `cognom`.
    * **NO serveix** per a cerques només per `nom` (perquè el nom és la segona part de l'índex).

### La regla de l'esquerra (Left-Prefix Rule)
Imagina una guia telefònica ordenada per `Província > Ciutat > Cognom`. 
* Pots trobar algú fàcilment si saps la Província. 
* Pots trobar-lo si saps Província i Ciutat. 
* Però si només saps el Cognom, la guia no et serveix de res perquè està organitzada primer per Província.

### Índexs de prefix (Prefix Index)
Si hem d'indexar una columna `VARCHAR(255)` però sabem que els primers 10 caràcters ja són prou diferents entre si, podem indexar només el prefix per estalviar espai.
* **Exemple:** `CREATE INDEX idx_email_curt ON usuaris(email(10));`
* **Avantatge:** L'índex ocupa molt menys espai al disc i a la memòria RAM.

### Índexs de cobertura (Covering Index)
Aquest és un concepte avançat: es dóna quan **totes** les columnes que demanes al `SELECT` estan incloses en l'índex.
* **Exemple:** Si l'índex és `(cognom, nom)` i fas `SELECT nom FROM alumnes WHERE cognom = 'Garcia';`.
* **Resultat:** MySQL no ha d'anar a la taula física (Clustered Index) per res, treu la informació directament de l'estructura de l'índex secundari. És el nivell màxim d'optimització.

<br>

## Implementació en SQL (Sintaxi)

A MySQL tenim diferents maneres de gestionar els índexs. La sintaxi pot variar lleugerament depenent de si estem creant la taula des de zero o si volem afegir l'índex a una taula que ja té dades.

### Creació d'índexs en el `CREATE TABLE`

Podem definir els índexs en el mateix moment en què dissenyem la taula. És la millor pràctica si ja coneixem quines seran les consultes més freqüents.

```sql
CREATE TABLE usuaris (
    id INT AUTO_INCREMENT,
    dni VARCHAR(9) NOT NULL,
    email VARCHAR(100),
    cognom VARCHAR(50),
    biografia TEXT,
    PRIMARY KEY (id),                 -- Índex Agrupat (Clustered)
    UNIQUE INDEX idx_dni (dni),       -- Índex Únic
    INDEX idx_cognom (cognom),        -- Índex Regular
    FULLTEXT INDEX idx_bio (biografia) -- Índex per a cerques de text
);
```

### Afegir indexs a taules existents

Si la taula ja existeix, tenim dues opcions pràcticament idèntiques. La més estàndard és `ALTER TABLE`.

#### Opció A: Utilitzant ALTER TABLE

```sql
-- Afegir un índex regular
ALTER TABLE usuaris ADD INDEX idx_email (email);

-- Afegir un índex únic
ALTER TABLE usuaris ADD UNIQUE INDEX idx_email_unic (email);
```

#### Opció B: Utilitzant CREATE INDEX

```sql
-- Sintaxi: CREATE INDEX nom_index ON taula(columna);
CREATE INDEX idx_cognom ON usuaris(cognom);

-- Índex compost (multicolumna)
CREATE INDEX idx_nom_complet ON usuaris(cognom, nom);
```

### Visualització dels índexs

Per saber quins índexs té una taula i comprovar si estan ben creats, utilitzem:

```sql
SHOW INDEX FROM usuaris;
```

Aquesta comanda ens mostrarà informació clau com la Cardinalitat (quants valors únics hi ha) i si l'índex permet valors nuls.

### Eliminació d`índexs

Si detectem que un índex no s'utilitza o que està alentint massa les operacions d'`INSERT`, l'hem d'esborrar.

```sql
-- Mitjançant ALTER TABLE (més comú)
ALTER TABLE usuaris DROP INDEX idx_cognom;

-- Les claus primàries s'esborren d'una manera especial
ALTER TABLE usuaris DROP PRIMARY KEY;
```

<br>

## Diagnosi amb EXPLAIN i optimització

Com sabem si MySQL realment està fent servir l'índex que hem creat? Per no haver d'endevinar-ho, utilitzem l'eina de diagnosi més potent de la qual disposem: la sentència `EXPLAIN`.

### La sentència EXPLAIN
Si posem la paraula `EXPLAIN` davant de qualsevol consulta `SELECT`, MySQL no executarà la consulta, sinó que ens mostrarà el **Pla d'Execució** (com pensa buscar les dades).

```sql
EXPLAIN SELECT * FROM usuaris WHERE cognom = 'Garcia';
```

### Columnes clau del resultat d'EXPLAIN:

- **type:** Indica com es busquen les dades.

ALL: Mal senyal. Significa Full Table Scan (llegeix tota la taula).

index: Llegeix tot l'índex (millor que ALL, però no ideal).

range: Molt bé. Busca en un rang de l'índex.

ref o const: Excel·lent. Troba les dades directament per l'índex.

- **possible_keys:** Llista d'índexs que MySQL podria fer servir.

- **key:** L'índex que MySQL **ha decidit fer servir** finalment.

- **rows:** Nombre estimat de files que MySQL haurà d'examinar. Com més baix, millor.

- **Extra:** Informació addicional (Ex: Using index significa que és un índex de cobertura).

### Quan un índex NO funciona

De vegades creem un índex però MySQL el ignora. Això sol passar per:

1. **Funcions sobre la columna:** Si fas `WHERE YEAR(data) = 2023`, MySQL no pot fer servir l'índex de data. Caldria fer `WHERE data BETWEEN '2023-01-01' AND '2023-12-31'`.

2. **Comodí al principi:** Un `LIKE '%garcia'` invalida l'índex. Un `LIKE 'garcia%'` sí que el pot fer servir.

3. **Baixa cardinalitat:** Si la columna només té dos valors (com "Sexe" o "Actiu/Inactiu"), MySQL sovint prefereix llegir tota la taula perquè l'índex no ajuda a filtrar prou.

### La Cardinalitat

Aquest és un concepte que els alumnes veuran al fer `SHOW INDEX`.

- **Alta Cardinalitat:** Molts valors diferents (ex: DNI, Email). Són els millors candidats per ser indexats.
- **Baixa Cardinalitat:** Molts valors repetits (ex: Estat Civil, País). Indexar aquestes columnes sol ser ineficient.

<br>

## Manteniment i fragmentació

Els índexs no són estructures estàtiques. A mesura que la taula rep operacions de `INSERT`, `UPDATE` i `DELETE`, l'estructura interna del B-Tree es pot desordenar i degradar.

### 7.1. Què és la fragmentació?
Quan esborrem files o actualitzem dades, queden "buits" o espais buits dins de les pàgines de l'índex al disc. 
* **Fragmentació interna:** Les pàgines de l'índex estan mig buides, ocupant més espai del necessari.
* **Fragmentació externa:** Les pàgines de l'índex no estan en ordre físic seqüencial, el que obliga al disc dur a fer més moviments per llegir-les.

**Conseqüència:** Les consultes es tornen més lentes tot i tenir índexs, perquè MySQL ha de llegir moltes més pàgines de disc per obtenir la mateixa informació.

### Com detectar la fragmentació?
Podem consultar l'estat de les taules per veure quant espai "sobra" (espai buit que es podria reaprofitar):

```sql
SHOW TABLE STATUS LIKE 'nom_de_la_taula';
```

Ens hem de fixar en la columna **Data_free**. Si aquest valor és molt alt en comparació amb la mida total de la taula, tenim un problema de fragmentació.

### Com solucionar-ho: OPTIMIZE TABLE

La manera més senzilla de "netejar" i reordenar els índexs a MySQL (motor InnoDB) és reconstruint la taula. Això compacta les dades i regenera els índexs de zero de manera ordenada.

```sql
OPTIMIZE TABLE usuaris;
```

#### Què fa exactament aquesta comanda?

1. Crea una còpia temporal de la taula.
2. Copia les dades de manera ordenada i sense buits.
3. Elimina la taula vella i reanomena la nova.
4. Regenera tots els índexs perquè siguin 100% eficients.

### El perill dels índexs no utilitzats

Tenir índexs que cap consulta fa servir és un error greu de manteniment.

- Alenteixen cada `INSERT` innecessàriament.
- Ocupen espai a la memòria RAM (Buffer Pool) que podria fer servir una altra consulta.

> [!NOTE]  
> **Recomanació:** És bona pràctica auditar els índexs un cop l'any (o cada cert temps segons l'ús) i eliminar aquells que no s'utilitzin.
