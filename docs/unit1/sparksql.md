
# Consultas con Spark SQL

<img src="../images/sparksql2.png" alt="Banner del Curso" width="400" >


## Introducción

Spark SQL es un módulo de Apache Spark que permite ejecutar consultas SQL sobre DataFrames distribuidos. Combina la familiaridad del lenguaje SQL con el rendimiento de Spark y su capacidad de trabajar con grandes volúmenes de datos de forma paralela.

En este capítulo aprenderás cómo usar SQL en PySpark, cómo registrar vistas temporales y cómo integrar SQL con transformaciones funcionales.

---

## ¿Qué es Spark SQL?

Spark SQL permite:

- Ejecutar consultas SQL estándar sobre DataFrames.
- Combinar operaciones SQL con transformaciones en Python.
- Optimizar automáticamente las consultas mediante el **Catalyst Optimizer**.
- Integrarse con fuentes como Hive, Parquet, JSON, JDBC, etc.

---

## Crear una vista temporal

Para ejecutar SQL sobre un DataFrame, primero debes registrar el DataFrame como una vista:

```python
df = spark.read.csv("data/ventas.csv", header=True, inferSchema=True)

df.createOrReplaceTempView("ventas")
```

Esto crea una **vista temporal en memoria**, similar a una tabla SQL.

---

## Consultar con SQL

Una vez registrada la vista, puedes usar `spark.sql()` para realizar consultas:

```python
resultado = spark.sql("""
    SELECT cliente, monto
    FROM ventas
    WHERE monto > 1000
    ORDER BY monto DESC
""")

resultado.show()
```

---

## Integrar SQL con operaciones funcionales

Puedes combinar fácilmente consultas SQL con transformaciones de PySpark:

```python
ventas_filtradas = spark.sql("SELECT * FROM ventas WHERE monto > 1000")
ventas_filtradas.groupBy("cliente").count().show()
```

---

## Vistas globales

Además de las vistas temporales, puedes usar vistas globales que están disponibles entre sesiones:

```python
df.createGlobalTempView("ventas_global")
spark.sql("SELECT * FROM global_temp.ventas_global").show()
```

---

## Operaciones SQL comunes

| SQL                        | Equivalente PySpark                        |
|----------------------------|---------------------------------------------|
| `SELECT columna`          | `df.select("columna")`                     |
| `WHERE`                   | `df.filter(...)`                           |
| `GROUP BY`                | `df.groupBy(...).agg(...)`                 |
| `ORDER BY`                | `df.orderBy(...)`                          |
| `JOIN`                    | `df1.join(df2, on="columna", how="inner")` |

---

## Cuándo preferir SQL

Spark SQL es útil cuando:

- El equipo tiene experiencia con SQL.
- Las transformaciones son más legibles en SQL.
- Se requiere migrar código SQL existente.

También es común en notebooks o dashboards, donde se escriben muchas consultas rápidas.

---

## Buenas prácticas

- Usa `createOrReplaceTempView()` para mantener nombres consistentes.
- Prefiere SQL cuando las consultas son complejas o se asemejan a lógica relacional.
- Combina SQL con PySpark solo cuando sea necesario para mantener claridad.
- Usa `EXPLAIN` en SQL para ver el plan de ejecución:

```python
spark.sql("SELECT * FROM ventas WHERE monto > 1000").explain()
```


---

## Referencias útiles

- [Spark SQL Documentation](https://spark.apache.org/sql/)
- [Catalyst Optimizer](https://databricks.com/glossary/catalyst-optimizer)
- [Spark SQL API Reference](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql.html)
- [Using SQL with DataFrames](https://spark.apache.org/docs/latest/sql-getting-started.html)

---

## Conclusión

Spark SQL es una herramienta poderosa que permite combinar la expresividad de SQL con el poder de procesamiento distribuido de Spark. Es ideal para consultas complejas, manipulación de grandes datasets y tareas en las que SQL es más natural que la API funcional.

En el próximo capítulo veremos cómo trabajar con datos en **tiempo real** usando Spark Streaming.
