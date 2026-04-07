# Control del llenguatge de dades (DCL) en PostgreSQL

## Introducció
El **Data Control Language (DCL)** en PostgreSQL s'utilitza per gestionar els privilegis i permisos dels usuaris en la base de dades. Les instruccions principals de DCL són `GRANT` i `REVOKE`, que permeten respectivament atorgar i revocar permisos sobre objectes de la base de dades com taules, vistes, seqüències i esquemes.

A PostgreSQl, el model de seguretat pivota sobre un concepte unificat: el rol. Aquesta entitat és extremadament versàtil, ja que actua simultàniament com a usuari (identitat individual) i com a grup (conjunt de permisos).

### Com funciona la jerarquia de rols?

- **Proprietat d'objectes:** Un rol pot ser el propietari de taules, vistes o funcions. Com a propietari, té control total sobre l'objecte i la potestat de decidir quins altres rols hi poden interactuar.
- **Gestió de privilegis:** Els rols permeten definir qui pot llegir, escriure o modificar dades mitjançant l'assignació de privilegis específics.
- **Herència i membreia:** Pots fer que un rol sigui "membre" d'un altre. Això crea una estructura jeràrquica on el rol membre "hereta" o adopta les capacitats i permisos del rol superior.

<br>

## Comandes essencials de psql

## Ordres essencials del client `psql`

