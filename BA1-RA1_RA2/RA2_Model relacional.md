# Model relacional

- [Introducció](#introducció)
- [El model relacional](#el-model-relacional)
- [Estructura del model relacional](#estructura-del-model-relacional)
- [Restriccions del model relacional](#restriccions-del-model-relacional)
- [Transformació d'un esquema E-R a un esquema relacional](#transformació-dun-esquema-ER-a-un-esquema-relacional)

## Introducció

Ja s’ha vist a la unitat anterior què és un sistema gestor de bases de dades i quins són els seus objectius. També es va veure l’arquitectura ANSI per a la seva aplicació en la creació de les bases de dades. S’han estudiat diferents models conceptuals de dades, però no s’ha arribat a realitzar cap disseny lògic.
Aquesta unitat tracta del que actualment és el principal model per a les aplicacions de processament de dades: el model relacional, que presenta una forma molt simple i potent de representar les dades.
Al llarg de la unitat s’exposen els fonaments del model de dades relacional i la seva aplicació per al disseny lògic de dades i de bases de dades relacionals.

## El model relacional

El model de dades relacional va ser desenvolupat per E.F. Codd per a IBM a finals dels anys seixanta. Proposa un model basat en la teoria matemàtica de les relacions, amb l’objectiu de mantenir la independència de l’estructura lògica respecte al mode d’emmagatzematge i altres característiques de tipus físic. El model de Codd persegueix, igual que la majoria dels models de dades, els següents objectius:

- **Independència física de les dades.** La manera d’emmagatzemar les dades no ha d’influir en la seva manipulació lògica.
- **Independència lògica de les dades.** Els canvis que es facin en els objectes de la base de dades no han de repercutir en els programes i usuaris que hi accedeixen.
- **Flexibilitat.** Per presentar als usuaris les dades de la manera més adequada segons l’aplicació que utilitzin.
- **Uniformitat** en la presentació de les estructures lògiques de les dades, que són taules, fet que facilita la concepció i manipulació de la base de dades per part dels usuaris.
- **Simplicitat.** Ja que les característiques anteriors, així com uns llenguatges d’usuari senzills, fan que aquest model sigui fàcil de comprendre i d’utilitzar per l’usuari.

Per aconseguir aquests objectius, Codd introdueix el concepte de relació (taula) com a estructura bàsica del model. Totes les dades d’una base de dades es representen en forma de relacions el contingut de les quals varia en el temps. El model relacional es basa en dues branques de les matemàtiques: la teoria de conjunts i la lògica de predicats. Això fa que sigui un model segur i robust.

L’any 1985, Codd publica les seves famoses dotze regles, analitzant alguns dels productes comercials de l’època, que ha de complir qualsevol base de dades per ser considerada relacional:

- **Regla d’informació.** Tota la informació d’una base de dades relacional està representada mitjançant valors en taules.
- **Regla d’accés garantit.** Es garanteix que totes les dades d’una base relacional són lògicament accessibles a través d’una combinació de nom de taula, valor de clau primària i nom de columna.
- **Tractament sistemàtic de valors nuls.** Els valors nuls es suporten en els SGBD per representar la manca d’informació d’una manera sistemàtica i independent dels tipus de dades.
- **Catàleg en línia dinàmic basat en el model relacional.** La descripció de la base de dades es representa en l’àmbit lògic de la mateixa manera que les dades ordinàries, de manera que els usuaris autoritzats poden accedir-hi utilitzant el mateix llenguatge relacional.
- **Regla de subllenguatge complet de dades.** Un sistema relacional pot suportar diversos llenguatges i diversos modes d’ús terminal. Tanmateix, hi ha d’haver almenys un llenguatge les sentències del qual es puguin expressar mitjançant alguna sintaxi ben definida, com cadenes de caràcters, i que ofereixi completament tots els punts següents:
- **Definició de dades.**
- **Definició de vistes.**
- **Manipulació de dades (interactiva i per programa).**
- **Restriccions d’integritat.**
- **Autorització.**
- **Gestió de transaccions (inici, confirmació i retrocés).**
- **Regla d’actualització de vista.** Totes les vistes que siguin teòricament actualitzables també han de ser actualitzables pel sistema.
- **Inserció, actualització i supressió d’alt nivell.** La capacitat de manejar una relació de base de dades o una relació derivada com un únic operand s’aplica no només a la recuperació de dades, sinó també a la seva inserció, actualització i supressió.
- **Independència física de les dades.** Els canvis que es facin tant en la representació de l’emmagatzematge com en els mètodes d’accés no han d’afectar ni els programes d’aplicació ni les activitats amb les dades.
- **Independència lògica de les dades.** De la mateixa manera, els canvis que es facin sobre les taules de la base de dades no han de modificar ni els programes ni les activitats amb les dades.
- **Independència de la integritat.** Les restriccions d’integritat específiques d’una base de dades relacional han d’estar definides mitjançant el subllenguatge de dades relacional i emmagatzemades al catàleg de la base de dades.
- **Independència de la distribució.** Un SGBD és independent de la distribució.
- **Regla de no subversió.** Si un SGBDR té un llenguatge de baix nivell (una fila cada vegada), aquest no es pot utilitzar per destruir o evitar les regles d’integritat o les restriccions expressades en el llenguatge relacional d’alt nivell (diverses files alhora).

El model relacional proposa una representació de la informació que:
- Origini esquemes que representin fidelment la informació, els objectes i les relacions existents entre ells que formen el domini del problema.
- Sigui fàcilment entesa pels usuaris.
- Permeti ampliar l’esquema de la base de dades sense modificar l’estructura lògica existent ni els programes d’aplicació.
- Permeti la màxima flexibilitat en la formulació de les consultes sobre la informació mantinguda a la base de dades.

## Estructura del model relacional

Com ja s’ha indicat anteriorment, la **relació** és l’element bàsic del **model relacional** i es representa com una **taula**, en la qual es poden distingir el **nom de la taula**, el **conjunt de columnes** que representen les propietats de la taula i que s’anomenen **atributs**, i el **conjunt de files** anomenades **tuples**, que contenen els valors que pren cadascun dels atributs per a cada element de la relació.

Una relació té una sèrie d’elements característics que la distingeixen d’una taula:
– No admet files duplicades.
– Les files i les columnes no estan ordenades.
– La taula és plana. En la intersecció d’una fila i una columna només pot haver-hi un valor; no s’admeten atributs multivaluats.

A la figura que es mostra a continuació es representa una relació anomenada **ALUMNE** en forma de taula.

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA1-RA1_RA2/images/Figura%2016.%20Representaci%C3%B3%20de%20una%20relaci%C3%B3%20en%20forma%20de%20taula.png" alt="SGBD" width="650" height="auto"/>
    <p><em>Figura 16: Representació d'una relació en forma de taula. Fuente: Sistemas gestores de bases de datos. (Ramos)</em></p>
  </div>

A continuació, s’exposen els elements que constitueixen el model relacional.

### A. Dominis i atributs

Es defineix **domini** com el conjunt finit de valors homogenis (tots del mateix tipus) i atòmics (indivisibles) que pot prendre cada atribut. Els valors continguts en una columna pertanyen a un domini que prèviament s’ha definit.
Tots els dominis tenen un **nom** i un **tipus de dada** associat. Existeixen dos tipus de dominis:

- **Dominis generals.** Són aquells els valors dels quals estan compresos entre un màxim i un mínim. Per exemple, el Codi_postal, format per tots els nombres enters positius de 5 xifres.
- **Dominis restringits.** Són els que pertanyen a un conjunt específic de valors. Per exemple, Sexe, que pot prendre els valors H o D.

Es defineix **atribut** com el paper o rol que desenvolupa un domini en una relació. Representa l’ús d’un domini per a una relació determinada. L’atribut aporta un **significat semàntic** a un domini. Per exemple, en la relació ALUMNE podem considerar els dominis següents:
- Atribut **NUM_MAT**, domini: conjunt d’enters formats per 4 dígits.
- Atribut **NOMBRE**, domini: conjunt de 15 caràcters.
- Atribut **APELLIDOS**, domini: conjunt de 20 caràcters.
- Atribut **CURSO**, domini: conjunt de 7 caràcters.

### B. Relacions

La **relació** es representa mitjançant una taula amb files i columnes. Un SGBD només necessita que l’usuari pugui percebre la base de dades com un conjunt de taules. Aquesta percepció només s’aplica a l’**estructura lògica** de la base de dades (nivell extern i conceptual de l’arquitectura de tres nivells ANSI-SPARC). No s’aplica a l’estructura física de la base de dades, que es pot implementar amb diferents estructures d’emmagatzematge.

En el model relacional, les relacions s’utilitzen per emmagatzemar informació sobre els objectes representats en la base de dades. Es representen gràficament com una **taula bidimensional** en què les files corresponen a registres individuals i les columnes als camps o atributs d’aquests registres.

La relació està formada per:
- **Atribut (columna).** Cada una de les columnes de la taula. Les columnes tenen un nom i poden contenir un conjunt de valors. Una columna s’identifica sempre pel seu nom, mai per la seva posició. L’ordre de les columnes en una taula és irrellevant.
- **Tupla (fila).** Representa una fila de la taula. A la Figura 17 apareix la taula EMPLEAT amb dues files o tuples.

De les taules se’n deriven els conceptes següents:
- **Cardinalitat.** És el nombre de files de la taula. En l’exemple anterior és dos.
- **Grau.** És el nombre de columnes de la taula. En l’exemple anterior, el grau és cinc.
- **Valor.** Està representat per la intersecció entre una fila i una columna. Per exemple, són valors de la taula EMPLEAT: 13407877B, Milagros Suela Sarro, 1500.
- **Valor nul.** Representa l’absència d’informació.

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA1-RA1_RA2/images/Figura%2017.%20Taula%20EMPLEAT%20amb%20dues%20files.png" alt="SGBD" width="650" height="auto"/>
    <p><em>Figura 17: Taula EMPLEADO amb dues files. Fuente: Sistemas gestores de bases de datos. (Ramos)</em></p>
  </div>

### C. Claus

En una relació no hi ha tuples repetides. S’identifiquen de manera única mitjançant els valors dels seus atributs. Cada fila ha d’estar associada a una clau que permeti identificar-la. De vegades, la fila es pot identificar amb un únic atribut, però altres vegades és necessari recórrer a més d’un atribut.

La clau ha de complir dos requisits:
- **Identificació unívoca.** En cada fila de la taula, el valor de la clau ha d’identificar-la de manera única.
- **No redundància.** No es pot descartar cap atribut de la clau per identificar la fila.

Es defineix **clau candidata** d’una relació com el conjunt d’atributs que identifiquen de manera **unívoca i mínima** (només els necessaris per identificar la tupla) cada tupla de la relació. Sempre hi ha almenys una clau candidata, ja que per definició no pot haver-hi dues tuples iguals. Hi haurà, doncs, un o més atributs que identifiquin la tupla.

Una relació pot tenir més d’una clau candidata, entre les quals es distingeixen:
- **Clau primària o principal (primary key):** és aquella clau candidata que l’usuari tria per identificar les tuples de la relació. No pot tenir valors nuls. Si només existeix una clau candidata, aquesta s’escollirà com a clau primària.
- **Clau alternativa:** són aquelles claus candidates que no han estat escollides com a clau primària.

S’anomena **clau aliena (foreign key)** d’una relació R1 al conjunt d’atributs els valors dels quals han de coincidir amb els valors de la **clau primària** d’una altra relació R2. Totes dues claus han d’estar definides sobre el mateix domini i són molt importants en l’estudi de la **integritat de dades** del model relacional.

#### Exemple

Disposem de les taules TDEPART i TEMPLE. Les columnes de la taula TDEPART són:
Nº de departament (NUMDEPT), Nom de departament (NOMDEPT), Pressupost (PRESUPUESTO).
Les claus candidates són el Nº de departament i el Nom de departament, ja que són únics i no es repetiran. Podem escollir com a clau primària el NUMDEPT. Normalment, es triaran com a claus primàries els camps de codis o números d’empleat, departament o article, que solen ser numèrics.

Les columnes de la taula TEMPLE són:
Nº d’empleat (NUMEMP), Cognom (APELLIDO), Nº de departament (NUMDEP), Salari (SALARIO).
La clau candidata és el NUMEMP. El cognom es pot repetir, per tant no es considera candidata. Com que només n’hi ha una, s’escull com a clau primària el NUMEMP.
L’atribut NUMDEP és una clau aliena; relaciona les taules TEMPLE i TDEPART.

La Figura 18 mostra el contingut de les taules TEMPLE i TDEPART amb les seves claus primàries i alienes.

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA1-RA1_RA2/images/Figura%2018.%20Taules%20TEMPLE%20i%20TDEPART%20amb%20les%20claus%20prim%C3%A0ries%20i%20foranas.png" alt="SGBD" width="850" height="auto"/>
    <p><em>Figura 18: Taula TEMPLE i TDEPART amb les claus primàries i foranes. Fuente: Sistemas gestores de bases de datos. (Ramos)</em></p>
  </div>

### D. Esquema de la base de dades

Una **base de dades relacional** és un conjunt de relacions normalitzades. Per representar l’**esquema** d’una base de dades, s’han d’indicar el **nom de les relacions**, els **atributs** d’aquestes, els **dominis** sobre els quals es defineixen aquests atributs, així com les **claus primàries** i les **claus alienes**.

En l’esquema, els noms de les relacions apareixen seguits dels noms dels atributs entre parèntesis. Les **claus primàries** són els atributs **subratllats**, i les **claus alienes** es representen mitjançant **diagrames referencials**.

L’esquema de la base de dades **d’empleats i departaments** és el següent:


TDEPART (**NUMDEPT**, NOMDEPT, PRESUPUESTO)

TEMPLE (**NUMEMP**, APELLIDO, NUMDEP, SALARIO)

  TEMPLE --> TEDEPART: Departamento al que pertenece el empleado


## Restriccions del model relacional

En tots els models de dades existeixen **restriccions** que, a l’hora de dissenyar una base de dades, s’han de tenir en compte. Les dades emmagatzemades a la base de dades han d’adaptar-se a les estructures imposades pel model i han de complir un conjunt de **regles** per garantir que són correctes.

El **model relacional** imposa dos tipus de restriccions. Algunes d’aquestes ja s’han esmentat en les propietats de les relacions i de les claus. Els tipus de restriccions són:

- **Restriccions inherents al model:** indiquen les característiques pròpies d’una relació que s’han de complir obligatòriament i que la diferencien d’una taula. No hi pot haver dues tuples iguals, l’ordre de les tuples i dels atributs no és rellevant, i cada atribut només pot prendre un únic valor del domini al qual pertany. Cap atribut que formi part de la clau primària d’una relació pot prendre un valor nul.
- **Restriccions semàntiques o de l’usuari:** representen la semàntica del món real. Aquestes fan que les ocurrències dels esquemes de la base de dades siguin vàlides. Els mecanismes que proporciona el model per a aquest tipus de restriccions són els següents:

a) **Restricció de clau primària (PRIMARY KEY):** permet declarar un o diversos atributs com a clau primària d’una relació.

b) **Restricció d’unicitat (UNIQUE):** permet definir claus alternatives. Els valors dels atributs no es poden repetir.

c) **Restricció d’obligatorietat (NOT NULL):** permet declarar si un o diversos atributs no poden prendre valors nuls.

d) **Integritat referencial o restricció de clau aliena (FOREIGN KEY):** s’utilitza per enllaçar relacions, mitjançant claus alienes, dins d’una base de dades. La integritat referencial indica que els valors de la clau aliena en la relació filla corresponen als de la clau primària en la relació pare.

A més de definir les claus alienes, cal tenir en compte les operacions de **supressió i actualització** que es realitzen sobre les tuples de la relació referenciada. Les possibilitats són:

- **Supressió i/o modificació en cascada (CASCADE):** la supressió o modificació d’una tupla en la relació pare (la que conté la clau primària) provoca la supressió o modificació de les tuples relacionades en la relació filla (la que conté la clau aliena). Per exemple, en el cas d’empleats i departaments, si s’esborra un departament de la taula TDEPART, s’esborraran els empleats que pertanyen a aquell departament. Igualment, si es modifica el NUMDEPT de la taula TDEPART, aquest canvi s’aplicarà als empleats del mateix departament.
- **Supressió i/o modificació restringida (RESTRICT):** en aquest cas no és possible realitzar la supressió o la modificació de les tuples de la relació pare si existeixen tuples relacionades a la relació filla. És a dir, no es podria esborrar un departament que tingui empleats.
- **Supressió i/o modificació amb assignació a nuls (SET NULL):** aquesta restricció permet posar la clau aliena de la taula referenciada a NULL si es produeix la supressió o modificació a la taula primària. Així, si s’esborra un departament, als empleats d’aquell departament se’ls assignarà NULL a l’atribut NUMDEPT.
- **Supressió i/o modificació amb assignació de valor per defecte (SET DEFAULT):** en aquest cas, el valor assignat a les claus alienes de la taula referenciada és un valor per defecte especificat durant la creació de la taula.

e) **Restriccions de verificació (CHECK):** aquesta restricció permet especificar condicions que han de complir els valors dels atributs. Cada vegada que es realitza una inserció o actualització de dades, es comprova si els valors compleixen la condició i es rebutja l’operació si no es compleix.

