---
grupo: G0X
tarea: Investigación Bibliográfica 3 - Agrupación, JOINs, Normalización, Vistas y Sub-consultas
fecha_entrega: 2026-11-01
integrante_1_carnet: CIF 2026011705
integrante_1_nombre: ANDERSON STEVEN FLORES PEREZ
integrante_2_carnet: CIF 2026011056
integrante_2_nombre: CAMILA NICOLE SALVADOR SAMAYOA
integrante_3_carnet: CIF 2026011595
integrante_3_nombre: PERLA ESMERALDA LÓPEZ BARRIENTOS
integrante_4_carnet: CIF 2026011586
integrante_4_nombre: JOSE ALFREDO RODRIGUEZ MONGE
integrante_5_carnet: CIF 20XXXXXX
integrante_5_nombre: Nombre Apellido (como aparece en Moodle)
docente: Mgtr. Rafael Torres
---

# Investigación Bibliográfica 3
## Base de Datos I (BAS I) — Semana 10 a Semana 15

***

## Introducción

<!-- ESCRIBE AQUÍ: 3-5 líneas. De qué trata el trabajo y por qué es importante para el curso. -->

***

## Caso del grupo

### Descripción del negocio
<!-- ESCRIBE AQUÍ: mínimo 50 palabras. Elijan un negocio real o inventado (farmacia, taller, cine, librería, etc.) y describan qué información necesita guardar. No usen tiendas en línea, empleados/departamentos ni alumnos/carreras: esos ya son los ejemplos de clase. -->

### Tablas del caso
<!-- ESCRIBE AQUÍ: los CREATE TABLE de 2 o 3 tablas relacionadas del negocio, con llave primaria y llave foránea, y al menos 5 INSERT por tabla. TODOS los ejemplos de código de los 4 bloques deben usar estas tablas. -->

```sql

```

***

## Bloque 1 — Funciones de Agregación y Agrupación

### Funciones de agregación (COUNT, SUM, AVG, MAX, MIN)
Las funciones de agregación de Oracle permiten resumir conjuntos de registros para elaborar reportes y tomar decisiones basadas en datos. A diferencia de una consulta que muestra todas las filas, una agregación puede devolver un solo resultado con la información total o generar un resultado por grupo. `COUNT(*)` cuenta las órdenes registradas, mientras que `COUNT(columna)` considera únicamente valores no nulos de esa columna. `SUM` acumula importes, `AVG` calcula la media aritmética, `MAX` identifica el valor más alto y `MIN` el más bajo. En el taller de reparación de computadoras, estas funciones ayudan a conocer cuántos servicios se solicitaron, el dinero asociado a las órdenes y los extremos de sus costos. Si se requiere analizar cada cliente o cada tipo de servicio por separado, estas mismas funciones pueden combinarse con `GROUP BY`. Conviene distinguir siempre entre el total de órdenes y el total de pagos efectivamente recibidos: una orden pendiente no representa necesariamente un ingreso cobrado.

```sql
SELECT
    COUNT(*) AS total_ordenes,
    SUM(monto_total) AS monto_acumulado,
    ROUND(AVG(monto_total), 2) AS promedio_orden,
    MAX(monto_total) AS monto_mayor,
    MIN(monto_total) AS monto_menor
FROM ordenes_taller;
```

Con las cinco órdenes de prueba, el total de registros es 5, la suma de importes es 285, el promedio es 57, el máximo es 110 y el mínimo 25. La suma incluye la orden pendiente; no equivale necesariamente al dinero ya cobrado.

