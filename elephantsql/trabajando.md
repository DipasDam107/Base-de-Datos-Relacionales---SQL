# Indice
- [BD World](#bd-world)
  - [Creando world](#creando-tabla-world)
  - [Metiendo datos](#metiendo-datos)
  
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
