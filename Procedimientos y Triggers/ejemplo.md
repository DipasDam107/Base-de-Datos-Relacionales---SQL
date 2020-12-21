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
