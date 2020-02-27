# Indice
- [BD World](#bd-world)
  - [Creando world](#creando-tabla-world)
  - [Metiendo datos](#metiendo-datos)
- [BD Nobel](#bd-nobel)
  - [Creando world](#creando-tabla-nobel)
  - [Metiendo datos](#metiendo-datos-nobel)
- [BD goal](#bd-goal)
- [BD movies](#bd-movies)
  
-----------------------------------------------

# BD World
Vamos a intentar recrear esta tabla de SQLZOO y trabajar con ella.

![image](../img/tabla1.png "Logo Title Text 1")

## Creando tabla world
Para crear la tabla world, es necesario utilizar la instrucción CREATE TABLE:
  > CREATE TABLE nombre tabla (campo1 tipo, campo2 tipo....);

Se toman las siguientes consideraciones:
  - El campo name será VARCHAR(40) y PK
  - El campo continent será VARCHAR(40)
  - Los campos population, gdp y area seran bigint
  
Por tanto, la instrucción para crear la tabla será la siguiente:

```SQL
  CREATE TABLE world(
    name VARCHAR(40) PRIMARY KEY,
    continent VARCHAR(40),
    area bigint,
    population bigint,
    gdp bigint
 );
```

![image](./img/img16.png "Logo Title Text 1")

Podríamos hacer un SELECT ahora de la misma tabla para ver si se ha creado correctamente, aunque evidentemente no devolvería ningun dato, ya que está vacía.

![image](./img/img17.png "Logo Title Text 1")


## Metiendo Datos
Para introducir datos en la tabla antes creada usamos la instrucción INSERT:
> INSERT INTO nombreTABLA [(orden de campos)] VALUES (valorcampo1, valorcampo2...);

Si bien el apartado de orden de campos se puede omitir, es útil para cuando no queremos introducir valores en todos los campos o lo queremos hacer en desorden, por la razón que sea. En ese caso, debemos tener cuidado de que el orden de los campos se correspondan con los valores introducidos al final.

Así pues, introducimos algunos valores de prueba directamente sacados de SQLZOO:

```SQL
INSERT INTO world VALUES('Afghanistan','Asia',652230,25500100,20343000000);
```

Podemos añadir múltiples tuplas en la misma sentencia, separando cada tupla con comas:

```SQL
INSERT INTO world VALUES('Albania','Europe',28748,2831741,12960000000),
                        ('Algeria','Africa',2381741,37100000,188681000000)
                        ('Andorra','Europe',468,78115,3712000000)
                        ;
```

Usando el orden de los campos, podríamos meter nombre y población, tomando el resto valores nulos:

```SQL
INSERT INTO world (name, population)
VALUES('Argentina',44270000);
```
A este punto tendríamos los siguientes datos:

![image](./img/img22.png "Logo Title Text 1")

# BD nobel
![image](../img/nobel.png "Logo Title Text 1")

## Creando tabla nobel
```SQL
    CREATE TABLE nobel(
      winner varchar(50) PRIMARY KEY,
      yr int NOT NULL,
      subject varchar(39) NOT NULL);
```
## Metiendo Datos Nobel
```SQL
INSERT INTO nobel (yr,subject,winner) VALUES (1960,'Chemistry','Willard F. Libby'),
                                             (1960,'Literature','Saint-John Perse'),
                                             (1960,'Medicine','Sir Frank Macfarlane Burnet'),
                                             (1960,'Medicine','Peter Madawar');

```

![image](./img/img23.png "Logo Title Text 1")

# BD goal

```SQL
CREATE TABLE eteam(
	id char(3) PRIMARY KEY,
	teamname nchar(30),
	coach nchar(30)
)

CREATE TABLE game(
	id integer PRIMARY KEY,
	mdate date NOT NULL,
	stadium nchar(30),
	team1 char(3),
	team2 char(3),
	FOREIGN KEY (team1) REFERENCES eteam(id),
	FOREIGN KEY (team2) REFERENCES eteam(id)
)

CREATE TABLE goal(
	matchid integer,
	teamid char(3),
	player nchar(30),
	gtime integer,
	PRIMARY KEY(matchid, gtime),
	FOREIGN KEY (teamid) REFERENCES eteam (id),
	FOREIGN KEY (matchid) REFERENCES game (id)
);

```

# BD movies
```SQL
CREATE TABLE actor(
	actor integer PRIMARY KEY,
	name nchar(30)	
)

CREATE TABLE movie(
	id integer PRIMARY KEY,
	title nchar(30) NOT NULL,
	yr integer,
	director integer,
	budget bigint,
	gross bigint,
	FOREIGN KEY (director) REFERENCES actor(id)	
)

CREATE TABLE casting(
	movieid integer,
	actorid integer,
	ord integer,
	PRIMARY KEY (movieid,actorid),
	FOREIGN KEY (movieid) REFERENCES movie(id),
	FOREIGN KEY (actorid) REFERENCES actor(id)
);
```
