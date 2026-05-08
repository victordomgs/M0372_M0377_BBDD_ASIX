# Creació de base de dades única: Azure SQL Database

En aquesta primera fase, creareu una base de dades única a Azure SQL Database mitjançant l'Azure Portal, un script de PowerShell o un script de la CLI d'Azure. A continuació, fareu una consulta de la base de dades mitjançant l'editor de consultes a l'Azure Portal.

#### Requisits previs

- Una subscripció d'Azure activa. Ja tenim!
- Treballarem amb l'Azure Portal. Opcionalment, utilitzeu la versió més recent d'Azure PowerShell o de la CLI d'Azure.

#### Permisos
**Per crear bases de dades a través de Transact-SQL:** es necessiten permisos de CREATE DATABASE. Per crear una base de dades, l'inici de sessió ha de correspondre a l'administrador del servidor (creat en aprovisionar el servidor lògic d'Azure SQL Database), a l'administrador de Microsoft Entra del servidor, o a un membre del rol de base de dades dbmanager a master.

**Per crear bases de dades a través del portal d'Azure, PowerShell, la CLI d'Azure o l'API REST**: es necessiten permisos de RBAC d'Azure, específicament el rol de Col·laborador, el rol de Col·laborador de base de dades SQL o el rol de Col·laborador de SQL Server.

## Crear una base de dades única

En aquest inici ràpid es crea una base de dades única en el **nivell de computació sense servidor (serverless)**.

Per crear una base de dades única a l’Azure Portal:

1. Aneu al centre d'**Azure SQL** a [aka.ms/azuresqlhub](https://aka.ms/azuresqlhub).

2. Al menú de recursos, desplegueu **Azure SQL Database** i seleccioneu **Bases de dades SQL**.

3. Seleccioneu el botó desplegable **+ Crear** i seleccioneu **BASE de dades SQL**.

4. A la pestanya **Bàsic** del formulari **Create SQL Database**, a **Detalls del projecte**, seleccioneu la **subscripció** d’Azure correcta.

5. A **Grup de recursos**, seleccioneu **Crear nou**, escriviu `miGrupoDeRecursos` i seleccioneu **Aceptar**.

A Nom de la base de dades, escriviu laMevaBaseDeDadesDeExemple.

A Servidor, seleccioneu Crear nou i ompliu el formulari Nou servidor amb els valors següents:

Nom del servidor: Escriviu elMeuServidorSql i afegiu alguns caràcters perquè el nom sigui únic. No es pot proporcionar un nom de servidor exacte perquè els noms dels servidors han de ser globalment únics per a tots els servidors d’Azure, no només dins d’una subscripció. L'Azure Portal us indicarà si el nom que escriviu està disponible o no.

Ubicació: Seleccioneu una ubicació a la llista desplegable.

Mètode d'autenticació: seleccioneu Ús de l'autenticació de SQL.

Inici de sessió de l'administrador del servidor: escriviu usuariazure.

Contrasenya: escriviu una contrasenya que compleixi els requisits i torneu-la a escriure al camp Confirmar contrasenya.

Important: No inclogueu cap informació personal, sensible o confidencial al camp de nom d'inici de sessió de l'administrador del servidor. Les dades especificades en aquest camp no es consideren dades del client.

Seleccioneu D’acord.

Deixeu Voleu utilitzar un grup elàstic de SQL? establert en No.

A Entorn de càrrega de treball, especifiqueu Desenvolupament per a aquest exercici.

L'Azure Portal proporciona una opció d'entorn de càrrega de treball que ajuda a establir prèviament algunes opcions de configuració. Aquests paràmetres es poden anul·lar. Aquesta opció només s'aplica a la pàgina de creació del portal de SQL Database. Altrament, l'opció d'entorn de càrrega de treball no afecta les llicències ni altres opcions de configuració de la base de dades.

L'elecció de l'entorn de desenvolupament estableix algunes opcions, com ara:

La redundància de l'emmagatzematge de còpia de seguretat és emmagatzematge amb redundància local (LRS). Té un cost menor i és adequat per a entorns de preproducció.

Compute + storage és d'ús general, sense servidor (serverless) amb un únic nucli virtual (vCore). Per defecte, hi ha un retard de pausa automàtica d'una hora.

Triar l'entorn de Producció estableix:

Per defecte, redundància d'emmagatzematge geogràfic.

Compute + storage d'ús general, aprovisionat amb 2 vCores i 32 GB d'emmagatzematge.

A Computació i emmagatzematge, seleccioneu Configurar base de dades.

En aquest inici ràpid s'utilitza una base de dades sense servidor, així que deixeu el nivell de servei establert en D'ús general (computació econòmica i sense servidor) i establiu el Nivell de computació en Sense servidor (serverless). Seleccioneu Aplica.

A Redundància de l'emmagatzematge de còpia de seguretat, trieu una opció de redundància per al compte on es desaran les còpies.

Seleccioneu Següent: Xarxes.

A la pestanya Xarxes, a Mètode de connectivitat, seleccioneu Punt de connexió públic.

A Regles de tallafoc (firewall), establiu Afegir adreça IP del client actual en Sí. Deixeu l'opció Permetre que els serveis i recursos d'Azure accedeixin a aquest servidor en No.

A Directiva de connexió, trieu la Directiva per defecte i deixeu la Versió mínima de TLS al valor 1.2.

Seleccioneu Següent: Seguretat.

A la pàgina Seguretat, podeu optar per iniciar una avaluació gratuïta de Microsoft Defender for SQL, així com configurar Ledger, Identitats gestionades i el xifratge de dades (TDE) si ho desitgeu. Seleccioneu Següent: Configuració addicional.

A la pestanya Configuració addicional, a la secció Orígens de dades, a Utilitzar dades existents, seleccioneu Exemple. Es crearà la base de dades d'exemple AdventureWorksLT perquè tingueu taules i dades per consultar. També podeu configurar la intercol·lació i la finestra de manteniment.

Seleccioneu Revisar i crear.

A la pàgina de revisió, un cop verificat tot, seleccioneu Crear.
