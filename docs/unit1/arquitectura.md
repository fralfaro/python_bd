
# Arquitectura de Spark

<img src="../images/arquitectura.png" alt="Banner del Curso" width="700" >


## Introducción

Comprender cómo funciona Apache Spark por dentro es clave para aprovechar al máximo su capacidad de procesamiento distribuido. En este capítulo, analizaremos la arquitectura de Spark, sus componentes principales y cómo interactúan durante la ejecución de un trabajo.

---

## ¿Por qué es importante entender la arquitectura?

Spark está diseñado para escalar horizontalmente. Esto significa que puede distribuir tareas en múltiples máquinas o núcleos. Saber cómo está organizado el motor te permitirá:

- Optimizar la ejecución de tus scripts.
- Comprender mejor errores y cuellos de botella.
- Tomar decisiones informadas sobre configuración y recursos.

---

## Componentes principales de Spark

La arquitectura de Spark se basa en un modelo maestro-trabajador (master-worker). A continuación se describen sus elementos clave:

### 1. Driver

- Es el programa principal que ejecutas.
- Contiene tu código PySpark y gestiona la ejecución del trabajo.
- Coordina la comunicación con el resto del clúster.

### 2. Cluster Manager

- Es el orquestador del clúster.
- Se encarga de asignar recursos a la aplicación Spark.
- Puede ser `Standalone`, `YARN`, `Mesos`, `Kubernetes`, etc.

### 3. Executors

- Son procesos que corren en los nodos del clúster.
- Ejecutan las tareas asignadas por el Driver.
- Cada executor tiene su propia memoria y caché.

### 4. Tasks

- Son las unidades más pequeñas de trabajo que ejecutan operaciones sobre particiones de datos.
- Son asignadas por el Driver a los Executors.

---

## Diagrama lógico

```text
   +------------------+
   |      Driver      |
   +--------+---------+
            |
            v
   +------------------+
   | Cluster Manager  |
   +--------+---------+
            |
    +-------+-------+
    |               |
    v               v
+--------+     +--------+
|Executor|     |Executor|
| Task 1 |     | Task 2 |
| Task 3 |     | Task 4 |
+--------+     +--------+
```

---

## ¿Qué es un DAG?

Spark representa cada trabajo como un **DAG** (Directed Acyclic Graph) de transformaciones. Esto permite:

- Planificar la ejecución antes de ejecutarla.
- Optimizar el orden de las operaciones.
- Evitar operaciones innecesarias (como cálculos duplicados).

Un DAG no es más que una estructura de nodos y flechas donde cada nodo representa una operación, y las flechas muestran la dependencia entre ellas.

---

## Modo de ejecución: etapas y tareas

Al ejecutar una acción (como `.count()` o `.collect()`), Spark:

1. Construye un **DAG** a partir de las transformaciones encadenadas.
2. Divide el DAG en **etapas** (stages), donde cada etapa puede ser ejecutada en paralelo.
3. Dentro de cada etapa, genera múltiples **tareas** (tasks), una por cada partición.
4. Envía estas tareas a los **executors**, los cuales las procesan y devuelven los resultados al driver.

---

## Ejemplo de ejecución

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder.appName("EjemploDAG").getOrCreate()
df = spark.read.csv("data/ventas.csv", header=True, inferSchema=True)

df_filtrado = df.filter(df["monto"] > 1000)
df_filtrado.select("cliente", "monto").show()
```

En este ejemplo:

- El DAG se construye con `.filter()` y `.select()`.
- La acción `.show()` dispara la ejecución.
- Spark calcula el plan óptimo, lo divide en etapas y ejecuta en paralelo.


---

## Referencias útiles

- [Apache Spark Overview](https://spark.apache.org/docs/latest/cluster-overview.html)
- [Understanding the DAG in Spark](https://spark.apache.org/docs/latest/rdd-programming-guide.html#rdd-operations)
- [Deploying Spark on Kubernetes](https://spark.apache.org/docs/latest/running-on-kubernetes.html)

---

## Conclusión

La arquitectura de Spark está pensada para escalar y optimizar automáticamente las operaciones sobre grandes volúmenes de datos. Comprender cómo se distribuye el trabajo entre el driver, el cluster manager y los executors te ayudará a escribir código más eficiente y depurar errores de forma más efectiva.

En el siguiente capítulo aprenderás cómo instalar PySpark en tu entorno local o en la nube, y cómo preparar tu entorno de trabajo.
