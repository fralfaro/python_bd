
# Lazy Evaluation 

<img src="../images/lazy.png" alt="Banner del Curso" width="600" >


## Introducción

Uno de los aspectos más poderosos (y menos intuitivos al principio) de PySpark es su modelo de **evaluación diferida**, conocido como *lazy evaluation*. A diferencia de otras librerías como Pandas, donde cada operación se ejecuta de inmediato, en PySpark las transformaciones no se ejecutan hasta que se llama explícitamente a una acción.

En este capítulo aprenderás qué significa esto, cómo funciona internamente y cómo aprovecharlo para escribir código más eficiente.

---

## ¿Qué es la evaluación diferida?

Cuando encadenas varias transformaciones en un DataFrame de PySpark, **nada se ejecuta en ese momento**. En su lugar, Spark construye internamente un **DAG (grafo acíclico dirigido)** con el plan de ejecución.

Este DAG es evaluado solo cuando llamas a una **acción**, como `show()`, `count()`, `collect()`, entre otras.

Esto permite a Spark aplicar optimizaciones automáticas, como:

- Reordenar operaciones para mejorar el rendimiento.
- Omitir cálculos innecesarios.
- Combinar etapas para reducir movimientos de datos.

---

## Ejemplo: ejecución diferida

```python
from pyspark.sql import SparkSession
from pyspark.sql.functions import col

spark = SparkSession.builder.appName("LazyEvaluationDemo").getOrCreate()

df = spark.read.csv("data/ventas.csv", header=True, inferSchema=True)

# Transformaciones (no ejecutan nada aún)
df_filtrado = df.filter(col("monto") > 1000)
df_seleccion = df_filtrado.select("cliente", "monto")

# Acción que dispara la ejecución
df_seleccion.show()
```

En este ejemplo, la lectura del archivo y las transformaciones no se ejecutan hasta que se llama a `.show()`. En ese momento, Spark analiza el plan, aplica optimizaciones y ejecuta las transformaciones de forma distribuida.

---

## Visualizar el plan de ejecución

Puedes ver el plan que Spark genera con el método `.explain()`:

```python
df_seleccion.explain()
```

Esto muestra el plan lógico y físico, incluyendo las etapas, operadores y particiones. Es útil para depurar y optimizar tu código.

---

## ¿Por qué es útil?

- **Eficiencia:** Spark evita pasos innecesarios y reduce el uso de memoria.
- **Escalabilidad:** El motor distribuye mejor las tareas cuando tiene una visión completa del flujo.
- **Modularidad:** Puedes construir pipelines complejos sin preocuparte por cuándo se ejecuta cada paso.

---

## Comparación con Pandas

| Característica        | Pandas                   | PySpark                    |
|------------------------|--------------------------|----------------------------|
| Ejecución              | Inmediata                | Diferida (lazy)            |
| Optimización automática| No                       | Sí                         |
| Planificación global   | No (paso a paso)         | Sí (plan de ejecución DAG) |
| Escalabilidad          | Limitada (RAM local)     | Distribuida en clúster     |

---

## Buenas prácticas

- **Encadena transformaciones:** Es más eficiente definir varias operaciones seguidas antes de ejecutar una acción.
- **Evita múltiples acciones innecesarias:** Cada acción genera una ejecución completa.
- **Usa `.explain()` para entender cómo Spark ejecutará tu código.**
- **Ten cuidado con `.collect()` o `.toPandas()`**, ya que traerán todos los datos al driver, ejecutando todo el DAG.


---

## Referencias útiles

- [Spark Programming Guide: RDDs and Lazy Evaluation](https://spark.apache.org/docs/latest/rdd-programming-guide.html#rdd-operations)
- [Optimizing Spark with Lazy Evaluation](https://towardsdatascience.com/why-does-spark-use-lazy-evaluation-a6c7f6f889f2)
- [PySpark DataFrame API](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql.html)

---

## Conclusión

La evaluación diferida es uno de los pilares de Spark. Gracias a ella, Spark puede optimizar tus operaciones y ejecutar solo lo necesario, de forma paralela y distribuida. Entender este modelo es esencial para escribir código eficiente, escalar tus análisis y evitar errores de rendimiento.
