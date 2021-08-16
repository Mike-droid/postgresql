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

# Encriptar un texto

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

| trenes    |
| --------- |
| id        |
| modelo    |
| capacidad |

| pasarejos   |
| ----------- |
| id          |
| nombre      |
| dirección   |
| fecha_viaje |

| trayectos   |
| ----------- |
| id          |
| id_estación |
| id_tren     |
| nombre      |

| viajes      |
| ----------- |
| id          |
| id_pasajero |
| id_tryaecto |
| inicio      |
| fin         |

- Estaciones -> Trayectos (1:M)
- Trenes -> Trayectos (1:M)
- Pasajeros -> Viajes (1:M)
- Trayectos <-> Viajes (M:M)

### Jerarquía de Bases de Datos

[Artículo en Platzi](https://platzi.com/clases/1480-postgresql/24848-jerarquia-de-bases-de-datos/)
