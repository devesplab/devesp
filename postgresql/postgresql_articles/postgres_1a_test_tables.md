---
layout: default
title:  Creando PostgreSQL Tablas de Prueba
permalink: /postgresql-test-tables/
parent: Artículos De Postgresql
has_children: false
has_toc: false
nav_order: 1
---

# Creando Tablas de Prueba en PostgreSQL

{: .no_toc }

<details open markdown="block">
  <summary>
    Tabla de contenidos
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

---

Este ejercicio muestra una forma básica de crear tablas que pueden ser usadas para propósitos de prueba.

Por ejemplo,

- Asegurarse de tener un entorno de base de datos funcional
- Probar acceso y permisos
- Validar replicación de base de datos

## Crear Tablas Manualmente

Puedes usar esto para crear rápidamente tablas y probar que tu configuración de Postgres funciona como esperas.

### Crear table1

Esto crea una tabla llamada "table1" y agrega tres valores.
```sql
CREATE TABLE table1 (website varchar(100));
INSERT INTO table1 VALUES ('devuser');
INSERT INTO table1 VALUES ('Created this on the Master on table1');
INSERT INTO table1 VALUES ('pg replication by cicd');
```

### Crear table2

Esto crea una tabla llamada "table2" y agrega tres valores.
```sql
CREATE TABLE tables2 (website varchar(100));
INSERT INTO tables2 VALUES ('devuser');
INSERT INTO tables2 VALUES ('Created this on the Master tables2');
INSERT INTO tables2 VALUES ('pg replication by cicd');
```

### Crear table3

Esto crea una tabla llamada "table3" y agrega tres valores.
```sql
CREATE TABLE table3 (website varchar(100));
INSERT INTO table3 VALUES ('devuser');
INSERT INTO table3 VALUES ('Created this on the Master table3');
INSERT INTO table3 VALUES ('pg replication by cicd');
```

Luego puedes probar desde la línea de comandos para listar las tablas.
```bash
-> psql -c "\dt"
```
Este comando lista todas las tablas en la base de datos actual. Deberías ver `table1`, `table2` y `table3` listadas.
 
Los siguientes comandos listan el contenido de las tablas.
```bash
-> psql -c "SELECT * FROM table1;"
-> psql -c "SELECT * FROM table2;"
-> psql -c "SELECT * FROM table3;"
```

Si las tablas aparecen y puedes listar el contenido, esto indica algunas cosas básicas:
- Tu servidor de Postgres está en funcionamiento
- Puedes conectarte al servidor de Postgres
- Puedes crear tablas
- Puedes insertar datos en las tablas
- Puedes consultar las tablas
- Puedes ver los datos en las tablas
- Puedes listar las tablas
- Tienes permisos para interactuar con el entorno de Postgres
- El entorno de Postgres puede procesar tus solicitudes

## Crear Tablas con un Script SQL

El siguiente script SQL crea tres tablas con algunos datos de ejemplo. Esto es útil para probar replicación u otras funcionalidades de base de datos sin tener que ingresar datos manualmente cada vez.
Puedes ejecutar este script para crear las tablas e insertar algunos datos iniciales. El script está diseñado para ser idempotente, lo que significa que no fallará si lo ejecutas varias veces sin cambios.

Este script creará tres tablas (users, products, orders) adecuadas para pruebas básicas de replicación. Puedes modificar las columnas según lo necesites para tu escenario de prueba.

ARCHIVO: `tables_test_create.sql`
```sql
-- Table 1: users
CREATE TABLE IF NOT EXISTS users (
id SERIAL PRIMARY KEY,
username VARCHAR(50) NOT NULL,
email VARCHAR(100) NOT NULL
);

INSERT INTO users (username, email) VALUES ('testuser', 'testuser@example.com');

-- Table 2: products
CREATE TABLE IF NOT EXISTS products (
id SERIAL PRIMARY KEY,
name VARCHAR(100) NOT NULL,
price NUMERIC(10,2) NOT NULL
);

INSERT INTO products (name, price) VALUES ('Sample Product', 19.99);

-- Table 3: orders
CREATE TABLE IF NOT EXISTS orders (
id SERIAL PRIMARY KEY,
user_id INTEGER REFERENCES users(id),
product_id INTEGER REFERENCES products(id),
order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

INSERT INTO orders (user_id, product_id) VALUES (1, 1);
```

Ejecuta el script así:
```bash
psql -f tables_test_create.sql
```
Este comando ejecuta el script SQL, creando las tablas e insertando los datos de ejemplo.
Después de ejecutar el script, puedes comprobar los resultados listando las tablas y consultando los datos.

Puedes usar el siguiente comando psql en tu terminal para comprobar los datos recién insertados en cada tabla:
```
psql -c "SELECT * FROM users;"
psql -c "SELECT * FROM products;"
psql -c "SELECT * FROM orders;"
```
Esto mostrará los datos en cada tabla, confirmando que las tablas fueron creadas y pobladas exitosamente.
Si ejecutas las sentencias `CREATE TABLE` y las tablas ya existen, PostgreSQL devolverá un error como este:
```log
ERROR: relation "users" already exists
```

Al crear tablas, es una buena práctica usar la cláusula `IF NOT EXISTS`. Esto previene errores si la tabla ya existe, permitiendo que tu script se ejecute sin interrupciones.
Sin la sentencia `IF NOT EXISTS`, cuando intentas crear una tabla que ya existe, PostgreSQL devolverá un error y el script puede detenerse.
Con la sentencia `IF NOT EXISTS`, no ocurre ningún error y la tabla permanece sin cambios si ya existe.

Crea un script para eliminar las tablas creadas arriba. Esto es útil para limpiar después de pruebas o para reiniciar el estado de la base de datos.

ARCHIVO: `tables_test_drop.sql`
```
-- Drop tables for replication test
DROP TABLE IF EXISTS orders;
DROP TABLE IF EXISTS products;
DROP TABLE IF EXISTS users;
```

Ejecuta el script así:
```bash
psql -f tables_test_drop.sql
```

Ejecuta el script en la instancia de base de datos PRIMARIA.

```bash
-> psql -f tables_test_create.sql
CREATE TABLE
INSERT 0 1
CREATE TABLE
INSERT 0 1
CREATE TABLE
INSERT 0 1
```

Verifica los resultados en la instancia de base de datos PRIMARIA y SECUNDARIA.

La salida debe ser la misma como indicación de que los datos se están replicando activamente.
```sql
-> psql -c "\dt"
List of relations
Schema | Name | Type | Owner
--------+----------+-------+----------
public | orders | table | postgres
public | products | table | postgres
public | users | table | postgres
(3 rows)

-> psql -c "SELECT * FROM users;"
id | username | email
----+----------+----------------------
1 | testuser | testuser@example.com
(1 row)

-> psql -c "SELECT * FROM products;"
id | name | price
----+----------------+-------
1 | Sample Product | 19.99
(1 row)

-> psql -c "SELECT * FROM orders;"
id | user_id | product_id | order_date
----+---------+------------+----------------------------
1 | 1 | 1 | 2025-07-15 17:56:56.096274
(1 row)
```

[Regresar a la página principal]({{site.baseurl}}/).