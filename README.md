# 📊 Modelización de Datos Multidimensionales — Casos Prácticos

> Proyecto final de la asignatura **Modelización de Datos Multidimensionales** · Grado en Ingeniería Informática · Universidad de Murcia (UMU), 4.º curso.

## 👥 Autores

| Nombre | GitHub |
|--------|--------|
| **Juan Francisco Manzanares** | [@juanfranciscomanzanares](https://github.com/juanfranciscomanzanares) |
| **Pablo** | — |

---

## 📝 Descripción

Este trabajo aborda dos casos prácticos de análisis y simulación en finanzas cuantitativas, aplicando conceptos clave de la asignatura: **cópulas**, **movimiento browniano fraccional** y **volatilidad estocástica**.

Los datos utilizados son los rendimientos logarítmicos diarios de cuatro grandes empresas tecnológicas de la bolsa estadounidense: **Amazon (AMZN)**, **Apple (AAPL)**, **NVIDIA (NVDA)** y **Google (GOOG)**, descargados de Yahoo Finance para el periodo 2020–2025.

---

## 🔬 Caso Práctico 1 — Modelización mediante Cópulas-Vine

### Objetivo

Modelar la **estructura de dependencia conjunta** entre las 4 acciones usando cópulas vine, un enfoque más flexible que la correlación lineal clásica.

### Metodología

1. **Selección de datos**: rendimientos logarítmicos diarios (log-returns) para garantizar estacionariedad.
2. **Ajuste de marginales**: se descartó la distribución normal y se ajustó una **t-Student** a cada serie, confirmando colas pesadas (leptocurtosis) con grados de libertad entre 3.4 y 4.9.
3. **Pseudo-observaciones**: transformación empírica al intervalo [0, 1] con `pobs()`.
4. **Análisis de dependencia**: gráfico de pares y **Tau de Kendall** (valores entre 0.385 y 0.483).
5. **Ajuste del modelo vine**: selección automática con criterio AIC → **estructura D-vine** (AIC = −2905.67).

### Hallazgos clave

| Par | Cópula seleccionada | Interpretación |
|-----|---------------------|----------------|
| GOOG – AMZN | t-Student (df = 4.78) | Fuerte dependencia de cola simétrica: caen y suben juntas en extremos |
| GOOG – AAPL | t-Student (df = 3.86) | Dependencia de cola aún más intensa |
| AMZN – NVDA | Survival Clayton | **Solo dependencia de cola inferior**: caen juntas, pero las subidas extremas son independientes |

---

## 📈 Caso Práctico 2 — Movimiento Browniano Fraccional (fGBM) y Modelo de Heston

### Objetivo

Analizar la **memoria** (persistencia) de la serie de precios de Google y simular escenarios futuros con modelos que superan al GBM estándar.

### Metodología y resultados

| Parámetro | Valor | Interpretación |
|-----------|-------|----------------|
| **Índice de Hurst** (*H*) | **0.86** | Fuerte persistencia. Rechaza el paseo aleatorio (*H* = 0.5) |
| **Deriva anualizada** (*μ*) | 29.8 % | Tendencia histórica de crecimiento |
| **Volatilidad anualizada** (*σ*) | 32.2 % | Nivel de riesgo esperable en tecnología |

### Simulaciones

- **fGBM**: 20 trayectorias a 250 días con Monte Carlo. Las trayectorias muestran suavidad y persistencia coherente con *H* = 0.86.
- **Modelo de Heston**: 15 trayectorias con volatilidad estocástica (Euler-Maruyama). Captura clústeres de volatilidad y efecto apalancamiento (ρ = −0.7).

---

## 🛠️ Tecnologías

| Categoría | Herramientas |
|-----------|-------------|
| Lenguaje | **R** (R Markdown) |
| Datos | `quantmod` (Yahoo Finance) |
| Distribuciones | `fitdistrplus`, `MASS` (t-Student) |
| Cópulas | `copula`, `VineCopula` |
| Browniano Fraccional | `mvtnorm`, `pracma` (Hurst) |
| Volatilidad Estocástica | `msde` (Heston SDE) |

---

## 📂 Estructura del repositorio

```
.
├── README.md              # Este archivo
├── tarea_final.Rmd        # Código fuente completo (R Markdown)
└── sesiones_de_clase/     # Material complementario de las sesiones
```

---

## ▶️ Cómo reproducir el análisis

1. Instalar **R** (≥ 4.0) y **RStudio**.
2. Instalar los paquetes necesarios:

```r
install.packages(c(
  "quantmod", "dplyr", "tibble", "fitdistrplus",
  "copula", "VineCopula", "MASS", "mvtnorm", "pracma", "msde"
))
```

3. Abrir `tarea_final.Rmd` en RStudio y ejecutar con **Knit → HTML**.

> ⚠️ El análisis descarga datos de Yahoo Finance en tiempo real, por lo que los resultados exactos pueden variar ligeramente según la fecha de ejecución.

---

## 📄 Licencia

Proyecto académico — Universidad de Murcia, curso 2024-2025.
