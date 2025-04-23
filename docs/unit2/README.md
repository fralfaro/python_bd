# ⚡ Análisis de Datos con Polars

La Unidad 2 te introduce al procesamiento eficiente de datos tabulares utilizando **Polars**, una librería moderna desarrollada en Rust y diseñada para ofrecer **altísimo rendimiento** con una sintaxis declarativa en Python. Aprenderás a trabajar tanto en modo inmediato (eager) como en modo diferido (lazy), a transformar datos mediante expresiones, y a estructurar pipelines analíticos limpios y escalables.

Esta unidad es clave para comprender cómo trabajar con grandes volúmenes de datos sin necesidad de clústeres distribuidos, aprovechando al máximo la capacidad de cómputo local.

---

## 🎯 Objetivos de la unidad

- Comprender la filosofía y arquitectura de Polars.
- Cargar y transformar datos usando la API expresiva de Polars.
- Utilizar expresiones para crear nuevas columnas y condicionar valores.
- Aplicar agregaciones y agrupaciones avanzadas sobre datos tabulares.
- Optimizar flujos de trabajo con el modo lazy.
- Comparar Polars con herramientas tradicionales como Pandas.

---

## 📚 Contenidos

| Tema | Descripción breve |
|------|--------------------|
| ⚙️ **Introducción a Polars** | Qué es Polars, por qué fue creado y en qué contextos se recomienda usar. |
| 📊 **Operaciones Básicas** | Carga de datos, selección, filtros y funciones exploratorias. |
| 🧱 **Transformaciones con Expresiones** | Uso de `pl.col`, `pl.when`, `pl.lit`, y cómo construir pipelines limpios. |
| 📈 **Agrupaciones y Agregaciones** | Agrupaciones simples y múltiples, funciones condicionales y personalizadas. |
| 🧠 **Lazy Mode** | Construcción de pipelines diferidos y visualización del plan de ejecución. |

---

## 🧠 Recomendación

Te sugerimos comenzar por la introducción conceptual para familiarizarte con las diferencias entre Polars y Pandas. Luego avanza gradualmente por los capítulos técnicos, experimentando en tus propios notebooks con los ejemplos entregados.

Para aprovechar completamente el modo lazy, asegúrate de revisar `lazy_mode.md` después de entender bien cómo funcionan las expresiones en modo eager.

---

## 📚 Prerrequisitos

- Conocimientos básicos de Python y estructuras tipo `DataFrame`
- Familiaridad con operaciones como filtrado, agrupación y transformación de columnas
- Entorno con Python 3.9+ y Polars instalado (`pip install polars`)

---

## ✅ ¿Qué lograrás al finalizar esta unidad?

- Manipular datos de forma eficiente con Polars en modo eager y lazy.
- Escribir pipelines de análisis de datos expresivos y optimizados.
- Utilizar expresiones declarativas en lugar de código imperativo.
- Comparar el rendimiento entre Polars y otras librerías como Pandas.
