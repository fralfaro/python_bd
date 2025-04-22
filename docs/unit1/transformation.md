
# Transformaciones y Acciones 

<img src="../images/transformation.png" alt="Banner del Curso" width="700" >


## Introducción

Uno de los conceptos más importantes al trabajar con PySpark es la separación entre **transformaciones** y **acciones**. Entender esta diferencia es clave para aprovechar correctamente el modelo de ejecución diferida (lazy evaluation) y escribir código eficiente.

En este capítulo, aprenderás qué operaciones se consideran transformaciones, cuáles son acciones, y cómo interactúan para construir y ejecutar un trabajo en Spark.

---

## ¿Qué son las transformaciones?

Las **transformaciones** definen un nuevo conjunto de datos a partir de uno existente. No se ejecutan inmediatamente, sino que se registran en un plan de ejecución (DAG) que Spark optimizará y ejecutará más adelante, cuando se invoque una acción.

Transformaciones comunes:

| Transformación     | Descripción                                       |
|--------------------|---------------------------------------------------|
| `select()`         | Selecciona columnas                               |
| `filter()`         | Filtra filas según una condición                  |
| `withColumn()`     | Añade o transforma una columna                    |
| `drop()`           | Elimina una o más columnas                        |
| `groupBy()`        | Agrupa filas para aplicar agregaciones            |
| `join()`           | Realiza combinaciones entre DataFrames            |
| `distinct()`       | Elimina duplicados                                |
| `orderBy()`        | Ordena las filas por una o más columnas           |

Ejemplo de transformación:

```python
df_filtrado = df.filter(df["monto"] > 1000)
```

---

## ¿Qué son las acciones?

Las **acciones** disparan la ejecución del plan de transformaciones. Cuando se llama a una acción, Spark evalúa todas las transformaciones anteriores, construye un plan físico y ejecuta el trabajo en el clúster.

Acciones comunes:

| Acción             | Descripción                                       |
|--------------------|---------------------------------------------------|
| `show()`           | Muestra las primeras filas                        |
| `collect()`        | Devuelve todos los resultados al driver (con cuidado) |
| `count()`          | Cuenta el número de filas                         |
| `first()`          | Retorna la primera fila                           |
| `take(n)`          | Devuelve las primeras `n` filas                   |
| `write()`          | Escribe los datos en disco                        |
| `foreach()`        | Aplica una función a cada fila                    |

Ejemplo de acción:

```python
df_filtrado.show()
```

---

## Ejemplo completo

```python
from pyspark.sql import SparkSession
from pyspark.sql.functions import col

spark = SparkSession.builder.appName("TransformacionesAcciones").getOrCreate()

df = spark.read.csv("data/ventas.csv", header=True, inferSchema=True)

# Transformaciones encadenadas
df_transformado = df.filter(col("monto") > 1000).select("cliente", "monto")

# Acción que dispara la ejecución
df_transformado.show()
```

---

## Lazy Evaluation (Evaluación Diferida)

Gracias a la separación entre transformaciones y acciones, Spark puede:

- **Optimizar** el plan de ejecución.
- **Evitar** cálculos intermedios innecesarios.
- **Combinar** operaciones en etapas más eficientes.

Esto es una gran ventaja frente a otras librerías que ejecutan cada paso inmediatamente.

---

## Buenas prácticas

- Encadena múltiples transformaciones antes de llamar a una acción.
- Usa `show()` en lugar de `collect()` para evitar traer grandes volúmenes de datos a la memoria local.
- Evita múltiples acciones en bucles: cada acción vuelve a ejecutar el plan completo.
- Usa `explain()` para ver cómo Spark planea ejecutar tus transformaciones.

---



## Referencias útiles

- [Transformations and Actions - Spark Docs](https://spark.apache.org/docs/latest/rdd-programming-guide.html#transformations)
- [Lazy Evaluation Explained](https://spark.apache.org/docs/latest/sql-programming-guide.html#performance-tuning)
- [API Reference: DataFrame](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql.html)

---


## Conclusión

La separación entre transformaciones y acciones es central en el modelo de ejecución de Spark. Esta arquitectura permite diferir la ejecución, aplicar optimizaciones automáticas y escalar el procesamiento a grandes volúmenes de datos.

Dominar esta idea te permitirá escribir código más eficiente y evitar errores comunes al trabajar con PySpark.

