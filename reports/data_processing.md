# Reporte de procesamiento – Instacart

**Evidencia:** GA1-220501094-01-AA1-EV06 – Limpieza, análisis y estructuración de datos con Pandas
**Autor:** Santiago Varela Peña
**Notebook asociado:** [`notebooks/pandas_labs.ipynb`](../notebooks/pandas_labs.ipynb)

**Pregunta orientadora:** ¿Cómo preparar y estructurar los datos de Instacart para obtener información confiable sobre los hábitos de compra?

Todas las cifras de este reporte provienen de las salidas del notebook (pandas 3.1.1 con pyarrow).

## 1. Limpieza y estandarización (Lección 2)

### Decisiones de limpieza

| Problema | Evidencia | Decisión |
|---|---|---|
| Pedidos duplicados en `orders` | 15 pedidos repetidos (30 filas), todos el miércoles a las 2 a. m.: fallo del sistema | **Eliminar** con `drop_duplicates` → 478 952 filas, `order_id` único |
| Tipos de dato más grandes de lo necesario | Identificadores y horas leídos como `int64`/`float64` | **Corregir** con `astype` (`int8`, `int16`, `int32`, `Int16`) |
| 28 817 nulos en `days_since_prior_order` | Coinciden exactamente con los pedidos con `order_number == 1` | **Conservar**: no existe pedido anterior |
| 1 258 productos sin nombre | Todos en el pasillo 100 y el departamento 21 (`missing`) | **Corregir** con `'Unknown'` |
| Texto sin estandarizar en `product_name` | 403 nombres con espacios dobles y 16 con espacio invisible (`\xa0`) | **Corregir** con `.str.replace`, `.str.strip` y `.str.title` → 6 321 nombres cambiaron |
| Duplicados implícitos de nombre | 141 nombres repetidos; 268 de los 280 productos implicados aparecen en pedidos | **Conservar y documentar**: se agrupa por `product_id` |
| 836 nulos en `add_to_cart_order` | Ocurren en 70 pedidos de al menos 65 productos; la posición máxima registrada es 64 | **Conservar**: el sistema no guardó posiciones mayores a 64 |

### Informe del pipeline de limpieza

| Tabla | Paso | Filas | Nulos | Memoria (MB) |
|---|---|---|---|---|
| orders | 0 crudo | 478 967 | 28 819 | 23,0 |
| orders | 1 sin duplicados | 478 952 | 28 817 | 23,0 |
| orders | 2 tipos | 478 952 | 28 817 | 7,2 |
| products | 0 crudo | 49 694 | 1 258 | 3,1 |
| products | 1 nombres | 49 694 | 0 | 3,1 |
| order_prods | 0 crudo | 4 545 007 | 836 | 145,4 |
| order_prods | 1 tipos | 4 545 007 | 836 | 54,5 |

La función `validar` confirma las reglas de calidad (`order_id` único, horas entre 0 y 23, productos con nombre, sin espacios invisibles, `reordered` solo 0 y 1), y el pipeline produce exactamente las mismas tablas que la limpieza manual.

Otros hallazgos del Reto 2: `order_prods` no tiene filas ni pares `order_id` + `product_id` repetidos; `aisles` y `departments` no necesitan limpieza de texto; los productos `'Unknown'` aparecen en 9 547 artículos de 8 254 pedidos y se conservan.

## 2. Análisis de hábitos de compra (Lección 3)

| Hallazgo | Cifra |
|---|---|
| Hora pico de pedidos | 10 a. m. (40 578 pedidos); lunes a las 10 es la celda más alta (7 881) |
| Día con más pedidos | Domingo (84 090 pedidos, 57 661 clientes) |
| Pedidos a los 7 días exactos | 44 577 (9,31 % del total) |
| Clientes con 10 pedidos o más | 6 327 de 157 437 |
| Mediana de espera: fin de semana vs. resto | 8 días vs. 7 días |
| Recompra por posición en el carrito | 0,679 en la posición 1 → 0,617 en la 5 |

### Perfil por segmento de cliente

| Segmento | Clientes | Pedidos | Espera mediana | % compras en la mañana |
|---|---|---|---|---|
| Ocasional (1 pedido) | 55 357 (35,2 %) | 55 357 | 16 días | 30,0 % |
| Regular (2 a 4) | 71 553 (45,4 %) | 191 649 | 11 días | 31,6 % |
| Frecuente (5 o más) | 30 527 (19,4 %) | 231 946 | 6 días | 35,2 % |

El 19,4 % de los clientes (frecuentes) hace casi la mitad de los pedidos y compra más en la mañana: una campaña de fidelización debería llegar en esa franja.

## 3. Combinación de tablas (Lección 4)

- La auditoría con `indicator=True` muestra 450 046 pedidos con productos y **28 906 pedidos sin productos registrados** (6 %).
- La **tabla maestra** une las cinco tablas a partir de `order_prods` con `how='left'` y `validate='m:1'`: 4 545 007 filas y 14 columnas, sin crear ni perder filas.
- Producto más pedido: Banana (66 050 veces, 33 274 clientes, tasa de recompra 0,844).
- Departamentos con más artículos: produce (1 328 302; tasa 0,651), dairy eggs (759 842; 0,671) y snacks (404 850; 0,574).
- Entre los 739 productos con al menos 1 000 pedidos, hay 18 líderes de recompra por departamento; el primero es Whole Organic Omega 3 Milk (0,863).
- Pasillos con mayor recompra: milk (0,781), water seltzer sparkling water (0,731) y fresh fruits (0,718); la menor es spices seasonings (0,152).

## 4. Archivos generados (`data/processed/`)

| Archivo | Contenido | En GitHub |
|---|---|---|
| `instacart_maestra.parquet` | Tabla maestra (4 545 007 × 14) | No: pesa 103,6 MB y supera el límite de 100 MB de GitHub. Se regenera ejecutando el notebook |
| `resumen_departamentos.csv` | Artículos y tasa de recompra por departamento | Sí |
| `ficha_productos.parquet` | Una fila por producto (45 573 × 7) | Sí |
| `lideres_por_departamento.csv` | Producto con mayor recompra de cada departamento (18 filas) | Sí |

## 5. Conclusión

Los datos de Instacart quedan limpios, validados y combinados en una tabla maestra confiable: se eliminaron solo los registros que eran errores (15 pedidos duplicados), se conservaron los nulos que tienen significado y cada unión se verificó con `validate` e `indicator`. Con esa base se identificó que la compra es habitual y predecible (hora pico a las 10 a. m., pico de recompra a los 7 días, 59 % de recompras) y que un grupo pequeño de clientes frecuentes concentra casi la mitad de los pedidos.
