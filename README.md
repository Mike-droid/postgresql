# Curso de PostgreSQL

## Configurar Postgres

### ¿Qué es Postgresql?

Es un motor de base de datos. Tenemos que tener en cuenta 3 cosas importantes:

1. Lenguaje
2. Motor
3. Servidor

Postgres es el motor. Existe desde 1986. Usa Objeto Relacional.

- A: Atomicity -> Separar las funciones desarrolladas en la BD como pequeñas tareas y ejecutarlas como un todo. Si alguna tarea falla se hace un rollback(Se deshacen los cambios)
- C: Consistency -> Todo lo que se desarrolló en base al objeto relacional. Los datos tienen congruencia.
- I: Isolation -> Varias tareas ejecutándose al mismo tiempo dentro de la BD.
- D: Durability -> Puedes tener seguridad que la información no se perderá por un fallo catastrófico. PostgreSQL guarda la información en una Bitácora.

¿Por qué PostgreSQL?

- Tipos de datos
- Integridad de datos
- Concurrencia, rendimiento
- Fiabilidad, recuperación antre desastres
- Seguridad
- Extensibilidad
- Internalización, búsqueda de texto
- Sistema de geolocalización

### Instalación y configuración de la Base de Datos

[Artículo en Platzi](Instalación y configuración de la Base de Datos)

Primer comando `CREATE DATABASE transporte_publico;`

Para entrar a esa BD usamos `\c transporte_publico;`

### Interacción con Postgres desde la Consola

Podemos accerder a la BD desde la GUI o la consola. Es más recomendable usar la consola.

- Ver todos los comandos disponibles: `\?`
- Mirar todas las BD: `\l`
- Tablas en la BD: `dt`
- Mostrar esquemas: `dn`
- Mostrar funciones disponibles: `\df`
- Mostrar las vistas: `\dv`
- Mostrar usuarios y roles: `\du`
- Crear BD: `CREATE DATABASE baseDatos1`
- Entrar a BD: `\c baseDatos1`
- Entrar a Tabla: `\c nombreTabla`
- Describir la tabla: `\d nombreTabla`
- Pedir ayuda con consultas de SQL: `\h`
- Consultar versión de Postgres: `SELECT version();`
- Volver a ejecutar el último comando ejecutado `\g`
- Ver historial de comandos ejecutados: `\s`
- Para guardar los comando en un archivo de texto plano: `\s nombreArchivo`
- Para ejecutar los comandos desde un archivo: `\i nombreArchivo`
- Abrir editor de texto plano: `\e`
- Abrir editor de texto plano y editar funciones: `\ef`
- Encender un cronómetro: `\timing` -> te dirá cuánto se tarda una instrucción en ejecutarse.
- Limpiar consola en linux: `\! clear`
- Limpiar consola en Windows: `\! cls`
- Cerrar consola: `\q`

### PgAdmin: Interacción con Postgres desde la Interfaz Gráfica

### Archivos de Configuración

Existen 3 archivos importantes.

1. postgresql.conf
2. pg_hba.conf
3. pg_ident.conf

Los puedes encontrar con `SHOW config_file;` y la ruta es -> C:\Program Files\PostgreSQL\13\data

Puedes editar estos archivos para usarlos a tu beneficio.

### Comandos más utilizados en PostgreSQL