A continuació es mostren dues sentències de creació de taules: la taula **FABRICANTES** (taula mestra o pare) i la taula **ARTICULOS** (taula relacionada o filla). Un fabricant fabrica molts articles. Observa les restriccions:

```sql
CREATE TABLE FABRICANTES(
  CD_FAB NUMBER(3) NOT NULL DEFAULT 100,
  NOMBRE VARCHAR2(15) UNIQUE,
  PAIS VARCHAR2(15) CONSTRAINT CK_PA CHECK(PAIS=UPPER(PAIS)),
  CONSTRAINT PK_FA PRIMARY KEY (CD_FAB),
  CONSTRAINT CK_NO CHECK(NOMBRE=UPPER(NOMBRE))
);

CREATE TABLE ARTICULOS(
  ARTIC VARCHAR2(20) NOT NULL,
  COD_FA NUMBER(3) NOT NULL,
  PESO NUMBER(3) NOT NULL CONSTRAINT CK1_AR CHECK (PESO>0),
  CATEGORIA VARCHAR2(10) NOT NULL,
  PRECIO_VENTA NUMBER(4) CONSTRAINT CK2_AR CHECK (PRECIO_VENTA>0),
  PRECIO_COSTO NUMBER(4) CONSTRAINT CK3_AR CHECK (PRECIO_COSTO>0),
  EXISTENCIAS NUMBER(5),
  CONSTRAINT PK_ART PRIMARY KEY (ARTIC, COD_FA, PESO, CATEGORIA),
  CONSTRAINT FK_ARFA FOREIGN KEY (COD_FA) REFERENCES FABRICANTES (CD_FAB) ON DELETE CASCADE,
  CONSTRAINT CK_CAT CHECK(CATEGORIA IN('Primera','Segunda','Tercera'))
);
```

