---
grupo: G0X
tarea: Consultas SQL integradoras — Guías 8 a 11
fecha_entrega: 2026-10-XX
integrante_1_carnet: CIF 20XXXXXX
integrante_1_nombre: Nombre Apellido
integrante_2_carnet: CIF 20XXXXXX
integrante_2_nombre: Nombre Apellido
integrante_3_carnet: CIF 20XXXXXX
integrante_3_nombre: Nombre Apellido
integrante_4_carnet: CIF 20XXXXXX
integrante_4_nombre: Nombre Apellido
integrante_5_carnet: CIF 20XXXXXX
integrante_5_nombre: Nombre Apellido
---

## Datos de referencia

Ejecuta primero este script completo en tu esquema de Oracle Database. Crea las tablas y carga los datos sobre los que trabajarán las 8 consultas, que van de menor a mayor complejidad y cubren desde la Guía 8 hasta la Guía 11 (se asume que las Guías 1 a 7 ya fueron completadas). Las tablas llevan el sufijo `_ej` para que no choquen con tablas de entregas anteriores.

```sql
-- ═══ Bloque 1: tienda en línea (usado en Guías 8, 9, 10 y 11) ═══
CREATE TABLE clientes_ej (
  id_cliente  NUMBER(5)    PRIMARY KEY,
  nombre      VARCHAR2(40) NOT NULL,
  apellido    VARCHAR2(40) NOT NULL,
  correo      VARCHAR2(60),
  telefono    VARCHAR2(15),
  ciudad      VARCHAR2(40)
);

CREATE TABLE productos_ej (
  id_producto NUMBER(5)    PRIMARY KEY,
  nombre      VARCHAR2(60) NOT NULL,
  categoria   VARCHAR2(30),
  precio      NUMBER(8,2)
);

CREATE TABLE pedidos_ej (
  id_pedido   NUMBER(6)    PRIMARY KEY,
  id_cliente  NUMBER(5)    REFERENCES clientes_ej(id_cliente),
  fecha       DATE,
  estado      CHAR(1)      CHECK (estado IN ('E','P','C'))
);

CREATE TABLE detalle_ej (
  id_pedido   NUMBER(6)    REFERENCES pedidos_ej(id_pedido),
  id_producto NUMBER(5)    REFERENCES productos_ej(id_producto),
  cantidad    NUMBER(4),
  PRIMARY KEY (id_pedido, id_producto)
);

INSERT INTO clientes_ej VALUES (1, 'Andrea', 'Gómez',    'andrea.gomez@mail.com',   '7011-2233', 'San Salvador');
INSERT INTO clientes_ej VALUES (2, 'Marco',  'Aguilar',  'marco.aguilar@mail.com',  '7899-4411', 'Santa Ana');
INSERT INTO clientes_ej VALUES (3, 'Lucía',  'Flores',   'lucia.flores@mail.com',   '7654-3210', 'San Salvador');
INSERT INTO clientes_ej VALUES (4, 'Jorge',  'Reyes',    'jorge.reyes@mail.com',    '7123-9988', 'San Miguel');
INSERT INTO clientes_ej VALUES (5, 'Sofía',  'Martínez', 'sofia.martinez@mail.com', '7555-1020', 'Santa Ana');
INSERT INTO clientes_ej VALUES (6, 'Diego',  'Castro',   'diego.castro@mail.com',   '7333-4567', 'San Salvador');

INSERT INTO productos_ej VALUES (1, 'Mouse inalámbrico',   'Accesorios',   12.50);
INSERT INTO productos_ej VALUES (2, 'Teclado mecánico',    'Accesorios',   35.00);
INSERT INTO productos_ej VALUES (3, 'Monitor 24 pulgadas', 'Pantallas',   145.00);
INSERT INTO productos_ej VALUES (4, 'Monitor 27 pulgadas', 'Pantallas',   210.00);
INSERT INTO productos_ej VALUES (5, 'Laptop 14 pulgadas',  'Computadoras', 650.00);
INSERT INTO productos_ej VALUES (6, 'Cámara web HD',       'Accesorios',   28.00);
INSERT INTO productos_ej VALUES (7, 'Tablet 10 pulgadas',  'Computadoras', 280.00);

-- estado: E = entregado, P = pendiente, C = cancelado
INSERT INTO pedidos_ej VALUES (1001, 1, DATE '2026-10-01', 'E');
INSERT INTO pedidos_ej VALUES (1002, 2, DATE '2026-10-02', 'E');
INSERT INTO pedidos_ej VALUES (1003, 1, DATE '2026-10-03', 'P');
INSERT INTO pedidos_ej VALUES (1004, 3, DATE '2026-10-04', 'E');
INSERT INTO pedidos_ej VALUES (1005, 4, DATE '2026-10-06', 'C');
INSERT INTO pedidos_ej VALUES (1006, 5, DATE '2026-10-07', 'P');
INSERT INTO pedidos_ej VALUES (1007, 2, DATE '2026-10-08', 'E');
INSERT INTO pedidos_ej VALUES (1008, 1, DATE '2026-10-09', 'E');

INSERT INTO detalle_ej VALUES (1001, 1, 2);
INSERT INTO detalle_ej VALUES (1001, 2, 1);
INSERT INTO detalle_ej VALUES (1002, 5, 1);
INSERT INTO detalle_ej VALUES (1003, 3, 1);
INSERT INTO detalle_ej VALUES (1004, 6, 2);
INSERT INTO detalle_ej VALUES (1004, 1, 1);
INSERT INTO detalle_ej VALUES (1005, 4, 1);
INSERT INTO detalle_ej VALUES (1006, 2, 2);
INSERT INTO detalle_ej VALUES (1007, 3, 2);
INSERT INTO detalle_ej VALUES (1007, 6, 1);
INSERT INTO detalle_ej VALUES (1008, 5, 1);
INSERT INTO detalle_ej VALUES (1008, 1, 3);

-- ═══ Bloque 2: tabla sin normalizar (usada en la Consulta 7, Guía 10 de Normalización) ═══
CREATE TABLE ventas_raw_ej (
  id_venta         NUMBER(5) PRIMARY KEY,
  id_cliente       NUMBER(5),
  nombre_cliente   VARCHAR2(60),
  ciudad_cliente   VARCHAR2(40),
  id_producto      NUMBER(5),
  nombre_producto  VARCHAR2(60),
  id_categoria     NUMBER(3),
  nombre_categoria VARCHAR2(30),
  precio           NUMBER(8,2),
  cantidad         NUMBER(4)
);

INSERT INTO ventas_raw_ej VALUES (1, 101, 'Ana Ruiz',   'San Salvador', 11, 'Audífonos',      1, 'Audio',   25.00, 2);
INSERT INTO ventas_raw_ej VALUES (2, 101, 'Ana Ruiz',   'San Salvador', 12, 'Parlante',       1, 'Audio',   40.00, 1);
INSERT INTO ventas_raw_ej VALUES (3, 102, 'Luis Pérez', 'Santa Ana',    11, 'Audífonos',      1, 'Audio',   25.00, 1);
INSERT INTO ventas_raw_ej VALUES (4, 103, 'Marta Cruz', 'San Miguel',   13, 'Cargador USB-C', 2, 'Energía', 15.00, 3);
INSERT INTO ventas_raw_ej VALUES (5, 102, 'Luis Pérez', 'Santa Ana',    13, 'Cargador USB-C', 2, 'Energía', 15.00, 1);
INSERT INTO ventas_raw_ej VALUES (6, 103, 'Marta Cruz', 'San Miguel',   12, 'Parlante',       1, 'Audio',   40.00, 2);

COMMIT;
```

