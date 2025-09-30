# Model entitat-relació

El **model de dades Entitat-Relació (E-R)**, proposat per **Peter Chen el 1976**, és un dels més utilitzats per a la representació conceptual de problemes del món real.  
El 1988 l’ANSI el va seleccionar com a model estàndard per als sistemes de diccionaris de recursos d’informació.

Es representa mitjançant **gràfics i taules**, i proposa l’ús de **taules bidimensionals** per descriure dades i relacions.

## Conceptes bàsics

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

## A. Relacions i conjunts de relacions

En el **model Entitat-Relació (E-R)**, una **relació** és l’associació entre diferents entitats.  
- Es representa amb un **rombe** i té **nom de verb** que la diferencia de les altres.  
- Normalment **no tenen atributs**. Si en tenen, vol dir que hi ha una **entitat associada** que encara no s’ha definit i que donarà lloc a una taula pròpia.  

### Conjunt de relacions
Un **conjunt de relacions** és el conjunt de totes les associacions del mateix tipus.  
Exemple: entre **ARTICLES** i **VENTES**, totes les vendes associades als articles formen un conjunt de relacions.

- La majoria de conjunts són **binaris** (entre dues entitats).  
- També poden existir relacions **n-àries** (entre més de dues entitats), ex. CLIENT – COMPTE – SUCURSAL.  
- Una relació no binària sempre es pot transformar en diverses relacions binàries, tot i que no sempre és la millor opció.  

### Paper d’una entitat
El **paper** és la funció que exerceix una entitat dins d’una relació. Normalment és implícit, però pot ser útil especificar-lo quan cal aclarir el significat de la relació.

### Relacions amb atributs
Algunes relacions poden tenir **atributs descriptius**.  
Exemple: la relació **COMPTE** pot tenir l’atribut **DATA_OPERACIO**, que indica l’última vegada que el client va accedir al seu compte (veure Figura 4).

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA1-RA1_RA2/images/Figura%204.%20Relaci%C3%B3%20amb%20atributs%20descriptius.png" alt="BD" width="450" height="auto"/>
    <p><em>Figura 4: Relació amb atributs descriptius.</em></p>
  </div>

### Diagrames d’estructures de dades en el model E-R

Els diagrames Entitat-Relació representen l’estructura lògica d’una BD de manera gràfica. Els símbols utilitzats són els següents:
- Rectangles per representar les entitats.
- El·lipses per als atributs. L’atribut que forma part de la clau primària va subratllat.
- Rombes per representar les relacions.
- Les línies, que uneixen atributs a entitats i a relacions, i entitats a relacions.

Si la fletxa té punta, en aquell sentit hi ha l’u, i si no en té, en aquell lloc hi ha els molts. L’orientació assenyala la cardinalitat.
- Si la relació té atributs associats, se li uneixen a la relació.
- Cada component s’etiqueta amb el nom del que representa.

A la Figura 4 es mostra un diagrama E-R corresponent a PROVEÏDORS-ARTICLES.
Un PROVEÏDOR SUBMINISTRA molts ARTICLES.

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA1-RA1_RA2/images/Figura%205.%20Diagrama%20E-R.%20un%20proveidor%20subministra%20molts%20articles.png" alt="BD" width="650" height="auto"/>
    <p><em>Figura 5: Diagrama E-R. Un proveidor subministra molts articles.</em></p>
  </div>

### Grau i cardinalitat de les relacions

Es defineix el **grau d’una relació** com el nombre de conjunts d’entitats que participen en el conjunt de relacions, o el que és el mateix, el nombre d’entitats que participen en una relació. Les relacions en què participen dues entitats són binàries o de grau dos. Si n’hi participen tres, seran ternàries o de grau tres. Els conjunts de relacions poden tenir qualsevol grau, tot i que l’ideal és tenir relacions binàries.

Les relacions en què només participa una entitat s’anomenen anell o de grau u; relacionen una entitat amb ella mateixa, i se les anomena relacions reflexives. Per exemple, l’entitat EMPLEAT pot tenir una relació CAP DE amb ella mateixa: un empleat és CAP DE molts empleats i, alhora, el cap també és un empleat.

Un altre exemple pot ser la relació DELEGAT DE dels alumnes d’un curs: el delegat també és alumne del curs. Vegeu la Figura 5.

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA1-RA1_RA2/images/Figura%206.%20Relacions%20de%20grau%201.png" alt="BD" width="450" height="auto"/>
    <p><em>Figura 6: Relacions de grau 1.</em></p>
  </div>

A la Figura 7 es mostra una relació de grau dos, que representa un proveïdor que subministra articles, i una altra de grau tres, que representa un client d’un banc que té diversos comptes, i cadascun en una sucursal:

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA1-RA1_RA2/images/Figura%207.%20Relacions%20de%20grau%202%20i%203.png" alt="BD" width="550" height="auto"/>
    <p><em>Figura 7: Relacions de grau 2 i 3.</em></p>
  </div>

En el model E-R es representen certes restriccions a les quals s’han d’ajustar les dades contingudes en una BD. Aquestes són les restriccions de les cardinalitats d’assignació, que expressen el nombre d’entitats a les quals pot associar-se una altra entitat mitjançant un conjunt de relació.

Les cardinalitats d’assignació es descriuen per a conjunts binaris de relacions. Són les següents:

- **1:1, u a u.** A cada element de la primera entitat li correspon només un de la segona entitat, i a l’inrevés. Per exemple, un client d’un hotel ocupa una habitació, o un curs d’alumnes pertany a una aula, i a aquella aula només hi assisteix aquell grup d’alumnes. Vegeu la Figura 8:

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA1-RA1_RA2/images/Figura%208.%20Representaci%C3%B3%20de%20relacions%201%20a%201.png" alt="BD" width="550" height="auto"/>
    <p><em>Figura 8: Representació de relacions 1 a 1.</em></p>
  </div>

