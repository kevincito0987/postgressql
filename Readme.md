# PostgreSQL con Docker

## Creación del Contenedor 🙏🏻

```bash
docker run -d --name postgres_container -e POSTGRES_USER=admin -e POSTGRES_PASSWORD=admin -e POSTGRES_DB=campus -p 5433:5432 -v pgdata:/var/lib/postgresql/data --restart=unless-stopped postgres:15
```

## Conectar al Contenedor de Docker

```bash
docker exec -it postgres_container bash
```

## Conectar con PostgreSQL bajo Consola

```bash
psql --host=localhost --username=admin -d campus --password


psql -h localhost -U admin -d campus -W
```

### Para usuario por defecto

```bash
psql ... --username=postgres ...
```

## Comandos PSQL

- `\l` : Lista las bases de datos
- `\c {db_name}`: Cambiar a una base de datos existente

## Tipos de datos

### Tipos de Datos Numéricos

| Name               | Storage Size | Description                     | Range                                                                                    |
| ------------------ | ------------ | ------------------------------- | ---------------------------------------------------------------------------------------- |
| `smallint`         | 2 bytes      | small-range integer             | -32768 to +32767                                                                         |
| `integer`          | 4 bytes      | typical choice for integer      | -2147483648 to +2147483647                                                               |
| `bigint`           | 8 bytes      | large-range integer             | -9223372036854775808 to +9223372036854775807                                             |
| `decimal`          | variable     | user-specified precision, exact | up to 131072 digits before the decimal point; up to 16383 digits after the decimal point |
| `numeric`          | variable     | user-specified precision, exact | up to 131072 digits before the decimal point; up to 16383 digits after the decimal point |
| `real`             | 4 bytes      | variable-precision, inexact     | 6 decimal digits precision                                                               |
| `double precision` | 8 bytes      | variable-precision, inexact     | 15 decimal digits precision                                                              |
| `smallserial`      | 2 bytes      | small autoincrementing integer  | 1 to 32767                                                                               |
| `serial`           | 4 bytes      | autoincrementing integer        | 1 to 2147483647                                                                          |
| `bigserial`        | 8 bytes      | large autoincrementing integer  | 1 to 9223372036854775807                                                                 |

### Tipos de Datos de Caracteres

| Name                                               | Description                              |
| -------------------------------------------------- | ---------------------------------------- |
| `character varying(*`n`*)`, `varchar(*`n`*)`       | variable-length with limit               |
| `character(*`n`*)`, `char(*`n`*)`, `bpchar(*`n`*)` | fixed-length, blank-padded               |
| `bpchar`                                           | variable unlimited length, blank-trimmed |
| `text`                                             | variable unlimited length                |

### Tipos de Datos Booleanos

| Name      | Storage Size | Description            |
| --------- | ------------ | ---------------------- |
| `boolean` | 1 byte       | state of true or false |

### [Tipos de Datos de Fecha y Hora](https://www.postgresql.org/docs/current/datatype-datetime.html)

| Name                                          | Storage Size | Description                           | Low Value        | High Value      | Resolution    |
| --------------------------------------------- | ------------ | ------------------------------------- | ---------------- | --------------- | ------------- |
| `timestamp [ (*`p`*) ] [ without time zone ]` | 8 bytes      | both date and time (no time zone)     | 4713 BC          | 294276 AD       | 1 microsecond |
| `timestamp [ (*`p`*) ] with time zone`        | 8 bytes      | both date and time, with time zone    | 4713 BC          | 294276 AD       | 1 microsecond |
| `date`                                        | 4 bytes      | date (no time of day)                 | 4713 BC          | 5874897 AD      | 1 day         |
| `time [ (*`p`*) ] [ without time zone ]`      | 8 bytes      | time of day (no date)                 | 00:00:00         | 24:00:00        | 1 microsecond |
| `time [ (*`p`*) ] with time zone`             | 12 bytes     | time of day (no date), with time zone | 00:00:00+1559    | 24:00:00-1559   | 1 microsecond |
| `interval [ *`fields`* ] [ (*`p`*) ]`         | 16 bytes     | time interval                         | -178000000 years | 178000000 years | 1 microsecond |

### Tipos de Datos Monetarios

| Name    | Storage Size | Description     | Range                                          |
| ------- | ------------ | --------------- | ---------------------------------------------- |
| `money` | 8 bytes      | currency amount | -92233720368547758.08 to +92233720368547758.07 |

### [Tipos de Datos Binarios](https://www.postgresql.org/docs/current/datatype-binary.html)