### GROUP BY y HAVING: diferencia entre filtrar filas y filtrar grupos
La cláusula `GROUP BY` reúne filas que tienen el mismo valor en una o varias columnas y permite calcular estadísticas para cada grupo. En una base de datos de servicios, por ejemplo, puede reunir las órdenes de cada cliente para averiguar cuántas contrató. La diferencia fundamental entre `WHERE` y `HAVING` está en el momento del filtrado. `WHERE` elimina registros individuales antes de realizar la agrupación: en nuestro ejemplo excluye las órdenes pendientes y conserva únicamente las pagadas. Después, `GROUP BY` agrupa las órdenes por identificador de cliente y las funciones `COUNT` y `SUM` calculan los resultados de cada uno. Finalmente, `HAVING` descarta los grupos que no cumplen la condición establecida, en este caso los clientes que tienen menos de dos órdenes pagadas. De ese modo, se evita confundir una condición de una fila, como el estado de pago, con una condición calculada sobre varias filas. La instrucción `ORDER BY` se agrega de manera explícita porque `GROUP BY` no garantiza ningún orden de presentación

```sql
SELECT
    id_cliente,
    COUNT(*) AS ordenes_pagadas,
    SUM(monto_total) AS total_pagado
FROM ordenes_taller
WHERE estado = 'PAGADA'
GROUP BY id_cliente
HAVING COUNT(*) >= 2
ORDER BY id_cliente;
```

Solo aparecen los clientes 101 y 102, cada uno con dos órdenes pagadas. Sus totales son 65 y 170, respectivamente.

### Investigación más allá de clase: funciones de agregación y valores NULL
En Oracle, `NULL` representa un valor ausente, desconocido o que no corresponde, y no debe confundirse con cero. Esta diferencia modifica el comportamiento de las funciones de agregación. `COUNT(*)` contabiliza todas las órdenes, aunque alguno de sus campos sea nulo; `COUNT(descuento)` solo cuenta las órdenes que tienen un descuento registrado. Asimismo, `SUM(descuento)` y `AVG(descuento)` omiten los descuentos nulos. En cambio, al utilizar `AVG(NVL(descuento, 0))` se sustituyen los valores nulos por cero durante el cálculo, por lo que el denominador incluye todas las órdenes. Esta sustitución es válida únicamente si, para el negocio, un descuento no registrado debe interpretarse como ausencia de descuento; de lo contrario, alteraría el significado de la información. Analizar los resultados de ambas expresiones permite detectar esa diferencia.

```sql
SELECT
    COUNT(*) AS total_ordenes,
    COUNT(descuento) AS con_descuento_registrado,
    SUM(descuento) AS suma_descuentos,
    ROUND(AVG(descuento), 2) AS promedio_no_nulos,
    ROUND(AVG(NVL(descuento, 0)), 2) AS promedio_con_ceros
FROM ordenes_taller;
```

Hay 5 órdenes y 3 descuentos registrados, incluidos los descuentos de valor cero. Su suma es 15, el promedio de descuentos no nulos es 5 y el promedio considerando los nulos como cero es 3.

***

## Bloque 2 — JOINs

### Tipos de JOIN: INNER, LEFT, RIGHT y FULL OUTER

Los operadores `JOIN` en bases de datos relacionales permiten combinar registros de dos o más tablas basándose en un campo común entre ellas. El **INNER JOIN** devuelve únicamente las filas que tienen una coincidencia exacta en ambas tablas descartando los valores huérfanos. Por otro lado, los operadores de tipo externo conservan los registros sin coincidencia de una o ambas partes: el **LEFT JOIN** devuelve todos los registros de la tabla izquierda y las coincidencias de la derecha (completando con nulos si no hay contraparte); el **RIGHT JOIN** opera de forma inversa, priorizando la tabla derecha; y el **FULL OUTER JOIN** combina ambos comportamientos, mostrando todos los registros de ambas tablas y relacionando los que coincidan mientras rellena con valores nulos los campos faltantes en cualquiera de los extremos. Su correcta elección optimiza el rendimiento y garantiza la integridad de la información recuperada.

Ejemplo de código aplicando las tablas del caso:

```sql
SELECT c.nombre_cliente, p.id_pedido, p.estado
FROM clientes_ej c
INNER JOIN pedidos_ej p ON c.id_cliente = p.id_cliente;

SELECT c.nombre_cliente, p.id_pedido
FROM clientes_ej c
LEFT JOIN pedidos_ej p ON c.id_cliente = p.id_cliente;

### El producto cartesiano: qué es y cómo evitarlo

Un producto cartesiano ocurre cuando se ejecutan consultas multitabla omitiendo u olvidando la condición de unión (`ON` o `WHERE`). Como resultado, el motor de base de datos combina cada fila de la primera tabla con absolutamente todas las filas de la segunda tabla, generando un volumen masivo de datos inútiles que saturan la memoria y degradan drásticamente el rendimiento del servidor. Para evitarlo, es fundamental especificar siempre los criterios de relación mediante las cláusulas de unión explícitas (`INNER JOIN ... ON`) o asegurarse de incluir los filtros de equidad correspondientes en la cláusula `WHERE` al utilizar sintaxis tradicional

### Investigación más allá de clase: la sintaxis antigua de Oracle con (+) frente a la sintaxis ANSI

La sintaxis tradicional de Oracle utilizaba el operador de signo más entre paréntesis `(+)` colocado en el lado de la condición donde faltaban datos para simular uniones externas (`LEFT` o `RIGHT JOIN`). No obstante, esta notación era propietaria, confusa en consultas complejas con múltiples tablas y propensa a errores críticos. En contraste, la sintaxis estándar ANSI introducida en SQL:1992 separa claramente la lógica de los filtros de selección (`WHERE`) de las reglas de relación estructural (`JOIN` y `ON`). La adopción de la normativa ANSI favorece la portabilidad del código entre diferentes motores gestores y mejora sustancialmente la legibilidad y el mantenimiento del código SQL.

Ejemplo comparativo:

```sql
-- Sintaxis Antigua Oracle (+)
SELECT e.nombre, d.nombre_departamento
FROM empleados e, departamentos d
WHERE e.id_depto = d.id_depto(+);

-- Sintaxis Estándar ANSI
SELECT e.nombre, d.nombre_departamento
FROM empleados e
LEFT JOIN departamentos d ON e.id_depto = d.id_depto;
***

## Bloque 3 — Normalización

### Dependencias funcionales y anomalías de datos
Una dependencia funcional es una relación lógica entre dos o más atributos dentro de una tabla de bases de datos, donde el valor de un atributo determina de manera única el valor de otro ($X \rightarrow Y$). Este concepto es el pilar fundamental de la normalización. Cuando no se respeta, surgen anomalías de inserción, actualización y borrado durante las operaciones de DML, las cuales se eliminan estructurando correctamente las tablas.

### Formas normales: 1FN, 2FN, 3FN y BCNF
<!-- ESCRIBE AQUÍ: mínimo 150 palabras. Incluye un ejemplo propio: una tabla sin normalizar del negocio del grupo y cómo queda después de aplicar hasta 3FN. -->

### Investigación más allá de clase: la desnormalización
La desnormalización es una técnica que introduce redundancia controlada agrupando tablas o duplicando datos para optimizar el rendimiento de lectura. Es altamente usada en entornos de Data Warehouse y sistemas OLAP para acelerar reportes analíticos pesados. Sin embargo, sus principales riesgos son el incremento del espacio de almacenamiento y la alta complejidad en las escrituras (INSERT, UPDATE, DELETE), ya que cualquier cambio exige actualizar todas las copias para evitar inconsistencias.

***

## Bloque 4 — Vistas y Sub-consultas

### Vistas en Oracle: concepto, creación y uso para seguridad
Una vista en Oracle es una tabla lógica definida mediante una consulta `SELECT` y basada en una o varias tablas existentes. Una vista normal no mantiene una copia independiente de sus filas: su contenido se obtiene al consultarla. Se crea con `CREATE VIEW` y puede modificarse mediante `CREATE OR REPLACE VIEW`, siempre que el usuario tenga los permisos necesarios. Las vistas sirven para reutilizar consultas, simplificar reportes y ofrecer una representación uniforme de los datos. También pueden contribuir a la seguridad: el taller necesita informar cuánto pagó cada cliente, pero no es indispensable revelar los teléfonos de los clientes ni los descuentos específicos de cada orden. Por ello, la vista del ejemplo publica solamente el identificador, el nombre, la cantidad de órdenes pagadas y el importe acumulado. Para restringir efectivamente el acceso se debe conceder permiso de consulta sobre la vista a un usuario de reportes y evitar concederle acceso directo a las tablas base. La vista, por sí sola, no reemplaza una política de permisos.

