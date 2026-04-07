# Introducció a PostgreSQL
PostgreSQL és un **sistema de gestió de bases de dades relacionals potent i de codi obert**. A continuació es detallen els primers passos per a la seva instal·lació i ús bàsic mitjançant el client psql.

## Instal·lació a Azure

1. A la barra superior de cerca escriu "PostgreSQL" i selecciona l'opció anomenada: **Servidores flexibles de Azure Database for PostgreSQL**:

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA3-RA6/images/Captura%201.png" width="650" height="auto"/>
  </div>

<br>

2. Seleccionem el botó **+ Crear**:

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA3-RA6/images/Captura%202.png" width="650" height="auto"/>
  </div>

<br>

3. S'haurà de crear un grup de recursos prèviament a aixecar el servei (podeu posar el nom que vulgueu):

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA3-RA6/images/Captura%203.png" width="650" height="auto"/>
  </div>

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA3-RA6/images/Captura%204.png" width="650" height="auto"/>
  </div>

> [!NOTE]
> En termes senzills, un recurs és qualsevol "peça" o servei individual que crees, gestiones o compres dins de la plataforma de Azure.
> Imagina que Azure és una caixa de peces de LLEC: cada maó, roda o motor que treus de la caixa per a construir alguna cosa és un recurs.

<br>

4. Seleccionem com a regió **Spain Central** i marquem a **Tipo de carga de trabajo: Desarrollo/pruebas**:

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA3-RA6/images/Captura%205.png" width="650" height="auto"/>
  </div>

<br>

5. Modifiquem el mètode d'autenticació per tal de simplificar-ho. Afegim un nom d'usuari i una contrasenya:

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA3-RA6/images/Captura%206.png" width="650" height="auto"/>
  </div>

<br>

6. Anem a l'apartat **Redes** i mantenim la connectivitat de xarxa tal com està: amb accés públic. I agreguem l'adreça IP del nostre router per tal de permetre la connexió al servei des de la màquina actual:

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA3-RA6/images/Captura%207.png" width="650" height="auto"/>
  </div>

> [!NOTE]
> Si ens intentem connectar al servei des de casa, necessitarem afegir la IP de casa modificant el servidor.

<br>

7. Els apartat de **Seguridad** i **Etiquetas** s'ha de deixar tal i com estan. Podem doncs, donar-li a **Crear**:

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA3-RA6/images/Captura%208.png" width="650" height="auto"/>
  </div>

<br>

## El client psql

`psql` és la interfície de línia de comandes per interactuar amb PostgreSQL que hem de tenir instal·lada al nostre ordinador.

Previ a seguir el flux de treball, necessitarem connectar-nos al servidor. Podem trobar la informació a  **Información general** al panell del servei. 

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA3-RA6/images/Captura%209.png" width="650" height="auto"/>
  </div>

En el meu cas, el host seria: `asixpostgres.postgres.database.azure.com`

### Flux de treball bàsic

1. **El comando per connectar-te**

```Bash
psql "host=asixpostgres.postgres.database.azure.com port=5432 dbname=postgres user=victor sslmode=require"
```

Posem la contrasenya i ja estaríem connectats.

2. **Llistar esquemes:** Visualitzem tots els esquemes dintre de la base de dades que tenim seleccionada.

```Bash
\dn+
```

3. **Provar una consulta:**

```Bash
SELECT * FROM COLECCIO;
```
