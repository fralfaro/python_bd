
#  Spark Streaming

<img src="../images/streaming.png" alt="Banner del Curso" width="600" >


## Introducción

Además del procesamiento por lotes (batch), Apache Spark permite procesar flujos de datos en tiempo real a través de **Structured Streaming**, un modelo que combina la simplicidad de los DataFrames con la potencia del procesamiento continuo.

En este capítulo conocerás los conceptos básicos de Spark Streaming, cómo configurarlo en PySpark, y cómo realizar tus primeras operaciones sobre flujos de datos.

---

## ¿Qué es Structured Streaming?

Structured Streaming es un motor de procesamiento de flujos de datos que permite ejecutar consultas incrementales sobre datos en movimiento usando la misma API que los DataFrames.

A diferencia de los modelos tradicionales basados en micro-batches puros, Structured Streaming ofrece:

- Integración nativa con la API de DataFrames.
- Procesamiento tolerante a fallos.
- Escalabilidad automática en un clúster Spark.
- Soporte para múltiples fuentes: archivos, Kafka, sockets, etc.

---

## Casos de uso comunes

- Monitorización de logs en tiempo real.
- Ingesta de datos de sensores (IoT).
- Análisis en tiempo real de clics o eventos de usuarios.
- Detección de fraudes o anomalías.
- Integración con Kafka para flujos de alto volumen.

---

## Requisitos y limitaciones

Structured Streaming está disponible en Spark 2.0+ y **requiere una fuente de datos compatible con streaming**, como:

- Directorios que reciben archivos nuevos.
- Sockets de red (para pruebas locales).
- Conectores como Kafka, Delta Lake o sockets TCP.

No se puede usar `show()` directamente en un stream. En su lugar, se utilizan "queries" activas que escriben en consola, archivos o memoria.

---

## Primer ejemplo: lectura desde un socket

Este ejemplo muestra cómo leer texto desde un socket TCP y contar palabras en tiempo real:

```python
from pyspark.sql import SparkSession
from pyspark.sql.functions import explode, split

spark = SparkSession.builder.appName("StreamingBasico").getOrCreate()

# Fuente de datos: socket local
lines = spark.readStream.format("socket") \
    .option("host", "localhost") \
    .option("port", 9999) \
    .load()

# Transformación: contar palabras
words = lines.select(explode(split(lines.value, " ")).alias("palabra"))
conteo = words.groupBy("palabra").count()

# Sink: escribir en consola
query = conteo.writeStream \
    .outputMode("complete") \
    .format("console") \
    .start()

query.awaitTermination()
```

Puedes usar `nc -lk 9999` en una terminal para enviar texto y probar el ejemplo.

---

## Modos de salida (`outputMode`)

- `append`: solo nuevas filas.
- `update`: solo filas modificadas.
- `complete`: muestra toda la tabla en cada iteración.

---

## Consideraciones importantes

- El stream se ejecuta de forma continua hasta que se detiene explícitamente.
- Puedes escribir a archivos, memoria, bases de datos o Kafka.
- Spark maneja automáticamente los micro-batches.

---

## Buenas prácticas

- Siempre especifica un `checkpointLocation` si el stream es crítico (para tolerancia a fallos).
- Usa `outputMode="append"` para eficiencia cuando no necesitas toda la tabla.
- Prueba localmente con sockets antes de pasar a Kafka u otros entornos productivos.
- No uses `collect()` ni `toPandas()` en un stream.


---

## Referencias útiles

- [Structured Streaming Overview](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html)
- [API de Structured Streaming](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql.streaming.html)
- [Ejemplos de uso](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html#quick-example)

---

## Conclusión

Structured Streaming permite extender el poder de Spark al procesamiento en tiempo real sin cambiar de paradigma. Su integración con la API de DataFrames facilita el desarrollo de pipelines unificados para datos por lotes y en streaming.

En proyectos reales, es común combinar Spark SQL y Streaming para construir soluciones completas de análisis en tiempo real.
