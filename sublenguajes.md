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
```sql
 CREATE TABLE <NombreTabla> (
	id INTEGER PRIMARY KEY,
	nome NCHAR(50) NOT NULL,
	apelidos NCHAR(200),
	nacido DATE,
	[CONSTRAINT <NombrePK>] PRIMARY KEY (atributo1, atributo2....)
	[CONSTRAINT <nombreRestriccion>] FOREIGN KEY (<Atributos>) REFERENCES <Nombre_tabla_referenciada>[(<Atributos_referenciados>)]
	[ON DELETE CASCADE|NO ACTION|SET NULL|SET DEFAULT]
	[ON UPDATE CASCADE|NO ACTION|SET NULL|SET DEFAULT]
	[MATCH FULL| MATCH PARTIAL]
	); <
	
```

#### CONSTRAINT PK
La constraint PRIMARY KEY indica que campo/s forman parte de la clave principal, que indica el campo que actúa como diferenciador a nivel tupla. Hay varias maneras de utilizarlo. 

En primer lugar podemos definir la clave principal en la misma definicion del atributo:

```sql
 CREATE TABLE <NombreTabla> (
	id INTEGER ** PRIMARY KEY **,
	...
	); <
```

Se puede definir al final, especialmente cuando la clave es compuesta:
```sql
 CREATE TABLE <NombreTabla> (
	id INTEGER ,
	nome NCHAR(50) NOT NULL,
	apelidos NCHAR(200),
	nacido DATE,
	PRIMARY KEY (id)
	);
```

Podemos darle nombre al constraint, útil especificamente para DBAs:
```sql
 CREATE TABLE <NombreTabla> (
	id INTEGER ,
	nome NCHAR(50) NOT NULL,
	apelidos NCHAR(200),
	nacido DATE,
	CONSTRAINT nombre PRIMARY KEY (id)
	);
```


#### CONSTRAINT FK
Nos permite establecer la relación entre varias tablas, especificando los campos que tienen en común.

Podemos especificar que operaciones se van a realizar si en la misma tabla se producen modificaciones, indicando como va a afectar a las que están enlazadas a través de clave ajena:

> Borrado: ON DELETE CASCADE|NO ACTION|SET NULL|SET DEFAULT
> Actualizaciones: ON UPDATE CASCADE|NO ACTION|SET NULL|SET DEFAULT

Tipos de Accion de modificacion y borrado:
	- [CASCADE]: El borrado de un registro, borra todos los registros de la otra tabla que referencien a esa tupla
	- [NO ACTION]  (Por defecto) : No hace nada
	- [SET DEFAULT]: Cambia el valor en la tabla ajena a un valor por defecto
	- [SET NULL]: Cambia el valor en la tabla ajena a un valor nulo

#### Tipos de datos
- CHAR: Cadenas Fijas. Rellena con espacios hasta llenar todo el dato
- NCHAR
- NCHAR VARYING
- VARCHAR
- TEXT
- SERIAL
- INTEGER
- SMALLINT: Entero Pequeño
- FLOAT
- NUMERIC
- DATE: Fecha
- TIME: Hora
- TIMESTAMP: Incluye Date y Time
- BOOLEAN

# GOTCHAs

## Cuantas Lenguajes SQL hay
Una. Hay seis sublenguajes.

## Importancia
El nucleo central de SQL está compuesto de DQL, DML y DDL

## Nomenclatura de tablas
Se suele utilizar nombres en singular con la primera letra mayúscula

# Enlaces
- Elephant SQL: https://www.elephantsql.com/
