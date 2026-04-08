# Còpies de seguretat al núvol

## Introducció a la Continuïtat Empresarial a Azure

La **continuïtat empresarial** en el núvol és el conjunt de mecanismes i procediments que permeten que una empresa segueixi funcionant davant d'interrupcions, gràcies a l'alta disponibilitat i la recuperació davant desastres.

### Conceptes Clau

Per entendre com Azure protegeix les dades, hem de conèixer dos indicadors fonamentals:

- **RPO (Recovery Point Objective):** És la quantitat màxima de dades que l'empresa pot admetre perdre (mesurat en temps des de l'últim backup).
- **RTO (Recovery Time Objective):** És el temps màxim que pot estar el sistema caigut fins que es restaura el servei.

### Escenaris de pèrdua de dades

Azure SQL Database està dissenyat per protegir-nos davant de diversos tipus d'incidències:

**1. Errors humans o malintencionats:** Com l'eliminació accidental de files, taules o, fins i tot, de tota la base de dades.

**2. Fallades d'infraestructura:** Problemes en el maquinari, la xarxa o el subministrament elèctric del centre de dades.

**3. Desastres a gran escala:** Incidències que afecten tota una regió geogràfica (com desastres naturals).

### Eines de Recuperació a Azure

El servei ofereix diferents solucions segons la gravetat del problema:

- **Còpies de seguretat automàtiques:** Azure realitza backups periòdics que permeten fer una **Restauració a un punt en el temps (PITR)**. Això serveix per recuperar dades esborrades per error tornant la base de dades a un estat anterior exacte.
- **Replicació geogràfica activa:** Permet tenir còpies llegibles de la base de dades en altres regions per si la regió principal falla.
- **Grups de migració per error (Failover):** Per a aplicacions crítiques, permet que el sistema passi automàticament a una regió secundària amb una pèrdua mínima de dades i temps.

<br>

## Còpies de seguretat automatitzades a Azure SQL Database

### Què és una còpia de seguretat de base de dades?

Les còpies de seguretat de base de dades són una part essencial de qualsevol estratègia de **continuïtat empresarial i recuperació davant desastres**, ja que ajuden a protegir les dades de danys o eliminacions. Aquestes còpies de seguretat permeten la restauració de la base de dades a un **moment donat** (conegut com a Point-in-Time Restore) dins del període de retenció configurat.

Si les regles de protecció de dades de l'empresa exigeixen que les còpies de seguretat estiguin disponibles durant un temps prolongat (fins a **10 anys**), es pot configurar la **retenció a llarg termini (LTR)** tant per a bases de dades úniques com per a grups de bases de dades.

### Freqüència de les còpies de seguretat

L'Azure SQL Database crea:

#### Còpies de seguretat completes

Una còpia de seguretat completa de la base de dades crea una còpia de tot el conjunt de dades. Això inclou la part del registre de transaccions per poder recuperar la base de dades completa després de restaurar una còpia de seguretat completa. Les còpies de seguretat completes representen l'estat de la base de dades en el moment en què ha finalitzat la còpia. Trobem dos tipologies de còpies de seguretat completas: 

**1. Model de recuperació simple:** Amb el model de recuperació simple, després de cada còpia de seguretat, la base de dades queda exposada a la pèrdua potencial de la feina realitzada en cas de desastre. El risc de pèrdua de dades augmenta amb cada actualització fins que es realitza la següent còpia de seguretat; en aquell moment, el risc de pèrdua torna a zero i comença un nou cicle de risc. Com més temps passa entre una còpia de seguretat i la següent, més gran és el risc de pèrdua de la feina. La següent il·lustració mostra el risc de pèrdua de dades en una estratègia que només utilitza còpies de seguretat completes de la base de dades.

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA3-RA6/images/Captura14.gif" width="450" height="auto"/>
  </div>

**2. Model de recuperació completa:** En les bases de dades que utilitzen la recuperació completa i optimitzada per a càrregues massives de registres, les còpies de seguretat de la base de dades són necessàries però no suficients. També es requereixen còpies de seguretat dels registres de transaccions.
La freqüència exacta de les còpies del registre de transaccions depèn de la mida del procés i de la quantitat d'activitat que tingui la base de dades. Quan es restaura una base de dades, el mateix servei determina quina còpia completa, diferencial o del registre de transaccions és necessari recuperar. La següent il·lustració mostra l'estratègia de còpia de seguretat menys complexa en un model de recuperació completa.

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA3-RA6/images/Captura15.gif" width="450" height="auto"/>
  </div>

#### Còpies de seguretat diferencials

La còpia de seguretat diferencial es basa en la còpia de seguretat de dades completa anterior més recent. Una còpia diferencial captura només les dades que han canviat després de l'última còpia completa. La còpia de seguretat completa en la qual es basa una diferencial s'anomena base de la diferencial.

