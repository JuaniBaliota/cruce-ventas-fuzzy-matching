# 🔍 Cruce de Ventas con Fuzzy Matching + Generador de Reportes en PDF

Proyecto de portfolio en **Python puro**: limpieza y cruce de datos "sucios" (nombres de empresas mal escritos, invertidos, mezclados con números) usando *fuzzy matching*, y generación automática de un reporte en PDF con tablas y gráficos.

## 📌 Contexto y objetivo

Se parte de dos datasets con información de ventas por empresa que, en la vida real, casi nunca llegan perfectamente estandarizados:

- `Ventas.csv`: contiene el registro de ventas por empresa, pero el nombre de la empresa aparece con errores de tipeo, letras reemplazadas por números (`Inn0vatech`), inconsistencias de mayúsculas/minúsculas e incluso strings invertidos (`snoituloS hceT` → *Tech Solutions*).
- `Vendedores.csv`: contiene el nombre "correcto" de cada empresa y el vendedor asignado.

El objetivo es **unir ambos datasets a pesar del ruido en los nombres**, para poder responder: ¿cuánto se vendió por empresa? ¿cuánto vendió cada vendedor? Y entregar esa respuesta en un reporte PDF listo para compartir.

## 🧠 Enfoque técnico

1. **Data cleaning**: normalización básica de texto (minúsculas, espacios).
2. **Fuzzy matching** con la librería [`thefuzz`](https://github.com/seatgeek/thefuzz): para cada nombre de empresa en `Ventas.csv`, se busca la coincidencia más parecida dentro de la lista de nombres "correctos" de `Vendedores.csv`, usando `fuzz.token_sort_ratio` (compara las palabras sin importar el orden, lo que resuelve los casos de nombres invertidos).
3. **Merge (left join)** de ambos datasets usando el nombre corregido como clave.
4. **Agregación**: cálculo del monto total vendido por empresa y por vendedor.
5. **Visualización**: gráficos de barras horizontales con `matplotlib`.
6. **Reporte automático**: generación de un PDF con `fpdf` que incluye título con fecha/hora de generación, tablas de resultados y los gráficos embebidos.

## 🛠️ Stack

- Python 3
- `pandas` — manipulación de datos
- `thefuzz` — fuzzy matching de strings
- `matplotlib` — visualización
- `fpdf` — generación de PDF

## 📂 Estructura del proyecto

```
├── cruce_ventas.py              # Script principal (todo el pipeline)
├── Ventas.csv                   # Dataset original de ventas (con nombres "sucios")
├── Vendedores.csv               # Dataset con nombres correctos y vendedor asignado
├── resultados_cruce.csv         # Salida: ventas ya cruzadas con nombre correcto y vendedor
├── registros_sin_cruce.csv      # Salida: registros que no lograron un match válido
├── ventas_por_empresa.png       # Gráfico: monto vendido por empresa
├── ventas_por_vendedor.png      # Gráfico: monto vendido por vendedor
└── reporte_ventas.pdf           # Reporte final con tablas + gráficos
```

## ▶️ Cómo correrlo

```bash
pip install pandas thefuzz fpdf matplotlib
python cruce_ventas.py
```

El script genera automáticamente los CSV, los PNG y el PDF en el directorio de trabajo definido dentro del script.

## 📊 Resultado

El reporte final (`reporte_ventas.pdf`) incluye:

- Tabla de monto vendido por empresa
- Tabla de monto vendido por vendedor
- Gráfico de barras de monto vendido por empresa
- Gráfico de barras de monto vendido por vendedor

**Empresa con mayor monto vendido:** Tech Solutions ($27,346.17)
**Vendedor con mayor monto vendido:** Vendedor 1 ($27,346.17)

## 🚀 Posibles mejoras a futuro

- Ajustar o exponer el umbral de similitud del fuzzy matching como parámetro configurable, para poder evaluar más fácil su impacto en los registros sin cruce.
- Analizar y tratar los casos de `registros_sin_cruce.csv` para mejorar la tasa de matcheo.
- Convertir el script en una función/CLI reutilizable con distintos datasets de entrada.
