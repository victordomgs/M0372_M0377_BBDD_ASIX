# Sistemes gestors de bases de dades

## 1. Introducció

Un **Sistema Gestor de Bases de Dades (SGBD)**, o **DBMS (Database Management System)**, és un conjunt de dades relacionades i estructurades, i un conjunt de programes que permeten accedir i gestionar aquestes dades. La col·lecció de dades es coneix com a **Base de Dades (BD)**.

Abans de l’aparició dels SGBD (dècada dels 70), la informació es gestionava mitjançant **sistemes de fitxers** sobre el sistema operatiu. Cada aplicació definia i gestionava les seves pròpies dades dins d’arxius. Qualsevol canvi en l’estructura dels arxius obligava a modificar els programes associats.

### Inconvenients dels sistemes de fitxers

- **Redundància i inconsistència:** dades duplicades i incoherents en diferents fitxers.
- **Dependència física-lògica:** els programes depenen de l’estructura dels arxius.
- **Dificultat d’accés:** cal crear programes específics per obtenir noves consultes.
- **Aïllament i separació de dades:** formats diferents i dispersió de la informació.
- **Accés concurrent complicat:** problemes si diversos usuaris actualitzen alhora.
- **Dependència del llenguatge:** incompatibilitat entre arxius creats amb diferents llenguatges.
- **Seguretat limitada:** difícil imposar restriccions d’accés.
- **Integritat feble:** cal afegir molt codi per controlar restriccions i consistència.

### Avantatges dels SGBD

Els SGBD neixen per superar aquests problemes. Els seus objectius principals són garantir **eficiència, seguretat i consistència** en la gestió de grans volums d’informació. Les dades es defineixen una sola vegada, amb **mínima duplicació** i accés **simultani** per diversos usuaris. A més, els **metadades** es guarden en un **diccionari de dades**.

### Serveis d'un SGBD

Un SGBD ha de proporcionar:

- **Definició de la BD:** estructures, tipus de dades, restriccions i relacions (mitjançant llenguatges de definició de dades).
- **Manipulació de dades:** consultes, insercions i actualitzacions (amb llenguatges de manipulació de dades).
- **Control d’accés i seguretat:** mecanismes per limitar l’accés als usuaris.
- **Integritat i consistència:** mantenir la correcció dels valors i evitar modificacions no autoritzades.
- **Accés compartit i control de concurrència**.
- **Còpies de seguretat i recuperació** en cas de fallades del sistema.

---

## 2. Arquitectura dels sistemes de bases de dades

L’any 1975, el comitè **ANSI-SPARC** va proposar una arquitectura de tres nivells per als SGBD, amb l’objectiu de separar els programes d’aplicació de la base de dades física. Aquesta arquitectura defineix els esquemes d’una BD en tres nivells d’abstracció:

### Nivells d’abstracció

- **Nivell intern o físic:** el més proper a l’emmagatzematge. Defineix com s’organitzen els arxius, els mètodes d’accés i la representació física de les dades.
- **Nivell conceptual:** descriu l’estructura completa de la BD (entitats, atributs, relacions i restriccions) amagant els detalls físics.
- **Nivell extern o de visió:** defineix les vistes dels usuaris, mostrant només la part de la BD que els interessa.

  <div style="text-align: center;">
    <img src="https://github.com/victordomgs/M0372_M0377_BBDD_ASIX/blob/main/BA1-RA1_RA2/images/Figura%201.%20Nivell%20d'abstracci%C3%B3%20de%20l'arquitectura%20ANSI.png" alt="ANSI" width="650" height="auto"/>
    <p><em>Figura 1: Nivell d'abstracció de l'arquitectura ANSI</em></p>
  </div>

### Funcionament

Quan un usuari fa una consulta:

1.	El SGBD valida l’esquema extern corresponent.
2.	Transforma la petició al nivell conceptual.
3.	Transforma després la petició al nivell intern i executa la consulta.
4.	Els resultats es retornen del nivell intern al conceptual i d’aquest a l’extern.

Per a una BD només existeix un esquema intern i un de conceptual, però es poden definir múltiples esquemes externs segons els usuaris.

### Independència de dades
Aquesta arquitectura introdueix dos tipus d’independència:

- **Independència lògica:** permet modificar l’esquema conceptual sense afectar els esquemes externs ni els programes.
- **Independència física:** permet reorganitzar o canviar l’estructura interna sense afectar l’esquema conceptual ni els externs.

### Limitacions
Encara que el model és teòricament molt útil, pocs SGBD han implementat completament aquesta arquitectura, ja que la transformació entre nivells pot reduir l’eficiència del sistema.

---

## 3. Components de les SGBD

Els **Sistemes Gestors de Bases de Dades (SGBD)** són paquets de programari complexos que proporcionen serveis per emmagatzemar i gestionar dades de manera eficient. Els principals components són:

### A. Llenguatges dels SGBD

