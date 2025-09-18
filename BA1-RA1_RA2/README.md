# Sistemes gestors de bases de dades

## 1. Introducció

Un **Sistema Gestor de Bases de Dades (SGBD)**, o **DBMS (Database Management System)**, és un conjunt de dades relacionades i estructurades, i un conjunt de programes que permeten accedir i gestionar aquestes dades. La col·lecció de dades es coneix com a **Base de Dades (BD)**.

Abans de l’aparició dels SGBD (dècada dels 70), la informació es gestionava mitjançant **sistemes de fitxers** sobre el sistema operatiu. Cada aplicació definia i gestionava les seves pròpies dades dins d’arxius. Qualsevol canvi en l’estructura dels arxius obligava a modificar els programes associats.

### Inconvenients dels sistemes de fitxers

- **Redundància i inconsistència:** dades duplicades i incoherents en diferents fitxers.
- **Dependència física-lògica:** els programes depenen de l’estructura dels arxius.
- **Dificultat d’accés:** cal crear programes específics per obtenir noves consultes.
- **Aïllament i separació de dades:** formats diferents i dispersió de la informació.
- **Accés concurrent complicat:** problemes si diversos usuaris actualitzen alhora.
- **Dependència del llenguatge:** incompatibilitat entre arxius creats amb diferents llenguatges.
- **Seguretat limitada:** difícil imposar restriccions d’accés.
- **Integritat feble:** cal afegir molt codi per controlar restriccions i consistència.

### Avantatges dels SGBD

Els SGBD neixen per superar aquests problemes. Els seus objectius principals són garantir **eficiència, seguretat i consistència** en la gestió de grans volums d’informació. Les dades es defineixen una sola vegada, amb **mínima duplicació** i accés **simultani** per diversos usuaris. A més, els **metadades** es guarden en un **diccionari de dades**.

### Serveis d'un SGBD

Un SGBD ha de proporcionar:

- **Definició de la BD:** estructures, tipus de dades, restriccions i relacions (mitjançant llenguatges de definició de dades).
- **Manipulació de dades:** consultes, insercions i actualitzacions (amb llenguatges de manipulació de dades).
- **Control d’accés i seguretat:** mecanismes per limitar l’accés als usuaris.
- **Integritat i consistència:** mantenir la correcció dels valors i evitar modificacions no autoritzades.
- **Accés compartit i control de concurrència**.
- **Còpies de seguretat i recuperació** en cas de fallades del sistema.

---

## 2. Arquitectura dels sistemes de bases de dades

L’any 1975, el comitè **ANSI-SPARC** va proposar una arquitectura de tres nivells per als SGBD, amb l’objectiu de separar els programes d’aplicació de la base de dades física. Aquesta arquitectura defineix els esquemes d’una BD en tres nivells d’abstracció:

### Nivells d’abstracció

- **Nivell intern o físic:** el més proper a l’emmagatzematge. Defineix com s’organitzen els arxius, els mètodes d’accés i la representació física de les dades.
- **Nivell conceptual:** descriu l’estructura completa de la BD (entitats, atributs, relacions i restriccions) amagant els detalls físics.
- **Nivell extern o de visió:** defineix les vistes dels usuaris, mostrant només la part de la BD que els interessa.

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA1-RA1_RA2/images/Figura%201.%20Nivell%20d'abstracci%C3%B3%20de%20l'arquitectura%20ANSI.png" alt="ANSI" width="650" height="auto"/>
    <p><em>Figura 1: Nivell d'abstracció de l'arquitectura ANSI</em></p>
  </div>

### Funcionament

Quan un usuari fa una consulta:

1.	El SGBD valida l’esquema extern corresponent.
2.	Transforma la petició al nivell conceptual.
3.	Transforma després la petició al nivell intern i executa la consulta.
4.	Els resultats es retornen del nivell intern al conceptual i d’aquest a l’extern.

Per a una BD només existeix un esquema intern i un de conceptual, però es poden definir múltiples esquemes externs segons els usuaris.

### Independència de dades
Aquesta arquitectura introdueix dos tipus d’independència:

- **Independència lògica:** permet modificar l’esquema conceptual sense afectar els esquemes externs ni els programes.
- **Independència física:** permet reorganitzar o canviar l’estructura interna sense afectar l’esquema conceptual ni els externs.

### Limitacions
Encara que el model és teòricament molt útil, pocs SGBD han implementat completament aquesta arquitectura, ja que la transformació entre nivells pot reduir l’eficiència del sistema.

---

## 3. Components de les SGBD

Els **Sistemes Gestors de Bases de Dades (SGBD)** són paquets de programari complexos que proporcionen serveis per emmagatzemar i gestionar dades de manera eficient. Els principals components són:

