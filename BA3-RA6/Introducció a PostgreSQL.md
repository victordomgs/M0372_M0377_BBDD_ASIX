# Introducció a PostgreSQL
PostgreSQL és un sistema de gestió de bases de dades relacionals potent i de codi obert. A continuació es detallen els primers passos per a la seva instal·lació i ús bàsic mitjançant el client psql.

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


## L'usuari Administrador (postgres)

- **Accés Inicial:** Quan s'instal·la per primera vegada, només l'usuari postgres pot connectar-se.
- **Seguretat:** Aquest usuari existeix tant a PostgreSQL com al sistema operatiu i, per defecte, no té contrasenya.
- **Validació:** Per accedir-hi, primer cal ser l'administrador (root o via sudo) del sistema operatiu.
- **Restricció Local:** L'usuari postgres només es pot utilitzar des de la mateixa màquina on s'executa el servidor.

> [!NOTE]
> En determinats entorns (com màquines Vagrant de desenvolupament), pot existir un usuari anomenat admin amb permisos per a connexions remotes.

## El client psql

`psql` és la interfície de línia de comandes per interactuar amb PostgreSQL.

### Flux de treball bàsic

1. **Canvi d'usuari:** Accedim al terminal com a usuari postgres.

```Bash
sudo su - postgres
```

2. **Llistar bases de dades:** Visualitzem totes les bases de dades existents al servidor.

```Bash
psql -l
```

3. **Connexió a una BD:** Accedim de forma interactiva a una base de dades específica (ex: hotel).

```Bash
psql -d hotel
```

### Comandes essencials de psql

## 1.5. Ordres essencials del client `psql`

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
