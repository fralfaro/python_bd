

# Operaciones básicas 


<img src="../images/operations.png" alt="Banner del Curso" width="400" >


## Introducción

Polars proporciona una interfaz rápida y expresiva para trabajar con datos tabulares. En este capítulo aprenderás a cargar datos, explorar columnas, realizar transformaciones comunes y exportar resultados utilizando la API en modo **eager** (evaluación inmediata).

---

## Importar Polars

Antes de comenzar, asegúrate de tener instalada la librería:

```bash
pip install polars
```

Y luego, en tu script o notebook:

```python
import polars as pl
```

---

## Crear un DataFrame desde una lista

```python
df = pl.DataFrame({
    "nombre": ["Ana", "Luis", "Pedro"],
    "edad": [28, 34, 25],
    "ciudad": ["Valparaíso", "Santiago", "La Serena"]
})

print(df)
```

---

## Leer un archivo CSV

```python
df = pl.read_csv("data/ventas.csv")
df.head(5)
```

Parámetros comunes:

- `has_header=True`: usa la primera fila como encabezado (por defecto).
- `infer_schema_length=1000`: filas a usar para inferir tipos.
- `separator=";"`: separador personalizado.

---

## Inspeccionar el DataFrame

```python
df.shape          # (n_filas, n_columnas)
df.columns        # Lista de nombres de columnas
df.schema         # Diccionario con nombre y tipo de cada columna
df.describe()     # Estadísticas básicas
```

---

## Seleccionar y renombrar columnas

```python
df.select(["cliente", "monto"])  # Selección simple

df = df.rename({"monto": "total_compra"})  # Renombrar columnas
```

---

## Filtrar filas

```python
df.filter(pl.col("monto") > 1000)

# Combinar condiciones
df.filter(
    (pl.col("monto") > 1000) & (pl.col("ciudad") == "Santiago")
)
```

---

## Crear nuevas columnas

```python
df = df.with_columns([
    (pl.col("monto") * 1.19).alias("monto_con_iva")
])
```

---

## Agrupar y agregar

```python
df.groupby("cliente").agg([
    pl.col("monto").sum().alias("total"),
    pl.col("monto").mean().alias("promedio")
])
```

---

## Ordenar resultados

```python
df.sort("monto", descending=True)
```

---

## Eliminar columnas

```python
df.drop("columna_innecesaria")
```

---

## Exportar a CSV

```python
df.write_csv("data/output.csv")
```

También puedes exportar a otros formatos como Parquet o JSON:

```python
df.write_parquet("data/output.parquet")
df.write_json("data/output.json")
```

---

## Buenas prácticas

- Usa `pl.Config.set_tbl_rows(n)` para controlar cuántas filas muestra por defecto.
- Evita `print(df)` para datasets grandes: usa `df.head()` o `df.sample(n)`.
- Encadena transformaciones usando `with_columns`, `filter`, `groupby`, etc.
- Considera el modo **lazy** para optimizar pipelines más complejos (lo veremos en el próximo capítulo).


---

## Referencias útiles

- [API de Polars: DataFrame](https://pola-rs.github.io/polars/py-polars/html/reference/api/expressions.html)
- [Guía de lectura y escritura](https://pola-rs.github.io/polars/py-polars/html/reference/io.html)
- [Ejemplos prácticos de uso](https://pola.rs/examples/)

---

## Conclusión

El modo eager de Polars permite ejecutar operaciones comunes de análisis de datos de manera rápida y eficiente. Su sintaxis, basada en expresiones y columnas, ofrece claridad y control, ideal para análisis exploratorios y procesamiento de datos estructurados.

En el próximo capítulo veremos cómo usar el modo **lazy**, que permite optimizar y encadenar múltiples operaciones sin ejecutarlas inmediatamente.
