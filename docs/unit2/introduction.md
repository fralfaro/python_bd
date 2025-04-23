# Introducción a Polars

<img src="../images/polars2.png" alt="Banner del Curso" width="300" >


## Introducción

Polars es una librería moderna para el análisis de datos en Python y Rust. Está diseñada para ser **extremadamente rápida, eficiente en memoria y escalable**, ofreciendo una alternativa moderna a Pandas, especialmente útil cuando se trabaja con datasets grandes o se necesita alto rendimiento.

En este capítulo exploraremos qué es Polars, por qué ha ganado popularidad en la comunidad de ciencia de datos y cómo se compara con otras librerías como Pandas y PySpark.

---

## ¿Qué es Polars?

Polars es una librería de procesamiento de datos columnar construida sobre Rust, con bindings en Python. Soporta un enfoque expresivo basado en expresiones (similar a SQL) y ofrece dos modos de ejecución:

- **Eager (modo inmediato):** estilo similar a Pandas, útil para exploración interactiva.
- **Lazy (modo diferido):** permite construir pipelines de transformación optimizados automáticamente antes de ejecutarse.

Polars es particularmente útil cuando se requiere:

- Rendimiento extremo sin necesidad de un clúster.
- Bajo uso de memoria en comparación con Pandas.
- Escritura concisa y expresiva de transformaciones complejas.

---

## ¿Por qué usar Polars?

Polars resuelve muchas de las limitaciones de Pandas:

| Limitación de Pandas      | Solución en Polars                           |
|---------------------------|----------------------------------------------|
| Alto uso de memoria       | Uso eficiente de estructuras columnar en Rust|
| Poca velocidad en grandes datasets | Motor optimizado, multithreading por defecto     |
| Código imperativo y repetitivo | Transformaciones declarativas con expresiones  |
| Sin ejecución diferida    | Lazy mode para optimización automática        |

---

## Comparación inicial con Pandas

```python
# En Pandas
df[df["columna"] > 100]["otra"] = df["otra"] * 2

# En Polars (modo expresivo)
df = df.with_columns(
    (pl.col("otra") * 2).alias("otra")
).filter(pl.col("columna") > 100)
```

> Aunque más explícito, el modelo de expresiones de Polars evita errores comunes, es más legible en flujos complejos y más eficiente.

---

## Instalación

Puedes instalar Polars fácilmente con pip:

```bash
pip install polars
```

Para usar la versión con soporte de Parquet, Arrow y Lazy:

```bash
pip install polars[lazy]
```

---

## Casos de uso ideales para Polars

- Análisis de datos tabulares de tamaño medio a grande.
- Reemplazo de pipelines lentos en Pandas.
- Preprocesamiento de datos en entornos de despliegue o backend.
- Ejecución local de transformaciones sin necesidad de Spark o clústeres distribuidos.

---

## Limitaciones actuales

- Comunidad más pequeña que Pandas (aunque en crecimiento).
- Algunas funciones estadísticas avanzadas no están disponibles.
- La API puede resultar menos familiar al principio por su estilo funcional y expresivo.


---

## Referencias útiles

- [Sitio oficial de Polars](https://pola.rs/)
- [Documentación en Python](https://pola-rs.github.io/polars/py-polars/html/index.html)
- [Comparación Polars vs Pandas](https://www.pola.rs/faq/why-polars.html)
- [Polars GitHub](https://github.com/pola-rs/polars)

---

## Conclusión

Polars es una excelente alternativa a Pandas cuando necesitas más velocidad, menor uso de memoria o una sintaxis más declarativa y robusta para tus transformaciones. Su diseño en Rust le permite competir con librerías como PySpark en escenarios locales, pero con mucha mayor simplicidad.

En los próximos capítulos exploraremos cómo trabajar con Polars en la práctica: cargando datos, aplicando transformaciones y usando expresiones poderosas para procesar información de forma eficiente.
