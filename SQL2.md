# Sublenguajes SQL
-DDL Data Definition Lenguage (Opera sobre los objetos de la BD. Tablas, fila, columna, indice...)

	-CREATE
	-ALTER
	-DROP

-DML Data Manipulation Language (Antes SELECT se incluía ahi. Opera sobre los datos)

	-UPDATE 
	-INSERT
	-DELETE

-DCL Data Control Language (Permisos)

	-GRANT
	-REVOKE
	
-SCL Session Control Language (Manejar dinamicamente propiedades de sesión de usuario)

	-ALTER SESSION
	-SET ROLE
	
-TCL Transaction Control Language (Unidad logica de procesado compuesta por varias transacciones)

	-Commit
	-Rollback
	-SafePoint

-DQL Data Query Language (Relativamente nuevo para englobar a SELECT, que es muy potente. Opera sobre los datos)

	-SELECT

# DDL - Data Definition Language
## CREATE
Podemos crear Bases de datos, tablas y usuarios.
	
### DATABASE
Estructura:
> CREATE DATABASE nombreBD;

Alternativa:
> CREATE SCHEMA nombreBD;

En teoria entre ambos existen diferencias relacionadas con los permisos.

Sintaxis Final:

> CREATE (SCHEMA|DATABASE) [IF NOT EXISTS] [CHARACTER SET <Nombre Charset>] <NombreBD>;

###
	```SQL
	```

# GOTCHAs

## Cuantas Lenguajes SQL hay
Una. Hay seis sublenguajes.

## Importancia
El nucleo central de SQL está compuesto de DQL, DML y DDL
