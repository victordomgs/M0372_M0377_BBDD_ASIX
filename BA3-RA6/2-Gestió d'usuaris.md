# Control del llenguatge de dades (DCL) en PostgreSQL

## Introducció
El **Data Control Language (DCL)** en PostgreSQL s'utilitza per gestionar els privilegis i permisos dels usuaris en la base de dades. Les instruccions principals de DCL són `GRANT` i `REVOKE`, que permeten respectivament atorgar i revocar permisos sobre objectes de la base de dades com taules, vistes, seqüències i esquemes.

---
## 1. Comandes principals de DCL

### 1.1. `GRANT`
La instrucció `GRANT` s'utilitza per concedir privilegis als usuaris o rols sobre certs objectes de la base de dades.

#### **Sintaxi:**
```sql
GRANT <privilegis> ON <objecte> TO <usuari | rol> [WITH GRANT OPTION];
```

#### **Exemple:**
```sql
GRANT SELECT, INSERT ON clients TO usuari1;
```
*Aquesta comanda atorga permisos de consulta (`SELECT`) i inserció (`INSERT`) sobre la taula `clients` a `usuari1`.*

##### **Amb `WITH GRANT OPTION`**
```sql
GRANT ALL PRIVILEGES ON clients TO usuari1 WITH GRANT OPTION;
```
*Això permet que `usuari1` pugui, al seu torn, concedir aquests mateixos privilegis a altres usuaris.*

---

### 1.2. `REVOKE`
La instrucció `REVOKE` s'utilitza per retirar permisos prèviament atorgats a un usuari o rol.

#### **Sintaxi:**
```sql
REVOKE <privilegis> ON <objecte> FROM <usuari | rol>;
```

#### **Exemple:**
```sql
REVOKE INSERT ON clients FROM usuari1;
```
*Aquesta comanda elimina el permís d'inserció (`INSERT`) a la taula `clients` per `usuari1`.*

##### **Revocar tots els privilegis:**
```sql
REVOKE ALL PRIVILEGES ON clients FROM usuari1;
```
*Això elimina tots els permisos de `usuari1` sobre la taula `clients`.*

---
## 2. Tipus de privilegis en PostgreSQL
PostgreSQL permet atorgar diferents tipus de privilegis sobre objectes de la base de dades. Aquests són alguns dels més utilitzats:

| Privilegi   | Descripció |
|-------------|--------------------------------------------------------------------------------|
| `SELECT`   | Permet consultar dades en una taula o vista. |
| `INSERT`   | Permet inserir noves files en una taula. |
| `UPDATE`   | Permet modificar files existents en una taula. |
| `DELETE`   | Permet eliminar files d'una taula. |
| `TRUNCATE` | Permet esborrar totes les files d'una taula de manera eficient. |
| `REFERENCES` | Permet crear claus foranes que apuntin a una taula. |
| `EXECUTE`  | Permet executar funcions o procediments emmagatzemats. |
| `USAGE`    | Permet accedir a esquemes i seqüències. |
| `ALL PRIVILEGES` | Atorga tots els privilegis disponibles sobre un objecte. |

---
## 3. Gestió d'usuaris i rols

### 3.1. Crear un usuari nou
```sql
CREATE USER usuari1 WITH PASSWORD 'contrasenya_segura';
```
*Això crea un usuari anomenat `usuari1` amb una contrasenya.*

> [!IMPORTANT]  
> Per defecte postgres té activada l'autenticació 'peer', per tant s'ha de modificar aquesta configuració.
> 
> Editem l'arxiu ```pg_hba.conf``` per canviar el mètode d'autenticació: ```sudo nano /etc/postgresql/14/main/pg_hba.conf```.
> 
> Busquem la següent línea: ```local   all   all   peer```.
> 
> I canviem per: ```local   all   all   md5```.


### 3.2. Crear un rol
```sql
CREATE ROLE gestor;
```
*Això crea un rol anomenat `gestor`.*

### 3.3. Assignar un rol a un usuari
```sql
GRANT gestor TO usuari1;
```
*Això assigna el rol `gestor` a `usuari1`, heretant els seus privilegis.*

### 3.4. Eliminar un usuari o rol
```sql
DROP USER usuari1;
DROP ROLE gestor;
```
*Aquestes comandes eliminen un usuari i un rol respectivament.*

---
## 4. Consideracions de seguretat
- **Evitar donar `ALL PRIVILEGES`** a tots els usuaris per minimitzar riscos de seguretat.
- **Utilitzar rols** en lloc d’atorgar privilegis directament als usuaris, facilitant la gestió de permisos.
- **Revocar permisos quan un usuari ja no els necessiti** per evitar accessos innecessaris.

---
## 5. Exemples pràctics

### **Escenari 1: Crear un usuari i atorgar-li privilegis**
```sql
CREATE USER empleat1 WITH PASSWORD 'segura123';
GRANT SELECT, INSERT ON clients TO empleat1;
```
*Aquestes comandes creen `empleat1` i li atorguen permisos per llegir i inserir dades a `clients`.*

### **Escenari 2: Crear un rol i assignar-li permisos**
```sql
CREATE ROLE admin_ventas;
GRANT ALL PRIVILEGES ON clients TO admin_ventas;
GRANT admin_ventas TO empleat1;
```
*Això crea un rol `admin_ventas` amb permisos complets sobre `clients` i l’assigna a `empleat1`.*

### **Escenari 3: Revocar permisos a un usuari**
```sql
REVOKE SELECT, INSERT ON clients FROM empleat1;
DROP USER empleat1;
```
*Aquesta operació elimina els permisos de `empleat1` i després esborra l’usuari.*
