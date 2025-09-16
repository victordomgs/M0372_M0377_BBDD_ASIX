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

Els SGBD neixen per superar aquests problemes. Els seus objectius principals són garantir eficiència, seguretat i consistència en la gestió de grans volums d’informació. Les dades es defineixen una sola vegada, amb mínima duplicació i accés simultani per diversos usuaris. A més, els metadades es guarden en un diccionari de dades.
---
## 2. Arquitectura dels sistemes de bases de dades