Una cadena binaria es una secuencia de octetos (o bytes). Las cadenas binarias se distinguen de las cadenas de caracteres de dos maneras. Primero, las cadenas binarias permiten específicamente almacenar octetos con valor cero y otros octetos "no imprimibles" (generalmente, octetos fuera del rango decimal de 32 a 126). Las cadenas de caracteres no permiten octetos con valor cero, ni tampoco permiten cualquier otro valor de octeto y secuencias de valores de octetos que sean inválidos según la codificación del conjunto de caracteres seleccionados en la base de datos. En segundo lugar, las operaciones en cadenas binarias procesan los bytes reales, mientras que el procesamiento de cadenas de caracteres depende de la configuración regional. En resumen, las cadenas binarias son apropiadas para almacenar datos que el programador considera como "bytes en bruto", mientras que las cadenas de caracteres son apropiadas para almacenar texto.

El tipo `bytea` soporta dos formatos para entrada y salida: el formato "hex" y el formato "escape" histórico de PostgreSQL. Ambos formatos son siempre aceptados en la entrada. El formato de salida depende del parámetro de configuración `bytea_output`; el valor predeterminado es hex. (Tenga en cuenta que el formato hex se introdujo en PostgreSQL 9.0; las versiones anteriores y algunas herramientas no lo entienden).

| Name    | Storage Size                               | Description                   |
| ------- | ------------------------------------------ | ----------------------------- |
| `bytea` | 1 or 4 bytes plus the actual binary string | variable-length binary string |

### [Tipos de Datos de Redes](https://www.postgresql.org/docs/current/datatype-net-types.html)

| Name       | Storage Size  | Description                      |
| ---------- | ------------- | -------------------------------- |
| `cidr`     | 7 or 19 bytes | IPv4 and IPv6 networks           |
| `inet`     | 7 or 19 bytes | IPv4 and IPv6 hosts and networks |
| `macaddr`  | 6 bytes       | MAC addresses                    |
| `macaddr8` | 8 bytes       | MAC addresses (EUI-64 format)    |

## [Tipos de Datos Geométricos](https://www.postgresql.org/docs/current/datatype-geometric.html)

| Name      | Storage Size | Description                      | Representation                      |
| --------- | ------------ | -------------------------------- | ----------------------------------- |
| `point`   | 16 bytes     | Point on a plane                 | (x,y)                               |
| `line`    | 32 bytes     | Infinite line                    | {A,B,C}                             |
| `lseg`    | 32 bytes     | Finite line segment              | ((x1,y1),(x2,y2))                   |
| `box`     | 32 bytes     | Rectangular box                  | ((x1,y1),(x2,y2))                   |
| `path`    | 16+16n bytes | Closed path (similar to polygon) | ((x1,y1),...)                       |
| `path`    | 16+16n bytes | Open path                        | [(x1,y1),...]                       |
| `polygon` | 40+16n bytes | Polygon (similar to closed path) | ((x1,y1),...)                       |
| `circle`  | 24 bytes     | Circle                           | <(x,y),r> (center point and radius) |

### Tipos de Datos JSON y XML

- `json`: Datos en formato JSON
- `jsonb`: Datos JSON en un formato binario más eficiente
- `xml`: Datos en formato XML

### Tipos de Datos Especiales

- `uuid`: Identificador único universal
- `array`: Arreglos de cualquier tipo de datos
- `composite`: Tipo compuesto de varios tipos de datos
- `range`: Rango de valores

### Enumeradores
```sql
    CREATE TYPE sexo AS ENUM('Masculino', 'Femenino', 'Otro');

    CREATE TABLE camper(
        name varchar(100) NOT NULL,
        sexo_camper sexo NOT NULL
    );

    CREATE TABLE ejemplo(id serial PRIMARY KEY, nombre varchar(100) NOT NULL, descripcion text NULL, precio numeric(10,2) NOT NULL, en_stock boolean NOT NULL, fecha_creacion_ date NOT NULL, hora_creacion
    time NOT NULL, fecha_hora timestamp NOT NULL, fecha_hora_zona, timestamp with time zone, duracion interv
    al, direccion_ip iner, direccion_mac macaddr, punto_geometrico point, datos_json json, datos__jsonb jsonb
    , identificador_unico uuid, cantidad_monetaria money, rangos int4range, colores_preferidos varchar(20)[])

    INSERT INTO ejemplo (nombre,descripcion, precio, en_stock, fecha_creacion_, hora_creacion, fecha_hora, fecha_hora_zona, duracion, direccion_ip, direccion_mac, punto_geometrico, datos_json, datos__jsonb, identificador_unico, cantidad_monetaria, rangos, colores_preferidos)values('Ejemplo A', 'Lorem ipsum-....', 9990.99, true, '2025-07-10', '20:30:20', '2025-07-10 20:30:20', '2025-07-10 20:30:10-05','1 day', '192.168.0.1', '08:00:27:00:00:00', '(10, 20)', '{"key":"value"}', '{"key":"value"}', 'b8ac502c-7049-4ae5-aa7e-642ad77ca4f1', '100.00', '[10, 20)', ARRAY['Rojo', 'Azul']);

```

