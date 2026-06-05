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

## Preguntas de investigación

| # | Tipo | Pregunta |
|---|------|----------|
| P1 | Clustering (no supervisado) | ¿Existen perfiles diferenciados de estudiantes según características académicas y socioeconómicas? |
| P2 | Regresión (supervisado) | ¿Qué variables predicen mejor el puntaje PAES? |
| P3 | Clasificación multiclase (supervisado) | ¿Se puede predecir el tipo de institución donde se matricula un estudiante? |
| P4 | Clasificación binaria (supervisado) | ¿Se puede predecir si un estudiante se matriculará fuera de su región? |