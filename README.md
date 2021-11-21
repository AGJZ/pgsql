# ¡Bienvenido!

Hey! Soy Aure, y vengo a enseñar una "pisca" de **PostgreSQL**  v12.9. 

No me sumergiré en grandes explicaciones dado que toda documentación se encuentra en [PostgreSQL Documentation](https://www.postgresql.org/docs/)

Iré actualizando la mini documentación, más que nada tomar esto como una guía rápida, tips, recordatorio, etc. Por llamarla de alguna manera. 

Recordar que las sintaxis expuestas en el documento, pueden ser simples, se ira introduciendo complejidad en las querys a medida que vaya actualizando esta mini documentación.

Sin más rodeos, comencemos...

# PGSQL | PostgreSQL | Postgres | ...

## Introducción 

En pocas palabras PostgreSQL:
- **Sistema de gestión de base de datos relacionales**
- **El motor de base de datos más avanzado**
- **Código abierto**


## Instalación de PostgreSQL / Ubuntu Server 20.04

> **Nota:** Actualización de lista de repositorios y PPA
**`sudo apt-get update `**

Tras considerar tener actualizada la lista para poder contar con una instalación de postgresql reciente, podremos realizar la instalación mediante:

```
 $ sudo apt-get install postgresql
```

> **Nota:**  Existe la posibilidad de instalar un paquete adicional
> **-contrib** aportando utilidades y funcionalidades adicionales.


## Accediendo por primera vez 

**Importante tener en cuenta**
> **Dolar " $ " determina que todo comando es a través del interprete de comandos.**
> **En caso de no mostrar dolar " $ ", nos encontramos dentro de la sesión interactiva de PostgreSQL, en psql** 

### Acceder a una sesión interactiva
Primera opción:
Accediendo al usuario **postgres**:
```
 $ sudo -i -u postgres
```

Y mediante el comando **psql** podemos iniciar una sesión interactiva de PostgreSQL.

Otra opción:
Abrir la sesión interactiva directamente:
```
 $ sudo -u postgres psql
```
Se entiende como, accediendo primero al usuario **postgres** y posteriormente ejecutando el comando **psql**.

### Salir de una sesión interactiva
Supongamos que nos encontramos dentro de la sesión y queremos salir de esta, tan sencillo como ejecutar:  `\q`

## Crear un usuario
De forma interactiva:
```
 $ createuser --interactive aure
```
Desde psql:
```
 CREATE USER aure WITH PASSWORD 'mypass';
```

## Listar roles\Usuarios
Listar estos es sencillo con: `\du`
Si queremos listar a datalle, se añade un "+" tras de "du":  `\du+`


![roles](https://github.com/AGJZ/pgsql/blob/main/img-md/roles.png)


## Conceder privilegios a un usuario


### Atributos
- LOGIN/ NOLOGIN: Permitir/Denegar iniciar sesión en PostgreSQL.
- INHERIT/ NOINHERIT: Permitir/Denegar la herencia de privilegios.
- CREATEDB/ NOCREATEDB: Permitir/Denegar la capacidad de crear nuevas bases de datos.
- SUPERUSER/ NOSUPERUSER: Permitir/Denegar permisos de superuser.
- CREATEUSER/ NOCREATEUSER: Permitir/Denegar crear nuevos usuarios.
- CREATEROLE/ NOCREATEROLE: Permitir/Denegar crear nuevos roles.

### Privilegios

Ejemplo 1: Conceder todos los privilegios a un usuario dentro de una base de datos:
```
GRANT ALL PRIVILEGES ON DATABASE primeradb TO aure;
```

Ejemplo 2: Conceder privilegio de creación de Base de Datos a un usuario:
```
ALTER ROLE aure CREATEDB;
```
Vemos antes y después de conceder estos atributos a un rol/usuario. Comprobamos con `\du [nombre usuario]`
![privcreatedb](https://github.com/AGJZ/pgsql/blob/main/img-md/privcreatedb.png)

Ejemplo 3: Quitar privilegio de creación de Base de Datos a un usuario:
```
ALTER ROLE aure NOCREATEDB;
```

Ejemplo 4: Conceder permiso de acceso usuario concreto:

```
ALTER ROLE aure LOGIN;
```


## Crear una base de datos
Dentro de la sesión interactiva, podemos crear una base de datos limpia:
```
CREATE DATABASE primeradb;
```

### Acceder a una base de datos
Para acceder utilizamos `\c` para conectar y posteriormente escribimos el nombre de la base de datos.

Ejemplo 1: Desde psql
```
\c primeradb
```
Ejemplo 2: Desde el interprete de comandos
```
$ sudo -h [ip db] -U [usuario] -d [nombre db]
```

## Crear esquemas
Sintaxis para creación de esquemas:
```
CREATE SCHEMA [nombre esquema];
```

Ejemplo 1:
```
CREATE SCHEMA jefes;
```
![createschema](https://github.com/AGJZ/pgsql/blob/main/img-md/createschema.png)

Ejemplo 2: Crear esquema para un usuario en concreto (Este esquema se creará con el nombre del usuario):
```
CREATE SCHEMA AUTHORIZATION aure;
```
![createschema2](https://github.com/AGJZ/pgsql/blob/main/img-md/createschema2.png)

> **Nota:** Si quieres saber más sobre los esquemas: https://www.postgresql.org/docs/12/ddl-schemas.html#DDL-SCHEMAS-CREATE 

## Crear tablas
Sintaxis para creación de tablas:
```
CREATE TABLE [nombre tabla] ([nombre columna] [tipo de dato de la columna]);
```
Ejemplo 1: Creando una tabla llamada **users**, cual contiene: 
- **id_user** dato de tipo **`serial`**, tipo de dato que incrementa tras cada insercción de datos en la columna. 
- **nombre** dato de tipo **`varchar(x)`** con longitud de 20 caracteres. 
- **calle** dato de tipo **`text`** cual longitud es "ilimitada".
- **n_telf** dato de tipo **`char(x)`** cual tiene longitud fija, si no se añaden los caracteres necesarios al insertar el dato, postgres dará un error.

> **Nota:** Algunos tipos de datos más... https://www.geeksforgeeks.org/postgresql-data-types/

![createtable](https://github.com/AGJZ/pgsql/blob/main/img-md/createtable.png)

## Borrar tablas
Sintaxis para borrar tablas:
Ejemplo 1:
```
DROP TABLE [nombre tabla];
```

Ejemplo 2: Con condicional, en caso de existir, borrar.
```
DROP TABLE IF EXISTS [nombre tabla];
```

## Insertar datos
Sintaxis para insertar datos dentro de una tabla:

Ejemplo 1:
```
INSERT INTO [nombre tabla] VALUES ('[Algún dato]');
```

Ejemplo 2: 
```
INSERT INTO [nombre tabla] ("[Entre doble comillas NOMBRE TABLA]") VALUES ('[valor de dato]',[Valor de dato númerico, etc..]);
```
## Borrar datos de una tabla
Sintaxis para borrar todos los datos de una tabla:
```
TRUNCATE TABLE [nombre table];
```

## Ver campos/datos/estructura de tablas
Sintaxis para ver datos de una tabla:

Ejemplo 1: Listar todo de una tabla
```
SELECT * FROM [nombre tabla]; 
```

Ejemplo 2: Listar datos de una columna de una tabla
```
SELECT [nombre columna] FROM [nombre tabla];
```
> **Nota:** Para listar varias tablas concretas, deben separarse por comas ->  ,

## Crando funciones
Ejemplo 1: Creando función que inserte datos dentro de una tabla y no devuelva nada (RETURN void AS $$) y en lenguaje postgresql
```
CREATE FUNCTION insertUser(nombre varchar(20),apellido varchar(20), telf integer) RETURNS
void AS $$
BEGIN
INSERT INTO usuarios values (id,nombre,apellido,telf);
END;
$$ LANGUAGE plpgsql;
```
Para utilizar esta función:
```
SELECT insertUser('Pepito','Jiménez',612345678);
```
Esto insertará los datos Pepito... dentro de la tabla **usuarios**.

Parece tontería hasta que te dicen que ya no tendrás que utilizar lo siguiente (ejemplo) para insertar datos dentro de una tabla, bastará con solo utilizar la función como se indica anteriormente:
```
INSERT INTO usuarios (nombre,apellido,telf) values ('Pepito','Jiménez',612345678);
```