A diferència de les sentències SQL, les ordres de gestió de `psql` comencen amb barra invertida (`\`) i no requereixen punt i coma al final.

### Gestió i Navegació

| Ordre | Descripció | Equivalent MySQL |
| :--- | :--- | :--- |
| `\l` | Llista totes les bases de dades del servidor | `SHOW DATABASES` |
| `\c <DB>` | Connecta a una base de dades específica | `USE <DB>` |
| `\i <fitxer>` | Executa un script SQL des d'un fitxer | `SOURCE <fitxer>` |
| `\q` | Surt de l'entorn `psql` | `EXIT` |
| `\x` | Activa/desactiva la visualització estesa de columnes | `\G` |

### Inspecció d'Estructura i Privilegis

| Ordre | Descripció |
| :--- | :--- |
| `\d` | Mostra totes les taules, vistes i seqüències |
| `\d <taula>` | Mostra l'estructura detallada d'una taula |
| `\dt` | Mostra només les taules de la base de dades |
| `\du` | Llista els rols (usuaris) i els seus privilegis globals |
| `\dp` | Mostra els privilegis d'accés detallats dels objectes |
| `\dn+` | Mostra els esquemes i els seus privilegis definits |

> [!TIP]
> Pots utilitzar comodins per filtrar la cerca. Per exemple, `\dt *.*` mostra totes les taules de tots els esquemes , i `\d esquema.taula` detalla una taula en un esquema concret.

<br>

## Creació de rols

Per crear un rol ho podem fer de dues formes:

- Amb l'ordre del sistema `createuser`.
- Amb les ordres SQL `CREATE ROLE` o `CREATE USER`.

> [!NOTE]  
> `CREATE USER` és idèntic a `CREATE ROLE` però pressuposa el privilegi `LOGIN`.

### Atributs de rol: poders especials del sistema

Més enllà dels permisos sobre taules, PostgreSQL permet assignar atributs de sistema als rols per definir què poden fer dins del servidor global.

- `SUPERUSER` (L'Administrador Total): Concedeix control absolut sobre tot el SGBD, ignorant qualsevol restricció de permisos. És l'atribut que té per defecte l'usuari postgres. Compte: Un superusuari pot fer qualsevol cosa, per tant, s'ha de fer servir amb extrema precaució.

- `CREATEDB` (Creador de Bases de Dades): Permet al rol crear noves bases de dades de manera autònoma. L'usuari que crea la BD esdevé automàticament el seu "owner" (propietari).

- `CREATEROLE` (Gestor d'Identitats): Atorga la capacitat de crear, modificar o eliminar altres rols. És ideal per a administradors de seguretat que no necessiten ser superusuaris però sí gestionar qui entra al sistema.

- `REPLICATION` (Permís de Rèplica): Un privilegi tècnic d'alt nivell. Permet que el rol (que també ha de tenir LOGIN) es connecti al servidor en mode replicació per copiar dades en temps real cap a un altre servidor secundari (mirroring).

- `PASSWORD` (Autenticació): Defineix la clau d'accés necessària per connectar-se. Sense una contrasenya (o un altre mètode d'autenticació configurat), el rol no podria validar la seva identitat des d'un client extern.

> [!NOTE]  
> `PASSWORD` només s'utilitza si l'usuari del PostgreSQL no es correspon amb un usuari del sistema operatiu i necessita, per tant, un sistema d'autenticació.

### Connexió i Vinculació: Usuaris del SO vs. PostgreSQL

A PostgreSQL, la manera com un usuari s'autentica depèn sovint de si el seu nom de rol coincideix amb el seu nom d'usuari a Linux/Windows. Això es divideix en dues grans modalitats:

#### 1. Usuaris Vinculats (Autenticació peer o ident)

Aquest model es basa en la confiança en el Sistema Operatiu.

- **Com funciona:** Si existeix un usuari al SO anomenat pep i creem un rol a PostgreSQL anomenat pep, el servidor pot assumir que si ja has iniciat sessió a l'ordinador, ja ets qui dius ser.

- **Avantatges:** No cal gestionar una contrasenya addicional dins de la base de dades.

- **Limitacions:** Normalment, només funciona per a connexions locals (dins del mateix servidor).

L'usuari ha d'estar validat prèviament pel SO.

#### 2. Usuaris No Vinculats (Autenticació per Contrasenya)

Són rols independents que no tenen per què existir com a usuaris reals de la màquina on corre el servidor.

- **Com funciona:** L'usuari es valida directament contra PostgreSQL mitjançant una credencial pròpia.

- **Característiques:** Permeten la connexió remota des de qualsevol altre equip de la xarxa.

- **Seguretat:** PostgreSQL emmagatzema un hash de la contrasenya per validar l'accés de manera independent al SO.

S'han de crear obligatòriament amb l'opció `PASSWORD 'clau'`.

### Exemples de creació d'usuaris i rols

El rol `marta` podrà connectar-se al PostgreSQL (és un usuari), i crear noves bases de dades. No es correspon amb un usuari del sistema operatiu, així que
li hem assignat una contrasenya:

```SQL
CREATE ROLE marta LOGIN CREATEDB PASSWORD '12345';
```

El mateix rol es pot crear amb `CREATE USER`, ja que té el privilegi `LOGIN`:

```SQL
CREATE USER marta CREATEDB PASSWORD '12345';
```

### Exemple d'eliminació de rols

Per eliminar un rol podem utilitzar `DROP ROLE` des del PostgreSQL o `dropuser` des del terminal.

Per eliminar el rol que hem creat a l'exemple anterior podem utilitzar:

```SQL
DROP ROLE marta;
```

O des del terminal:

```
$ dropuser marta
```

### Ús de rols com a grups d'usuaris

A mesura que un sistema creix, gestionar permisos usuari per usuari es torna ineficient i perillós. La solució a PostgreSQL és l'herència de rols, que permet crear estructures de permisos organitzades i fàcils de mantenir.

Aquí tens una reescriptura optimitzada d'aquest concepte:

#### Gestió de Rols de Grup (Herència de Permisos)

En lloc d'assignar permisos directament a cada persona, el més recomanable és crear Rols de Grup. Aquests rols actuen com a "contenidors" de privilegis.

1. Crear el Rol de Grup:

Per a un grup de permisos, normalment no volem que ningú es connecti directament amb el nom del grup, per tant, no li assignem el privilegi `LOGIN`.

```SQL
CREATE ROLE admins; -- Rol sense LOGIN (funciona com a grup)
GRANT ALL PRIVILEGES ON SCHEMA public TO admins;
```

2. Assignar Usuaris al Grup (GRANT):
   
Perquè un usuari "hereti" els poders d'un grup, utilitzem la comanda `GRANT` seguida del nom del grup. Això el fa membre del rol.

```SQL
-- Afegim en Joan i la Maria al grup d'administradors
GRANT admins TO joan, maria;
```

3. Revocar la Pertinença (REVOKE):

```sql
-- En Joan deixa de ser administrador
REVOKE admins FROM joan;
```

#### Avantatges d'aquest model

- **Administració centralitzada:** Si canvien els permisos del departament (per exemple, s'afegeix una nova taula), només cal modificar el permís al rol `admins` i automàticament tots els seus membres tindran l'accés actualitzat.

- **Seguretat i claredat:** És molt més fàcil auditar qui pertany al grup `comptabilitat` que revisar centenars de línies de permisos individuals.

- **Escalabilitat:** Pots crear jerarquies complexes, on un grup (ex. `caps_area`) sigui membre d'un altre grup (ex. `empleats`), heretant així tots els permisos de la base.

Si un usuari canvia de departament o ja no necessita aquells privilegis, simplement el traiem del grup. Això és molt més ràpid que revocar cada permís individual sobre les taules.

<br>

## L'Herència de Privilegis: Automàtica vs. Manual

PostgreSQL permet que un usuari adopti els permisos dels seus grups de dues maneres diferents, depenent de l'atribut `INHERIT`.

### El comportament per defecte: INHERIT

Si un rol té l'atribut `INHERIT`, pot utilitzar immediatament qualsevol permís atorgat als grups dels quals és membre.

- **Exemple:** Si el grup personal pot llegir nomines, i en Pere és membre de personal, en Pere pot fer `SELECT` directament.

### El control manual: NOINHERIT

Si es crea un rol amb `NOINHERIT`, l'usuari no té els permisos del grup en connectar-se. Ha d'executar una comanda per "activar" el grup:

- **Comanda:** `SET ROLE nom_del_grup`;

- **Efecte:** És equivalent al su de Linux. En aquell moment, l'usuari "es converteix" en el grup, guanyant els seus permisos però perdent temporalment els propis.

- **Retorn:** Per tornar a la identitat original, s'usa `SET ROLE NONE` o `RESET ROLE`.

> [!NOTE]  
> Els atributs de sistema com `SUPERUSER`, `CREATEDB` o `CREATEROLE` mai s'hereten. Per usar-los des d'un grup, cal fer `SET ROLE` obligatòriament.

<br>

## Privilegis

Els privilegis que es poden definir a un objecte depenen del tipus d'objecte de què es tracti.

La taula següent mostra privilegis habituals segons el tipus d'objecte:

### Privilegis segons el tipus d'objecte

| Privilegi | Base de dades | Esquema | Taula | Columna | Seqüència | Funció |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **SELECT** | | | X | X | X | |
| **INSERT** | | | X | X | | |
| **UPDATE** | | | X | X | X | |
| **DELETE** | | | X | X | | |
| **TRUNCATE** | | | X | | | |
| **REFERENCES** | | | X | X | | |
| **TRIGGER** | | | X | | | |
| **USAGE** | | X | | | X | |
| **CREATE** | X | X | | | | |
| **CONNECT** | X | | | | | |
| **TEMP** | X | | | | | |
| **EXECUTE** | | | | | | X |

El significat d'aquests privilegis és el següent:

- `SELECT`, `INSERT`, `UPDATE`, `DELETE`, `TRUNCATE`: permeten realitzar les
operacions corresponents sobre l'objecte (taula, columna o vista), així com
altres operacions que necessiten d'aquests permisos per poder-se realitzar
(per exemple, si per fer un `UPDATE` necessitem referir-nos a valors que ja
existeixen, haurem de tenir també `SELECT`).

- `REFERENCES`: permet la creació de claus foranes. Necessitem tenir aquest
permís sobre les dues taules afectades.

- `TRIGGER`: permet la creació de disparadors sobre una taula.

- `CREATE`: depenent d'on s'hagi atorgat el privilegi, permet la creació
d'esquemes, taules, índexs, etc. dins de l'objecte. Per exemple, necessitem
_CREATE_ sobre una base de dades per poder crear un esquema a dins, i
necessitem _CREATE_ sobre un esquema per crear-hi taules.

- `CONNECT`: permet a un usuari connectar-se a una base de dades.

- `TEMP`: permet la creació de taules temporals en una base de dades.

- `EXECUTE`: permet l'execució d'una determinada funció.

- `USAGE`: depèn de l'objecte on s'assigna. Pot permetre l'ús d'un determinat
llenguatge de programació per crear nous procediments, o la consulta dels
objectes que hi ha creats en un determinat esquema.

- `ALL PRIVILEGES`: atorga tots els privilegis sobre un objecte.

### Assignació de privilegis

Per assignar privilegis a rols que no siguin el propietari d'un objecte
s'utilitza la instrucció `GRANT`.

#### Privilegis per taules

```SQL
GRANT { { SELECT | INSERT | UPDATE | DELETE | TRUNCATE | REFERENCES | TRIGGER }
    [, ...] | ALL [ PRIVILEGES ] }
    ON { [ TABLE ] table_name [, ...]
         | ALL TABLES IN SCHEMA schema_name [, ...] }
    TO role_specification [, ...] [ WITH GRANT OPTION ]
```

#### Assignació de privilegis a taules


- Dóna permís de lectura a totes les taules de `pagila` a l'usuari `joan`:

```SQL
GRANT SELECT ON ALL TABLES IN SCHEMA pagila TO joan;
```

- Dóna permís de modificació sobre les taules `film`, `film_actor` i `actor` a l'usuari `joan`.

```SQL
GRANT INSERT, UPDATE, DELETE ON film, film_actor, actor TO joan;
```

- Dóna tots els permisos sobre totes les taules de `pagila` a l'usuari `maria`:

```SQL
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA pagila TO maria;
```

#### Assignació de privilegis per columnes

```SQL
GRANT { { SELECT | INSERT | UPDATE | REFERENCES } ( column_name [, ...] )
    [, ...] | ALL [ PRIVILEGES ] ( column_name [, ...] ) }
    ON [ TABLE ] table_name [, ...]
    TO role_specification [, ...] [ WITH GRANT OPTION ]
```

#### Assignació de privilegis a columnes

- Permet actualitzar el preu de lloguer, la durada de lloguer, i el cost de
reemplaçament de les pel·lícules als usuaris `mike` i `jon`:

```SQL
GRANT UPDATE (rental_rate, rental_duration, replacement_cost)
ON film TO mike, jon;
```

#### Privilegis per taules

```SQL
GRANT { { USAGE | SELECT | UPDATE }
    [, ...] | ALL [ PRIVILEGES ] }
    ON { SEQUENCE sequence_name [, ...]
         | ALL SEQUENCES IN SCHEMA schema_name [, ...] }
    TO role_specification [, ...] [ WITH GRANT OPTION ]
```

#### Privilegis per bases de dades

```SQL
GRANT { { CREATE | CONNECT | TEMPORARY | TEMP } [, ...] | ALL [ PRIVILEGES ] }
    ON DATABASE database_name [, ...]
    TO role_specification [, ...] [ WITH GRANT OPTION ]
```

#### Privilegis per funcions

```SQL
GRANT { EXECUTE | ALL [ PRIVILEGES ] }
    ON { FUNCTION function_name ( [ [ argmode ] [ arg_name ] arg_type [, ...] ] ) [, ...]
         | ALL FUNCTIONS IN SCHEMA schema_name [, ...] }
    TO role_specification [, ...] [ WITH GRANT OPTION ]
```

#### Privilegis per esquemes

```SQL
GRANT { { CREATE | USAGE } [, ...] | ALL [ PRIVILEGES ] }
    ON SCHEMA schema_name [, ...]
    TO role_specification [, ...] [ WITH GRANT OPTION ]
```

#### Revocació de privilegis

```SQL
=> REVOKE CREATE ON myschema FROM joan;
```
