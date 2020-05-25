# DQL #
El lenguaje DQL (Doctrine Query Language), es un lenguaje de consulta para nuestro modelo de objetos. Instruccions con **SELECT**, opera sobre datos.

## CREATE ##
Nos sirve para crear tablas o esquemas en nuestra base de datos.
Sintaxis:
~~~sql
CREATE (table|schema) <nome_taboa>{
    column1 datatype,
    column2 datatype,
    ...
};
~~~~

Cuando la clave primaria es una sola se puede especificar de esta manera:
~~~sql
CREATE (table|schema) <nome_taboa>{
    column1 datatype PRIMARY KEY,
    column2 datatype,
    ...
};
~~~
Cuando la clave primaria esta comuesta por dos atributos lo especificamos de esta manera:
~~~sql
CREATE (table|schema) <nome_taboa>{
    column1 datatype,
    column2 datatype,
    ...
     PRIMARY KEY (column1,column2)
};
~~~
De la misma forma especificamos la clave foranea indicando cual es su referencia:
~~~sql
CREATE (table|schema) <nome_taboa>{
    column1 datatype,
    column2 datatype,
    ...
     FOREIGN KEY (column1,column2) REFERENCES <nome_tabla> [(column)]
}; 
~~~
Para indicar que un atributo no es nulo lo indicamos con la particula **NOT NULL**:
~~~sql
    column1 datyatype NOT NULL,
~~~
Podemos especificar cuando la fila hija es borrada o actualizada como queremos que afecte en la tabla padre, para ello, tenemos tres particulas:

###### NO ACTION ######
La que se establece por defecto, al borrar o modificar datos de la table padre no tiene efecto en la tabla hija. No es necesario expresarlo semanticamente.

###### CASCADE ######
Al borrar datos de la tabla padre, tambien se borran los datos de la tabla hija. Este formato es el mas utilizado.
Sintaxis:
~~~sql
CREATE (table|schema) <nome_taboa>{
    column1 datatype,
    column2 datatype PRIMARY KEY,
    ...
    FOREIGN KEY (column1) REFERENCES <nome_tabla> [(column)]
    ON DELETE CASCADE
    ON UPDATE CASCADE
}; 
~~~

###### SET NULL ######
Al borrar o modificar datos de la tabla padre, los datos de la tabla hijo de ponen a **NULL**.
~~~sql
CREATE (table|schema) <nome_taboa>{
    column1 datatype,
    column2 datatype PRIMARY KEY,
    ...
    FOREIGN KEY (column1) REFERENCES <nome_tabla> [(column)]
    ON DELETE SET NULL
    ON UPDATE SET NULL
};
~~~
###### SET DEFAULT ######
É un método polo cal se fai un axuste dos datos. 
~~~sql
CREATE (table|schema) <nome_taboa>{
    column1 datatype,
    column2 datatype PRIMARY KEY,
    ...
    FOREIGN KEY (column1) REFERENCES <nome_tabla> [(column)]
    ON DELETE SET DEFAULT
    ON UPDATE SET DEFAULT
};
~~~
###### RESTRICCIONES #######
Todas las restricciones se hacen a partir de la partícula **CONSTRAINT** <nome_da_restriccion>. Hay varios tipos:

###### UNICIDAD ######
Para todas aquellas claves secundarias que no llegaron a ser claves primarias.
~~~sql
CONSTRAINT <nome_da_restriccion>
    UNIQUE (<atributo>)
~~~
###### COMPROBACIÓN ######
Se utiliza para introducir un predicado.
~~~sql
CONSTRAINT <nome_da_restriccion>
    CHECK (predicado)
~~~

Podemos espeficicar cuando queremos que se inicie la comprobación de las restricciones en el mismo momento, opción por defecto, o al final de la transacción, con la particula **INITIALLY**:
~~~sql
CONSTRAINT <nome_da_restriccion>
    CHECK (predicado)
    INITIALLY INMEDIATE|DEFERRATE;
~~~

## DROP ##
Nos sirve para borrar tablas o esquemas.
Sintaxis:
~~~sql
DROP SCHEMA|TABLE <nome_da_tabla>;
~~~
De la misma forma podemos indicar si queremos que se borren las tablas que dependan desta o que no se sean modificadas.
###### CASCADE ######
Las tablas que dependian de esta serán borradas.
###### RESTRICT ######
No permite borrar la tabla si tiene tablas hijas.

## ALTER ##
Nos sirve para modificar tablas una vez que lla estan creadas. Podemos añadir columnas, añadir restricciones, introducir claves foranias...
Sintaxis:
~~~sql
ALTER TABLE <nome_da_taboa>
    ADD [column] <atributo><DOMINIO>;
~~~
~~~sql
ALTER TABLE <nome_da_taboa>
    ADD CONSTRAINT <nombre_da_claveForanea>
    FOREIGN KEY (<atributo>)
    REFERENCES <nome_da_taboa> ;
~~~

# DML #
El lenguaje DML (Data Manipulation Language) permite consultar o modificar los datos contenidos en una base de datos. Opera sobre los datos.

## INSERT ##
Sirve para introducir datos.
Sintaxis:
~~~sql
INSERT INTO <nome_da_taboa> [(column1,column2...)]
VALUES (value1,value2... | SELECT(...));
~~~
## UPDATE ##
Sirve para modificar datos de una tabla y añadirle un predicado.
Sintaxis:
~~~sql
UPDATE <nome_da_taboa>
SET column = value, 
WHERE predicado;
~~~
## DELETE ##
Sirve para borrar datos de una tabla con un predicado ou non.
Sintaxis:
~~~sql
DELETE FROM <nome_da_taboa>
~~~
 # TIPO DE VARIABLES #
## NUMEROS ##
-INTEGER: GIGINT,SMALLINT
-DECIMAL: EXACTO
-REAL: FLOAT,INEXACTO,IEEE 754
## TEXTO ##
-CHAR:lonxitude fixa
-VARCHAR:lonxitude variable
-TEXT
## TIEMPO ##
-DATA: DD/MM/AAAA
-TIME: HORA,MINUTOS,SEGUNDOS
-TIMESTAMP:DATE+TIME
## OTROS ##
-BOOLEAN
-MONEY
-UUIDE
-JSON