La següent il·lustració mostra com funciona una còpia de seguretat diferencial:

- Abast de la còpia: Si tenim, per exemple, 24 extensions de dades i només 6 han canviat, la còpia diferencial contindrà exclusivament aquestes sis extensions.
- Mecanisme tècnic: L'operació de còpia de seguretat diferencial es basa en una pàgina de mapa de bits que conté un bit per cada extensió.
- Detecció de canvis: Per cada extensió que s'ha actualitzat des que es va crear la base, el bit s'estableix en 1 en el mapa de bits, indicant al sistema que aquesta dada ha d' ser inclosa en la següent diferencial.

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA3-RA6/images/Captura17.png" width="450" height="auto"/>
  </div>

#### Còpies de seguretat de registres de transaccions (només SQL Server)

Un administrador de bases de dades normalment crea una còpia de seguretat completa de la base de dades, per exemple, setmanalment; si ho desitja, també pot crear una sèrie de còpies de seguretat diferencials de la base de dades a intervals més curts, per exemple, diàriament. Amb independència de les còpies de seguretat de la base de dades, l'administrador de la base de dades fa còpies de seguretat del registre de transaccions cada poc temps. En el cas d'un tipus de còpia de seguretat concret, l'interval òptim dependrà de diversos factors, com ara la importància de les dades, la mida de la base de dades i la càrrega de treball del servidor.

### Redundància de l'emmagatzematge de les còpies

El mecanisme de redundància guarda diverses còpies de les dades per protegir-les de fallades de maquinari, talls de llum o desastres naturals.

#### Punts clau per configurar a Azure:

- **Configuració per defecte:** Les noves bases de dades utilitzen redundància geogràfica (GRS), replicant les dades en una regió secundària per permetre la restauració si la regió principal cau.
- **Entorns de treball:** desenvolupament: S’estableix per defecte la **redundància local (LRS)** per reduir costos, sent ideal per a entorns de proves. Producció: S’estableix per defecte la **redundància geogràfica (GRS)** per a màxima seguretat.Flexibilitat: Es pot canviar la redundància en qualsevol moment (de geogràfica a local o de zona) per garantir que les dades no surtin d'una regió específica.
- **Aplicació dels canvis:** Si modifiques la redundància d'una base de dades ja existent, el canvi només afectarà les còpies futures i pot trigar fins a 48 hores a aplicar-se.
- **Àmbit:** La configuració escollida s'aplica tant a les còpies de retenció a curt termini (STR) com a les de llarg termini (LTR).

Azure permet escollir entre: 

- **Emmagatzematge amb redundància local (LRS):** copia les còpies de seguretat de forma sincrònica tres vegades dins d'una única ubicació física a la regió primària. L'LRS és l'opció d'emmagatzematge menys costosa, però no es recomana per a aplicacions que requereixin resistència a interrupcions regionals o una garantia d'alta durabilitat de les dades.

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA3-RA6/images/Captura20.svg" width="600" height="auto"/>
  </div>

- **Emmagatzematge amb redundància de zona (ZRS):** copia les còpies de seguretat de forma sincrònica en tres zones de disponibilitat d'Azure a la regió primària. Actualment, només està disponible en determinades regions.

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA3-RA6/images/Captura21.svg" width="600" height="auto"/>
  </div>

- **Emmagatzematge amb redundància geogràfica (GRS):** copia les còpies de seguretat de forma sincrònica tres vegades dins d'una única ubicació física a la regió primària mitjançant LRS. A continuació, copia les dades de forma asincrònica tres vegades en una sola ubicació física a la regió secundària aparellada.

El resultat és el següent:
- Tres còpies sincròniques a la regió primària.
- Tres còpies sincròniques a la regió aparellada que s'han copiat de la regió primària a la regió secundària de forma asincrònica.

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA3-RA6/images/Captura22.svg" width="600" height="auto"/>
  </div>

- **Emmagatzematge amb redundància de zona geogràfica (GZRS):** combina l'alta disponibilitat que proporciona la redundància entre zones de disponibilitat (ZRS) amb la protecció davant d'interrupcions regionals que ofereix la replicació geogràfica (GRS).

El procés funciona de la següent manera:
- A la regió primària: es realitza una còpia de seguretat de forma sincrònica en tres zones de disponibilitat d'Azure.
- A la regió secundària aparellada: les dades es copien de forma asincrònica tres vegades en una sola ubicació física.Aquesta opció es recomana per a aplicacions que requereixen la màxima coherència, durabilitat i disponibilitat, juntament amb una gran resistència per a la recuperació davant desastres.

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA3-RA6/images/Captura23.png" width="600" height="auto"/>
  </div>
