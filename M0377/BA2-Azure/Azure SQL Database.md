# Creació de base de dades única: Azure SQL Database

En aquesta primera fase, creareu una base de dades única a Azure SQL Database mitjançant l'Azure Portal, un script de PowerShell o un script de la CLI d'Azure. A continuació, fareu una consulta de la base de dades mitjançant l'editor de consultes a l'Azure Portal.

#### Requisits previs

- Una subscripció d'Azure activa. **Ja tenim!**
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

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/M0377/BA2-Azure/images/Figura%201.png" width="650" height="auto"/>
  </div>
  
4. A la pestanya **Bàsic** del formulari **Create SQL Database**, a **Detalls del projecte**, seleccioneu la **subscripció** d’Azure correcta.

5. A **Grup de recursos**, seleccioneu **Crear nou**, escriviu `miGrupoDeRecursos` i seleccioneu **Aceptar**.

6. A Nom de la base de dades, escriviu `miBaseDeDatosDeEjemplo`.

7. A **Servidor**, seleccioneu **Crear nuevo** i ompliu el formulari **Nuevo servidor** amb els valors següents:

- **Nom del servidor:** Escriviu elMeuServidorSql i afegiu alguns caràcters perquè el nom sigui únic. No es pot proporcionar un nom de servidor exacte perquè els noms dels servidors han de ser globalment únics per a tots els servidors d’Azure, no només dins d’una subscripció. L'Azure Portal us indicarà si el nom que escriviu està disponible o no.
- **Ubicació:** Seleccioneu una ubicació a la llista desplegable.
- **Mètode d'autenticació:** seleccioneu Ús de l'autenticació de SQL.
- **Inici de sessió de l'administrador del servidor:** escriviu usuariazure.
- **Contrasenya:** escriviu una contrasenya que compleixi els requisits i torneu-la a escriure al camp Confirmar contrasenya.

>[!IMPORTANT]
> No inclogueu cap informació personal, sensible o confidencial al camp de nom d'inici de sessió de l'administrador del servidor. Les dades especificades en aquest camp no es consideren dades del client.

8. Seleccioneu **Aceptar**.
Important: 

9. Deixeu **¿Quiere usar un grupo elástico de SQL?** establert en **No**.

10. A **Entorno de carga de trabajo**, especifiqueu **Desarrollo** per a aquest exercici.

L'Azure Portal proporciona una opció **d'entorn de càrrega de treball** que ajuda a establir prèviament algunes opcions de configuració. Aquests paràmetres es poden anul·lar. Aquesta opció només s'aplica a la **pàgina de Crear portal de SQL Database**. Altrament, l'opció **d'entorn de càrrega de treball** no afecta les llicències ni altres opcions de configuració de la base de dades.

- L'elecció de l'entorn de **desarrollo** estableix algunes opcions, com ara:
- La **Redundancia del almacenamiento de copia de seguridad** és emmagatzematge amb redundància local (LRS). Té un cost menor i és adequat per a entorns de preproducció.
- **Compute + storage** és d'ús general, sense servidor (serverless) amb un únic nucli virtual (vCore). Per defecte, hi ha un retard de pausa automàtica d'una hora.
- Triar l'entorn de **Producción** estableix:
- Per defecte, redundància d'emmagatzematge geogràfic.
- **Compute + storage** d'ús general, aprovisionat amb 2 vCores i 32 GB d'emmagatzematge.

1. A **Proceso y almacenamiento**, seleccioneu **Configurar base de datos**.

2. En aquest inici ràpid s'utilitza una base de dades sense servidor, així que deixeu el **nivel de servicio** establert en **De uso general (proceso económico y sin servidor)** i establiu el **Nivel de proceso** en **Sin servidor**. Seleccioneu **Aplicar**.

3. A **Redundancia del almacenamiento de copia de seguridad**, trieu una opció de redundància per al compte on es desaran les còpies.

4. Seleccioneu **Siguiente: Redes**.

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/M0377/BA2-Azure/images/Figura%202.png" width="650" height="auto"/>
  </div>

5. A la pestanya **Redes**, a **Métode de conectividad**, seleccioneu **Punto de conexión público**.

6. A **Reglas de firewall**, establiu **Agregar dirección IP del cliente actual** en **Sí**. Deixeu l'opció **Permitir que los servicios y recursos de Azure accedan a este grupo de servidores** en **No**.

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/M0377/BA2-Azure/images/Figura%203.png" width="650" height="auto"/>
  </div>

7. A **Directiva de conexión**, trieu la Directiva per defecte i deixeu la **Versión mínima de TLS** al valor 1.2.

8. Seleccioneu **Siguiente: Seguridad**.

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/M0377/BA2-Azure/images/Figura%204.png" width="650" height="auto"/>
  </div>

9. A la pestanya **Configuración adicional**, a la secció **Orígenes de datos**, a **Usar datos existentes**, seleccioneu **Ejemplo**. Es crearà la base de dades d'exemple `AdventureWorksLT` perquè tingueu taules i dades per consultar. 

10. Seleccioneu **Revisar + crear**.

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/M0377/BA2-Azure/images/Figura%205.png" width="650" height="auto"/>
  </div>

11. A la pàgina de revisió, un cop verificat tot, seleccioneu **Crear**.

<br>

**Documentació:** https://learn.microsoft.com/es-es/azure/azure-sql/database/single-database-create-quickstart?view=azuresql&tabs=azure-portal
