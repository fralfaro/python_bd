# DataFrames en PySpark

<img src="../images/dataframe.png" alt="Banner del Curso" width="600" >


## Introducción

Los `DataFrames` son una de las estructuras fundamentales para trabajar con datos en PySpark. Inspirados en los `DataFrames` de Pandas y R, ofrecen una forma eficiente y expresiva de manipular grandes volúmenes de datos distribuidos.

En este capítulo, aprenderás cómo crear, explorar y transformar DataFrames usando PySpark.

---

## ¿Qué es un DataFrame en PySpark?

Un `DataFrame` en PySpark es una colección distribuida de datos organizados en columnas, con un esquema explícito (nombres y tipos de datos). Internamente, está optimizado para ejecución distribuida en clústeres, soporta ejecución diferida (*lazy evaluation*) y se integra con SQL.

---

## Crear un DataFrame a partir de un archivo CSV

La forma más común de iniciar un análisis es cargar un archivo desde disco:

```python
df = spark.read.csv("data/ventas.csv", header=True, inferSchema=True)
```

Parámetros comunes:

- `header=True`: toma la primera fila como nombres de columnas.
- `inferSchema=True`: detecta automáticamente los tipos de datos.

---

## Crear un DataFrame manualmente

También puedes crear un DataFrame desde una lista de tuplas y una lista de nombres de columnas:

```python
datos = [("Ana", 25), ("Luis", 30), ("Pedro", 28)]
columnas = ["nombre", "edad"]

df = spark.createDataFrame(datos, columnas)
df.show()
```

---

## Explorar el contenido del DataFrame

Algunas funciones útiles para inspeccionar tus datos:

```python
df.show(5)         # Muestra las primeras 5 filas
df.printSchema()   # Muestra el esquema (nombres y tipos de columnas)
df.columns         # Lista de nombres de columnas
df.describe().show()  # Estadísticas descriptivas
```

---

## Seleccionar columnas y filtrar filas

```python
# Seleccionar una o varias columnas
df.select("nombre").show()
df.select("nombre", "edad").show()

# Filtrar filas
df.filter(df["edad"] > 27).show()
df.where(df["nombre"] == "Ana").show()
```

---

## Agregaciones y funciones de grupo

```python
df.groupBy("edad").count().show()
df.agg({"edad": "avg"}).show()
```

---

## Ordenar y renombrar columnas

```python
df.orderBy("edad", ascending=False).show()
df = df.withColumnRenamed("edad", "edad_años")
```

---

## Añadir columnas derivadas

Puedes crear nuevas columnas a partir de otras usando expresiones:

```python
from pyspark.sql.functions import col

df = df.withColumn("edad_doble", col("edad") * 2)
df.show()
```

---

## Eliminar columnas

```python
df = df.drop("edad_doble")
```

---

## Uniones entre DataFrames

```python
df1 = spark.createDataFrame([("Ana", 1), ("Luis", 2)], ["nombre", "id"])
df2 = spark.createDataFrame([(1, "Chile"), (2, "Perú")], ["id", "pais"])

df_join = df1.join(df2, on="id", how="inner")
df_join.show()
```

Tipos de join disponibles: `inner`, `left`, `right`, `outer`.

---

## Conversión a Pandas (solo si es pequeño)

```python
df_pandas = df.toPandas()
```

> Úsalo solo si el conjunto de datos es pequeño y cabe en memoria.


---

## Referencias útiles

- [PySpark DataFrame API](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql.html)
- [Spark SQL Guide](https://spark.apache.org/docs/latest/sql-programming-guide.html)
- [Transformations vs Actions](https://spark.apache.org/docs/latest/rdd-programming-guide.html#transformations)

---

## Conclusión

Los DataFrames en PySpark son una herramienta poderosa y expresiva para procesar datos distribuidos. Dominarlos es clave para trabajar de forma eficiente con grandes volúmenes de información. A diferencia de Pandas, los DataFrames de PySpark están diseñados para escalar horizontalmente, permitiendo análisis que superan las capacidades de una sola máquina.

En el siguiente capítulo aprenderás a aplicar **transformaciones y acciones**, que son los bloques fundamentales del procesamiento en Spark.