## Constrains.

```sql

    CREATE TABLE empleados(
        id serial,
        nombre varchar(100) NOT NULL,
        edad integer NOT NULL,
        salario numeric(10,2) NOT NULL,
        fecha_contrato date,
        vigente boolean DEFAULT true
    ); 
    ALTER TABLE empleados ADD PRIMARY KEY(id);

    CREATE TABLE departamentos (
        id serial, 
        nombre varchar(100) NOT NULL,
        vigente boolean DEFAULT true,
        PRIMARY KEY(id)    
        );

    ALTER TABLE empleados ADD CONSTRAINT fk_departamento_id FOREIGN KEY(departamento_id) REFERENCES departamentos(id);

    ALTER TABLE empleados ADD COLUMN departamento_id integer NOT NULL;

    ALTER TABLE empleados ADD UNIQUE(nombre);

    ALTER TABLE empleados ADD CONSTRAINT edadEmpleado CHECK (edad>=18);
    
    ALTER TABLE empleados ALTER COLUMN salario SET DEFAULT 400.00;
```


### Taller contstraint.

```SQL
CREATE TABLE country (
    id serial,
    name varchar(50)
);

CREATE TABLE region (
    id serial,
    name varchar(50),
    idcountry integer
);

CREATE TABLE city (
    id serial,
    name varchar(50),
    idregion integer
);

ALTER TABLE country ADD PRIMARY KEY(id);
ALTER TABLE region ADD PRIMARY KEY(id);
ALTER TABLE city ADD PRIMARY KEY(id);

ALTER TABLE country ALTER COLUMN name SET NOT NULL;
ALTER TABLE region ALTER COLUMN name SET NOT NULL;
ALTER TABLE city ALTER COLUMN name SET NOT NULL;

ALTER TABLE region ADD CONSTRAINT fk_country_id FOREIGN KEY(idcountry) REFERENCES country(id);
ALTER TABLE city ADD CONSTRAINT fk_region_id FOREIGN KEY(idregion) REFERENCES region(id);

ALTER TABLE country ALTER COLUMN name SET DEFAULT '';
ALTER TABLE region ALTER COLUMN name SET DEFAULT '';
ALTER TABLE city ALTER COLUMN name SET DEFAULT '';
```


## Taller Hoy.


