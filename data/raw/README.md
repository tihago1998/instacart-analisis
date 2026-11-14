# Datos crudos

Los cinco CSV de Instacart no se publican en GitHub por su tamaño. El proyecto se ejecuta en Google Colab y lee los archivos desde Google Drive:

```
MyDrive/instacart-datasets/
├── instacart_orders.csv
├── order_products.csv
├── products.csv
├── aisles.csv
└── departments.csv
```

Todos usan `;` como separador: `pd.read_csv(ruta + "archivo.csv", sep=";")`.

Para ejecutar en local, copia los cinco archivos a esta carpeta (`data/raw/`).
