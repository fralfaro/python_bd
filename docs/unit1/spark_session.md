
# Crear una SparkSession

<img src="../images/sparksesion.png" alt="Banner del Curso" width="600" >


## Introducción

Antes de comenzar a trabajar con PySpark, necesitas iniciar una sesión de Spark. Esta sesión es el punto de entrada para acceder a todas las funcionalidades del motor, como leer datos, aplicar transformaciones, ejecutar SQL o hacer streaming.

En este capítulo aprenderás qué es la `SparkSession`, cómo configurarla correctamente y cómo usarla para cargar datos por primera vez.

---

## ¿Qué es una SparkSession?

La `SparkSession` es el objeto principal para interactuar con el motor de Spark. A partir de Spark 2.0, reemplaza a objetos como `SQLContext` y `HiveContext`, y proporciona una interfaz unificada para trabajar con:

- DataFrames
- SQL
- UDFs (funciones definidas por el usuario)
- Streaming
- Configuraciones del entorno

---

## Crear una SparkSession

En la mayoría de los casos, bastará con el siguiente bloque para iniciar tu sesión:

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("MiAplicacion") \
    .getOrCreate()
```

Esto creará una sesión con la configuración por defecto. Si ya existe una sesión activa, `getOrCreate()` la reutilizará.

---

## Parámetros comunes en `builder`

Puedes personalizar tu sesión usando métodos encadenados:

```python
spark = SparkSession.builder \
    .appName("MiProyecto") \
    .master("local[*]") \
    .config("spark.executor.memory", "2g") \
    .getOrCreate()
```

| Parámetro         | Descripción                                      |
|------------------|--------------------------------------------------|
| `.appName()`      | Nombre de la aplicación (aparece en logs y UI). |
| `.master()`       | Modo de ejecución (`local[*]`, `yarn`, etc.).    |
| `.config()`       | Añade configuraciones personalizadas.           |

---

## Verificar que Spark está activo

Una vez creada la sesión, puedes consultar algunas propiedades útiles:

```python
print(spark.version)              # Versión de Spark
print(spark.sparkContext.appName) # Nombre de la aplicación
print(spark.sparkContext.master)  # Modo de ejecución
```

---

## Cargar un archivo CSV

Veamos un ejemplo simple para leer un archivo CSV y mostrar su contenido:

```python
df = spark.read.csv("data/ventas.csv", header=True, inferSchema=True)
df.show(5)
```

- `header=True` indica que la primera fila contiene los nombres de las columnas.
- `inferSchema=True` le dice a Spark que detecte automáticamente los tipos de datos.

---

## Apagar la sesión

Al final del programa o del notebook, puedes cerrar la sesión con:

```python
spark.stop()
```

Esto libera los recursos usados por Spark y evita que se mantenga en memoria innecesariamente.

---

## Buenas prácticas

- Usa `getOrCreate()` para evitar errores si ya existe una sesión activa.
- Evita crear múltiples sesiones en el mismo notebook.
- Configura `master("local[*]")` si estás trabajando en local con múltiples núcleos.
- Nombra bien tu aplicación con `.appName()` para facilitar la depuración.


---

## Referencias útiles

- [API de SparkSession](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.SparkSession.html)
- [Configuración de Spark](https://spark.apache.org/docs/latest/configuration.html)
- [Guía de lectura de archivos](https://spark.apache.org/docs/latest/sql-data-sources-load-save-functions.html)

---

## Conclusión

La `SparkSession` es el primer paso para comenzar a trabajar con PySpark. Saber cómo crearla, configurarla y utilizarla correctamente te permitirá manipular datos distribuidos y comenzar a explorar los componentes clave de Spark.

En el siguiente capítulo, aprenderás cómo trabajar con estructuras fundamentales como los `DataFrames`, una de las herramientas más potentes para manipular datos en PySpark.