```sql
CREATE OR REPLACE VIEW vw_resumen_clientes_taller AS
SELECT
    o.id_cliente,
    c.nombre AS cliente,
    COUNT(*) AS ordenes_pagadas,
    SUM(o.monto_total) AS total_pagado
FROM ordenes_taller o
INNER JOIN clientes_taller c ON c.id_cliente = o.id_cliente
WHERE o.estado = 'PAGADA'
GROUP BY o.id_cliente, c.nombre;

SELECT
    id_cliente,
    cliente,
    ordenes_pagadas,
    total_pagado
FROM vw_resumen_clientes_taller
ORDER BY id_cliente;
```

La vista muestra únicamente el resumen de órdenes pagadas y no expone el teléfono ni los descuentos individuales. Para conceder permisos a un usuario de reportes que ya exista, el administrador puede ejecutar `GRANT SELECT ON vw_resumen_clientes_taller TO usuario_reportes;`, siempre que disponga de los privilegios necesarios.

### Sub-consultas: de una fila, de varias filas y correlacionadas
* **Sub-consultas de una fila, de varias filas y correlacionadas (Sección 4.2):**
Una subconsulta es una sentencia SQL anidada dentro de otra consulta principal (en cláusulas como SELECT, FROM o WHERE) que permite resolver operaciones complejas por etapas de manera eficiente.

  * **Sub-consultas de una fila (Escalares):** Retornan un único valor (una fila y una columna) y se utilizan comúnmente con operadores de comparación estándar como `=`, `<`, `>` o `<>`. 
    ```sql
    SELECT nombre, precio FROM productos_ej WHERE precio > (SELECT AVG(precio) FROM productos_ej);
    ```

  * **Sub-consultas de varias filas:** Retornan una lista de múltiples valores y requieren el uso de operadores especiales de conjunto como `IN`, `ANY` o `ALL` para evaluar las condiciones adecuadamente.
    ```sql
    SELECT nombre, correo FROM clientes_ej WHERE id_cliente IN (SELECT id_cliente FROM pedidos_ej WHERE estado = 'E');
    ```

  * **Sub-consultas correlacionadas:** Se evalúan fila por fila en relación directa con la consulta externa, lo que significa que dependen de los datos de afuera y utilizan operadores de existencia como `EXISTS` o `NOT EXISTS`.
    ```sql
    SELECT c.nombre, c.ciudad FROM clientes_ej c WHERE EXISTS (SELECT 1 FROM pedidos_ej p WHERE p.id_cliente = c.id_cliente);
    ```

### Investigación más allá de clase: vistas materializadas
<!-- ESCRIBE AQUÍ: mínimo 80 palabras. ¿En qué se diferencia una vista materializada de una vista normal? ¿Cuándo conviene usarla? -->

### Sub-consulta, JOIN o vista: ¿cuál conviene usar?
<!-- ESCRIBE AQUÍ: mínimo 100 palabras. Resuelvan un mismo problema del negocio del grupo de dos formas distintas (por ejemplo, con sub-consulta y con JOIN) y argumenten cuál prefieren y por qué. -->

***

## Conclusiones

<!-- ESCRIBE AQUÍ: mínimo un párrafo por integrante, o una conclusión grupal de al menos 150 palabras -->

***

## Bibliografía

<!-- Mínimo 5 fuentes en formato APA 7.ª edición, numeradas. Al menos una debe ser documentación oficial de Oracle (Oracle Help Center). -->

1.
2.
3.
4.
5.