```sql
    CREATE TABLE categorias(
        id_categoria serial,
        descripcion varchar(45),
        estado boolean
    );

    ALTER TABLE categorias ADD PRIMARY KEY(id_categoria);
    ALTER TABLE categorias ALTER COLUMN descripcion SET NOT NULL;
    ALTER TABLE categorias ALTER COLUMN estado SET DEFAULT true;


    CREATE TABLE productos(
        id_producto serial,
        nombre varchar(45),
        id_categoria integer,
        codigo_barras varchar(150),
        precio_venta decimal(16,2),
        cantidad_stock integer,
        estado boolean
    );
    ALTER TABLE productos ADD PRIMARY KEY(id_producto);
    ALTER TABLE productos ADD CONSTRAINT fk_categoria_id FOREIGN KEY(id_categoria) REFERENCES categorias(id_categoria) ON DELETE CASCADE ON UPDATE CASCADE;
    ALTER TABLE productos ALTER COLUMN nombre SET NOT NULL;
    ALTER TABLE productos ALTER COLUMN codigo_barras SET NOT NULL;
    ALTER TABLE productos ALTER COLUMN precio_venta SET NOT NULL;
    ALTER TABLE productos ALTER COLUMN cantidad_stock SET NOT NULL;
    ALTER TABLE productos ALTER COLUMN estado SET DEFAULT true;

    CREATE TABLE clientes(
        id serial,
        nombre varchar(40),
        apellidos varchar(100),
        celular varchar(20),
        direccion varchar(80),
        correo_electronico varchar(70)
    );

    ALTER TABLE clientes ADD PRIMARY KEY(id);
    ALTER TABLE clientes ALTER COLUMN nombre SET NOT NULL;
    ALTER TABLE clientes ALTER COLUMN apellidos SET NOT NULL;
    ALTER TABLE clientes ALTER COLUMN celular SET NOT NULL;
    ALTER TABLE clientes ALTER COLUMN direccion SET NOT NULL;
    ALTER TABLE clientes ALTER COLUMN correo_electronico SET NOT NULL;
    ALTER TABLE clientes ADD UNIQUE(correo_electronico);
    ALTER TABLE clientes ALTER COLUMN estado SET DEFAULT true;

    CREATE TABLE compras(
        id_compra serial,
        id_cliente integer,
        fecha timestamp,
        medio_pago char(1),
        comentario varchar(300),
        estado char(1)
    );
    ALTER TABLE compras ADD PRIMARY KEY(id_compra);
    ALTER TABLE compras ALTER COLUMN fecha SET NOT NULL;
    ALTER TABLE compras ALTER COLUMN medio_pago SET NOT NULL;
    ALTER TABLE compras ALTER COLUMN estado SET DEFAULT true;
    ALTER TABLE compras ADD CONSTRAINT fk_cliente_id FOREIGN KEY(id_cliente) REFERENCES clientes(id) ON DELETE CASCADE ON UPDATE CASCADE;


    CREATE TABLE compras_productos(
        id_compra integer,
        id_producto integer,
        cantidad integer,
        total decimal(16,2),
        estado boolean
    );
    ALTER TABLE compras_productos ADD PRIMARY KEY(id_compra, id_producto);
    ALTER TABLE compras_productos ALTER COLUMN cantidad SET NOT NULL;
    ALTER TABLE compras_productos ALTER COLUMN total SET NOT NULL;
    ALTER TABLE compras_productos ALTER COLUMN estado SET DEFAULT true;
    ALTER TABLE compras_productos ADD CONSTRAINT fk_compra_id FOREIGN KEY(id_compra) REFERENCES compras(id_compra) ON DELETE CASCADE ON UPDATE CASCADE;
    ALTER TABLE compras_productos ADD CONSTRAINT fk_producto_id FOREIGN KEY(id_producto) REFERENCES productos(id_producto) ON DELETE CASCADE ON UPDATE CASCADE;


    -- DML INSERTS.

    INSERT INTO categorias (descripcion) VALUES
    ('Electrónica'),
    ('Ropa'),
    ('Libros'),
    ('Hogar'),
    ('Alimentos');  

    -- Inserts para la tabla clientes
    INSERT INTO clientes (nombre, apellidos, celular, direccion, correo_electronico) VALUES
    ('Juan', 'Perez Garcia', '5512345678', 'Av. Reforma 101, CDMX', 'juan.perez@email.com'),
    ('Maria', 'Lopez Diaz', '5587654321', 'Calle Madero 20, Monterrey', 'maria.lopez@email.com'),
    ('Carlos', 'Rodriguez Sanchez', '5511223344', 'Blvd. del Sol 50, Guadalajara', 'carlos.r@email.com');

    -- Inserts para la tabla productos
    INSERT INTO productos (nombre, id_categoria, codigo_barras, precio_venta, cantidad_stock) VALUES
    ('Laptop Gamer', 1, '1234567890123', 25000.00, 50),
    ('Camiseta de Algodón', 2, '9876543210987', 250.50, 200),
    ('El Señor de los Anillos', 3, '1122334455667', 350.75, 100),
    ('Juego de Sábanas', 4, '9988776655443', 600.00, 80),
    ('Arroz Blanco 1kg', 5, '5566778899001', 35.00, 500);

    -- Inserts para la tabla compras
    -- Nota: 'id_cliente' debe coincidir con los IDs de los clientes insertados.
    INSERT INTO compras (id_cliente, fecha, medio_pago, comentario, estado) VALUES
    (1, '2024-08-15 10:30:00', 'C', 'Entrega a domicilio urgente', 'P'),
    (2, '2024-08-14 15:45:00', 'T', 'Paquetería estándar', 'E'),
    (3, '2024-08-13 12:00:00', 'C', 'Recoger en tienda', 'P');

    -- Inserts para la tabla compras_productos
    -- Nota: 'id_compra' y 'id_producto' deben coincidir con las compras y productos.
    -- El campo 'total' es la 'cantidad' * 'precio_venta' del producto.
    INSERT INTO compras_productos (id_compra, id_producto, cantidad, total) VALUES
    (1, 1, 1, 25000.00), -- 1 Laptop
    (1, 2, 2, 501.00), -- 2 Camisetas
    (2, 3, 1, 350.75), -- 1 Libro
    (3, 5, 5, 175.00); -- 5 Arroces


    --Esquemas.
\dn para esquemas.
``` 