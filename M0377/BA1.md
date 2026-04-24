# Creació de vistes

Una _vista_ és similar a una taula, però no guarda dades reals, sinó que les dades que s'hi mostren són el resultat d'executar una SELECT sobre altres taules.

Per exemple, a la base de dades Sakila tenim la vista `film_list` on podem veure tota la informació de les pel·lícules recopilada: el títol, els actors que hi apareixen, els seus gèneres, etc.

Aquestes dades provenen de diverses taules (el títol de `film`, els actors de `film_actor` i `actor`, i els gèneres de `film_category` i `category`, però en aquesta vista s'han recopilat de manera que visualment sigui més còmode consultar-les.

Una vista no emmagatzema dades noves, sinó que el que s'hi veu és el resultat d'executar una SELECT que s'ha guardat com a part de la definició de la vista (podem veure la SELECT en qüestió executant `SHOW CREATE TABLE film_list`).

Sobre una vista podem executar qualsevol SELECT però no podem executar-hi la major part de sentències de modificació de dades, perquè és complicat saber a quines taules reals s'haurien d'aplicar les modificacions.

Per crear una vista s'utilitza la sentència `SELECT VIEW`. Per exemple, sobre la base de dades Chinook podríem crear una vista que ens resumís la informació de cada àlbum:

```sql
CREATE VIEW AlbumInfo AS
  SELECT al.Title AS Album, a.Name AS Artist, GROUP_CONCAT(t.Name) AS Tracks
  FROM Album al
  JOIN Artist a ON al.ArtistId=a.ArtistId
  JOIN Track t ON al.AlbumId=t.AlbumId
  GROUP BY al.AlbumId, al.Title, a.Name;
```

Aquesta vista conté el títol de l'àlbum, el nom de l'artista, i una llista amb el nom de les cançons incloses.

> [!NOTE]
> La funció _GROUP_CONCAT()_ uneix tots els valors d'una columna que s'han unit per causa d'un _GROUP BY_ en una única cadena de text.

Podem fer consultes sobre aquesta vista:

```sql
SELECT * FROM AlbumInfo WHERE Artist LIKE 'The Rolling Stones'\G
*************************** 1. row ***************************
 Album: Hot Rocks, 1964-1971 (Disc 1)
Artist: The Rolling Stones
Tracks: Under My Thumb,Play With Fire,Get Off Of My Cloud,Paint It Black,
Let's Spend The Night Together,Heart Of Stone,As Tears Go By,
19th Nervous Breakdown,Ruby Tuesday,Time Is On My Side,Satisfaction,
Mother's Little Helper
*************************** 2. row ***************************
 Album: No Security
Artist: The Rolling Stones
Tracks: Out Of Control,Intro,Flip The Switch,Saint Of Me,Live With Me,
The Last Time,Gimmie Shelters,Corinna,Sister Morphine,Thief In The Night,
You Got Me Rocking,Memory Motel,Wainting On A Friend,Respectable
*************************** 3. row ***************************
 Album: Voodoo Lounge
Artist: The Rolling Stones
Tracks: Sparks Will Fly,Moon Is Up,Brand New Car,Blinded By Rainbows,
Mean Disposition,You Got Me Rocking,New Faces,I Go Wild,Suck On The Jugular,
Thru And Thru,Love Is Strong,The Worst,Out Of Tears,Sweethearts Together,
Baby Break It Down
```

Aquesta vista no permetrà modificacions perquè el GROUP BY fa que no es pugui saber quines eren les files originals a modificar.

Al següent exemple creem una vista que ens mostra la informació dels temes i el nom del seu gènere:

```sql
CREATE VIEW TrackGenre AS
  SELECT TrackId, Track.Name AS TrackName, Genre.Name AS GenreName
  FROM Track
  JOIN Genre ON Track.GenreId=Genre.GenreId;
```

En aquest cas podem, per exemple, executar UPDATE perquè el SGBD pot deduir quina és la dada real que ha de modificar:

```sql
SELECT * FROM TrackGenre WHERE TrackId=10;
+---------+------------+-----------+
| TrackId | TrackName  | GenreName |
+---------+------------+-----------+
|      10 | Evil Walks | Rock      |
+---------+------------+-----------+
1 row in set (0.00 sec)
```

```sql
UPDATE TrackGenre SET GenreName='Blues' WHERE TrackId=10;
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```

> [!WARNING]
> El canvi que s'ha fet potser no era el que esperàvem: s'ha modificat el nom del gènere Rock i se li ha dit Blues, no s'ha modificat el `GenreId` del tema.