### A. Llenguatges dels SGBD

Els SGBD ofereixen llenguatges i interfícies segons el tipus d’usuari (administradors, dissenyadors, programadors i usuaris finals):
- **Llenguatge de definició de dades (LDD/DDL):** defineix l’esquema conceptual i intern de la BD, les vistes i les estructures d’emmagatzematge.
- **Llenguatge de manipulació de dades (LMD/DML):** permet consultar, inserir, modificar i eliminar dades. Pot ser:
  ⦁	**Procedural:** el programador indica les operacions pas a pas (ex. BD jeràrquiques o en xarxa).
  ⦁	**No procedural (declaratiu):** l’usuari indica què vol obtenir sense especificar com (ex. SQL, QBE).
- **Llenguatges de quarta generació (4GL):** eines que faciliten el desenvolupament ràpid d’aplicacions (ex. Oracle SQL Forms, Reports, PL/SQL).

### B. Diccionari de dades

El **diccionari de dades** és el repositori que descriu la BD i els seus objectes. Inclou informació sobre:
- Estructura lògica i física.
- Definició d’objectes (taules, vistes, índexs, procediments...).
- Espai assignat i utilitzat.
- Restriccions d’integritat.
- Privilegis i rols dels usuaris.
- Auditoria d’accessos.

#### Característiques d’un bon diccionari de dades:

- Suport als models conceptual, lògic, intern i extern.
- Integració dins el SGBD.
- Actualització automàtica dels canvis en la BD i en els programes.
- Emmagatzematge en mitjans d’accés directe per a una recuperació eficient.

### C. Seguretat i integritat

Un SGBD ha de garantir:
- **Control d’accessos:** evitar accessos no autoritzats.
- **Restriccions d’integritat:** assegurar la consistència de les dades.
- **Còpies de seguretat i recuperació:** restaurar la BD en cas d’error o desastre.
- **Accés concurrent segur:** mantenir la consistència quan diversos usuaris actualitzen alhora.

### D. Administrador de la BD (DBA)

El **DBA** és el responsable de la BD i té el màxim nivell de privilegis. Les seves funcions inclouen:
- Instal·lar i configurar el SGBD.
- Crear i mantenir bases de dades i esquemes.
- Gestionar comptes i permisos d’usuaris.
- Controlar recursos de disc i rendiment del sistema.
- Planificar i executar còpies de seguretat i restauració.
- Supervisar l’ús de la BD, monitorar accessos i detectar anomalies.
- Establir normes, protocols i polítiques d’ús.
- Formar usuaris i donar suport als equips de desenvolupament.
  
Amb el temps, els DBA disposen d’eines cada cop més avançades que faciliten la seva tasca diària.

---

## 4. Model de dades

Un dels objectius principals d’un **SGBD** és proporcionar als usuaris una visió **abstracta** de les dades, amagant els detalls de com estan emmagatzemades físicament.  
Aquesta abstracció es representa amb els **models de dades**, que s’organitzen en tres nivells segons l’arquitectura ANSI-SPARC:

## Nivells d’abstracció
- **Nivell físic**: descriu com s’emmagatzemen realment les dades (fitxers, sectors, índexs, etc.).  
- **Nivell lògic o conceptual**: descriu l’estructura global de la BD (entitats, atributs, relacions i restriccions).  
- **Nivell extern o de vistes**: mostra la part de la BD que interessa a un usuari o grup d’usuaris.  

En una BD hi ha **un únic nivell físic i lògic**, però es poden definir **múltiples nivells externs** (vistes).

Per fer-nos una idea dels tres nivells d'abstracció, ens imaginem un arxiu d'articles amb els següents registres:

```C
 struct ARTICLES
 { int Cod;
 char Deno[15];
 int cant_magatzem;
 int cant_minima;
 int uni_venuda;
 float PVP;
 char reposar;
 struct VENDES Tvendes[12];
 };
```

El nivell físic és el conjunt de bytes que es troben emmagatzemats a l'arxiu en un dispositiu magnetic, que pot ser un disc, una pista o un sector determinat. 

A nivell lògic compren la descripció i la relació amb altres registres que es fan del registre dins d'un programa, amb un llenguatge de programació. 

L'últim nivell d'abstracció, **l'extern**, és la visió d'aquestes dades que té un usuari quan executa aplicacions que operen amb ells, l'usuari no sap el detall de les dades.

