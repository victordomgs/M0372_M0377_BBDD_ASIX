# Introducció a PostgreSQL
PostgreSQL és un sistema de gestió de bases de dades relacionals potent i de codi obert. A continuació es detallen els primers passos per a la seva instal·lació i ús bàsic mitjançant el client psql.

## Instal·lació en Debian

Per instal·lar PostgreSQL en sistemes basats en Debian, utilitzarem el gestor de paquets apt:

```Bash
# Actualitzem els repositoris
sudo apt-get update

# Instal·lem el servidor i el client
sudo apt-get install postgresql
L'usuari Administrador (postgres)
```

## L'usuaari Administrador (postgres)

- **Accés Inicial:** Quan s'instal·la per primera vegada, només l'usuari postgres pot connectar-se.

- **Seguretat:** Aquest usuari existeix tant a PostgreSQL com al sistema operatiu i, per defecte, no té contrasenya.

- **Validació:** Per accedir-hi, primer cal ser l'administrador (root o via sudo) del sistema operatiu.

- **Restricció Local:** L'usuari postgres només es pot utilitzar des de la mateixa màquina on s'executa el servidor.

[!NOTE]
En determinats entorns (com màquines Vagrant de desenvolupament), pot existir un usuari anomenat admin amb permisos per a connexions remotes.

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

[cite_start]A diferència de les sentències SQL, les ordres de gestió de `psql` comencen amb barra invertida (`\`) i no requereixen punt i coma al final[cite: 38].

### Gestió i Navegació
| Ordre | Descripció | Equivalent MySQL |
| :--- | :--- | :--- |
| `\l` | [cite_start]Llista totes les bases de dades del servidor [cite: 38] | `SHOW DATABASES` |
| `\c <DB>` | [cite_start]Connecta a una base de dades específica [cite: 38] | `USE <DB>` |
| `\i <fitxer>` | [cite_start]Executa un script SQL des d'un fitxer [cite: 38] | `SOURCE <fitxer>` |
| `\q` | [cite_start]Surt de l'entorn `psql` [cite: 38] | `EXIT` |
| `\x` | [cite_start]Activa/desactiva la visualització estesa de columnes [cite: 38] | `\G` |

### Inspecció d'Estructura i Privilegis
| Ordre | Descripció |
| :--- | :--- |
| `\d` | [cite_start]Mostra totes les taules, vistes i seqüències [cite: 38] |
| `\d <taula>` | [cite_start]Mostra l'estructura detallada d'una taula [cite: 38] |
| `\dt` | [cite_start]Mostra només les taules de la base de dades [cite: 39] |
| `\du` | [cite_start]Llista els rols (usuaris) i els seus privilegis globals [cite: 39] |
| `\dp` | [cite_start]Mostra els privilegis d'accés detallats dels objectes [cite: 39] |
| `\dn+` | [cite_start]Mostra els esquemes i els seus privilegis definits [cite: 39] |

> [!TIP]
> Pots utilitzar comodins per filtrar la cerca. [cite_start]Per exemple, `\dt *.*` mostra totes les taules de tots els esquemes [cite: 42][cite_start], i `\d esquema.taula` detalla una taula en un esquema concret[cite: 43].
