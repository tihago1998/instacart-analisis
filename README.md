# Análisis de hábitos de compra – Instacart

Proyecto de práctica de ciencia de datos de **Santiago Varela Peña** (SENA, Tecnología en Análisis y Desarrollo de Software).

## Descripción

Instacart registra pedidos, productos, pasillos, departamentos y los artículos de cada pedido. Este proyecto usa Python y Pandas para explorar esos datos, diagnosticar su calidad y prepararlos para entender los hábitos de compra de los clientes.

**Pregunta orientadora:** ¿Cómo preparar y estructurar los datos de Instacart para obtener información confiable sobre los hábitos de compra?

## Datos

Cinco archivos CSV separados por `;`:

| Archivo | Contenido | Filas | Columnas |
|---|---|---|---|
| `instacart_orders.csv` | Un registro por pedido: cliente, día, hora y días desde el pedido anterior | 478.967 | 6 |
| `order_products.csv` | Productos de cada pedido, posición en el carrito y si es recompra | 4.545.007 | 4 |
| `products.csv` | Catálogo de productos con su pasillo y departamento | 49.694 | 4 |
| `aisles.csv` | Nombres de los pasillos | 134 | 2 |
| `departments.csv` | Nombres de los departamentos | 21 | 2 |

Los CSV **no se suben a GitHub** (`order_products.csv` supera el tamaño recomendado). Ver [`data/raw/README.md`](data/raw/README.md).

## Estructura

```
instacart-analisis/
├── README.md
├── .gitignore
├── requirements.txt
├── data/
│   ├── raw/                              # CSV originales (no se publican)
│   └── processed/                        # archivos generados por pandas_labs.ipynb
│       ├── instacart_maestra.parquet     # tabla maestra (no se publica: 103,6 MB)
│       ├── resumen_departamentos.csv
│       ├── ficha_productos.parquet
│       └── lideres_por_departamento.csv
├── notebooks/
│   ├── data_quality.ipynb                # EV04: exploración y calidad inicial
│   └── pandas_labs.ipynb                 # EV06: lecciones 2, 3 y 4
└── reports/
    ├── data_quality.md                   # EV04: reporte de calidad
    └── data_processing.md                # EV06: reporte de procesamiento
```

## Contenido de `pandas_labs.ipynb` (EV06)

Las lecciones se ejecutan en orden en un solo cuaderno, porque cada una usa las variables de la anterior.

| Lección | Explicación | Laboratorio guiado | Reto |
|---|---|---|---|
| 2. Limpieza y estandarización | Duplicados, tipos, nulos, texto y duplicados implícitos | Pipeline de limpieza con informe de cambios | 7 ejercicios |
| 3. Filtrado, ordenamiento y agrupación | Máscaras, `query`, `groupby`, `agg`, `pivot_table`, `transform` | Perfil de hábitos de compra | 7 ejercicios |
| 4. Combinación de datasets | `merge`, `validate`, `indicator`, `concat`, `melt` y guardado | Tabla de análisis por producto | 7 ejercicios |

Resultados principales en [`reports/data_processing.md`](reports/data_processing.md).

## Cómo ejecutar

El cuaderno detecta el entorno: en Google Colab lee los datos de Drive y en local los lee de `data/raw/`.

**En Google Colab**

1. Sube los cinco CSV a Google Drive en la carpeta `MyDrive/instacart-datasets/`.
2. Abre el notebook en Colab: **Archivo → Abrir cuaderno → GitHub**, busca `tihago1998/instacart-analisis` y elige `notebooks/pandas_labs.ipynb`.
3. Ejecuta todas las celdas en orden (**Entorno de ejecución → Ejecutar todas**) y autoriza el acceso a Drive.
4. Los archivos procesados se guardan en `MyDrive/instacart-datasets/processed/`.

**En local (Jupyter o VSCode)**

```bash
python -m venv venv
venv\Scripts\activate            # Windows
pip install -r requirements.txt
# copiar los cinco CSV a data/raw/
cd notebooks
jupyter nbconvert --to notebook --execute --inplace pandas_labs.ipynb
```

Los archivos procesados se guardan en `data/processed/`. Se requiere `pyarrow` para los archivos Parquet.

## Ramas

| Rama | Uso |
|---|---|
| `main` | Versión estable |
| `develop` | Integración de las actividades |
| `feature/pandas-data-processing` | EV06: lecciones 2, 3 y 4 |

## Evidencias

| Evidencia | Contenido |
|---|---|
| GA1-220501094-01-AA1-EV04 | Exploración de datos de Instacart: `notebooks/data_quality.ipynb` y `reports/data_quality.md` |
| GA1-220501094-01-AA1-EV06 | Limpieza, análisis y estructuración con Pandas: `notebooks/pandas_labs.ipynb`, `data/processed/` y `reports/data_processing.md` |
