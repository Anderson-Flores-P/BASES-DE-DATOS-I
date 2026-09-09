---
grupo: G0X
tarea: Consultas SQL integradoras — Guías 3 a 7
fecha_entrega: 2026-09-XX
integrante_1_carnet: CIF 202601175
integrante_1_nombre: ANDERSON STEVEN FLORES PEREZ
integrante_2_carnet: CIF 2026011056
integrante_2_nombre: CAMILA NICOLE SALVADOR SAMAYOA
integrante_3_carnet: CIF 20XXXXXX
integrante_3_nombre: Nombre Apellido
integrante_4_carnet: CIFF 20XXXXXX
integrante_4_nombre: Nombre Apellido
integrante_5_carnet: CIF 20XXXXXX
integrante_5_nombre: Nombre Apellido
---

## Datos de referencia

Ejecuta primero este script completo en tu esquema de Oracle Database. Crea las tablas y carga los datos sobre los que trabajarán las 7 consultas, que van de menor a mayor complejidad y cubren desde la Guía 3 hasta la Guía 7 (se asume que las Guías 1 y 2 ya fueron completadas).

```sql
-- ═══ Bloque 1: tablas relacionales básicas (usadas en Guías 3 y 4) ═══
CREATE TABLE clientes (
  id_cliente  NUMBER(5)    PRIMARY KEY,
  nombre      VARCHAR2(60) NOT NULL,
  email       VARCHAR2(60),
  telefono    VARCHAR2(20)
);

CREATE TABLE productos (
  id_producto NUMBER(5)     PRIMARY KEY,
  nombre      VARCHAR2(60)  NOT NULL,
  precio      NUMBER(6,2),
  stock       NUMBER(5)
);

CREATE TABLE pedidos (
  id_pedido   NUMBER(6)    PRIMARY KEY,
  id_cliente  NUMBER(5)    REFERENCES clientes(id_cliente),
  fecha       DATE,
  total       NUMBER(8,2)
);

INSERT INTO clientes VALUES (1, 'Carlos Gómez',   'carlos@mail.com', '7011-2233');
INSERT INTO clientes VALUES (2, 'Beatriz Lima',   'bea@mail.com',    '7022-3344');
INSERT INTO clientes VALUES (3, 'Jorge Alas',     'jorge@mail.com',  '7033-4455');
INSERT INTO clientes VALUES (4, 'Rosa Martínez',  'rosa@mail.com',   '7044-5566');

INSERT INTO productos VALUES (1, 'Mouse inalámbrico', 12.50, 40);
INSERT INTO productos VALUES (2, 'Teclado mecánico',  35.00, 15);
INSERT INTO productos VALUES (3, 'Monitor 24"',       145.00, 8);
INSERT INTO productos VALUES (4, 'Cámara web HD',     28.00, 10);

INSERT INTO pedidos VALUES (501, 1, DATE '2026-07-10', 47.50);
INSERT INTO pedidos VALUES (502, 2, DATE '2026-07-11', 145.00);
INSERT INTO pedidos VALUES (503, 1, DATE '2026-07-12', 35.00);
INSERT INTO pedidos VALUES (504, 3, DATE '2026-07-13', 12.50);
INSERT INTO pedidos VALUES (505, 1, DATE '2026-07-14', 28.00);

-- ═══ Bloque 2: tabla para funciones y control de flujo (usada en Guías 6 y 7) ═══
CREATE TABLE alumnos1 (
    id_alumno NUMBER(5)     PRIMARY KEY,
    nombre    VARCHAR2(60)  NOT NULL,
    correo    VARCHAR2(80),
    telefono  VARCHAR2(15),
    promedio  NUMBER(4,2),
    becado    VARCHAR2(5)
);

INSERT INTO alumnos1 VALUES (901, 'Andrea Gómez',  'andrea.gomez@ues.edu.sv',  '7011-2233', 9.2, 'Sí');
INSERT INTO alumnos1 VALUES (902, 'Marco Aguilar',  'marco.aguilar@ues.edu.sv', '7899-4411', 7.8, 'No');
INSERT INTO alumnos1 VALUES (903, 'Lucía Flores',   'lucia.flores@ues.edu.sv',  '7654-3210', 6.5, 'No');
INSERT INTO alumnos1 VALUES (904, 'Jorge Reyes',    'jorge.reyes@ues.edu.sv',   '7123-9988', 5.4, 'Sí');

COMMIT;
```

