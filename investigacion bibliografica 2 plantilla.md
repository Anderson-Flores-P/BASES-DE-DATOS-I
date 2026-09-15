---
grupo: G0X
tarea: Investigación Bibliográfica 2 - Oracle SQL Developer, Funciones y Control de Flujo
fecha_entrega: 2026-09-20
integrante_1_carnet: CIF 2026011705
integrante_1_nombre: ANDERSON STEVEN FLORES PEREZ
integrante_2_carnet: CIF 2026011056
integrante_2_nombre: CAMILA NICOLE SALVADOR SAMAYOA
integrante_3_carnet: CIF 2026011586
integrante_3_nombre: JOSE ALFREDO RODRIGUEZ MONGE
integrante_4_carnet: CIF 2026011377
integrante_4_nombre: KELLY RODRIGUEZ ALVARADO 
integrante_5_carnet: CIF 2026011595
integrante_5_nombre: PERLA ESMERALDA LÓPEZ BARRIENTOS
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
Las funciones de una sola fila (Single-Row Functions) en Oracle SQL son herramientas predefinidas que operan sobre un único valor de entrada por cada fila de una tabla y devuelven exactamente un solo resultado procesado por cada fila consultada. Estas funciones pueden ser utilizadas en las cláusulas SELECT, WHERE y ORDER BY. Se dividen en varias categorías principales según el tipo de dato que manipulan: funciones de caracteres para transformar y formatear texto, funciones numéricas para realizar cálculos matemáticos y redondeos, funciones de fecha para la manipulación e ingeniería de tiempos, funciones de conversión para transformar tipos de datos explícitamente, y funciones generales para la gestión de valores nulos y lógica condicional.

### Funciones de cadena (UPPER, LOWER, SUBSTR, TRIM, LENGTH, entre otras)
Las funciones de cadena permiten manipular y transformar valores de texto almacenados en la base de datos de manera flexible. `UPPER` convierte todos los caracteres de un texto a mayúsculas, mientras que `LOWER` los transforma a minúsculas, siendo fundamentales para estandarizar búsquedas sin importar cómo se ingresaron los datos. La función `SUBSTR` extrae una porción específica de una cadena indicando la posición inicial y la cantidad de caracteres deseados. Por su parte, `TRIM` elimina los espacios en blanco sobrantes al inicio y al final de una cadena de texto para evitar inconsistencias de formato. Finalmente, `LENGTH` calcula y devuelve el número total de caracteres presentes en una cadena.

```sql
-- Ejemplo de funciones de cadena
SELECT 
    nombre,
    UPPER(nombre) AS nombre_mayus,
    LOWER(nombre) AS nombre_minus,
    SUBSTR(nombre, 1, 3) AS tres_primeras_letras,
    TRIM(nombre) AS nombre_sin_espacios,
    LENGTH(TRIM(nombre)) AS cantidad_caracteres
FROM 
    alumnos1;
```
### Funciones numéricas (ROUND, TRUNC, MOD, entre otras)
Las funciones numéricas ejecutan operaciones matemáticas avanzadas sobre datos de tipo NUMBER y devuelven valores numéricos procesados. La función `ROUND` redondea un número al entero más cercano o al número de decimales especificado según las reglas matemáticas estándar. A diferencia del redondeo, la función `TRUNC` corta o trunca un número a una cantidad determinada de decimales sin aproximar el valor final. Por último, la función `MOD` calcula y devuelve el residuo o resto resultante de una división entera entre dos números, siendo ideal para identificar valores pares, impares o secuencias cíclicas en consultas de bases de datos.

```sql
-- Ejemplo de funciones numéricas
SELECT 
    salario,
    ROUND(salario, 1) AS salario_redondeado,
    TRUNC(salario, 1) AS salario_truncado,
    MOD(id_empleado, 2) AS es_par_o_impar
FROM 
    empleados;

***

## Bloque 3 — Funciones de Fecha

### SYSDATE y operaciones con fechas (MONTHS_BETWEEN, ADD_MONTHS)
Oracle Database incorpora funciones especializadas para trabajar con fechas, lo que permite calcular antigüedades, vencimientos, períodos de tiempo y fechas futuras directamente desde una consulta SQL. La función SYSDATE devuelve la fecha y la hora actuales del sistema operativo donde se ejecuta el servidor de la base de datos, y su resultado es de tipo DATE.
Oracle también permite realizar operaciones aritméticas con fechas. Por ejemplo, sumar un número a una fecha equivale a agregar esa cantidad de días. La función MONTHS_BETWEEN(fecha1, fecha2) calcula la cantidad de meses transcurridos entre dos fechas: si la primera fecha es posterior a la segunda, el resultado es positivo; cuando las fechas no coinciden en el mismo día del mes, el resultado puede incluir una parte decimal. Por su parte, ADD_MONTHS(fecha, cantidad) suma o resta una cantidad determinada de meses y devuelve un valor de tipo DATE.
Estas funciones resultan útiles para determinar cuánto tiempo lleva registrado un alumno, calcular la antigüedad de un empleado o establecer fechas futuras de pago, renovación o vencimiento (Oracle, 2021).

### TO_CHAR y funciones de conversión de tipos de dato
Las funciones de conversión permiten transformar un valor de un tipo de dato a otro dentro de una sentencia SQL. Entre las más utilizadas en Oracle se encuentran TO_CHAR, TO_DATE y TO_NUMBER.
TO_CHAR convierte fechas o números en cadenas de caracteres y permite aplicar modelos de formato. Por ejemplo, una fecha puede mostrarse como DD/MM/YYYY sin modificar el valor DATE almacenado en la base de datos. TO_DATE realiza la operación inversa para las fechas: interpreta una cadena de texto y la convierte en un valor DATE de acuerdo con el formato indicado. TO_NUMBER convierte cadenas de caracteres compatibles en valores numéricos de tipo NUMBER.
El uso de conversiones explícitas es importante cuando los datos provienen de formularios, archivos o sistemas externos, porque evita depender de formatos implícitos que pueden cambiar según la configuración regional de la sesión. Los modelos de formato indican cómo Oracle debe interpretar o presentar el dato, pero no modifican su representación interna en la base de datos (Oracle, 2021).

```sql
SELECT
    TO_CHAR(SYSDATE, 'DD/MM/YYYY HH24:MI:SS') AS fecha_actual,
    TO_DATE('20/09/2026', 'DD/MM/YYYY') AS fecha_entrega,
    TO_NUMBER('125.50', '999D99',
              'NLS_NUMERIC_CHARACTERS = ''.,''') AS cantidad
FROM dual;

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