f) **Asercions (ASSERTION):** són semblants a la restricció anterior, però en aquest cas, en lloc d’afectar una sola relació com el **CHECK**, poden afectar dues o més relacions. La condició s’estableix sobre elements de diferents relacions i pot implicar subconsultes. La definició d’una aserció ha de tenir un **nom** i té vida pròpia dins la base de dades.

g) **Desencadenadors (TRIGGER):** les restriccions anteriors són declaratives; aquest tipus, en canvi, és **procedimental**. L’usuari pot especificar una sèrie d’accions a realitzar davant d’una determinada condició. L’usuari escriu el procediment que s’aplicarà segons el resultat d’aquesta condició. Els desencadenadors estan suportats a partir dels estàndards **SQL3**.

A continuació es mostra un **trigger** de base de dades que audita les operacions d’inserció i supressió de dades a la taula **EMPLE**. Cada vegada que es realitza una operació d’actualització o supressió, s’insereix a la taula **AUDITAREMPLE** una fila que conté la data i hora de l’operació, el número i cognom de l’empleat afectat, i el tipus d’operació realitzada. La creació de **triggers** es veurà en les unitats següents.

```sql
CREATE OR REPLACE TRIGGER auditar_act_emp
BEFORE INSERT OR DELETE ON EMPLE
FOR EACH ROW
BEGIN
  IF DELETING THEN
    INSERT INTO AUDITAREMPLE
    VALUES(TO_CHAR(sysdate,'DD/MM/YY*HH24:MI*')
    || :OLD.EMP_NO || '*' || :OLD.APELLIDO || '* BORRADO ');
  ELSIF INSERTING THEN
    INSERT INTO AUDITAREMPLE
    VALUES(TO_CHAR(sysdate,'DD/MM/YY*HH24:MI*')
    || :NEW.EMP_NO || '*' || :NEW.APELLIDO || '* INSERCION ');
  END IF;
END;
```