Si traslladem l'exemple a una BD relacional específica n'hi haurà, com en el cas anterior, un únic nivell intern i un únic nivell lògic o conceptual, però n'hi pot haver diversos nivells externs, cadascun definit per a un o per a diversos usuaris. Podria ser el
següent:

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA1-RA1_RA2/images/Figura%202.%20Vista%20de%20la%20BD%20per%20un%20usuari.png" alt="BD" width="650" height="auto"/>
    <p><em>Figura 2: Vista de la BD per a un usuari.</em></p>
  </div>

**Nivell extern:** visió parcial de les taules de la BD segons l'usuari. Per exemple, la vista que es mostra a la Figura 2 obté el llistat de notes d'alumnes amb les dades següents: Curs, Nom, Nom d'assignatura i Nota.

**Nivell lògic i conceptual:** definició de totes les taules, columnes, restriccions, claus i relacions. En aquest exemple, disposem de tres taules que hi estan relacionades:

- Taula *ALUMNES*. Columnes: num_matricula, nom, curs, adreça, poblacio. Clau: num_matricula. A més, té una relació amb NOTES, ja que un alumne pot tenir notes en diverses assignatures.
- Taula *ASSIGNATURES*. Columnes: codi, nom_assignatura. Clau: codi. Està relacionada amb NOTES, ja que per a una assignatura hi ha diverses notes, tantes com a alumnes la cursin.
- Taula *NOTES*. Columnes: num_matricula, codi, nota. Està relacionada amb ALUMNES i ASSIGNATURES, ja que un alumne té notes en diverses assignatures, i d'una assignatura hi ha diverses notes, tantes com alumnes.

Podem representar les relacions de les taules en el nivell lògic com es mostra a la Figura 3:

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA1-RA1_RA2/images/Figura%203.%20Representaci%C3%B3%20de%20les%20relacions%20entre%20taules%20al%20nivell%20l%C3%B2gic.png" alt="BD" width="650" height="auto"/>
    <p><em>Figura 3: Representació de les relacions entre taules al nivell lògic.</em></p>
  </div>

**Nivell intern:** En una BD les taules s'emmagatzemen en fitxers de dades de la BD. Si hi ha claus, es creen índexs per accedir a les dades, tot això contingut al disc dur, en una pista i en un sector, que només el SGBD coneix. Davant d'una petició, sap a quina pista, a quin sector, a quin fitxer de dades i a quins índexs accedir.
 
Els **models de dades** són conjunts de conceptes i eines per descriure l’estructura d’una BD, les seves relacions i restriccions. La descripció d’una BD amb un model de dades s’anomena **esquema**.

### 1. Models lògics basats en objectes
- Descriuen dades al nivell conceptual i extern.  
- Permeten estructuració flexible i restriccions de dades.  
- **Exemples**:  
  - **Model Entitat-Relació (E-R)**: el més utilitzat actualment.  
  - **Model orientat a objectes**: incorpora conceptes del model E-R i cada cop té més ús.  
- Molts SGBD actuals són **relacionals amb extensions orientades a objectes**.

### 2. Models lògics basats en registres
- Descriuen dades al nivell conceptual i físic.  
- La BD s’organitza en **registres de format fix** (o variable).  
- No representen directament codi, sinó que s’associen amb llenguatges de consulta i actualització.  
- **Exemples**:  
  - **Model relacional**: dades i relacions representades en taules (files = tuples, columnes = atributs).  
  - **Model de xarxa**.  
  - **Model jeràrquic**.  
- El **model relacional** és el més acceptat i serà tractat en la següent unitat.

### 3. Models físics de dades
- Descriuen **com s’emmagatzemen les dades**: formats de registres, organització d’arxius, mètodes d’accés.  
- Exemples coneguts: **model unificador** i **model de memòria d’elements**.

---

## 5. Model entitat-relació

El **model de dades Entitat-Relació (E-R)**, proposat per **Peter Chen el 1976**, és un dels més utilitzats per a la representació conceptual de problemes del món real.  
El 1988 l’ANSI el va seleccionar com a model estàndard per als sistemes de diccionaris de recursos d’informació.

Es representa mitjançant **gràfics i taules**, i proposa l’ús de **taules bidimensionals** per descriure dades i relacions.

### Conceptes bàsics

- **Entitat**: objecte del món real que té interès per a l’organització (ex. ALUMNES, CLIENTS).  
  → Es representa amb un **rectangle**.
- **Conjunt d’entitats**: grup d’entitats del mateix tipus (ex. conjunt d’ALUMNES). Poden coincidir amb altres conjunts.
- **Entitat forta**: no depèn d’una altra per existir (ex. ALUMNE).  
- **Entitat feble**: depèn d’una entitat forta per existir (ex. NOTA depèn d’ALUMNE).  
  → Es representa amb **rectangle de doble línia**.
