## CREACIÓN BASE DE DATOS ##

A continuación crearemos la **BASE DE DATOS DE INVESTIGACIÓN** desde la terminal de comandos de Linux. Para acceder a la aplicación utilizamos el siguiente comando 
~~~~
sudo mysql
~~~~

Con este comando ya estamos listos para crear nuestra base de datos.
Primero eliminamos cualquiera base de datos que tengo el mismo nombre que la que vamos crear para que no haya ningún conflico.

![imagen](https://github.com/AlbertoMartinezPose/tarea-3-bases/blob/master/0.1.PNG)

La creamos y me diante el comando:
~~~~
use <nombredatabase>
~~~~
Nos introducimos dentro y ya podemos crear tablas.
![imagen](https://github.com/AlbertoMartinezPose/tarea-3-bases/blob/master/0.2.PNG)


Y así con todas las tablas que el siguiente código:
~~~~sql

CREATE TABLE Departamento (
	Nome_Departamento Nome_válido,
	Teléfono char(9) NOT NULL,
	Director Nome_válido,
	CONSTRAINT PK_Departamento
		PRIMARY KEY(Nome_Departamento)
)
CREATE TABLE Ubicacion (
	Nome_Sede Nome_válido,
	Nome_Departamento Nome_válido,
	PRIMARY KEY(Nome_Sede,Nome_Departamento),
	CONSTRAINT FK_Sede_Ubicacion
		FOREIGN KEY(Nome_Sede) 
		REFERENCES "proxectoClase".sede (Nome_Sede),
		ON DELETE CASCADE
		ON UPDATE CASCADE
	CONSTRAINT FK_Departamento_Ubicacion
		FOREIGN KEY(Nome_Departamento)
		REFERENCES "proxectoClase".departamento (Nome_Departamento)
ON DELETE CASCADE
		ON UPDATE CASCADE
);
CREATE TABLE Grupo (
	Nome_Grupo Nome_válido,
	Nome_Departamento Nome_válido,
	Area Nome_válido NOT NULL,
	Líder Tipo_DNI,
	PRIMARY KEY(Nome_Grupo,Nome_Departamento),
	CONSTRAINT FK_Departamento_Grupo
		FOREIGN KEY(Nome_Departamento)
		REFERENCES "proxectoClase".departamento (Nome_Departamento)
		ON DELETE CASCADE
		ON UPDATE CASCADE
);
ALTER TABLE Proxecto ADD FOREIGN KEY (Nome_Grupo, Nome_Departamento)
REFERENCES Grupo (Nome_Grupo, Nome_Departamento) ON DELETE SET NULL ON UPDATE CASCADE;

ALTER TABLE Profesor ADD FOREIGN KEY (Nome_Grupo, Nome_Departamento)
REFERENCES Grupo (Nome_Grupo, Nome_Departamento) ON DELETE SET NULL ON UPDATE CASCADE;

ALTER TABLE Departamento
ADD CONSTRAINT fk_profesor_departamento
	FOREIGN KEY (Director) 
	REFERENCES "proxectoClase".profesor (DNI)
	ON DELETE SET NULL
	ON UPDATE CASCADE;
	
CREATE TABLE Participa (
	DNI Tipo_DNI,
	Código_Proxecto Tipo_Código,
	Data_Inicio date NOT NULL,
	Data_Cese date,
	Dedicación char(20) NOT NULL,
	PRIMARY KEY(DNI,Código_Proxecto),
	CHECK (Data_Inicio < Data_Cese)
);

CREATE TABLE Financia(
	Nome_Programa VARCHAR(30),
	Codigo_Proxecto INTEGER,
	Numero_Proxecto INTEGER NOT NULL,
	Cantidade_Financiada DECIMAL NOT NULL,
	PRIMARY KEY (Nome_Programa, Codigo_Proxecto)
);
ALTER TABLE Financia ADD FOREIGN KEY (Nome_Programa)
REFERENCES Programa (Nome_Programa) 
ON UPDATE CASCADE 
ON DELETE CASCADE;

ALTER TABLE Financia ADD FOREIGN KEY (Codigo_Proxecto)
REFERENCES Proxecto (Codigo_Proxecto) 
ON DELETE CASCADE 
ON UPDATE CASCADE;
~~~~

Una vez creada la base de datos introducirnos dentro para ver sus elementos, por ejemplo para ver las tablas creadas utilizaremos el comando:
~~~~
SHOW TABLES;
~~~~
![imagen](https://github.com/AlbertoMartinezPose/tarea-3-bases/blob/master/0.3.PNG)

Si queremos ver en profundidad cada tabla utilizaremos el siguiente comando:
~~~~
DESC <nombre_de_la_tabla>
~~~~
![imagen](https://github.com/AlbertoMartinezPose/tarea-3-bases/blob/master/0.4.PNG?raw=true)

Ahora crearemos la **BASE DE DATOS NAVES ESPACIAIS** pero esta vez utilizaremos un script que nos facilitará el trabajo. Para ello necesitamos cualquier editor de testo  donde poder guardar el archivo en formato *sql*.
El código de la base de datos es el siguiente:
~~~~sql
DROP SHCEMA IF EXISTS Naves_Espaciais;

CREATE SCHEMA Naves_Espaciais;

USE Naves_Espaciais;

CREATE TABLE Planeta (
  Código_Planeta CHAR(5)          PRIMARY KEY,
  Nome_Planeta   VARCHAR(40) NOT NULL UNIQUE,
  Galaxia        CHAR(15)    NOT NULL,
  Coordenadas    CHAR(15)    NOT NULL,
  UNIQUE(Coordenadas)
 );
 
 CREATE TABLE Raza (
  Nome_Raza       VARCHAR(40)  PRIMARY KEY,
  Altura          INTEGER      NOT NULL,  -- cm
  Anchura         INTEGER      NOT NULL,  -- cm
  Peso            INTEGER      NOT NULL,  -- g
  Poboación_Total INTEGER      NOT NULL
);

 CREATE TABLE Habita (
  Código_Planeta    CHAR(5),
  Nome_Raza         VARCHAR(40),
  Poboación_Parcial INTEGER     NOT NULL,
  PRIMARY KEY (Código_Planeta, Nome_Raza),
  FOREIGN KEY (Código_Planeta)
    REFERENCES Planeta (Código_Planeta)
    ON UPDATE Cascade
    ON DELETE Cascade,
  FOREIGN KEY (Nome_Raza)
    REFERENCES Raza (Nome_Raza)
    ON UPDATE CASCADE
    ON DELETE CASCADE
);

CREATE TABLE Servizo (
  Clave_Servizo CHAR(5),
  Nome_Servizo  VARCHAR(40),
  PRIMARY KEY (Clave_Servizo, Nome_Servizo)
);

CREATE TABLE Dependencia (
  Código_Dependencia CHAR(5),
  Nome_Dependencia   VARCHAR(40) NOT NULL,
  Función            VARCHAR(20),
  Localización       VARCHAR(20),
  Clave_Servizo      CHAR(5) NOT NULL,
  Nome_Servizo       VARCHAR(40) NOT NULL,
  PRIMARY KEY (Código_Dependencia),
  UNIQUE (Nome_Dependencia),
  FOREIGN KEY (Clave_Servizo, Nome_Servizo)
    REFERENCES Servizo (Clave_Servizo, Nome_Servizo)
    ON DELETE Cascade
    ON UPDATE Cascade
);

CREATE TABLE Cámara (
  Código_Dependencia CHAR(5),
  Categoría          VARCHAR(40) NOT NULL,
  Capacidade         INTEGER     NOT NULL,
  PRIMARY KEY (Código_Dependencia),
  FOREIGN KEY (Código_Dependencia)
    REFERENCES Dependencia (Código_Dependencia)
    ON DELETE Cascade
    ON UPDATE Cascade
);

CREATE TABLE Tripulación (
  Código_Tripulación CHAR(5) PRIMARY KEY,
  Nome_Tripulación   VARCHAR(40),
  Categoría          CHAR(20)    NOT NULL,
  Antigüidade        INTEGER     DEFAULT 0,
  Procedencia        CHAR(20),
  Adm                CHAR(20)    NOT NULL,
  Código_Dependencia CHAR(5) NOT NULL,
  Código_Cámara      CHAR(5) NOT NULL,
  FOREIGN KEY (Código_Cámara)
    REFERENCES Cámara (Código_Dependencia)
    ON UPDATE Cascade
    ON DELETE Cascade,
  FOREIGN KEY (Código_Dependencia)
    REFERENCES Dependencia (Código_Dependencia)
    ON UPDATE Cascade
    ON DELETE Cascade;
);

CREATE TABLE Visita (
  Código_Tripulación CHAR(5),
  Código_Planeta     CHAR(5),
  Data_Visita        DATE,
  Tempo              INTEGER      NOT NULL,
  PRIMARY KEY (Código_Tripulación, Código_Planeta, Data_Visita),
  FOREIGN KEY (Código_Tripulación)
    REFERENCES Tripulación (Código_Tripulación)
    ON UPDATE CASCADE
    ON DELETE CASCADE,
  FOREIGN KEY (Código_Planeta)
    REFERENCES Planeta (Código_Planeta)
    ON UPDATE CASCADE
    ON DELETE CASCADE 
);
~~~~
Una vez que tenemos el código ejecutamos el script:
![imagen](https://github.com/AlbertoMartinezPose/tarea-3-bases/blob/master/1.1.PNG)

A continuación entramos en mysql con los comandos que ya vimos, y verificamos que la base de datos y las tablas se han creado perfectamente:

![imagen](https://github.com/AlbertoMartinezPose/tarea-3-bases/blob/master/1.2.PNG)

![imagen](https://github.com/AlbertoMartinezPose/tarea-3-bases/blob/master/1.3.PNG)