## Transformació d'un esquema E-R a un esquema relacional

Un cop obtingut l’**esquema conceptual** mitjançant el **model E-R**, cal definir el **model lògic de dades**. Les regles bàsiques per transformar un esquema conceptual E-R en un **esquema relacional** són les següents:

- Cada **entitat** es transforma en una **taula**.
- Cada **atribut** es transforma en una **columna** dins d’una taula.
- L’**identificador únic** de l’entitat es converteix en **clau primària**.
- Cada **relació N:M** es transforma en una **taula** que tindrà com a **clau primària** la concatenació dels atributs clau de les entitats que associa.

#### Donat l’esquema que es mostra a la Figura 2.4, en què es representen les compres d’articles que fan els clients, cal convertir l’esquema E-R en relacional.

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA1-RA1_RA2/images/Figura%2019.%20Esquema%20E-R%20CLIENTE-COMPRA-ARTICULOS.png" alt="SGBD" width="850" height="auto"/>
    <p><em>Figura 19: Esquema E-R CLIENTE-COMPRA-ARTICULOS. Fuente: Sistemas gestores de bases de datos. (Ramos)</em></p>
  </div>
  
1. Cada entitat es converteix en una taula, i els atributs de les entitats es converteixen en columnes de les taules. Ho representem així:

CLIENTE (**COD_CLIEN**, NOMBRE, DIRECCIÓN, TELEFONO)

ARTICULOS (**COD_ARTICULO**, PRECIO, STOCK, DENOMINACIÓN)


2. La relació N:M es converteix en una taula. El nom que se li dóna és el de la relació, en aquest cas COMPRA. La clau primària estarà formada per la concatenació de les claus de les taules anteriors. Aquestes, al seu torn, passen a ser claus alienes i fan referència a les taules CLIENTE i ARTICULOS.

En aquesta relació, a més d’afegir les claus de les entitats anteriors, s’afegeixen els atributs que intervenen en la relació.

La taula queda així:

COMPRA (**COD_CLIEN (FK)**, **COD_ARTICULO (FK)**, UNI_VEND, FECHA_VENTA)

Les opcions de supressió i de modificació solen ser en cascada. A continuació es mostra el codi que crearia aquestes tres taules:

```sql
CREATE TABLE CLIENTE (
  COD_CLIEN NUMBER(6) NOT NULL PRIMARY KEY,
  NOMBRE VARCHAR2(15),
  DIRECCION VARCHAR2(15),
  TELEFONO NUMBER(10));

CREATE TABLE ARTICULOS(
  COD_ARTICULO NUMBER(6) NOT NULL PRIMARY KEY,
  PRECIO NUMBER (6,2),
  STOCK NUMBER (4),
  DENOMINACION VARCHAR2(15));

CREATE TABLE COMPRA(
  COD_CLIEN NUMBER(6) NOT NULL,
  COD_ARTICULO NUMBER(6) NOT NULL,
  UNI_VEND NUMBER (4),
  FECHA_VENTA DATE,
  CONSTRAINT PK_COMPRA PRIMARY KEY (COD_CLIEN,COD_ARTICULO),
  CONSTRAINT FK_CLIEN FOREIGN KEY (COD_CLIEN)
    REFERENCES CLIENTE (COD_CLIEN)
    ON DELETE CASCADE ON UPDATE CASCADE,
  CONSTRAINT FK_ARTIC FOREIGN KEY (COD_ARTICULO)
    REFERENCES ARTICULOS (COD_ARTICULO)
    ON DELETE CASCADE ON UPDATE CASCADE);
```