[Artículo en Platzi](https://platzi.com/clases/1480-postgresql/24849-comandos-mas-utilizados-en-postgresql/)

```sql
CREATE TABLE tren(
  id serial NOT NULL,
  modelo character varying,
  capacidad integer, constraint tren_pkey PRIMARY KEY (id)
);

INSERT INTO tren(modelo, capacidad) VALUES ('Volvo 1', 100);

SELECT * FROM tren;

UPDATE tren SET modelo = 'Honda 0726' WHERE id = 1;

SELECT * FROM tren;

DELETE FROM tren WHERE id = 1;

SELECT * FROM tren;

-- Encriptar un texto

SELECT sha256('abc');
```

### Presentación del Proyecto

Reto

- Pasajero
- Trayecto
- Estación
- Tren
- Viaje

### Tipos de datos

Principales:

- Numéricos
- Monetarios
- texto
- Binarios
- Fecha / Hora
- Boolean

Especiales:

- Geométricos
- Dirección de Red
- Texto tipo bit
- XML, JSON
- Arreglos

### Diseñando nuestra base de datos: estructura de las tablas

**SIEMPRE** haz el diagrama antes de ponerte a usar SQL.

| Estaciones |
| ---------- |
| id         |
| nombre     |
| dirección  |

| Trenes    |
| --------- |
| id        |
| modelo    |
| capacidad |

| Pasarejos        |
| ---------------- |
| id               |
| nombre           |
| dirección        |
| fecha_nacimiento |

| Trayectos   |
| ----------- |
| id          |
| id_estación |
| id_tren     |
| nombre      |

| Viajes      |
| ----------- |
| id          |
| id_pasajero |
| id_trayecto |
| inicio      |
| fin         |

- Estaciones -> Trayectos (1:M)
- Trenes -> Trayectos (1:M)
- Pasajeros -> Viajes (1:M)
- Trayectos <-> Viajes (M:M)

### Jerarquía de Bases de Datos

[Artículo en Platzi](https://platzi.com/clases/1480-postgresql/24848-jerarquia-de-bases-de-datos/)

## Gestión de la información en bases de datos

### Creación de tablas

Acciones:

- Create -> Crear tabla
- Alter -> Modificar tabla
- Drop -> Eliminar tabla

Creando tabla Pasajeros:

```sql
CREATE TABLE IF NOT EXISTS public."Pasajeros"
(
    id serial NOT NULL,
    nombre character varying(100) NOT NULL,
    direccion_residencia character varying NOT NULL,
    fecha_nacimiento date NOT NULL,
    CONSTRAINT pasajero_pkey PRIMARY KEY (id)
);

ALTER TABLE public."Pasajeros"
    OWNER to postgres;

------------------------------------

INSERT INTO public."Pasajeros"(
  nombre, direccion_residencia, fecha_nacimiento)
  VALUES ('Pasajero 1', 'Dirección 1', '2021-08-17'); -- Formato AAAA-MM-DD
```

Creando tabla Estaciones:

```sql
CREATE TABLE IF NOT EXISTS public."Estaciones"
(
    id serial NOT NULL,
    nombre character varying NOT NULL,
    direccion character varying NOT NULL,
    CONSTRAINT estacion_pkey PRIMARY KEY (id)
);

ALTER TABLE public."Estaciones"
    OWNER to postgres;

--------------------------------------

INSERT INTO public."Estaciones"(
  nombre, direccion)
  VALUES ('Estación 1', 'Dirección 1');
```

Creando tabla de Trenes:

```sql
CREATE TABLE IF NOT EXISTS public."Trenes"
(
    id serial NOT NULL,
    modelo integer NOT NULL,
    capacidad integer NOT NULL,
    CONSTRAINT tren_pkey PRIMARY KEY (id)
);

ALTER TABLE public."Trenes"
    OWNER to postgres;

--------------------------------------

INSERT INTO public."Trenes"(
  modelo, capacidad)
  VALUES (2020, 100);
```

Creando tablas de Trayectos:

```sql
CREATE TABLE IF NOT EXISTS public."Trayectos"
(
    id serial NOT NULL,
    id_estacion serial NOT NULL,
    id_tren serial NOT NULL,
    nombre character varying NOT NULL,
    CONSTRAINT trayecto_pkey PRIMARY KEY (id),
    CONSTRAINT estacion_fkey FOREIGN KEY (id_estacion)
        REFERENCES public."Estaciones" (id) MATCH SIMPLE
        ON UPDATE CASCADE
        ON DELETE CASCADE
        NOT VALID,
    CONSTRAINT tren_fkey FOREIGN KEY (id_tren)
        REFERENCES public."Trenes" (id) MATCH SIMPLE
        ON UPDATE CASCADE
        ON DELETE CASCADE
        NOT VALID
);

ALTER TABLE public."Trayectos"
    OWNER to postgres;

----------------------------------

INSERT INTO public."Trayectos"(
  id_estacion, id_tren, nombre)
  VALUES (1, 1, 'Trayecto 1');
```

Creando tabla de Viajes:

```sql
CREATE TABLE IF NOT EXISTS public."Viajes"
(
    id serial NOT NULL,
    id_pasajero serial NOT NULL,
    id_trayecto serial NOT NULL,
    inicio date NOT NULL,
    fin date NOT NULL,
    CONSTRAINT viaje_pkey PRIMARY KEY (id),
    CONSTRAINT pasajero_fkey FOREIGN KEY (id_pasajero)
        REFERENCES public."Pasajeros" (id) MATCH SIMPLE
        ON UPDATE CASCADE
        ON DELETE CASCADE
        NOT VALID,
    CONSTRAINT trayecto_fkey FOREIGN KEY (id_trayecto)
        REFERENCES public."Trayectos" (id) MATCH SIMPLE
        ON UPDATE CASCADE
        ON DELETE CASCADE
        NOT VALID
);

ALTER TABLE public."Viajes"
    OWNER to postgres;

----------------------------------

INSERT INTO public."Viajes"(
  id_pasajero, id_trayecto, inicio, fin)
  VALUES (1, 1, '2019-10-31', '2019-11-02');
```

### Particiones

Las particiones son útiles cuando tenemos mucha información guardada en una tabla.

Las particiones hacen:

- Seperación física de datos -> Guardar partes de la tabla en diferentes espacios del disco
- Estructura lógica -> Se conserva la misma estructura

### Creación de Roles

Los roles sirven para dar priviligios a un usuario en una base de datos. Por defecto el rol que tienen las tablas asignadas es el rol de "posgres", el cual tiene acceso total a todas las acciones. Es peligroso seguir usando este rol porque se podría borrar toda la base de datos.

- Crear rol o usuario (es lo mismo): `CREATE ROLE nombre_rol` / `CREATE USER nombre_usuario`
- Listado de usuarios: `\dg`

Le damos acceso a conectarse: `ALTER ROLE usuario_consulta WITH LOGIN`;

Le damos el privilegio de super usuario: `ALTER ROLE usuario_consulta WITH SUPERUSER;`

Le asignamos una contraseña `ALTER ROLE usuario_consulta WITH PASSWORD 'etc123'` **No usar una contraseña como la de prueba en un entorno real, es solo una prueba**

- Eliminar rol: `DROP ROLE nombre_rol`

```sql
CREATE ROLE usuario_consulta WITH
  LOGIN
  NOSUPERUSER
  NOCREATEDB
  NOCREATEROLE
  INHERIT
  NOREPLICATION
  CONNECTION LIMIT -1
  PASSWORD 'xxxxxx'; --contraseña oculta
COMMENT ON ROLE usuario_consulta IS 'Este usuario solo podrá consultar datos';
```

Modificando persmisos en tablas para un rol:

```sql
GRANT INSERT, SELECT, UPDATE ON TABLE public."Estaciones" TO usuario_consulta;

GRANT INSERT, SELECT, UPDATE ON TABLE public."Pasajeros" TO usuario_consulta;

GRANT INSERT, SELECT, UPDATE ON TABLE public."Trayectos" TO usuario_consulta;

GRANT INSERT, SELECT, UPDATE ON TABLE public."Trenes" TO usuario_consulta;

GRANT INSERT, SELECT, UPDATE ON TABLE public."Viajes" TO usuario_consulta;

```

### Llaves foráneas

Las FK tienen una tabla de origen, una tabla de destino y una serie de acciones. Permiten que haya relación entre las tablas.

Es muy importante seleccionar la pestaña "Action" para que indique si debe hacer algo o no cuando se hacen cambio o eliminaciones en la tabla de referencia.

### Inserción y consulta de datos

### Inserción masiva de datos

[Mokaroo](https://mockaroo.com/) es una página para crear datos de prueba.

También existe [Generate Data](https://www.generatedata.com/).
