# Instrucciones — Consultas SQL Guías 8 a 11

## 1. Formación de grupo

Descarga el archivo `EjerciciosGuias8a11.md` y ábrelo con cualquier editor de texto (Bloc de notas, VS Code, etc.). No cambies el formato del archivo ni la extensión `.md`.

En el encabezado, reemplaza `G0X` por el número real de tu grupo, y en `fecha_entrega` escribe `2026-10-28`.

Completa los 5 campos `integrante_N_carnet` e `integrante_N_nombre` con los datos reales de cada miembro del grupo. Escribe el carnet con el formato `CIF 20XXXXXX` (CIF, un espacio y el número). Escribe el nombre exactamente como aparece en Moodle.

Si el grupo tiene menos de 5 integrantes, borra las líneas que no uses.

## 2. Prepara los datos en Oracle

Antes de resolver cualquier consulta, copia y ejecuta completo (F5) el script de la sección "Datos de referencia" en tu SQL Worksheet de Oracle SQL Developer (tu propio esquema, no un esquema compartido).

Verifica con `SELECT * FROM clientes_ej;`, `SELECT * FROM productos_ej;`, `SELECT * FROM pedidos_ej;`, `SELECT * FROM detalle_ej;` y `SELECT * FROM ventas_raw_ej;` que las cinco tablas se crearon y tienen datos (6, 7, 8, 12 y 6 filas, respectivamente) antes de continuar.

Si aparece `ORA-00955`, es porque ya ejecutaste el script antes: las tablas ya existen y puedes continuar con ellas.

## 3. Resuelve las 8 consultas

Cada consulta tiene un enunciado y un bloque de código con el comentario `-- Escribe aquí tu consulta`. Borra ese comentario y escribe tu sentencia SQL en su lugar, dentro del mismo bloque ` ```sql ... ``` `.

Antes de copiarla al archivo, ejecuta cada consulta en Oracle y confirma que el resultado tiene sentido con el enunciado. Respeta el orden de columnas y el ordenamiento que pide cada enunciado.

Las consultas 7 y 8 llevan varias sentencias en el mismo bloque (`CREATE TABLE`, `INSERT`, `CREATE VIEW`, `SELECT`). Termina cada una con punto y coma.

No agregues explicaciones, comentarios adicionales ni texto fuera del bloque de código. Solo se evaluará la sentencia SQL.

## 4. Ponderación (Rúbrica)

La calificación se distribuye así, y se evalúa por grupo, no individualmente:

| Criterio | Peso |
| --- | --- |
| Seguir indicaciones (formato del archivo, nombre exacto, carnets con formato CIF 20XXXXXX, nombres como en Moodle, sin texto fuera de los bloques de código) | 5% |
| Consulta 1 — Guía 8: Generación de usuarios y enmascarado de datos | 10% |
| Consulta 2 — Guía 8: DECODE y CASE integrados | 10% |
| Consulta 3 — Guía 9: Funciones de agregación con GROUP BY | 10% |
| Consulta 4 — Guía 9: Filtrado de grupos con HAVING | 10% |
| Consulta 5 — Guía 10: INNER JOIN de varias tablas | 15% |
| Consulta 6 — Guía 10: LEFT JOIN combinado con agregación | 10% |
| Consulta 7 — Guía 10: Normalización hasta 3FN (CREATE TABLE con llaves + INSERT ... SELECT) | 15% |
| Consulta 8 — Guía 11: Vistas integradas (CREATE VIEW + consulta sobre la vista) | 15% |
| **Total** | **100%** |

Cada consulta se califica en dos niveles: ejecuta sin errores en Oracle (mitad del puntaje de esa consulta) y produce el resultado correcto según el enunciado (la otra mitad). Una consulta que ejecuta pero da un resultado incorrecto obtiene solo la mitad de su peso.

## 5. Entrega

Fecha límite de entrega: miércoles 28 de octubre de 2026. Guarda el archivo con el mismo nombre `EjerciciosGuias8a11.md` y súbelo en el espacio habilitado en la plataforma antes de esa fecha.

No conviertas el archivo a Word, PDF ni ningún otro formato: se entrega tal cual, en `.md`. Las entregas después del 28 de octubre no se aceptan como trabajos tardíos.