En la **transformació de relacions 1:N** existeixen dues solucions:

- **Transformar la relació en una taula.** Es fa com si es tractés d’una relació N:M. Aquesta solució s’aplica quan es preveu que en un futur la relació es convertirà en N:M, o bé quan la relació té **atributs propis**. També es crea una nova taula quan la **cardinalitat és opcional**, és a dir, (0,1) i (0,M). La **clau** d’aquesta taula és la de l’entitat del costat “molts”.
- **Propagar la clau.** Aquest cas s’aplica quan la **cardinalitat és obligatòria**, és a dir, quan tenim cardinalitats (1,1) i (0,M) o (1,M). Es **propaga l’atribut principal** de l’entitat que té cardinalitat màxima 1 cap a la que té cardinalitat màxima N, desapareixent el nom de la relació. Si existeixen **atributs propis** en la relació, aquests també es propagaran.

#### Donat l’esquema que es mostra a la Figura 2.6, en què es representa la classificació dels llibres en temes, cal convertir l’esquema E-R en relacional.

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA1-RA1_RA2/images/Figura%2020.%20Esquema%20E-R%20TEMA-CLASIFICA-LIBRO.png" alt="SGBD" width="850" height="auto"/>
    <p><em>Figura 20: Esquema E-R TEMA-CLASIFICA-LIBRO. Fuente: Sistemas gestores de bases de datos. (Ramos)</em></p>
  </div>