- **Atributs**: propietats que descriuen una entitat (ex. matrícula, nom, adreça).  
  → Es representen amb una **el·lipse**.  
- **Domini**: conjunt de valors permesos d’un atribut (ex. “Nom” → cadenes de text).
- **Identificador o superclau**: conjunt d’atributs que identifiquen de forma única una entitat.  
  Ex. DNI + Nom + Adreça.
- **Claus candidates**: superclaus mínimes que identifiquen de forma única una entitat.  
  Ex. DNI, Número de la Seguretat Social.
- **Clau primària**: clau candidata escollida pel dissenyador de la BD.  
  Ha de ser **única, no nul·la, estable i fàcil de gestionar**.  
  → Es representa **subratllada**.
- **Clau forana (foreign key)**: atribut d’una entitat que és clau primària en una altra. Representa relacions entre taules.  
  Ex. En **VENTES(codi venda, data, codi article, unitats)**, el **codi article** és clau forana que referencia **ARTICLES(codi article, descripció, stock)**.

### A. Relacions i conjunts de relacions

En el **model Entitat-Relació (E-R)**, una **relació** és l’associació entre diferents entitats.  
- Es representa amb un **rombe** i té **nom de verb** que la diferencia de les altres.  
- Normalment **no tenen atributs**. Si en tenen, vol dir que hi ha una **entitat associada** que encara no s’ha definit i que donarà lloc a una taula pròpia.  

#### Conjunt de relacions
Un **conjunt de relacions** és el conjunt de totes les associacions del mateix tipus.  
Exemple: entre **ARTICLES** i **VENTES**, totes les vendes associades als articles formen un conjunt de relacions.

- La majoria de conjunts són **binaris** (entre dues entitats).  
- També poden existir relacions **n-àries** (entre més de dues entitats), ex. CLIENT – COMPTE – SUCURSAL.  
- Una relació no binària sempre es pot transformar en diverses relacions binàries, tot i que no sempre és la millor opció.  

#### Paper d’una entitat
El **paper** és la funció que exerceix una entitat dins d’una relació. Normalment és implícit, però pot ser útil especificar-lo quan cal aclarir el significat de la relació.

#### Relacions amb atributs
Algunes relacions poden tenir **atributs descriptius**.  
Exemple: la relació **COMPTE** pot tenir l’atribut **DATA_OPERACIO**, que indica l’última vegada que el client va accedir al seu compte (veure Figura 4).

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA1-RA1_RA2/images/Figura%204.%20Relaci%C3%B3%20amb%20atributs%20descriptius.png" alt="BD" width="650" height="auto"/>
    <p><em>Figura 4: Relació amb atributs descriptius.</em></p>
  </div>

#### Diagrames d’estructures de dades en el model E-R

Els diagrames Entitat-Relació representen l’estructura lògica d’una BD de manera gràfica. Els símbols utilitzats són els següents:
– Rectangles per representar les entitats.
– El·lipses per als atributs. L’atribut que forma part de la clau primària va subratllat.
– Rombes per representar les relacions.
– Les línies, que uneixen atributs a entitats i a relacions, i entitats a relacions.

Si la fletxa té punta, en aquell sentit hi ha l’u, i si no en té, en aquell lloc hi ha els molts. L’orientació assenyala la cardinalitat.
– Si la relació té atributs associats, se li uneixen a la relació.
– Cada component s’etiqueta amb el nom del que representa.

A la Figura 4 es mostra un diagrama E-R corresponent a PROVEÏDORS-ARTICLES.
Un PROVEÏDOR SUBMINISTRA molts ARTICLES.

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA1-RA1_RA2/images/Figura%205.%20Diagrama%20E-R.%20un%20proveidor%20subministra%20molts%20articles.png" alt="BD" width="650" height="auto"/>
    <p><em>Figura 5: Diagrama E-R. Un proveidor subministra molts articles.</em></p>
  </div>

#### Grau i cardinalitat de les relacions

Es defineix el **grau d’una relació** com el nombre de conjunts d’entitats que participen en el conjunt de relacions, o el que és el mateix, el nombre d’entitats que participen en una relació. Les relacions en què participen dues entitats són binàries o de grau dos. Si n’hi participen tres, seran ternàries o de grau tres. Els conjunts de relacions poden tenir qualsevol grau, tot i que l’ideal és tenir relacions binàries.

Les relacions en què només participa una entitat s’anomenen anell o de grau u; relacionen una entitat amb ella mateixa, i se les anomena relacions reflexives. Per exemple, l’entitat EMPLEAT pot tenir una relació CAP DE amb ella mateixa: un empleat és CAP DE molts empleats i, alhora, el cap també és un empleat.

