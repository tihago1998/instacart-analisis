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
│   ├── raw/                 # CSV originales (no se publican)
│   └── processed/           # archivos generados por pandas_labs.ipynb
├── notebooks/
│   ├── data_quality.ipynb   # EV04: exploración y calidad inicial
│   └── pandas_labs.ipynb    # EV06: limpieza, análisis y combinación
└── reports/
    └── data_quality.md      # EV04: reporte de calidad
```

## Cómo ejecutar (Google Colab)

1. Sube los cinco CSV a Google Drive en la carpeta `MyDrive/instacart-datasets/`.
2. Abre el notebook en Colab: **Archivo → Abrir cuaderno → GitHub**, busca `tihago1998/instacart-analisis` y elige el notebook.
3. Ejecuta la primera celda para montar Google Drive y autoriza el acceso.
4. Ejecuta todas las celdas en orden (**Entorno de ejecución → Ejecutar todas**).

Para ejecutarlo en local: `pip install -r requirements.txt`, copia los CSV a `data/raw/` y cambia la variable `ruta` del notebook a `../data/raw/`.

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