---

## Consulta 1 — Guía 3: Manipulación de datos en Oracle (Peso: 10%)

Enunciado: Insertar un nuevo producto ("Audífonos Bluetooth", precio 22.00, stock 25) y luego actualizar el stock de "Mouse inalámbrico" restando 3 unidades. Confirma los cambios.

```sql
-- INSERT INTO productos VALUES (5, 'Audífonos Bluetooth', 22.00, 25);
UPDATE productos SET stock = stock - 3 WHERE nombre = 'Mouse inalámbrico';
COMMIT;

```

---

## Consulta 2 — Guía 4: Llaves primarias y foráneas (Peso: 15%)

Enunciado: Crea una tabla nueva `detalle_pedido` que registre qué productos incluye cada pedido, con las columnas `id_detalle` (llave primaria), `id_pedido` (llave foránea hacia `pedidos`), `id_producto` (llave foránea hacia `productos`) y `cantidad`. Luego inserta un registro que indique que el pedido 501 incluyó 2 unidades del producto "Mouse inalámbrico".

```sql
-- Escribe aquí tu CREATE TABLE con las llaves foráneas, y el INSERT correspondiente

```

---

## Consulta 3 — Guía 5: Verificación de tablas en Oracle SQL Developer (Peso: 10%)

Enunciado: Después de crear una tabla usando el asistente visual "New Table" de Oracle SQL Developer (por ejemplo, la tabla `detalle_pedido` de la consulta anterior, o una tabla `CLIENTES_GUI` de práctica), escribe la consulta contra el diccionario de datos que confirme, sin abrir el árbol de Tables, el nombre de columna, tipo de dato y si admite nulos (NULLABLE) de cada columna de esa tabla.

```sql
-- Escribe aquí tu consulta contra USER_TAB_COLUMNS

```

---

## Consulta 4 — Guía 6: Funciones de cadena (Peso: 15%)

Enunciado: Sobre la tabla `alumnos1`, muestra el nombre de cada alumno en formato "Nombre Propio" (usa `INITCAP`), junto con la longitud real de ese nombre sin espacios sobrantes, ordenado de menor a mayor longitud.

```sql
-- CREATE TABLE detalle_pedido (
    id_detalle NUMBER(5) PRIMARY KEY,
    id_pedido NUMBER(5) REFERENCES pedidos(id_pedido),
    id_producto NUMBER(5) REFERENCES productos(id_producto),
    cantidad NUMBER(5)
);

INSERT INTO detalle_pedido VALUES (1, 501, 1, 2);
COMMIT;

```

---

## Consulta 5 — Guía 6: Funciones numéricas y de fecha (Peso: 15%)

Enunciado: Muestra el nombre de cada alumno junto con su promedio redondeado a un decimal y su correo, únicamente para los alumnos cuyo promedio sea mayor o igual a 7. Ordena de mayor a menor promedio.

```sql
-- Escribe aquí tu consulta

```

---

## Consulta 6 — Guía 7: DECODE y CASE (Peso: 15%)

Enunciado: Muestra el nombre de cada alumno, su estatus de beca traducido con `DECODE` ("Sí" → "Con beca activa", "No" → "Sin beca"), y su categoría de rendimiento usando `CASE`: "Excelente" si el promedio es mayor o igual a 9, "Aprobado" si es mayor o igual a 7, y "Reprobado" en cualquier otro caso.

```sql
-- Escribe aquí tu consulta

```

---

## Consulta 7 — Guía 7: Transformación y control de flujo combinados (Peso: 20%)

Enunciado: Genera una "ficha resumida" de cada alumno con: el correo enmascarado (mostrando solo los primeros 3 caracteres antes de la arroba y el dominio completo, el resto oculto con asteriscos), y la categoría de rendimiento del alumno (mismo criterio de la Consulta 6), en una sola consulta. Ordena por categoría y luego por nombre.

```sql
-- Escribe aquí tu consulta

```
