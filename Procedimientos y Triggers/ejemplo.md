# Procedimientos Almacenados/Funciones
Las funciones o procedimientos almacenados son programas almacenados en la base de datos y que ejecutan su código en el momento en el que son llamados. Para crear una función seguimos la siguiente estructura:

```sql
CREATE OR REPLACE FUNCTION <nombre de la función>( <parámetros> ) RETURNS <tipo que devuelve> AS 
$$
DECLARE
   <declaramos variables locales>
BEGIN
   <cuerpo de la función>
END;
$$
LANGUAGE plpgsql IMMUTABLE;
```

La diferencia de las funciones con los procedimientos es que los segundos no devuelven un valor (Aunque se sigue podiendo utilizar RETURN para poner fin al programa):

```sql
CREATE OR REPLACE PROCEDURE <nombre del procedimiento>( <parámetros> ) AS 
$$
DECLARE
   <declaramos variables locales>
BEGIN
   <cuerpo de la función>
END;
$$
LANGUAGE plpgsql;
```

## Parametros
Se especifican los parámetros que se pasan en la llamada al método. Ejemplo:

```sql
CREATE OR REPLACE FUNCTION springEquinox(tstz DATE, julian BOOLEAN)
```

## RETURNS
Se especifica el tipo de parámetro que va a devolver la función:

```sql
CREATE OR REPLACE FUNCTION springEquinox(tstz DATE, julian BOOLEAN) RETURNS TIMESTAMP
```

## DECLARE
Dentro del DECLARE van las variables que se van a declarar para usar dentro de la función. En la misma declaración pueden realizarse asignaciones y operaciones. La estructura es Nombre TipoDato:

```sql
DECLARE
   v   DOUBLE PRECISION;
   ve  DOUBLE PRECISION;
   jdn INTEGER;
   ut  DOUBLE PRECISION;
   x   INTEGER;
   z   INTEGER;
   m   INTEGER;
   d   INTEGER;
   y   INTEGER;  
   daysPer400Years        BIGINT = 146097;
   fudgedDaysPer4000Years BIGINT = 1460970 + 31;
```

## Cuerpo (BEGIN y END)
Es donde va el programa propiamente dicho. BEGIN marca el inicio del mismo y END su final. Dentro se pueden usar bucles, IFs, etc... Las asignaciones se hacen con := :
```sql
BEGIN
   v   := (EXTRACT(YEAR FROM tstz)::DOUBLE PRECISION - 2000.0)/1000.0;
   ve  := 2451623.80984 + 365242.37404 * v + 0.05169 * v * v - 0.00411 * v * v * v - 0.00057 * v * v * v * v;
   ve  := ve + 0.5;
   jdn := floor(ve);
   ut  := ve - jdn;
   x   := jdn + 68569;

   IF julian THEN
      x:=x+38;
      daysPer400Years:=146100;
      fudgedDaysPer4000Years:=1461000+1;
   END IF;
   
   z := 4 * x / daysPer400Years;
   x := x - (daysPer400Years * z + 3) / 4;
   y := 4000 * (x + 1) / fudgedDaysPer4000Years;
   x := x - 1461 * y / 4 + 31;
   m := 80 * x / 2447;
   d := x - (2447 * m / 80);
   x := m / 11;
   m := m + 2 - 12 * x;
   y := 100 * (z - 49) + y + x ; 

   IF y <= 0 THEN
      y := y-1;
   END IF;
   
   RETURN make_timestamp(y::SMALLINT,m::SMALLINT,d::SMALLINT,(ut*24)::SMALLINT,((ut*24-floor(ut*24))*60)::SMALLINT,0);
END;
```

## Delimitadores
Debe especificarse un delimitador de principio y fin de procedimiento, que puede ser variable. Debe elegirse caracteres que no se vayan a encontrar en el programa, como los dos simobolos de dolar ($$) en este caso.

# Triggers
Los triggers o disparadores son programas que se ejecutan cuando se produce un cambio en nuestra base de datos, como por ejemplo una inserción, borrado o modificacion en una tabla. Para crear un trigger seguimos la siguiente estructura:

```sql
    CREATE TRIGGER check_update
    BEFORE|AFTER UPDATE|DELETE|INSERT ON <nombreTabla>
    FOR EACH ROW
    WHEN <condicion>
    EXECUTE FUNCTION <Nombre_Funcion_a_Ejecutar>
```

