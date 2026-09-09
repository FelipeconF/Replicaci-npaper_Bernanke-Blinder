# Replicaciónpaper_Bernanke-Blinder
eplicación econométrica en Stata del artículo de Bernanke &amp; Blinder (1992) . Analiza la transmisión de choques en la tasa de fondos federales sobre la actividad económica, los precios y el crédito bancario mediante un VAR Estructural (SVAR) con identificación de Cholesky  y datos de FRED para el Seminario de Macroeconometría del ITAM
# Replicación y Extensión de Bernanke & Blinder (1992) 
---

##  Descripción del Proyecto

Este repositorio contiene el código econométrico en **Stata**, los datos macroeconómicos y la documentación formal para la **replicación y extensión** del artículo seminal de **Ben S. Bernanke y Alan S. Blinder (1992)**: *"The Federal Funds Rate and the Channels of Monetary Transmission"*, publicado en el *American Economic Review* [1, 7].

El objetivo principal de esta investigación es analizar la transmisión de los shocks de política monetaria a la economía real y al sistema bancario mediante la estimación de un modelo de **Vectores Autorregresivos Estructurales (SVAR)** [2, 8, 9].

---

## 🛠️ Metodología y Marco Econométrico

Siguiendo las mejores prácticas de series de tiempo del seminario:

1. **Especificación del VAR en Niveles:** De acuerdo con la literatura (Stock, 1987; Sims et al., 1990), el VAR se estima en niveles para preservar las relaciones dinámicas de largo plazo y garantizar estimaciones super-consistentes [10].
2. **Estrategia de Identificación (Cholesky):** Se utiliza un esquema de restricciones contemporáneas de corto plazo (matriz triangular inferior) [8, 11, 12]. Se asume que las variables macroeconómicas reales no responden de manera contemporánea (dentro del mismo mes) a los shocks de política monetaria [2, 8, 13]:
   
   \\[\begin{pmatrix} u_t^{\text{Actividad}} \\ u_t^{\text{Precios}} \\ u_t^{\text{Interés}} \end{pmatrix} = \begin{pmatrix} b_{11} & 0 & 0 \\ b_{21} & b_{22} & 0 \\ b_{31} & b_{32} & b_{33} \end{pmatrix} \begin{pmatrix} \varepsilon_t^{\text{Oferta/Demanda}} \\ \varepsilon_t^{\text{Precios}} \\ \varepsilon_t^{\text{Política Monetaria}} \end{pmatrix}\\]

3. **Pruebas de Diagnóstico y Análisis Dinámico:**
   - Criterios de información para rezagos óptimos (`varsoc` - HQIC/AIC) [14, 15].
   - Prueba de estabilidad del VAR por autovalores dentro del círculo unitario (`varstable`) [16, 17].
   - Test de autocorrelación de residuos para verificar el supuesto de ruido blanco (`wntestq`) [15, 18].
   - Funciones de Impulso-Respuesta Estructurales (`oirf`) y Descomposición de Varianza (`fevd`) [19-21].

---

## 📊 Datos y Fuentes

Todas las series provienen de **FRED (Federal Reserve Economic Data)** a frecuencia mensual [22, 23]:

| Variable | Descripción | Serie FRED | Transformación |
| :--- | :--- | :--- | :--- |
| **Y1 (Actividad)** | Tasa de Desempleo / Producción Industrial | `UNRATE` / `INDPRO` | Nivel / \\(\log(\cdot) \times 100\\) [14] |
| **Y2 (Precios)** | Índice de Precios al Consumidor (IPC) | `CPIAUCSL` | \\(\log(\cdot) \times 100\\) [14] |
| **Y3 (Política)** | Tasa de Fondos Federales | `FEDFUNDS` | Nivel (porcentaje) [14] |

---

## 📁 Estructura del Repositorio

```text
├── data/               # Bases de datos procesadas (.dta) y raw de FRED
├── do_files/           # Scripts de Stata (.do)
│   ├── 01_data_prep.do # Limpieza y transformación de series de tiempo
│   ├── 02_var_spec.do  # Pruebas de raíz unitaria, lags y estabilidad
│   └── 03_svar_irf.do  # Estimación del SVAR, OIRFs y FEVD
├── figures/            # Gráficas exportadas de impulsos-respuesta
├── tables/             # Tablas de descomposición de varianza y coeficientes
├── reports/            # Entregables escritos en PDF (Reportes 1 a 4)
├── main.do             # Archivo maestro de ejecución
└── README.md           # Descripción general del repositorio
