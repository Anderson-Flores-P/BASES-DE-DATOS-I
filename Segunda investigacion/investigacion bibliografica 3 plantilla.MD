---
grupo: G0X
tarea: Investigación Bibliográfica 3 - Agrupación, JOINs, Normalización, Vistas y Sub-consultas
fecha_entrega: 2026-11-01
integrante_1_carnet: CIF 20XXXXXX
integrante_1_nombre: Nombre Apellido (como aparece en Moodle)
integrante_2_carnet: CIF 20XXXXXX
integrante_2_nombre: Nombre Apellido (como aparece en Moodle)
integrante_3_carnet: CIF 20XXXXXX
integrante_3_nombre: Nombre Apellido (como aparece en Moodle)
integrante_4_carnet: CIF 20XXXXXX
integrante_4_nombre: Nombre Apellido (como aparece en Moodle)
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
<!-- ESCRIBE AQUÍ: mínimo 100 palabras, incluye al menos un ejemplo de código con las tablas del caso -->

### GROUP BY y HAVING: diferencia entre filtrar filas y filtrar grupos
<!-- ESCRIBE AQUÍ: mínimo 100 palabras, incluye al menos un ejemplo de código que use WHERE y HAVING en la misma consulta -->

### Investigación más allá de clase: funciones de agregación y valores NULL
<!-- ESCRIBE AQUÍ: mínimo 80 palabras. ¿Qué hacen COUNT(*), COUNT(columna) y AVG cuando hay valores nulos? ¿Qué diferencia hay entre COUNT(*) y COUNT(columna)? Incluye un ejemplo. -->

***

## Bloque 2 — JOINs

### Tipos de JOIN: INNER, LEFT, RIGHT y FULL OUTER
<!-- ESCRIBE AQUÍ: mínimo 120 palabras, incluye al menos un ejemplo de código con las tablas del caso -->

### El producto cartesiano: qué es y cómo evitarlo
<!-- ESCRIBE AQUÍ: mínimo 80 palabras -->

### Investigación más allá de clase: la sintaxis antigua de Oracle con (+) frente a la sintaxis ANSI
<!-- ESCRIBE AQUÍ: mínimo 80 palabras. Muestra la misma consulta escrita de las dos formas y explica por qué hoy se recomienda la sintaxis ANSI. -->

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
<!-- ESCRIBE AQUÍ: mínimo 100 palabras, incluye al menos un ejemplo de CREATE VIEW con las tablas del caso -->

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
