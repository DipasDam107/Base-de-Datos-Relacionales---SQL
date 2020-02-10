# Sublenguajes SQL
- DDL Data Definition Lenguage (Opera sobre los objetos de la BD. Tablas, fila, columna, indice...)

	-CREATE
	-ALTER
	-DROP

- DML Data Manipulation Language (Antes SELECT se incluía ahi. Opera sobre los datos)

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
Para crear base de datos, utilizamos la siguiente sintaxis:

> CREATE (SCHEMA|DATABASE) [IF NOT EXISTS] [CHARACTER SET <Nombre Charset> [COLLATE <Nombre_Variante>]] << NombreBD >>;

#### SCHEMA o DATABASE
En general se comportan de la misma manera, habiendo algunas diferencias en el tratamiento de permisos.

Estructura:
> CREATE DATABASE nombreBD;

Alternativa:
> CREATE SCHEMA nombreBD;

#### IF NOT EXISTS
Comprueba si la base de datos que vamos a crear ya existe en el SGBD.

#### CHARACTER SET y COLLATE
CHARACTER SET especifica el conjunto de caracteres que se va a utilizar (Ejemplo: latin1), y COLLATE nos ayuda a elegir la variante esepecífica dentro de dicho conjunto (Ejemplo: latin1_swedish_ci)

### TABLE
Para crear Tablas, utilizamos la siguiente sintaxis:

> CREATE TABLE <NombreTabla> (
	id INTEGER PRIMARY KEY,
	nome NCHAR(50) NOT NULL,
	apelidos NCHAR(200),
	nacido DATE
	); <
	
#### Tipos de datos
CHAR - Cadenas Fijas. Rellena con espacios hasta llenar todo el dato

NCHAR

NCHAR VARYING

VARCHAR

TEXT

SERIAL

INTEGER

SMALLINT - Entero Pequeño

FLOAT

NUMERIC

DATE - Fecha

TIME - Hora

TIMESTAMP - Incluye Date y Time

BOOLEAN

# GOTCHAs

## Cuantas Lenguajes SQL hay
Una. Hay seis sublenguajes.

## Importancia
El nucleo central de SQL está compuesto de DQL, DML y DDL

## Nomenclatura de tablas
Se suele utilizar nombres en singular con la primera letra mayúscula
