
# Agrupaciones y Agregaciones 

<img src="../images/groupby.svg" alt="Banner del Curso" width="600" >

## Introducción

Una de las operaciones más comunes en análisis de datos es **agrupar por una o más variables** y luego aplicar funciones de agregación, como sumas, promedios o conteos. En Polars, esto se realiza utilizando los métodos `groupby()` y `agg()` sobre `DataFrame` o `LazyFrame`.

En este capítulo profundizaremos en cómo usar estas funciones de forma eficiente y expresiva, incluyendo múltiples agregaciones, alias, condiciones y expresiones personalizadas.

---

## Estructura básica

```python
df.groupby("columna").agg([
    pl.col("otra_columna").sum()
])
```

Esto agrupa por `"columna"` y suma los valores de `"otra_columna"`.

---

## Ejemplo práctico

```python
import polars as pl

df = pl.DataFrame({
    "cliente": ["Ana", "Luis", "Ana", "Luis", "Pedro"],
    "monto": [100, 250, 300, 400, 150],
    "producto": ["A", "B", "A", "C", "B"]
})

df.groupby("cliente").agg([
    pl.col("monto").sum().alias("total"),
    pl.count().alias("compras")
])
```

---

## Agrupaciones múltiples

Puedes agrupar por más de una columna:

```python
df.groupby(["cliente", "producto"]).agg([
    pl.col("monto").mean().alias("promedio_monto")
])
```

---

## Varias agregaciones sobre la misma columna

```python
df.groupby("cliente").agg([
    pl.col("monto").sum(),
    pl.col("monto").mean(),
    pl.col("monto").max()
])
```

---

## Agregaciones condicionales

Puedes usar expresiones condicionales dentro de una agregación:

```python
df.groupby("cliente").agg([
    (pl.when(pl.col("monto") > 200)
       .then(1)
       .otherwise(0)
    ).sum().alias("compras_altas")
])
```

Esto cuenta cuántas compras superaron los 200 por cliente.

---

## Agrupación sobre múltiples columnas dinámicamente

Con `pl.all()` puedes aplicar una operación a todas las columnas (excepto las agrupadas):

```python
df.groupby("producto").agg([
    pl.all().sum()
])
```

También puedes excluir columnas:

```python
df.groupby("producto").agg([
    pl.exclude("cliente").mean()
])
```

---

## Agregaciones con funciones personalizadas

Puedes aplicar funciones definidas por ti:

```python
def rango(col):
    return col.max() - col.min()

df.groupby("cliente").agg([
    rango(pl.col("monto")).alias("rango_montos")
])
```

---

## Uso en LazyFrame

La sintaxis es la misma, pero recuerda finalizar con `.collect()`:

```python
df.lazy().groupby("cliente").agg([
    pl.col("monto").sum()
]).collect()
```

---

## Buenas prácticas

- Usa `.alias()` para nombrar las columnas agregadas de forma clara.
- Evita calcular varias veces la misma expresión (reutiliza con `alias()`).
- Prefiere agrupar por múltiples columnas si existen relaciones jerárquicas (cliente + producto).
- En modo `lazy`, agrupar primero puede mejorar la eficiencia si reduces el volumen de datos temprano.


---

## Referencias útiles

- [Polars API: groupby](https://pola-rs.github.io/polars/py-polars/html/reference/api/polars.DataFrame.groupby.html)
- [Agregaciones disponibles](https://pola-rs.github.io/polars/py-polars/html/reference/expressions.html)
- [Guía oficial de agregaciones](https://pola.rs/book/user-guide/grouping.html)

---

## Conclusión

Las operaciones de agrupación y agregación en Polars son extremadamente expresivas y eficientes, tanto en modo eager como lazy. Su integración con expresiones permite construir análisis complejos con un código claro y conciso.

Dominar `groupby()` + `agg()` es fundamental para todo trabajo de análisis exploratorio, reporting y preparación de datos para machine learning.