- **1:N, u a molts.** A cada element de la primera entitat li corresponen un o més elements de la segona entitat, i a cada element de la segona entitat li correspon només un de la primera entitat. Per exemple, un proveïdor subministra molts articles (vegeu la Figura 9).

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA1-RA1_RA2/images/Figura%209.%20Representaci%C3%B3%20de%20relacions%201%20a%20molts.png" alt="BD" width="550" height="auto"/>
    <p><em>Figura 9: Representació de relacions 1 a molts.</em></p>
  </div>

- **N:1, molts a u.** És el mateix cas que l’anterior però a l’inrevés; a cada element de la primera entitat li correspon un element de la segona, i a cada element de la segona entitat li corresponen diversos de la primera.

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA1-RA1_RA2/images/Figura%2010.%20Representaci%C3%B3%20de%20relacions%20molts%20a%20molts.png" alt="BD" width="550" height="auto"/>
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
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA1-RA1_RA2/images/Figura%2011.%20Diagrama%20E-R%20de%20les%20relacions%20entre%20departaments%20i%20empleats.png" alt="BD" width="950" height="auto"/>
    <p><em>Figura 11: Diagrama E-R de les relacions entre departaments i empleats.</em></p>
  </div>

### Generalització i jerarquies de generalització

Les **generalitzacions** proporcionen un mecanisme d’abstracció que permet especialitzar una entitat (que s’anomenarà supertipus) en subtipus, o el que és el mateix, generalitzar els subtipus en el supertipus.
Una generalització s’identifica si trobem una sèrie d’atributs comuns a un conjunt d’entitats, i uns atributs específics que identificaran unes característiques.
Els atributs comuns descriuran el supertipus i els particulars els subtipus. Una de les característiques més importants de les jerarquies és l’herència, per la qual els atributs d’un supertipus són heretats pels seus subtipus. Si el supertipus participa en una relació, els subtipus també hi participaran.

Per exemple, en una empresa de construcció podrem identificar les següents entitats:

- EMPLEAT, amb els atributs N_EMPLE (clau primària), NOM, ADREÇA, DATA_NAIX, SOU i LLOC.
- ARQUITECTE, amb els atributs d’empleat més els atributs específics: COMISSIONS i NUM_PROJECTES.
- ADMINISTRATIU, amb els atributs d’empleat més els atributs específics: PULSACIONS i NIVELL.
- ENGINYER, amb els atributs d’empleat més els atributs específics: ESPECIALITAT i ANYS_EXPERIÈNCIA.

A la Figura 12 es representa aquest exemple de generalització.

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA1-RA1_RA2/images/Figura%2012.%20Representaci%C3%B3%20d'una%20generalitzaci%C3%B3.png" alt="BD" width="550" height="auto"/>
    <p><em>Figura 12: Representació d'una generalització.</em></p>
  </div>

### Entitats dèbils

Una **entitat dèbil** és una entitat dins d’un model de dades que **no té prou atributs propis per identificar-se de manera única**.
A diferència de les **entitats fortes**, que disposen d’una **clau primària pròpia**, **les entitats dèbils necessiten de l’existència d’una altra entitat** (anomenada entitat forta o propietària) per poder identificar-se.

Característiques principals:

- **Dependència d’existència:** Una entitat dèbil no pot existir sense la seva entitat forta associada.
- **Identificador parcial:** Les entitats dèbils tenen un atribut o conjunt d’atributs anomenats identificador parcial, que només les distingeix dins del context de l’entitat forta.
- **Clau primària composta:** La clau primària de l’entitat forta (com a clau forana). El seu identificador parcial.

Per exemple, una empresa que porta la gestió de les factures: 

- FACTURA, entitat forta. Atributs: id_factura, client i data.
- LINIA_FACTURA, dentificador parcial: Num_linia. Altres atributs: producte, quantitat i preu.

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA1-RA1_RA2/images/Figura%2013.%20Relaci%C3%B3%20d%C3%A8bil.png" alt="BD" width="550" height="auto"/>
    <p><em>Figura 13: Entitat dèbil.</em></p>
  </div>

### Agregacions

Una limitació del model E-R és que no és possible expressar relacions entre relacions. En aquests casos es realitza una agregació, que és una abstracció a través de la qual les relacions es tracten com a entitats de nivell més alt. Per exemple, considerem una relació entre EMPLEATS i PROJECTES, un empleat treballa en diversos projectes durant unes hores determinades i en aquest treball utilitza unes eines determinades. La representació del diagrama d’estructures es mostra a la Figura 14 següent:

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA1-RA1_RA2/images/Figura%2014.%20Una%20relaci%C3%B3%20amb%20una%20altra%20relaci%C3%B3.png" alt="BD" width="550" height="auto"/>
    <p><em>Figura 14: Una relació amb una altra relació.</em></p>
  </div>

Si considerem l’agregació, tenim que la relació TREBALL amb les entitats EMPLEAT i PROJECTE es pot representar com un conjunt d’entitats anomenades TREBALL, que es relacionen amb l’entitat EINES mitjançant la relació UTILITZA. Vegeu la Figura 15:

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA1-RA1_RA2/images/Figura%2015.%20Conjunt%20d'entitats%20i%20relacions%20per%20representar%20una%20relaci%C3%B3%20entre%20una%20relaci%C3%B3.png" alt="BD" width="550" height="auto"/>
    <p><em>Figura 15: Conjunt d'entitats i relacions per representar una relació entre una relació.</em></p>
  </div>
