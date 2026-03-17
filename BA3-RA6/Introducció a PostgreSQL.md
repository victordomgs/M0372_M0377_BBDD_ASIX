# Introducció a PostgreSQL
PostgreSQL és un sistema de gestió de bases de dades relacionals potent i de codi obert. A continuació es detallen els primers passos per a la seva instal·lació i ús bàsic mitjançant el client psql.

## Instal·lació a Azure



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
