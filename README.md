# Trabajo Minería de Datos — Grupo 10

**Análisis de Factores Socioeconómicos y Académicos en el Acceso a la Educación Superior Chilena**

Integrantes: Benjamin Jilberto · Fabian Pacheco · Matias Sanhueza · Pablo Suarez · Vicente Garrido

---

## Estructura del repositorio

```
├── Hito1/          → Informe y presentación del Hito 1
├── Hito2/          → Notebooks del Hito 2 (experimentos completos)
│   ├── 00_carga_datos.ipynb        → Join de los 3 datasets + variable migra
│   ├── 01_preprocesamiento.ipynb   → Correcciones y feature engineering
│   ├── 02_P1_clustering.ipynb      → KMeans + Ward + DBSCAN
│   ├── 03_P2_regresion.ipynb       → Predicción puntaje PAES
│   ├── 04_P3_clasificacion.ipynb   → Predicción tipo de institución
│   └── 05_P4_migracion.ipynb       → Predicción migración estudiantil
├── data/
│   ├── raw/        → CSVs del MINEDUC (NO en git — ver instrucciones abajo)
│   ├── processed/  → Datos procesados en Parquet (NO en git)
│   └── samples/    → Muestra de 1000 filas (SÍ en git)
└── README.md
```

---

## Datasets

Los datasets son públicos del MINEDUC. Solicitar acceso al Google Drive del grupo.

| ID | Nombre archivo | Descripción | Fuente |
|----|----------------|-------------|--------|
| A | `A_puntajes.csv` | Inscritos PAES 2025 — Puntajes y escolaridad | DEMRE/MINEDUC |
| B | `B_socioeconomico.csv` | Inscritos PAES 2025 — Domicilio y datos socioeconómicos | DEMRE/MINEDUC |
| C | `C_matricula.csv` | Matrícula Educación Superior 2007-2025 | SIES/MINEDUC |

### Setup inicial

```bash
# 1. Clonar el repo
git clone https://github.com/PabloSuarezT/Trabajo-Miner-a-de-Datos.git
cd Trabajo-Miner-a-de-Datos

# 2. Instalar dependencias
pip install pandas numpy scikit-learn matplotlib seaborn scipy joblib pyarrow

# 3. Descargar CSVs desde Google Drive y colocarlos en data/raw/

# 4. Ejecutar notebook de carga (genera los Parquet)
jupyter notebook Hito2/00_carga_datos.ipynb
```

---

## Correcciones metodológicas respecto al Hito 1

Los siguientes errores y mejoras fueron identificados al iniciar el Hito 2. Es importante que el equipo los conozca para no repetirlos y para justificarlos en el informe.

### Errores corregidos

| # | Variable / Aspecto | Problema en Hito 1 | Corrección en Hito 2 |
|---|--------------------|--------------------|----------------------|
| 1 | `CODIGO_REGION_DOMICILIO` | Se usaba como variable numérica en clustering (región 13 > región 1 matemáticamente, sin sentido) | Se excluye del clustering; se analiza solo en post-hoc para describir los clusters |
| 2 | `PTJE_NEM` y `PTJE_RANKING` | Se usaban ambos juntos (correlación = 0.991, casi idénticos — duplicación de información) | Se elimina `PTJE_NEM`; se usa solo `PTJE_RANKING` (siempre ≥ NEM, más informativo) |
| 3 | Variable `migra` | El cálculo daba 100% de migración porque `REGION_SEDE` usa nombres cortos ("Metropolitana") y `CODIGO_REGION_DOMICILIO` usa códigos numéricos — nunca coincidían | Se mapean los nombres cortos a códigos numéricos antes de comparar. Resultado correcto: ~14% migra |

### Mejoras incorporadas

| # | Aspecto | Antes | Ahora |
|---|---------|-------|-------|
| 4 | `RAMA_EDUCACIONAL` | No se usaba en ningún experimento | Se convierte en `es_tecnico` (binaria 0/1) e incluye en todos los experimentos. Resultó ser la 2ª variable más importante en regresión (17%) |
| 5 | Desbalance de clases (P3) | Los clasificadores no consideraban el desbalance (ratio 5.6x entre CRUCH y CFT) | Todos los modelos usan `class_weight='balanced'`; se reporta F1-macro como métrica principal en lugar de accuracy |
| 6 | Formato de datos | Se trabajaba sobre CSVs crudos en cada ejecución | Se guardan Parquets procesados en `data/processed/` (~5x más pequeños, ~10x más rápidos). El pipeline se divide en notebooks numerados con responsabilidades claras |

---

## Preguntas de investigación

| # | Tipo | Pregunta |
|---|------|----------|
| P1 | Clustering (no supervisado) | ¿Existen perfiles diferenciados de estudiantes según características académicas y socioeconómicas? |
| P2 | Regresión (supervisado) | ¿Qué variables predicen mejor el puntaje PAES? |
| P3 | Clasificación multiclase (supervisado) | ¿Se puede predecir el tipo de institución donde se matricula un estudiante? |
| P4 | Clasificación binaria (supervisado) | ¿Se puede predecir si un estudiante se matriculará fuera de su región? |