1. Es converteixen en taula les dues entitats:

TEMAS (**COD_TEMA**, DESCRIPCIÓN)

LIBROS (**COD_LIBRO**, AUTOR, ISBN, TÍTULO, NUM_EJEMPLARES)

2. Es propaga la clau de l’entitat TEMAS a l’entitat LIBROS. L’entitat LIBROS queda així:

LIBROS (**COD_LIBRO**, AUTOR, ISBN, TÍTULO, NUM_EJEMPLARES, **COD_TEMA (FK)**)

En la **transformació de relacions 1:1** es tenen en compte les **cardinalitats** de les entitats que hi participen. Existeixen dues solucions:

- **Transformar la relació en una taula.** Si les entitats tenen cardinalitats (0,1), la relació es converteix en una taula.
- **Propagar la clau.** Si una de les entitats té cardinalitat (0,1) i l’altra (1,1), convé **propagar la clau** de l’entitat amb cardinalitat (1,1) a la taula resultant de l’entitat amb cardinalitat (0,1).
Si totes dues entitats tenen cardinalitats (1,1), es pot propagar la clau de qualsevol d’elles a la taula resultant de l’altra. En aquest cas, també es poden **afegir els atributs** d’una entitat a l’altra, resultant una **única taula** amb tots els atributs de les entitats i de la relació (si n’hi hagués), escollint com a **clau primària** una de les dues.

#### Considerem la relació EMPLEADO–OCUPA–PUESTOTRABAJO. Un empleat ocupa un sol lloc de treball, i aquest lloc de treball és ocupat per un sol empleat o per cap. L’esquema E-R es mostra a la Figura 2.8; cal transformar-lo al model relacional.

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA1-RA1_RA2/images/Figura%2021.%20Esquema%20E-R%20EMPLEADO-OCUPA-PUESTOTRABAJO.png" alt="SGBD" width="850" height="auto"/>
    <p><em>Figura 21: Esquema E-R EMPLEADO-OCUPA-PUESTOTRABAJO. Fuente: Sistemas gestores de bases de datos. (Ramos)</em></p>
  </div>

En aquest cas, la clau es propaga des de l’entitat PUESTOTRABAJO, amb cardinalitat (1,1), cap a l’entitat EMPLEADO, amb cardinalitat (0,1).

Les taules es representen així:

PUESTOTRABAJO (**COD_PUESTO**, DESCRIPCIÓN, OFICINA, DESPACHO, MESA)

EMPLEADO (**COD_EMPLE**, NOMBRE, DIRECCIÓN, TELEFONO, **COD_PUESTO (FK)**)
