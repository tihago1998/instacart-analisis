# Reporte de calidad inicial – Instacart

**Evidencia:** GA1-220501094-01-AA1-EV04
**Autor:** Santiago Varela Peña
**Notebook asociado:** [`notebooks/data_quality.ipynb`](../notebooks/data_quality.ipynb)

## 1. Dimensiones de los archivos

| Archivo | Filas | Columnas |
|---|---|---|
| instacart_orders.csv | 478.967 | 6 |
| products.csv | 49.694 | 4 |
| aisles.csv | 134 | 2 |
| departments.csv | 21 | 2 |
| order_products.csv | 4.545.007 | 4 |

## 2. Campos que relacionan los conjuntos de datos

- `order_id`: une **orders** con **order_products**.
- `product_id`: une **products** con **order_products**.
- `aisle_id`: une **products** con **aisles**.
- `department_id`: une **products** con **departments**.
- `user_id` solo está en **orders** e identifica al cliente.

## 3. Diagnóstico de calidad

| Archivo | Observaciones |
|---|---|
| instacart_orders.csv | `days_since_prior_order` tiene 28.819 valores nulos, probablemente el primer pedido de cada cliente; por eso es `float64`. Hay 478.952 `order_id` únicos frente a 478.967 filas: 15 registros duplicados. |
| products.csv | `product_name` tiene 1.258 valores nulos. |
| aisles.csv | Sin valores nulos. |
| departments.csv | Sin valores nulos, pero el departamento 21 se llama "missing": productos sin categoría real. |
| order_products.csv | `add_to_cart_order` tiene 836 valores nulos; por eso es `float64` aunque la posición debería ser entera. |

## 4. Indicadores básicos

| Indicador | Valor |
|---|---|
| Usuarios distintos | 157.437 |
| Promedio de `order_number` | 17,16 |
| Promedio de `days_since_prior_order` (sin nulos) | 11,10 días |
| Artículos recomprados (`reordered == 1`) | 2.683.838 (cerca del 59 %) |
| `order_hour_of_day` | media 13,45; mediana 13; mín. 0; máx. 23; Q1 10; Q3 16 |
| `add_to_cart_order` | media 8,35; mediana 6; mín. 1; máx. 64; Q1 3; Q3 11 |

## 5. Limitaciones y acciones de limpieza propuestas

| Problema | Acción propuesta |
|---|---|
| 15 pedidos duplicados en orders | Revisar y eliminar duplicados exactos. |
| 1.258 productos sin nombre | Marcarlos con un valor como "Unknown" para no perder sus compras. |
| Departamento "missing" (id 21) | Tratarlo como categoría desconocida, no como un departamento real. |
| 28.819 nulos en `days_since_prior_order` | Conservarlos: indican el primer pedido del cliente. |
| 836 nulos en `add_to_cart_order` | Revisar los pedidos afectados antes de analizar la posición en el carrito. |
| `order_dow` y `order_hour_of_day` sin etiquetas | Documentar la equivalencia de los códigos (0 = domingo, según la convención del dataset). |

## 6. Conclusión

Los datos permiten describir los hábitos de compra (clientes recurrentes, compra concentrada entre las 10 y las 16 horas, 59 % de recompras), pero deben limpiarse antes de un análisis más profundo: los nulos, los duplicados y los productos sin nombre o departamento pueden alterar los resultados.