Els SGBD ofereixen llenguatges i interfícies segons el tipus d’usuari (administradors, dissenyadors, programadors i usuaris finals):
- **Llenguatge de definició de dades (LDD/DDL):** defineix l’esquema conceptual i intern de la BD, les vistes i les estructures d’emmagatzematge.
- **Llenguatge de manipulació de dades (LMD/DML):** permet consultar, inserir, modificar i eliminar dades. Pot ser:
  ⦁	**Procedural:** el programador indica les operacions pas a pas (ex. BD jeràrquiques o en xarxa).
  ⦁	**No procedural (declaratiu):** l’usuari indica què vol obtenir sense especificar com (ex. SQL, QBE).
- **Llenguatges de quarta generació (4GL):** eines que faciliten el desenvolupament ràpid d’aplicacions (ex. Oracle SQL Forms, Reports, PL/SQL).

### B. Diccionari de dades

El **diccionari de dades** és el repositori que descriu la BD i els seus objectes. Inclou informació sobre:
- Estructura lògica i física.
- Definició d’objectes (taules, vistes, índexs, procediments...).
- Espai assignat i utilitzat.
- Restriccions d’integritat.
- Privilegis i rols dels usuaris.
- Auditoria d’accessos.

#### Característiques d’un bon diccionari de dades:

- Suport als models conceptual, lògic, intern i extern.
- Integració dins el SGBD.
- Actualització automàtica dels canvis en la BD i en els programes.
- Emmagatzematge en mitjans d’accés directe per a una recuperació eficient.

### C. Seguretat i integritat

Un SGBD ha de garantir:
- **Control d’accessos:** evitar accessos no autoritzats.
- **Restriccions d’integritat:** assegurar la consistència de les dades.
- **Còpies de seguretat i recuperació:** restaurar la BD en cas d’error o desastre.
- **Accés concurrent segur:** mantenir la consistència quan diversos usuaris actualitzen alhora.

### D. Administrador de la BD (DBA)

El **DBA** és el responsable de la BD i té el màxim nivell de privilegis. Les seves funcions inclouen:
- Instal·lar i configurar el SGBD.
- Crear i mantenir bases de dades i esquemes.
- Gestionar comptes i permisos d’usuaris.
- Controlar recursos de disc i rendiment del sistema.
- Planificar i executar còpies de seguretat i restauració.
- Supervisar l’ús de la BD, monitorar accessos i detectar anomalies.
- Establir normes, protocols i polítiques d’ús.
- Formar usuaris i donar suport als equips de desenvolupament.
  
Amb el temps, els DBA disposen d’eines cada cop més avançades que faciliten la seva tasca diària.

---

## 3. Model de dades

Un dels objectius principals d’un **SGBD** és proporcionar als usuaris una visió **abstracta** de les dades, amagant els detalls de com estan emmagatzemades físicament.  
Aquesta abstracció es representa amb els **models de dades**, que s’organitzen en tres nivells segons l’arquitectura ANSI-SPARC:

## Nivells d’abstracció
- **Nivell físic**: descriu com s’emmagatzemen realment les dades (fitxers, sectors, índexs, etc.).  
- **Nivell lògic o conceptual**: descriu l’estructura global de la BD (entitats, atributs, relacions i restriccions).  
- **Nivell extern o de vistes**: mostra la part de la BD que interessa a un usuari o grup d’usuaris.  

En una BD hi ha **un únic nivell físic i lògic**, però es poden definir **múltiples nivells externs** (vistes).

Per fer-nos una idea dels tres nivells d'abstracció, ens imaginem un arxiu d'articles amb els següents registres:

```C
 struct ARTICULOS
 { int Cod;
 char Deno[15];
 int cant_almacen;
 int cant_minima ;
 int uni_vendidas;
 float PVP;
 char reponer;
 struct VENTAS Tventas[12];
 };
```

Els **models de dades** són conjunts de conceptes i eines per descriure l’estructura d’una BD, les seves relacions i restriccions. La descripció d’una BD amb un model de dades s’anomena **esquema**.

### 1. Models lògics basats en objectes
- Descriuen dades al nivell conceptual i extern.  
- Permeten estructuració flexible i restriccions de dades.  
- **Exemples**:  
  - **Model Entitat-Relació (E-R)**: el més utilitzat actualment.  
  - **Model orientat a objectes**: incorpora conceptes del model E-R i cada cop té més ús.  
- Molts SGBD actuals són **relacionals amb extensions orientades a objectes**.

### 2. Models lògics basats en registres
- Descriuen dades al nivell conceptual i físic.  
- La BD s’organitza en **registres de format fix** (o variable).  
- No representen directament codi, sinó que s’associen amb llenguatges de consulta i actualització.  
- **Exemples**:  
  - **Model relacional**: dades i relacions representades en taules (files = tuples, columnes = atributs).  
  - **Model de xarxa**.  
  - **Model jeràrquic**.  
- El **model relacional** és el més acceptat i serà tractat en la següent unitat.

### 3. Models físics de dades
- Descriuen **com s’emmagatzemen les dades**: formats de registres, organització d’arxius, mètodes d’accés.  
- Exemples coneguts: **model unificador** i **model de memòria d’elements**.

