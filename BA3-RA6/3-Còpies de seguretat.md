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

## Alta Disponibilitat i Redundància a Azure

L'arquitectura d'Azure està dissenyada per garantir que les dades estiguin sempre disponibles, fins i tot quan es fan tasques de manteniment o hi ha errors de maquinari. Per aconseguir-ho, s'utilitzen diferents nivells de **redundància**:

### 1. Redundància Local (LRS)

- **Com funciona:** És el nivell bàsic. Es fan tres còpies de les dades dins d'un mateix centre de dades (una única ubicació física).
- **Protecció:** Protegeix contra fallades de disc o d'un servidor concret.
- **Limitació:** Si el centre de dades sencer té un problema (per exemple, un incendi), les dades es podrien perdre.

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA3-RA6/images/Captura%2010.png" width="550" height="auto"/>
  </div>

### 2. Redundància de Zona (ZRS)

- **Com funciona:** Les dades es repliquen de forma sincrònica en tres Zones de Disponibilitat diferents dins de la mateixa regió.
- **Què és una Zona?:** Cada zona és un centre de dades independent amb la seva pròpia alimentació, refrigeració i xarxa.
- **Protecció:** Si un centre de dades sencer cau, el servei continua funcionant des de les altres zones sense interrupció.

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA3-RA6/images/Captura%2013.png" width="550" height="auto"/>
  </div>

### 3. Redundància Geogràfica (GRS / GZRS)

- **Com funciona:** A més de la redundància local o de zona, les dades es copien de forma asincrònica a una **regió secundària** situada a centenars de quilòmetres de distància.
- **Protecció:** És la protecció màxima contra desastres regionals totals.
- **Restauració Geogràfica:** Permet recuperar la base de dades en una regió diferent si la regió principal queda totalment inoperativa.

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA3-RA6/images/Captura%2011.png" width="550" height="auto"/>
  </div>

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA3-RA6/images/Captura%2012.png" width="550" height="auto"/>
  </div>
