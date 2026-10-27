# 🏦 Home Credit Default Risk — Ensamble Multi-Modelo con 5-Fold Cross-Validation

Este repositorio contiene la solución completa desarrollada para la competición **Home Credit Default Risk** en la plataforma Omnicampus. El objetivo principal es predecir la probabilidad de que un cliente incumpla con el pago de su préstamo crediticio (`TARGET`) evaluando el Área Bajo la Curva ROC (**ROC-AUC**).

---

## 📊 Resultados Principales y Métricas de Desempeño

Mediante un esquema de **Validación Cruzada Estratificada de 5 Pliegues (Stratified 5-Fold Cross-Validation)** y la creación de **ratios financieros**, se logró escalar el rendimiento predictivo desde un baseline inicial de $0.7142$ hasta un puntaje histórico **Out-Of-Fold (OOF) de 0.7488**.

### 📈 Tabla Comparativa de Modelos (Evaluación Out-Of-Fold)

| Modelo / Método | Algoritmo | Métrica (OOF ROC-AUC) | Configuración / Pesos |
| :--- | :--- | :---: | :--- |
| **Modelo Base 1** | Logistic Regression | 0.7142 | Estandarizado con `StandardScaler`. |
| **Modelo Base 2** | Random Forest | 0.7179 | Regularizado (`max_depth=7`, `min_samples_leaf=20`). |
| **Modelo Avanzado 1** | CatBoost | 0.7440 | `iterations=500`, `learning_rate=0.03`, `depth=6`. |
| **Modelo Avanzado 2** | XGBoost | 0.7474 | `n_estimators=500`, `learning_rate=0.03`, `max_depth=6`. |
| **Modelo Avanzado 3** | LightGBM | 0.7478 | `n_estimators=500`, `learning_rate=0.03`, `num_leaves=31`. |
| 🏆 **Ensamble Final** | **Weighted Blend** | **0.7488** | **Promedio Ponderado:** `50% LGBM + 30% CB + 20% XGB`. |

> **Nota de Validación:** Cada modelo fue entrenado y validado a lo largo de 5 iteraciones estratificadas para garantizar la máxima estabilidad y evitar el sobreajuste al *leaderboard* público.

---

## 🛠️ Ingeniería de Características y Preprocesamiento

El pipeline de datos incluye las siguientes transformaciones principales:

1. **Selección Ampliada de Variables:**
   * Inclusión de indicadores demográficos, crediticios e institucionales (`ORGANIZATION_TYPE`, `DAYS_REGISTRATION`, `DAYS_ID_PUBLISH`, `CODE_GENDER`, `CNT_CHILDREN`).
2. **Ingeniería de Ratios Financieros (*Feature Engineering*):**
   * **`PAYMENT_RATE`**: $\frac{\text{AMT\_ANNUITY}}{\text{AMT\_CREDIT}}$ (Proporción anual del crédito total).
   * **`INCOME_CREDIT_PERC`**: $\frac{\text{AMT\_INCOME\_TOTAL}}{\text{AMT\_CREDIT}}$ (Años de ingreso para cubrir la deuda).
   * **`ANNUITY_INCOME_PERC`**: $\frac{\text{AMT\_ANNUITY}}{\text{AMT\_INCOME\_TOTAL}}$ (Compromiso del ingreso anual en la cuota).
   * **`EXT_SOURCE_MEAN`**: Promedio ponderado de puntuaciones externas (`EXT_SOURCE_2` y `EXT_SOURCE_3`).
   * **`INCOME_PER_PERSON`**: Ingreso disponible ajustado por tamaño familiar.
3. **Limpieza e Imputación:**
   * Tratamiento de la anomalía técnica en `DAYS_EMPLOYED` (`365243` $\rightarrow$ `NaN`).
   * Conversión de `DAYS_BIRTH` a edad real en años (`AGE_YEARS`).
   * Agrupación y *One-Hot Encoding* sobre la antigüedad de vehículos (`OWN_CAR_AGE`).
   * Imputación de valores nulos con la media calculada exclusivamente sobre el conjunto de entrenamiento (prevención de *Data Leakage*).

---

## 📁 Estructura del Proyecto

```text
├── input/
│   ├── train.csv                      # Dataset de entrenamiento
│   ├── test.csv                       # Dataset de prueba para la competición
│   └── sample_submission.csv          # Formato oficial de envío
├── output/
│   └── submission.csv                 # Predicciones ponderadas finales generadas
├── HomeCredit_columns_description.xlsx# Diccionario detallado de características
├── tutorial.ipynb                     # Notebook principal reproducible
├── tutorial.zip                       # Archivo comprimido para entrega Honors/Outstanding
└── README.md                          # Documentación del proyecto