---

## Consulta 1 — Guía 8: Generación de usuarios y enmascarado de datos (Peso: 10%)

Enunciado: Sobre la tabla `clientes_ej`, muestra para cada cliente: su nombre completo (nombre y apellido separados por un espacio), su usuario del sistema y su teléfono enmascarado. El usuario es la parte del correo antes de la arroba, con el punto reemplazado por un guion bajo (por ejemplo, `andrea.gomez@mail.com` → `andrea_gomez`). El teléfono enmascarado muestra solo los últimos 4 dígitos con el formato `****-2233`. Ordena por `id_cliente`.

```sql
-- Escribe aquí tu consulta

```

---

## Consulta 2 — Guía 8: DECODE y CASE integrados (Peso: 10%)

Enunciado: Sobre la tabla `pedidos_ej`, muestra el `id_pedido`, la fecha con formato `DD/MM/YYYY`, el estado traducido con `DECODE` (`'E'` → "Entregado", `'P'` → "Pendiente", `'C'` → "Cancelado") y una columna de seguimiento calculada con `CASE`: "Atrasado" si el pedido está pendiente y su fecha es anterior al 5 de octubre de 2026, "En proceso" si está pendiente en cualquier otra fecha, y "Cerrado" en cualquier otro caso. Ordena por fecha.

