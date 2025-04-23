
# Lazy Mode 

<img src="../images/lazy.png" alt="Banner del Curso" width="600" >


### Introducción

Polars ofrece dos modos de ejecución: **eager** (inmediato) y **lazy** (evaluación diferida). Mientras que en el modo eager cada operación se ejecuta inmediatamente, en el modo lazy las operaciones se registran y se ejecutan solo cuando es necesario, permitiendo aplicar optimizaciones automáticas.

Este enfoque es similar al que usan motores como Apache Spark y permite obtener mejores tiempos de ejecución y menor consumo de memoria, especialmente en pipelines complejos.

---

## ¿Qué es el modo lazy?

El modo lazy permite construir un **plan de ejecución** completo que luego es optimizado por Polars antes de ejecutarse. Esto incluye:

- Eliminación de pasos innecesarios.
- Reordenamiento de operaciones para mejorar eficiencia.
- Ejecución en paralelo en múltiples núcleos.

Es ideal cuando necesitas encadenar múltiples transformaciones, leer archivos grandes o automatizar procesos de preprocesamiento.

---

## Crear un LazyFrame

```python
import polars as pl

lf = pl.read_csv("data/ventas.csv").lazy()
```

Esto no ejecuta la lectura del archivo todavía. Solo crea un objeto `LazyFrame` que contiene el plan.

---

## Encadenar transformaciones

```python
lf = (
    pl.read_csv("data/ventas.csv").lazy()
    .filter(pl.col("monto") > 1000)
    .with_columns([
        (pl.col("monto") * 1.19).alias("monto_con_iva")
    ])
    .select(["cliente", "monto_con_iva"])
)
```

Aún no se ha leído ni procesado el archivo. El plan se ejecuta **cuando se llama a `.collect()`**:

```python
df = lf.collect()
```

---

## Visualizar el plan de ejecución

```python
lf.explain()
```

Esto muestra el plan lógico que Polars construyó. Es muy útil para entender cómo se optimiza el flujo.

---

## Optimización automática

El motor de Polars aplicará optimizaciones como:

- **Proyección empujada:** eliminar columnas innecesarias.
- **Filtrado empujado:** aplicar filtros lo antes posible.
- **Combinación de pasos:** agrupar operaciones para evitar materializaciones intermedias.

Estas optimizaciones son completamente automáticas y no requieren intervención del usuario.

---

## Comparación Eager vs Lazy

```python
# Eager
df = pl.read_csv("data/ventas.csv")
df = df.filter(pl.col("monto") > 1000)
df = df.select(["cliente", "monto"])

# Lazy
lf = (
    pl.read_csv("data/ventas.csv").lazy()
    .filter(pl.col("monto") > 1000)
    .select(["cliente", "monto"])
)
df = lf.collect()
```

En modo eager, cada paso se ejecuta de inmediato. En modo lazy, se construye el pipeline completo antes de ejecutar.

---

## Recomendaciones para usar Lazy

- Lectura y transformación de archivos grandes (CSV, Parquet, etc.).
- Preprocesamiento en pipelines de ML.
- Proyectos en producción donde la eficiencia es clave.
- Validación o limpieza de datos en pasos encadenados.

---

## Buenas prácticas

- Siempre finaliza con `.collect()` para materializar el resultado.
- Usa `.explain()` si tienes dudas sobre la eficiencia de tu pipeline.
- Mantén la lógica declarativa y evita operaciones fuera del pipeline.
- Combina `with_columns`, `select`, `filter`, y `groupby` para expresar todo dentro del flujo lazy.


---

## Referencias útiles

- [Documentación oficial del modo Lazy](https://pola-rs.github.io/polars/py-polars/html/reference/lazy.html)
- [Explicación de optimizaciones automáticas](https://pola.rs/book/user-guide/lazy.html)
- [Ejemplos avanzados de LazyFrame](https://pola.rs/book/user-guide/expressions/lazy_examples.html)

---

## Conclusión

El modo lazy de Polars permite construir pipelines declarativos y eficientes, optimizados automáticamente antes de ejecutarse. Esta característica es especialmente valiosa al trabajar con grandes volúmenes de datos o al desarrollar flujos de procesamiento reutilizables y escalables.

Dominar el modo lazy es clave para aprovechar todo el potencial de Polars como herramienta de ingeniería de datos de alto rendimiento.
