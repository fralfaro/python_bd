
# Transformaciones (expresiones) 

<img src="../images/expresiones.svg" alt="Banner del Curso" width="600" >


## Introducción

Polars utiliza un modelo expresivo basado en operaciones sobre columnas, en lugar de manipulación imperativa de datos. Este enfoque facilita la escritura de pipelines de transformación más seguros, legibles y rápidos.

En este capítulo aprenderás a utilizar expresiones (`pl.col`, `pl.when`, `pl.lit`, etc.) para transformar columnas, condicionar valores, agregar múltiples columnas y componer operaciones complejas de manera eficiente.

---

## ¿Qué es una expresión en Polars?

Una **expresión** es una instrucción que describe cómo transformar o computar una columna. Las expresiones son evaluadas dentro de métodos como:

- `select()`
- `with_columns()`
- `filter()`
- `groupby().agg()`

Esto permite separar el **qué** queremos hacer del **cuándo** se ejecuta (especialmente útil en modo lazy).

---

## Seleccionar columnas con `pl.col`

```python
import polars as pl

df = pl.DataFrame({
    "nombre": ["Ana", "Luis", "Pedro"],
    "edad": [28, 34, 25],
    "monto": [1000, 2500, 1800]
})

df.select([
    pl.col("nombre"),
    pl.col("monto") * 1.19
])
```

---

## Crear nuevas columnas con `with_columns`

```python
df = df.with_columns([
    (pl.col("monto") * 1.19).alias("monto_con_iva"),
    (pl.col("edad") + 5).alias("edad_futura")
])
```

---

## Condiciones con `pl.when`

```python
df = df.with_columns([
    pl.when(pl.col("monto") > 2000)
      .then("alta")
      .otherwise("media")
      .alias("categoria_compra")
])
```

También puedes anidar condiciones:

```python
df.with_columns([
    pl.when(pl.col("monto") > 2000).then("alta")
     .when(pl.col("monto") > 1000).then("media")
     .otherwise("baja")
     .alias("rango")
])
```

---

## Literales y funciones con `pl.lit`

```python
df.with_columns([
    (pl.col("edad") + pl.lit(1)).alias("edad_incrementada")
])
```

---

## Uso de `pl.all()` y `pl.exclude()`

```python
# Aplicar una transformación a todas las columnas numéricas
df.select([
    pl.exclude("nombre") * 2
])
```

---

## Agregaciones con expresiones

```python
df.groupby("categoria_compra").agg([
    pl.col("monto").mean().alias("promedio_monto"),
    pl.col("monto").max().alias("mayor_compra")
])
```

---

## Encadenamiento de operaciones

```python
df = (
    df.with_columns([
        (pl.col("monto") * 1.19).alias("con_iva")
    ])
    .filter(pl.col("edad") > 30)
    .select(["nombre", "con_iva"])
)
```

---

## Buenas prácticas

- Nombra tus columnas nuevas con `.alias()` para evitar sobreescribir datos accidentalmente.
- Encadena expresiones en lugar de escribir múltiples pasos intermedios.
- Usa `pl.when` para lógica condicional clara, especialmente en reemplazo de `.apply()` imperativos.
- Revisa los tipos de columnas con `df.schema` para evitar errores de tipo.


---

## Referencias útiles

- [Expresiones en Polars](https://pola-rs.github.io/polars/py-polars/html/reference/expressions.html)
- [API de `pl.col`, `pl.when`, `pl.lit`](https://pola-rs.github.io/polars/py-polars/html/reference/api/expressions.html)
- [Ejemplos del modelo expresivo](https://pola.rs/book/user-guide/expressions.html)

---

## Conclusión

El modelo basado en expresiones es una de las principales ventajas de Polars. Permite construir pipelines de transformación claros, eficientes y fácilmente optimizables. Este enfoque se vuelve aún más poderoso en el contexto del modo lazy, que veremos en el próximo capítulo.