## Ejemplo de Trigger
Primero Cambio la tabla words y le añado una columna para contar los documentos:
```sql
ALTER TABLE words
ADD documents INTEGER DEFAULT 0;
```
Reinicio los valores de los campos serial, para que empiecen desde 1 nuevamente:

```sql
SELECT SETVAL((SELECT pg_get_serial_sequence('documents', 'document')), 1, false);

SELECT SETVAL((SELECT pg_get_serial_sequence('words', 'word')), 1, false);
```

Inserto valores en las tablas words y documents:

```sql
INSERT INTO documents(title,words) VALUES('Harry Potter y la piedra filosofal',default);
INSERT INTO documents(title,words) VALUES('Los triggers de la selva',default);
INSERT INTO documents(title,words) VALUES('la pandemia mundial de 2020: la primera',default);

INSERT INTO words(term,documents) VALUES('Los', default);
INSERT INTO words(term,documents) VALUES('triggers', default);
INSERT INTO words(term,documents) VALUES('de', default);
INSERT INTO words(term,documents) VALUES('la', default);
INSERT INTO words(term,documents) VALUES('selva', default);
INSERT INTO words(term,documents) VALUES('casa', default);

```
## Con Parametros

Creo la función de trigger para borrado, inserción y modificación (Se pasa un parámetro al crear el trigger):

```sql
CREATE OR REPLACE FUNCTION onMovimientoDocumentoEnPalabra()
RETURNS TRIGGER AS
$$
DECLARE
	modo INTEGER = TG_ARGV[0];
BEGIN
	IF modo = 1 THEN
	--Se crea registro
	UPDATE words SET documents=documents+1 WHERE word=NEW.word;
	END IF;
	IF modo = 2 THEN
	--Se borra registro
		UPDATE words SET documents=documents-1 WHERE word=OLD.word;
	END IF;
	IF modo = 3 AND OLD.word<>NEW.word THEN
		--Se modifica registro
		UPDATE words SET documents=documents-1 WHERE word=OLD.word;
		UPDATE words SET documents=documents+1 WHERE word=NEW.word;
	END IF;
	RETURN NEW;
END
$$
LANGUAGE plpgsql;
```

Creo los triggers pasando el parámetro en cuestión:
```sql
CREATE TRIGGER onInsertPalabraEnDocumentosTrigger AFTER INSERT ON documents_words
FOR EACH ROW
EXECUTE FUNCTION onMovimientoDocumentoEnPalabra(1);

CREATE TRIGGER onDeletePalabraEnDocumentosTrigger AFTER DELETE ON documents_words
FOR EACH ROW
EXECUTE FUNCTION onMovimientoDocumentoEnPalabra(2);

CREATE TRIGGER onUpdatePalabraEnDocumentosTrigger AFTER UPDATE ON documents_words
FOR EACH ROW
EXECUTE FUNCTION onMovimientoDocumentoEnPalabra(3);

#
```

## Sin parámetros
Para hacerlo sin parámetros debemos comprobar los valores de los registros OLD y NEW, de esta manera sabríamos identificar si es una inserción, borrado o modificación:

```sql
CREATE OR REPLACE FUNCTION onMovimientoDocumentoEnPalabra()
RETURNS TRIGGER AS
$$
BEGIN
	IF OLD IS NULL THEN
	--Se crea registro
	UPDATE words SET documents=documents+1 WHERE word=NEW.word;
	END IF;
	IF NEW IS NULL THEN
	--Se borra registro
		UPDATE words SET documents=documents-1 WHERE word=OLD.word;
	END IF;
	IF OLD IS NOT NULL AND NEW IS NOT NULL AND OLD.word<>NEW.word THEN
		--Se modifica registro
		UPDATE words SET documents=documents-1 WHERE word=OLD.word;
		UPDATE words SET documents=documents+1 WHERE word=NEW.word;
	END IF;
	RETURN NEW;
END
$$
LANGUAGE plpgsql;

```

Creo los triggers sin pasar parámetros:

```sql

CREATE TRIGGER onInsertPalabraEnDocumentosTrigger AFTER INSERT ON documents_words
FOR EACH ROW
EXECUTE FUNCTION onMovimientoDocumentoEnPalabra();

CREATE TRIGGER onDeletePalabraEnDocumentosTrigger AFTER DELETE ON documents_words
FOR EACH ROW
EXECUTE FUNCTION onMovimientoDocumentoEnPalabra();

CREATE TRIGGER onUpdatePalabraEnDocumentosTrigger AFTER UPDATE ON documents_words
FOR EACH ROW
EXECUTE FUNCTION onMovimientoDocumentoEnPalabra();
#
```
