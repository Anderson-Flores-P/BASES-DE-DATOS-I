# TAREA: Consultas SQL — Integrante 3

**Estudiante:** Perla López  
**Asignación:** Consulta 5 (Guía 6) + Consulta 6 (Guía 7)

---

## Consulta 5 — Guía 6: Funciones numéricas y de fecha

### Enunciado

Mostrar los alumnos con más de 36 meses desde su fecha de ingreso, indicando su promedio original, el promedio truncado y redondeado a un decimal, la fecha de ingreso con formato `DD/MM/YYYY` y la antigüedad calculada en meses.

### Consulta SQL

```sql
SELECT
    id_alumno,
    INITCAP(TRIM(nombre)) AS nombre,
    promedio,
    TRUNC(promedio, 1) AS promedio_truncado,
    ROUND(promedio, 1) AS promedio_redondeado,
    TO_CHAR(fecha_ingreso, 'DD/MM/YYYY') AS fecha_ingreso,
    ROUND(MONTHS_BETWEEN(SYSDATE, fecha_ingreso), 1) AS meses_antiguedad
FROM alumnos
WHERE MONTHS_BETWEEN(SYSDATE, fecha_ingreso) > 36
ORDER BY meses_antiguedad DESC;
```

### Explicación

Esta consulta utiliza funciones numéricas y de fecha vistas en la Guía 6.

- `TRUNC(promedio, 1)` trunca el promedio a un decimal sin redondearlo.
- `ROUND(promedio, 1)` redondea el promedio a un decimal.
- `TO_CHAR(fecha_ingreso, 'DD/MM/YYYY')` muestra la fecha de ingreso en un formato más legible.
- `SYSDATE` obtiene la fecha actual del servidor.
- `MONTHS_BETWEEN` calcula los meses transcurridos entre la fecha actual y la fecha de ingreso.
- El `WHERE` muestra únicamente los alumnos con más de 36 meses de antigüedad.

> **Nota:** El valor de `meses_antiguedad` puede cambiar según el día en que se ejecute la consulta, porque `SYSDATE` utiliza la fecha actual del servidor.

---

## Consulta 6 — Guía 7: Lógica con DECODE y CASE

### Enunciado

Mostrar los alumnos con su promedio, clasificarlos según su rendimiento académico y determinar si poseen o no un número telefónico registrado.

### Consulta SQL

```sql
SELECT
    id_alumno,
    INITCAP(TRIM(nombre)) AS nombre,
    promedio,
    CASE
        WHEN promedio >= 9 THEN 'Excelente'
        WHEN promedio >= 7 THEN 'Bueno'
        ELSE 'Regular'
    END AS categoria,
    DECODE(
        telefono,
        NULL, 'Sin telefono',
        'Con telefono'
    ) AS estado_telefono
FROM alumnos
ORDER BY promedio DESC;
```

### Explicación

La consulta utiliza `CASE` y `DECODE`, herramientas de control de flujo vistas en la Guía 7.

`CASE` clasifica a los alumnos según su promedio:

- Promedio mayor o igual a 9 → `Excelente`
- Promedio mayor o igual a 7 → `Bueno`
- Promedio menor a 7 → `Regular`

`DECODE` verifica el valor del campo `telefono`:

- Si es `NULL` → `Sin telefono`
- Si contiene un valor → `Con telefono`

Se utiliza `CASE` para trabajar con rangos de valores y `DECODE` para una comparación directa.
