---
grupo: G0X
tarea: Investigación Bibliográfica 2 - Oracle SQL Developer, Funciones y Control de Flujo
fecha_entrega: 2026-09-20
integrante_1_carnet: CIF 2026011705
integrante_1_nombre: ANDERSON STEVEN FLORES PEREZ
integrante_2_carnet: CIF 2026011056
integrante_2_nombre: CAMILA NICOLE SALVADOR SAMAYOA
integrante_3_carnet: CIF 20XXXXXX
integrante_3_nombre: Nombre Apellido (como aparece en Moodle)
integrante_4_carnet: CIF 2026011377
integrante_4_nombre: KELLY RODRIGUEZ ALVARADO 
integrante_5_carnet: CIF 20XXXXXX
integrante_5_nombre: Nombre Apellido (como aparece en Moodle)
docente: Mgtr. Rafael Torres
---

# Investigación Bibliográfica 2
## Base de Datos I (BAS I) — Semana 5 a Semana 8

***

## Introducción

La presente investigación bibliográfica aborda las herramientas y características fundamentales de Oracle SQL Developer, así como el uso avanzado de funciones de cadena, numéricas, de fecha y de control de flujo. Su estudio es de gran importancia para el curso de Bases de Datos I, ya que permite comprender y dominar los mecanismos esenciales para la gestión eficiente, manipulación y consulta de información en entornos relacionales modernos.

***

## Bloque 1 — Oracle SQL Developer

### ¿Qué es Oracle SQL Developer y para qué sirve?
Oracle SQL Developer es una herramienta de entorno de desarrollo integrado (IDE) gratuita y de código abierto proporcionada por Oracle Corporation, diseñada específicamente para simplificar y optimizar la gestión de bases de datos relacionales. Su función principal es ofrecer a los desarrolladores y administradores una interfaz gráfica intuitiva y completa desde la cual pueden escribir, ejecutar, probar y depurar código SQL y PL/SQL de manera sumamente eficiente. Además, permite administrar usuarios, visualizar objetos de la base de datos, generar informes detallados y realizar migraciones de datos desde otros sistemas gestores de bases de datos hacia Oracle con total seguridad y control.

### Creación de bases de datos y tablas mediante interfaz gráfica
La creación de bases de datos y tablas mediante una interfaz gráfica en Oracle SQL Developer representa una alternativa visual muy práctica frente al uso exclusivo de comandos por consola. A través de su explorador de conexiones, los usuarios pueden gestionar objetos de manera interactiva seleccionando opciones con el ratón. Para crear una tabla, por ejemplo, basta con hacer clic derecho sobre la categoría de tablas, seleccionar la opción de nueva tabla y rellenar un formulario estructurado que solicita el nombre del objeto y el listado de columnas deseadas. En este panel se configuran de forma visual los tipos de datos correspondientes (como NUMBER, VARCHAR2 o DATE), las restricciones de nulidad (NOT NULL) y la asignación de llaves primarias o foráneas. Esta metodología gráfica minimiza considerablemente los errores de sintaxis comunes en la programación manual y agiliza el diseño estructural inicial de cualquier proyecto de bases de datos.

***

## Bloque 2 — Funciones de Cadena y Numéricas

### Funciones de una sola fila: concepto y categorías
<!-- ESCRIBE AQUÍ: mínimo 80 palabras -->

### Funciones de cadena (UPPER, LOWER, SUBSTR, TRIM, LENGTH, entre otras)
<!-- ESCRIBE AQUÍ: mínimo 100 palabras, incluye al menos un ejemplo de código -->

### Funciones numéricas (ROUND, TRUNC, MOD, entre otras)
<!-- ESCRIBE AQUÍ: mínimo 100 palabras, incluye al menos un ejemplo de código -->

***

## Bloque 3 — Funciones de Fecha

### SYSDATE y operaciones con fechas (MONTHS_BETWEEN, ADD_MONTHS)
<!-- ESCRIBE AQUÍ: mínimo 100 palabras -->

### TO_CHAR y funciones de conversión de tipos de dato
<!-- ESCRIBE AQUÍ: mínimo 100 palabras, incluye al menos un ejemplo de código -->

***

## Bloque 4 — Transformación de Datos y Control de Flujo

### Funciones combinadas de transformación (REPLACE, SUBSTR aplicados a casos reales)
Las funciones REPLACE y SUBSTR permiten trabajar y transformar textos dentro de una consulta SQL sin cambiar los datos originales. REPLACE sirve para buscar una parte de un texto y sustituirla por otra, mientras que SUBSTR permite extraer una cantidad determinada de caracteres desde una posición específica. Estas funciones pueden ser útiles en situaciones reales, como quitar guiones de un número telefónico, cambiar caracteres o extraer una parte de un código o correo electrónico. Al combinarlas, se pueden preparar los datos para mostrarlos de una forma más ordenada y fácil de entender.

### DECODE y CASE: diferencias y casos de uso
DECODE y CASE se utilizan en Oracle SQL para mostrar diferentes resultados dependiendo de una condición. DECODE permite comparar un valor con varias opciones y devolver un resultado según la opción que coincida. Por ejemplo, si una columna indica si un alumno está becado con "Sí" o "No", DECODE puede cambiar esos valores por textos más claros. CASE funciona de una forma más flexible, ya que permite utilizar condiciones como mayor, menor o igual. Por ejemplo, se puede utilizar para clasificar el promedio de un alumno como "Excelente", "Aprobado" o "Reprobado". Una diferencia importante es que DECODE es más sencillo cuando se comparan valores específicos, mientras que CASE resulta más útil cuando se necesitan varias condiciones. Ambas opciones ayudan a presentar la información de una consulta de una manera más clara para el usuario.

```sql
SELECT
    nombre,
    DECODE(becado,
           'Sí', 'Con beca activa',
           'No', 'Sin beca') AS estado_beca,
    CASE
        WHEN promedio >= 9 THEN 'Excelente'
        WHEN promedio >= 7 THEN 'Aprobado'
        ELSE 'Reprobado'
    END AS categoria_rendimiento
FROM alumnos1;
```

***

## Conclusiones

<!-- ESCRIBE AQUÍ: mínimo un párrafo por integrante, o una conclusión grupal de al menos 150 palabras -->

***

## Bibliografía

<!-- Mínimo 4 fuentes en formato APA 7.ª edición, numeradas. -->

1.
2.
3.
4.
