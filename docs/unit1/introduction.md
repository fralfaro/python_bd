#  Introducción a PySpark

<img src="../images/pyspark.png" alt="Banner del Curso" width="500" >


## Introducción

Apache Spark es uno de los motores de procesamiento más potentes y utilizados en el ecosistema Big Data. Su capacidad para procesar grandes volúmenes de datos de manera distribuida, rápida y en memoria lo convierte en una herramienta clave en la ingeniería de datos moderna.

En este capítulo, exploraremos qué es PySpark, por qué es relevante en proyectos de datos a gran escala y cómo se posiciona frente a otras herramientas tradicionales como Pandas.

---

## ¿Qué es Apache Spark?

Apache Spark es un motor de análisis de datos distribuido de código abierto, diseñado para procesar grandes volúmenes de datos de manera paralela y eficiente.

Fue creado en la Universidad de California, Berkeley, como una evolución del modelo MapReduce de Hadoop. A diferencia de este último, Spark permite procesamiento en memoria (in-memory), lo que reduce significativamente los tiempos de ejecución.

Spark permite trabajar con distintos tipos de procesamiento:

- **Batch** (por lotes)
- **Streaming** (en tiempo real)
- **Machine Learning**
- **SQL**
- **Gráficos** (GraphX)

---

## ¿Qué es PySpark?

**PySpark** es la interfaz de Python para Apache Spark. Gracias a PySpark, los desarrolladores Python pueden escribir código para trabajar con Spark sin necesidad de utilizar Scala o Java.

Entre sus capacidades más destacadas están:

- Trabajar con DataFrames distribuidos (similar a Pandas, pero escalable).
- Ejecutar operaciones SQL con `Spark SQL`.
- Procesar datos en tiempo real con `Structured Streaming`.
- Realizar tareas de machine learning con `MLlib`.

> 🔧 PySpark traduce las instrucciones escritas en Python al núcleo de ejecución de Spark, lo que permite trabajar a gran escala sin perder la flexibilidad de Python.

---

## ¿Por qué usar PySpark en lugar de Pandas?

Aunque **Pandas** es excelente para análisis de datos en memoria, presenta limitaciones cuando:

- Los datos superan los **8-16GB** (el tamaño de la RAM).
- Se requiere procesamiento paralelo o distribuido.
- Se necesita conectar múltiples fuentes de datos grandes.

| Característica     | Pandas               | PySpark                      |
|--------------------|----------------------|------------------------------|
| Tamaño de datos    | En memoria (limitado)| Escalable a clúster         |
| Paralelismo        | No                   | Sí, distribuido              |
| Tipado             | Dinámico             | Estático (inferido)          |
| Tiempo de carga    | Rápido en pequeños   | Optimizado para grandes      |
| Lenguaje base      | Python puro          | Python sobre Spark (Scala)   |

---

## Casos de uso típicos para PySpark

- Procesamiento de logs de servidores o aplicaciones.
- Limpieza y transformación de datasets de terabytes.
- Preparación de datos para modelos de machine learning distribuidos.
- Análisis de comportamiento de usuarios en tiempo real.
- Consultas complejas sobre archivos Parquet, ORC, CSV distribuidos.

---

## Primer vistazo al código PySpark

Este es un ejemplo básico para iniciar una sesión de Spark y leer un archivo:

```python
from pyspark.sql import SparkSession

# Crear una SparkSession
spark = SparkSession.builder.appName("MiPrimerPySpark").getOrCreate()

# Leer un CSV (puede ser local o desde Google Drive)
df = spark.read.csv("data/ventas.csv", header=True, inferSchema=True)

# Mostrar las primeras filas
df.show()
```

> 🧪 Puedes probar este ejemplo fácilmente en Google Colab con `!pip install pyspark`.

---

## Ventajas principales de PySpark

- **Escalabilidad horizontal:** puedes escalar tu trabajo desde tu laptop hasta cientos de nodos en un clúster.
- **Lenguaje conocido:** no necesitas aprender Java o Scala.
- **Ecosistema rico:** acceso a Spark SQL, MLlib, GraphFrames, entre otros.
- **Interoperabilidad:** puede conectarse con Hadoop, Hive, Kafka, Cassandra, Delta Lake, etc.


---

## Referencias útiles

- [Apache Spark - Sitio oficial](https://spark.apache.org/)
- [Documentación de PySpark](https://spark.apache.org/docs/latest/api/python/)
- [Comparativa PySpark vs Pandas (Databricks)](https://www.databricks.com/glossary/pyspark-vs-pandas)
- [Guía de instalación en Colab (pyspark)](https://colab.research.google.com/github/databricks/koalas/blob/master/docs/source/user_guide/10min.ipynb)

---

## Conclusión

PySpark es una herramienta fundamental en la ingeniería de datos moderna. Su combinación de potencia, escalabilidad y compatibilidad con Python lo convierten en una opción natural para procesar y analizar grandes volúmenes de datos.

Dominar PySpark abre la puerta a trabajar con proyectos de Big Data reales, donde Pandas ya no es suficiente. En los próximos capítulos, aprenderás a construir DataFrames, ejecutar transformaciones distribuidas y optimizar tus consultas como un profesional.
