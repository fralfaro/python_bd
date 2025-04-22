
# RDD en PySpark

<img src="../images/rdd.png" alt="Banner del Curso" width="400" >


# Introducción 

Antes de la introducción de los DataFrames, Spark se basaba en una estructura fundamental llamada **RDD** (Resilient Distributed Dataset). Aunque su uso ha disminuido en aplicaciones modernas, los RDDs siguen siendo relevantes para operaciones de bajo nivel y control más explícito sobre la ejecución distribuida.

En este capítulo exploraremos qué son los RDDs, cómo se crean, cuándo son útiles y cómo se comparan con las APIs más modernas de PySpark.

---

## ¿Qué es un RDD?

Un RDD es una colección inmutable y distribuida de objetos que puede ser procesada en paralelo. Cada partición del RDD puede residir en un nodo distinto del clúster.

Características clave:

- **Distribuido:** fragmentado automáticamente en múltiples nodos.
- **Inmutable:** cualquier transformación crea un nuevo RDD.
- **Perecedero:** se puede volver a calcular desde el origen si un nodo falla.
- **Tolerante a fallos:** gracias al modelo de linaje (DAG).

---

## ¿Cuándo usar RDD?

Aunque los DataFrames y Datasets son preferidos por simplicidad y rendimiento, los RDDs son útiles cuando:

- Necesitas control fino sobre las particiones o el flujo de datos.
- Trabajas con datos no estructurados o transformaciones personalizadas.
- Implementas algoritmos complejos que no encajan bien en SQL o DataFrames.

---

## Crear un RDD en PySpark

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder.appName("RDDDemo").getOrCreate()
sc = spark.sparkContext  # Acceso al contexto de Spark (nivel bajo)

# Crear un RDD desde una lista
rdd = sc.parallelize([1, 2, 3, 4, 5])

# Ver los elementos
print(rdd.collect())
```

---

## Transformaciones y acciones en RDD

Las operaciones en RDD siguen el mismo modelo de **transformación + acción** que los DataFrames.

### Transformaciones comunes:

- `map()`
- `filter()`
- `flatMap()`
- `union()`
- `distinct()`

### Acciones comunes:

- `collect()`
- `count()`
- `first()`
- `reduce()`
- `take(n)`

Ejemplo:

```python
pares = rdd.filter(lambda x: x % 2 == 0)
cuadrados = pares.map(lambda x: x * x)
print(cuadrados.collect())  # [4, 16]
```

---

## RDD vs DataFrame

| Característica         | RDD                         | DataFrame                    |
|------------------------|-----------------------------|------------------------------|
| Nivel de abstracción   | Bajo                        | Alto                         |
| Tipo de datos          | Objetos Python genéricos    | Tablas con columnas          |
| Optimización automática| No                          | Sí (Catalyst Optimizer)      |
| Tipado                 | No estructurado             | Estructurado con schema      |
| Rendimiento            | Más bajo                    | Más alto                     |
| Flexibilidad           | Alta                        | Limitada a operaciones tabulares |

---

## Casos donde un RDD aún es útil

- Procesamiento de logs no estructurados.
- Algoritmos personalizados (como PageRank).
- Cuando necesitas granularidad sobre cómo se manejan las particiones.
- Operaciones que requieren acceso a nivel de fila completo sin esquema.

---

## Conversión entre RDD y DataFrame

Puedes convertir un RDD a DataFrame si defines el esquema:

```python
from pyspark.sql import Row

rdd = sc.parallelize([Row(nombre="Ana", edad=30), Row(nombre="Luis", edad=25)])
df = spark.createDataFrame(rdd)
df.show()
```

Y también puedes ir de DataFrame a RDD:

```python
df_rdd = df.rdd
```


---

## Referencias útiles

- [RDD Programming Guide (Apache Spark)](https://spark.apache.org/docs/latest/rdd-programming-guide.html)
- [Transformations and Actions in RDD](https://spark.apache.org/docs/latest/rdd-programming-guide.html#transformations)
- [PySpark RDD API](https://spark.apache.org/docs/latest/api/python/reference/pyspark.rdd.html)

---

## Conclusión

Los RDDs representan la base original de Spark y siguen siendo útiles para tareas donde se necesita control detallado sobre los datos o cuando las estructuras tabulares no son suficientes. Sin embargo, para la mayoría de los casos prácticos, especialmente en proyectos empresariales, se recomienda usar DataFrames por simplicidad y eficiencia.

Conocer RDD te ayuda a entender mejor cómo Spark opera internamente y te prepara para resolver casos avanzados o no convencionales.
