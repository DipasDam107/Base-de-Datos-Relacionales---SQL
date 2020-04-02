# Indice
- [Instalacion MYSQL](#instalar-mysql-server-en-linux)
- [Crear bases de datos MYSQL](#crear-bases-de-datos-en-mysql)
  - [Base de datos de Investigacion](#base-de-datos-de-investigacion)
  - [Base de datos de Naves Espaciales](#base-de-datos-de-naves-espaciales)

# Instalar MYSQL Server en Linux
Para instalar MYSQL Server lo primero que tenemos que hacer es actualizar la lista de paquetes disponibles en los repositorios:

![image](../img/mysql1.png "Logo Title Text 1")

Una vez hecha dicha actualizacion instalamos el paquete mysql-server, que es el que nos permite trabajar con bases de datos. Instalamos aquellas dependencias que el Linux considere oportunas:

![image](../img/mysql2.png "Logo Title Text 1")

Con MYSQL-Server instalado, ya podemos empezar a trabajar. Para ello, ejecutamos MYSQL como usuario root, dandole la contraseña pertinente:

![image](../img/mysql3.png "Logo Title Text 1")

Ya entramos en el intérprete de comandos de MYSQL, hacemos pruebas creando una base de datos cualquiera:

![image](../img/mysql4.png "Logo Title Text 1")

Comprobamos las bases de datos existentes:

![image](../img/mysql5.png "Logo Title Text 1")

[Volver al Indice](#indice)

# Crear Bases de Datos en MYSQL

## Base de datos de Investigacion
Creamos la base de datos Investigación, con la siguiente estructura:

```sql
DROP SCHEMA IF EXISTS Proyectos_Investigacion;
 
CREATE SCHEMA Proyectos_Investigacion;

CREATE TABLE Proyectos_Investigacion.Sede (
	Nome_Sede VARCHAR(30) PRIMARY KEY,
	Campus VARCHAR(30) NOT NULL
);

CREATE TABLE Proyectos_Investigacion.Programa(
	Nome_Programa VARCHAR(30) PRIMARY KEY
);

CREATE TABLE Proyectos_Investigacion.Proxecto(
	Codigo_Proxecto INTEGER PRIMARY KEY,
	Nome_Proxecto VARCHAR(30) UNIQUE NOT NULL,
	Orzamento DECIMAL NOT NULL,
	Data_Inicio DATE NOT NULL,
	Data_Fin DATE,
	Nome_Grupo VARCHAR(30),
	Nome_Departamento VARCHAR(30),
	CHECK (Data_Inicio<Data_Fin),
	CHECK (Codigo_Proxecto>0)
);



CREATE TABLE Proyectos_Investigacion.Profesor(
	Dni CHAR(9) PRIMARY KEY,
	Nome_Profesor VARCHAR(30) NOT NULL,
	Titulacion VARCHAR(30) NOT NULL,
	Experiencia INTEGER,
	Nome_Grupo VARCHAR(30),
	Nome_Departamento VARCHAR(30),
	CHECK (LENGTH(Dni)=9)
);

CREATE TABLE Proyectos_Investigacion.Grupo(
	Nome_Grupo VARCHAR(30),
	Nome_Departamento VARCHAR(30),
	Area VARCHAR(30) NOT NULL,
	Lider CHAR(9),
	PRIMARY KEY (Nome_Grupo, Nome_Departamento)
);

CREATE TABLE Proyectos_Investigacion.Departamento(
	Nome_Departamento VARCHAR(30) PRIMARY KEY,
	Telefono INTEGER NOT NULL,
	Director CHAR(9)
);

CREATE TABLE Proyectos_Investigacion.Ubicacion(
 	Nome_Sede VARCHAR(30),
	Nome_Departamento VARCHAR(30),
	PRIMARY KEY(Nome_Sede, Nome_Departamento)
	
);

CREATE TABLE Proyectos_Investigacion.Participa(
	Dni CHAR(9),
	Codigo_Proxecto INTEGER,
	Data_Inicio DATE NOT NULL,
	Data_Cese DATE,
	Participacion VARCHAR(30),
	PRIMARY KEY (Dni, Codigo_Proxecto),
	CHECK(Data_Cese IS NULL OR (Data_Cese IS NOT NULL AND Data_Cese>Data_Inicio)) 
);

CREATE TABLE Proyectos_Investigacion.Financia(
	Nome_Programa VARCHAR(30),
	Codigo_Proxecto INTEGER,
	Numero_Proxecto INTEGER NOT NULL,
	Cantidade_Financiada DECIMAL NOT NULL,
	PRIMARY KEY (Nome_Programa, Codigo_Proxecto)
);

ALTER TABLE Proyectos_Investigacion.Proxecto ADD FOREIGN KEY (Nome_Grupo, Nome_Departamento)
REFERENCES Proyectos_Investigacion.Grupo (Nome_Grupo, Nome_Departamento) ON DELETE SET NULL ON UPDATE CASCADE;

ALTER TABLE Proyectos_Investigacion.Profesor ADD FOREIGN KEY (Nome_Grupo, Nome_Departamento)
REFERENCES Proyectos_Investigacion.Grupo (Nome_Grupo, Nome_Departamento) ON DELETE SET NULL ON UPDATE CASCADE;

ALTER TABLE Proyectos_Investigacion.Grupo ADD FOREIGN KEY (Lider)
REFERENCES Proyectos_Investigacion.Profesor (Dni) ON DELETE SET NULL ON UPDATE CASCADE;

ALTER TABLE Proyectos_Investigacion.Grupo ADD FOREIGN KEY (Nome_Departamento)
REFERENCES Proyectos_Investigacion.Departamento (Nome_Departamento) ON DELETE CASCADE ON UPDATE CASCADE;

ALTER TABLE Proyectos_Investigacion.Participa ADD FOREIGN KEY (Codigo_Proxecto)
REFERENCES Proyectos_Investigacion.Proxecto (Codigo_Proxecto) ON UPDATE CASCADE;

ALTER TABLE Proyectos_Investigacion.Participa ADD FOREIGN KEY (Dni)
REFERENCES Proyectos_Investigacion.Profesor (Dni) ON UPDATE CASCADE;

ALTER TABLE Proyectos_Investigacion.Financia ADD FOREIGN KEY (Nome_Programa)
REFERENCES Proyectos_Investigacion.Programa (Nome_Programa) ON UPDATE CASCADE ON DELETE CASCADE;

ALTER TABLE Proyectos_Investigacion.Financia ADD FOREIGN KEY (Codigo_Proxecto)
REFERENCES Proyectos_Investigacion.Proxecto (Codigo_Proxecto) ON DELETE CASCADE ON UPDATE CASCADE;

ALTER TABLE Proyectos_Investigacion.Ubicacion ADD FOREIGN KEY (Nome_Sede)
REFERENCES Proyectos_Investigacion.Sede (Nome_Sede) ON DELETE CASCADE ON UPDATE CASCADE;

ALTER TABLE Proyectos_Investigacion.Ubicacion ADD FOREIGN KEY (Nome_Departamento)
REFERENCES Proyectos_Investigacion.Departamento (Nome_Departamento) ON DELETE CASCADE ON UPDATE CASCADE;

ALTER TABLE Proyectos_Investigacion.Departamento ADD FOREIGN KEY (Director)
REFERENCES Proyectos_Investigacion.Profesor (Dni) ON DELETE SET NULL ON UPDATE CASCADE;
```

Comprobamos que nuestra base de datos esta entre las existentes en nuestro servidor con el comando `SHOW DATABASES`:

![image](../img/mysql6.png "Logo Title Text 1")

Pasamos a usar dicha base de datos con el comando `USE <NombreBD>`:

![image](../img/mysql7.png "Logo Title Text 1")

Comprobamos las tablas de la base de datos `SHOW TABLES`:

![image](../img/mysql8.png "Logo Title Text 1")

El comando `DESC` nos permite obtener una descripción detallada de cada base de datos:

![image](../img/mysql9.png "Logo Title Text 1")

![image](../img/mysql10.png "Logo Title Text 1")

![image](../img/mysql11.png "Logo Title Text 1")

## Base de datos de Naves Espaciales

```sql
DROP SCHEMA IF EXISTS Naves_Espaciais;
 
CREATE SCHEMA Naves_Espaciais;

CREATE TABLE Naves_Espaciais.Servizo(
	Clave_Servizo CHAR(9),
	Nome_Servizo VARCHAR(30),
	PRIMARY KEY(Clave_Servizo, Nome_Servizo),
	CHECK(LENGTH(Clave_Servizo)=9)	
);

CREATE TABLE Naves_Espaciais.Dependencia(
	Codigo_Dependencia INTEGER PRIMARY KEY AUTO_INCREMENT,
	Nome_Dependencia VARCHAR(30) NOT NULL UNIQUE,
	Clave_Servizo CHAR(9) NOT NULL,
	Nome_Servizo VARCHAR(30) NOT NULL,
	Funcion VARCHAR(30),
	Localizacion VARCHAR(30),
	FOREIGN KEY(Clave_Servizo, Nome_Servizo) REFERENCES Naves_Espaciais.Servizo(Clave_Servizo, Nome_Servizo)
	ON UPDATE CASCADE
	ON DELETE CASCADE
);

CREATE TABLE Naves_Espaciais.Camara(
	Codigo_Dependencia INTEGER PRIMARY KEY,
	Categoria VARCHAR(30) NOT NULL,
	Capacidade INTEGER NOT NULL,
	CHECK(Capacidade > 0),
	FOREIGN KEY(Codigo_Dependencia) REFERENCES Naves_Espaciais.Dependencia(Codigo_Dependencia)
	ON DELETE CASCADE
	ON UPDATE CASCADE
);

CREATE TABLE Naves_Espaciais.Raza(
	Nome_Raza VARCHAR(30) PRIMARY KEY,
	Altura DECIMAL NOT NULL,
	Anchura DECIMAL  NOT NULL,
	Peso DECIMAL NOT NULL,
	Poboacion_Total BIGINT NOT NULL,
	CHECK(Altura > 0 AND Anchura > 0 AND Peso > 0 AND Poboacion_Total >= 0)
);

CREATE TABLE Naves_Espaciais.Planeta(
	Codigo_Planeta INTEGER PRIMARY KEY AUTO_INCREMENT,
	Nome_Planeta VARCHAR(30) UNIQUE NOT NULL,
	Galaxia VARCHAR(30) NOT NULL,
	Coordenadas VARCHAR(15) UNIQUE NOT NULL
);

CREATE TABLE Naves_Espaciais.Habita(
	Codigo_Planeta INTEGER,
	Nome_Raza VARCHAR(30),
	Poboacion_Parcial BIGINT NOT NULL,
	CHECK (Poboacion_Parcial>=0),
	PRIMARY KEY(Codigo_Planeta, Nome_Raza),
	FOREIGN KEY(Codigo_Planeta) REFERENCES Naves_Espaciais.Planeta(Codigo_Planeta)
	ON UPDATE CASCADE ON DELETE CASCADE,
	FOREIGN KEY(Nome_Raza) REFERENCES Naves_Espaciais.Raza(Nome_Raza)
	ON UPDATE CASCADE ON DELETE CASCADE
);

CREATE TABLE Naves_Espaciais.Tripulacion(
	Codigo_Tripulacion INTEGER PRIMARY KEY AUTO_INCREMENT,
	Nome_Tripulacion VARCHAR(30) NOT NULL,
	Codigo_Camara INTEGER NOT NULL,
	Codigo_Dependencia INTEGER NOT NULL,
	Categoria VARCHAR(30) NOT NULL,
	Antiguedade INTEGER NOT NULL,
	Procedencia VARCHAR(30) NOT NULL,
	Administracion VARCHAR(30) NOT NULL,
	CHECK(Antiguedade>=0),
	CHECK(Administracion='En servicio' OR Administracion='De baja'),
	FOREIGN KEY(Codigo_Camara) REFERENCES Naves_Espaciais.Camara(Codigo_Dependencia)
	ON UPDATE CASCADE ON DELETE CASCADE,
	FOREIGN KEY(Codigo_Dependencia) REFERENCES Naves_Espaciais.Dependencia(Codigo_Dependencia)
	ON UPDATE CASCADE ON DELETE CASCADE
);

CREATE TABLE Naves_Espaciais.Visita(
	Codigo_Tripulacion INTEGER,
	Codigo_Planeta INTEGER,
	Data_Visita DATE,
	Tempo INTEGER NOT NULL,
	CHECK(Tempo>=0),
	PRIMARY KEY(Codigo_Tripulacion,Codigo_Planeta,Data_Visita),
	FOREIGN KEY(Codigo_Tripulacion) REFERENCES Naves_Espaciais.Tripulacion(Codigo_Tripulacion)
	ON UPDATE CASCADE ON DELETE CASCADE,
	FOREIGN KEY(Codigo_Planeta) REFERENCES Naves_Espaciais.Planeta(Codigo_Planeta)
	ON UPDATE CASCADE ON DELETE CASCADE
);
```
