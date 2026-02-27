# 📡 TelecomX LATAM – Análisis de Evasión de Clientes (Churn Analysis)

[![Python](https://img.shields.io/badge/Python-3.8%2B-3776AB?style=flat&logo=python&logoColor=white)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-2.x-150458?style=flat&logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![Seaborn](https://img.shields.io/badge/Seaborn-0.13%2B-4C9BE8?style=flat)](https://seaborn.pydata.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=flat&logo=jupyter&logoColor=white)](https://jupyter.org/)
[![Alura Latam](https://img.shields.io/badge/Alura-ONE%20Data%20Science-0077B5?style=flat)](https://www.aluracursos.com/)

---

## 📌 Descripción del Problema de Negocio

**TelecomX** es una empresa de telecomunicaciones que opera en América Latina y enfrenta un índice de **evasión de clientes (Churn) de ~26%**, por encima del promedio de la industria.

Cada cliente perdido representa no solo ingresos recurrentes mensuales sino también el costo de reemplazarlo, que puede ser **5× mayor** al costo de retenerlo. El objetivo de este proyecto es identificar los factores clave que llevan a un cliente a abandonar la empresa y proponer estrategias de retención basadas en datos.

---

## 🗂️ Estructura del Proyecto

```
telecom_x/
├── TelecomX_LATAM.ipynb       # Notebook principal del análisis
├── TelecomX_Data.json         # Dataset crudo (7,267 registros, JSON anidado)
├── TelecomX_diccionario.md    # Diccionario de datos (metadata)
├── README.md                  # Este archivo
├── correlacion_evasion.png    # Heatmap de correlación (generado)
├── evasion_contrato.png       # Viz 1: Evasión vs Tipo de Contrato (generado)
├── evasion_antiguedad.png     # Viz 2: Evasión vs Antigüedad (generado)
└── evasion_cargos.png         # Viz 3: Evasión vs Cargos Mensuales (generado)
```

---

## 🛠️ Tecnologías Utilizadas

| Tecnología | Uso |
|---|---|
| **Python 3.8+** | Lenguaje base del análisis |
| **Pandas** | Manipulación y transformación de datos (`json_normalize`) |
| **NumPy** | Operaciones numéricas vectorizadas |
| **Seaborn** | Visualizaciones estadísticas de alta calidad |
| **Matplotlib** | Personalización de gráficas y exportación |
| **JSON** | Lectura y aplanamiento del dataset anidado |
| **Jupyter Notebook** | Entorno de análisis interactivo |

---

## 🔄 Pipeline ETL (Extracción → Transformación → Carga)

### 1. Extracción
- Carga del archivo JSON con `json.load()`
- Aplanamiento del JSON anidado (4 niveles: `customer`, `phone`, `internet`, `account`) usando **`pd.json_normalize(sep='.')`**
- Resultado: DataFrame de 7,267 filas × 21 columnas

### 2. Transformación

| Paso | Descripción |
|---|---|
| **Renombrado** | 21 columnas traducidas al español desde el diccionario de datos |
| **Limpieza crítica** | `Cargos_Totales` tenía strings vacíos `" "` en clientes nuevos (tenure=0); se reemplazaron por `0.0` antes de la conversión a `float64` |
| **Tipos de dato** | `Ciudadano_Mayor` (int→categórico), `Evasion` (Yes/No→Sí/No), `Antiguedad` (Int64) |
| **Feature 1** | `Cuentas_Diarias = Cargos_Mensuales / 30` |
| **Feature 2** | `Total_Servicios` = suma de servicios opcionales activos por cliente (max: 8) |

### 3. Análisis (EDA)
- Distribución de la variable objetivo (Evasión)
- Estadísticas descriptivas de variables numéricas
- **Matriz de correlación (Heatmap)** para identificar drivers numéricos de churn
- 4 visualizaciones comparativas entre grupos de evasión

---

## 📊 Visualizaciones Generadas

| # | Visualización | Insight Principal |
|---|---|---|
| 1 | **Heatmap de Correlación** | `Antiguedad` es el predictor negativo más fuerte de Evasión (r≈-0.35) |
| 2 | **Evasión vs Tipo de Contrato** | Contratos mes a mes tienen 43% churn vs 3% en contratos bianuales |
| 3 | **Evasión vs Antigüedad** (Histograma + KDE) | El pico de evasión ocurre en los primeros 12 meses |
| 4 | **Evasión vs Cargos Mensuales** (Boxplot + Violin) | Clientes que evaden pagan $74/mes vs $61 los que permanecen |

---

## 💡 Principales Conclusiones (Insights)

1. **El tipo de contrato es el predictor más poderoso de evasión.** Los clientes con contrato mensual (month-to-month) evaden a una tasa de ~43%, comparado con ~3% en contratos bianuales.

2. **Los clientes nuevos (< 12 meses) son el segmento de mayor riesgo.** La evasión se concentra masivamente en el primer año de vida del cliente, indicando una oportunidad clave en el onboarding.

3. **El mayor costo mensual no garantiza fidelidad.** Los clientes que pagan más por mes tienen mayor tendencia a evadir, lo que señala un problema de percepción de valor en los paquetes premium.

4. **Más servicios contratados → mayor lealtad.** Los clientes con 5+ servicios activos presentan tasas de churn significativamente menores, confirmando el efecto ancla (*switching cost*) de los bundles.

5. **La antigüedad acumulada es una barrera natural.** Una vez que un cliente supera los 24 meses, la probabilidad de evasión cae drásticamente.

---

## 🎯 Recomendaciones Estratégicas

1. **Migración a contratos anuales:** Incentivar el cambio de mensual a anual con descuentos del 10-15% en los primeros 6 meses.
2. **Onboarding intensivo:** Programa de check-ins proactivos en meses 1, 3 y 6 para reducir el churn temprano.
3. **Revisión de propuesta de valor premium:** Reposicionar los paquetes de alto costo para justificar la permanencia en el segmento >$65/mes.

---

## 🚀 Cómo Ejecutar el Proyecto

```bash
# 1. Clonar el repositorio
git clone https://github.com/TU_USUARIO/telecomx-latam-churn.git
cd telecomx-latam-churn

# 2. Instalar dependencias
pip install pandas numpy matplotlib seaborn

# 3. Abrir el notebook
jupyter notebook TelecomX_LATAM.ipynb
# O en Google Colab: subir los 3 archivos y ejecutar Kernel → Restart & Run All
```

---

## 🎓 Sobre Este Proyecto

Este proyecto fue desarrollado como parte del **Desafío de Data Science** del programa **[One Oracle Next Education (ONE)](https://www.aluracursos.com/one)**, impartido por **[Alura Latam](https://www.aluracursos.com/)**.

El desafío simula un caso de negocio real en el que se aplica el pipeline completo de ciencia de datos: desde la ingestión de datos en crudo hasta la entrega de insights y recomendaciones estratégicas.

---

*📧 Desarrollado por: [Tu Nombre] | 📅 Febrero 2026*