```sql
-- Escribe aquí tu consulta

```

---

## Consulta 3 — Guía 9: Funciones de agregación con GROUP BY (Peso: 10%)

Enunciado: Sobre la tabla `productos_ej`, muestra por cada categoría: la cantidad de productos, el precio promedio redondeado a 2 decimales, el precio más alto y el precio más bajo. Ordena alfabéticamente por categoría.

```sql
-- Escribe aquí tu consulta

```

---

## Consulta 4 — Guía 9: Filtrado de grupos con HAVING (Peso: 10%)

Enunciado: Sobre la tabla `pedidos_ej`, muestra el `id_cliente` y la cantidad de pedidos **no cancelados** de cada cliente, pero únicamente de los clientes que tengan **2 o más** pedidos no cancelados. Ordena de mayor a menor cantidad de pedidos.

```sql
-- Escribe aquí tu consulta

```

---

## Consulta 5 — Guía 10: INNER JOIN de varias tablas (Peso: 15%)

Enunciado: Muestra el detalle de los pedidos **entregados**: nombre completo del cliente, `id_pedido`, nombre del producto, cantidad y subtotal (cantidad multiplicada por el precio del producto). Necesitarás unir `clientes_ej`, `pedidos_ej`, `detalle_ej` y `productos_ej`. Ordena por `id_pedido` y luego por nombre del producto.

```sql
-- Escribe aquí tu consulta

```

---

## Consulta 6 — Guía 10: LEFT JOIN combinado con agregación (Peso: 10%)

Enunciado: Muestra **todos** los clientes con su nombre completo, su ciudad y la cantidad total de pedidos que han realizado (en cualquier estado). Los clientes que nunca han hecho un pedido deben aparecer con 0. Ordena de mayor a menor cantidad de pedidos y luego por nombre completo.

```sql
-- Escribe aquí tu consulta

```

---

## Consulta 7 — Guía 10: Normalización hasta 3FN (Peso: 15%)

Enunciado: La tabla `ventas_raw_ej` no está normalizada: repite los datos del cliente, del producto y de la categoría en cada venta, y el nombre de la categoría depende del producto y no de la venta (dependencia transitiva). Diseña y crea en Oracle el esquema en 3FN con estas cuatro tablas, cada una con su llave primaria y las llaves foráneas que correspondan:

- `categorias_n` (`id_categoria`, `nombre_categoria`)
- `productos_n` (`id_producto`, `nombre_producto`, `precio`, `id_categoria`)
- `clientes_n` (`id_cliente`, `nombre_cliente`, `ciudad_cliente`)
- `ventas_n` (`id_venta`, `id_cliente`, `id_producto`, `cantidad`)

Después, llena las cuatro tablas a partir de `ventas_raw_ej` usando `INSERT INTO ... SELECT DISTINCT ...` (sin escribir los datos a mano) y confirma los cambios.

```sql
-- Escribe aquí tus CREATE TABLE (en el orden correcto por las llaves foráneas), tus INSERT ... SELECT y el COMMIT

```

---

## Consulta 8 — Guía 11: Vistas integradas (Peso: 15%)

Enunciado: Crea la vista `v_ventas_entregadas_ej` que muestre, para cada producto de cada pedido **entregado**: el `id_pedido`, el nombre completo del cliente (con el alias `cliente`), la ciudad, el nombre del producto (con el alias `producto`), la cantidad y el subtotal (con el alias `subtotal`). Luego, usando **solo la vista**, muestra por cada ciudad el monto total vendido (con el alias `total_vendido`), ordenado de mayor a menor.

```sql
-- Escribe aquí tu CREATE VIEW y la consulta sobre la vista

```