Un altre exemple pot ser la relació DELEGAT DE dels alumnes d’un curs: el delegat també és alumne del curs. Vegeu la Figura 5.

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA1-RA1_RA2/images/Figura%206.%20Relacions%20de%20grau%201.png" alt="BD" width="650" height="auto"/>
    <p><em>Figura 6: Relacions de grau 1.</em></p>
  </div>

A la Figura 7 es mostra una relació de grau dos, que representa un proveïdor que subministra articles, i una altra de grau tres, que representa un client d’un banc que té diversos comptes, i cadascun en una sucursal:

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA1-RA1_RA2/images/Figura%207.%20Relacions%20de%20grau%202%20i%203.png" alt="BD" width="650" height="auto"/>
    <p><em>Figura 7: Relacions de grau 2 i 3.</em></p>
  </div>

En el model E-R es representen certes restriccions a les quals s’han d’ajustar les dades contingudes en una BD. Aquestes són les restriccions de les cardinalitats d’assignació, que expressen el nombre d’entitats a les quals pot associar-se una altra entitat mitjançant un conjunt de relació.

Les cardinalitats d’assignació es descriuen per a conjunts binaris de relacions. Són les següents:

- **1:1, u a u.** A cada element de la primera entitat li correspon només un de la segona entitat, i a l’inrevés. Per exemple, un client d’un hotel ocupa una habitació, o un curs d’alumnes pertany a una aula, i a aquella aula només hi assisteix aquell grup d’alumnes. Vegeu la Figura 8:

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA1-RA1_RA2/images/Figura%208.%20Representaci%C3%B3%20de%20relacions%201%20a%201.png" alt="BD" width="650" height="auto"/>
    <p><em>Figura 8: Representació de relacions 1 a 1.</em></p>
  </div>

- **1:N, u a molts.** A cada element de la primera entitat li corresponen un o més elements de la segona entitat, i a cada element de la segona entitat li correspon només un de la primera entitat. Per exemple, un proveïdor subministra molts articles (vegeu la Figura 9).

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA1-RA1_RA2/images/Figura%209.%20Representaci%C3%B3%20de%20relacions%201%20a%20molts.png" alt="BD" width="650" height="auto"/>
    <p><em>Figura 9: Representació de relacions 1 a molts.</em></p>
  </div>

- **N:1, molts a u.** És el mateix cas que l’anterior però a l’inrevés; a cada element de la primera entitat li correspon un element de la segona, i a cada element de la segona entitat li corresponen diversos de la primera.

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA1-RA1_RA2/images/Figura%2010.%20Representaci%C3%B3%20de%20relacions%20molts%20a%20molts.png" alt="BD" width="650" height="auto"/>
    <p><em>Figura 10: Representació de relacions molts a molts.</em></p>
  </div>

La cardinalitat d’una entitat serveix per conèixer el seu grau de participació en la relació, és a dir, el nombre de correspondències en què cada element de l’entitat intervé. Mesura l’obligatorietat de correspondència entre dues entitats.

La representem entre parèntesis indicant els valors màxim i mínim: (màxim, mínim). Els valors per a la cardinalitat són: (0,1), (1,1), (0,N), (1,N) i (M,N). El valor 0 s’indica quan la participació de l’entitat és opcional.

A la Figura 11, que es mostra a continuació, es representa el diagrama E-R en què comptem amb les següents entitats:

- **EMPLEAT** està formada pels atributs Núm. Emple, Cognom, Salari i Comissió, essent l’atribut Núm. Emple la clau principal (representat amb el subratllat).
- **DEPARTAMENT** està formada pels atributs Núm. Depart, Nom i Localitat, essent l’atribut Núm. Depart la clau principal.

S’han definit dues relacions:

- La relació «pertany» entre les entitats EMPLEATS i DEPARTAMENT, el tipus de correspondència de la qual és 1:N, és a dir, a un departament li pertanyen zero o més empleats (0,N). Un empleat pertany a un departament i només a un (1,1).
- La relació «responsable», que associa l’entitat EMPLEAT amb ella mateixa. El seu tipus de correspondència és 1:N, és a dir, un empleat és cap de zero o més empleats (0,N). Un empleat té un cap i només un (1,1). Vegeu la Figura 1.10:

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA1-RA1_RA2/images/Figura%2011.%20Diagrama%20E-R%20de%20les%20relacions%20entre%20departaments%20i%20empleats.png" alt="BD" width="650" height="auto"/>
    <p><em>Figura 11: Diagrama E-R de les relacions entre departaments i empleats.</em></p>
  </div>
