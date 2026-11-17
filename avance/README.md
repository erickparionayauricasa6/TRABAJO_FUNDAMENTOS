# PMD1: Proyecto Integrador de Datos · Checkpoint de Avance Técnico (Semana 6)
**Curso:** Fundamentos de Gestión de Datos  
**Institución:** TECSUP · Programa de Big Data y Ciencia de Datos · Periodo: 2026-II  
**Docente:** Pilar Rocío Sayán Mejía  
**Caso Asignado:** Caso 07: Taller (Órdenes de reparación vehicular)  
**Evaluación:** Checkpoint de Avance Técnico (Puntaje: 4 puntos hacia el Informe Técnico Final)

[![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/erickparionayauricasa6/TRABAJO_FUNDAMENTOS/blob/main/avance/PMD1_Checkpoint_Semana06_Grupo07_Caso_Taller.ipynb)
[![Google Drive](https://img.shields.io/badge/Google%20Drive-Materiales%20y%20Entregables-4285F4?style=flat&logo=googledrive&logoColor=white)](https://drive.google.com/drive/folders/1xayZ4OjWy6y084RwEPpOco70yljhE2D_?usp=sharing)
[![Python](https://img.shields.io/badge/Python-3.10%2B-blue?style=flat&logo=python&logoColor=white)](https://www.python.org/)
[![SQLite](https://img.shields.io/badge/SQLite-3.0-003B57?style=flat&logo=sqlite&logoColor=white)](https://www.sqlite.org/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-v1.0%2B-F7931E?style=flat&logo=scikitlearn&logoColor=white)](https://scikit-learn.org/)

---

## 1. Identificación del Equipo y Distribución de Aportes

| N.° | Integrantes | Código | Sección | Responsabilidad Metodológica | Aporte (%) |
| :---: | :--- | :---: | :---: | :--- | :---: |
| 1 | **Pariona Yauricasa, Erick** | 1421035 | C24-A | Integración SQL en SQLite, diseño de la vista analítica `vw_analitica` y comprobación de integridad referencial | 25 % |
| 2 | **Ticona Corrales, Gabriel Humberto** | - | C24-A | Análisis exploratorio (EDA), estadísticas descriptivas y elaboración de gráficos interpretados | 25 % |
| 3 | **Gonzano Rojas, Edir** | - | C24-A | Diagnóstico de calidad, cuantificación de incidencias, bitácora de limpieza y tratamientos | 25 % |
| 4 | **Pinglo Rodriguez, Diego Alonso Dante** | - | C24-A | Modelado de regresión lineal simple (partición 80/20), cálculo de métricas R², RMSE y definición de pendientes | 25 % |

**Total de aporte grupal:** 100 % (Participación paritaria y coordinada).

---

## 2. Descripción del Caso y Problema de Negocio

### 2.1 Contexto y Unidad Operativa
La empresa en estudio es una red automotriz de mantenimiento y reparación con cinco sedes operativas en Lima (Norte, Sur, Este, Oeste y Centro). En su operación cotidiana, cada vehículo que ingresa genera una orden de trabajo donde intervienen recepcionistas y mecánicos en momentos distintos.
* **Unidad de Análisis:** Cada fila de la tabla `Operaciones` representa **una orden de servicio vehicular individual** atendida en una de las sedes.

### 2.2 Problema Central
El personal de recepción y el jefe de taller no cuentan con un procedimiento estandarizado ni un modelo cuantitativo para estimar cuántas horas de trabajo demandará cada vehículo al momento de agendar citas. Esto provoca asignaciones de tiempo erróneas que derivan en saturación de las bahías mecánicas, demoras en las entregas a los clientes o tiempos muertos no programados.

### 2.3 Pregunta Guía del Proyecto
> *¿Cuántas horas adicionales de reparación se asocian a cada año de antigüedad del vehículo y qué perfiles de órdenes requieren mayor tiempo o especialización técnica?*

### 2.4 Objetivos
* **Objetivo General:** Integrar y depurar la información operativa del taller mediante SQLite y pandas, estimar una función lineal del tiempo de servicio en función de la antigüedad del automóvil y establecer la base técnica para la segmentación con K-Means.
* **Objetivos Específicos:**
  1. Relacionar las cuatro tablas del caso en SQLite asegurando integridad referencial (0 huérfanas).
  2. Explorar los datos mediante estadísticas de tendencia central, dispersión y visualizaciones.
  3. Diagnosticar problemas de calidad y depurarlos en una bitácora transparente conservando la representatividad en `dataset_limpio`.
  4. Entrenar y evaluar un modelo de regresión lineal simple (80/20) reportando R², MSE, RMSE y MAE.

---

## 3. Estructura del Repositorio y Enlaces Externos

```
avance/
├── ../07_Taller_PMD1.xlsx                                  # Libro Excel original con las 4 hojas del caso
├── caso_pmd1_taller.db                                  # Base de datos SQLite con tablas base, vista y tablas procesadas
├── Informe_Avance_Checkpoint_Semana06_Caso07_Taller.docx # Informe técnico formal según plantilla oficial de Tecsup
├── PMD1_Checkpoint_Semana06_Grupo07_Caso_Taller.ipynb   # Notebook de Google Colab completamente ejecutado
├── PMD1_Checkpoint_Semana06_Grupo07_Caso_Taller.pdf     # Documento PDF con código, salidas y figuras renderizadas
└── README.md                                            # Documentación técnica completa del avance
```

### Enlaces de Interés:
* **Carpeta de Google Drive (Materiales y Respaldos):**  
  [https://drive.google.com/drive/folders/1xayZ4OjWy6y084RwEPpOco70yljhE2D_?usp=sharing](https://drive.google.com/drive/folders/1xayZ4OjWy6y084RwEPpOco70yljhE2D_?usp=sharing)
* **Cuaderno ejecutable en Google Colab:**  
  [Abrir en Google Colab](https://colab.research.google.com/github/erickparionayauricasa6/TRABAJO_FUNDAMENTOS/blob/main/avance/PMD1_Checkpoint_Semana06_Grupo07_Caso_Taller.ipynb)
* **Repositorio de GitHub:**  
  [https://github.com/erickparionayauricasa6/TRABAJO_FUNDAMENTOS.git](https://github.com/erickparionayauricasa6/TRABAJO_FUNDAMENTOS.git)

---

## 4. Integración SQL en SQLite

Se descargó el libro de datos desde el repositorio oficial del curso y se poblaron cuatro tablas en `caso_pmd1_taller.db`:

| Tabla | Tipo / Rol en el Modelo | Clave Primaria (PK) | N.° Filas |
| :--- | :--- | :---: | :---: |
| `operaciones` | Tabla de hechos (transaccional) | `id_operacion` | 466 |
| `clientes` | Dimensión cliente | `id_cliente` | 120 |
| `catalogo` | Dimensión sistemas del vehículo y precios | `id_catalogo` | 20 |
| `sedes` | Dimensión talleres y zonas | `id_sede` | 5 |

### Verificación de Integridad Referencial
Se ejecutaron consultas SQL de comprobación confirmando la inexistencia de claves foráneas huérfanas:
* Huérfanos hacia `clientes`: **0**
* Huérfanos hacia `catalogo`: **0**
* Huérfanos hacia `sedes`: **0**

### Vista Analítica (`vw_analitica`)
Se implementó mediante `LEFT JOIN` para preservar todas las órdenes operativas, resultando en una estructura consolidada de **466 filas y 13 columnas**.  
Consultas de negocio aplicadas:
* Agrupación con `GROUP BY id_sede HAVING COUNT(*) > 80`: Identificó que las sedes **Sur (106 órdenes)** y **Norte (104 órdenes)** representan el 45 % de la carga operativa.
* Análisis por sistema mecánico: Los sistemas de **Motor (media 6.12 h)** y **Transmisión (media 5.84 h)** acumulan los mayores consumos de mano de obra y costo de repuestos.

---

## 5. Análisis Exploratorio de Datos (EDA)

* **Variable Objetivo ($Y$):** `horas_reparacion`
  * Media: **4.88 horas** | Desv. Estándar: **2.65 horas**
  * Mediana: **4.31 horas** | Rango Intercuartílico: **[2.62, 6.11] horas**
  * Rango de variación: Mínimo 0.65 h hasta Máximo 14.65 h.
* **Variable Predictora ($X$):** `antiguedad_auto_anios`
  * Media: **9.17 años** | Desv. Estándar: **4.54 años**
  * Mediana: **9.0 años** | Rango: 1 a 22 años de antigüedad.
* **Correlación Lineal:**
  * Coeficiente de Pearson: **$r = 0.703$** (asociación lineal positiva fuerte).

### Visualizaciones Incluidas en el Notebook
1. **Histograma y Densidad (KDE) de Horas:** Muestra asimetría positiva a la derecha; el 75 % de las atenciones toma menos de 6.11 horas, con una cola extendida hacia reparaciones mayores.
2. **Dispersión Antigüedad vs. Horas:** Confirma una clara tendencia lineal ascendente donde vehículos de más de 12 años requieren frecuentemente más de 6 horas de servicio.
3. **Volumen de Horas por Sede y Sistema:** Muestra consistencia técnica en las 5 sedes, lideradas por intervenciones de Motor y Transmisión.

---

## 6. Diagnóstico y Bitácora de Limpieza de Datos

| Incidencia Detectada | Cantidad | Tratamiento Aplicado | Impacto en Registros |
| :--- | :---: | :--- | :---: |
| **Registros duplicados** | 6 filas | Eliminación conservando la primera ocurrencia | 466 → 460 filas |
| **Formatos de fecha** | 20 casos | Conversión de `D/M/AAAA` a estándar `AAAA-MM-DD` (`datetime`) | Fechas estandarizadas |
| **Espacios en blanco** | En textos | Aplicación de `.str.strip()` en campos categóricos | Cadenas uniformizadas |
| **Nulos en $X$ o $Y$** | 7 filas (3 en $Y$, 4 en $X$) | Eliminación de las 7 filas para no distorsionar la pendiente | 460 → 453 filas |
| **Nulos secundarios** | 5 en kilometraje | Imputación con la mediana (124.0 mil km) por robustez ante asimetría | 5 valores imputados |

* **Tratamiento Técnico de Valores Extremos (IQR):**  
  Se detectaron 19 órdenes que superan el límite superior ($Q_3 + 1.5 \times IQR = 11.35\text{ h}$). Se verificó que corresponden a **vehículos antiguos (media de 14.6 años)** con cambio de componentes pesados (motor y caja). Eliminar estas filas sesgaría el modelo hacia reparaciones simples, por lo que **se mantuvieron intactas**.
* **Conjunto Depurado:**  
  La tabla `dataset_limpio` quedó guardada en SQLite con **453 filas válidas (97.2 % de retención)**.

---

## 7. Avance de Regresión Lineal Simple

Se utilizó scikit-learn con partición 80 % para entrenamiento (362 órdenes) y 20 % para prueba (91 órdenes) con `random_state = 42`:

### Ecuación Estimada
$$\text{horas\_reparacion} = 1.1148 + 0.4113 \times \text{antiguedad\_auto\_anios}$$

### Parámetros y Métricas
* **Intercepto ($b_0 = 1.1148\text{ h}$):** Tiempo base inicial de inspección, recepción, escaneo y montaje en rampa para un auto de 0 años.
* **Pendiente ($b_1 = 0.4113\text{ h/año}$):** Cada año adicional de antigüedad se asocia en promedio con 0.41 horas (~25 minutos) más de labor en taller.
* **$R^2$ en datos de prueba:** **0.5262 (52.6 %)** de la varianza explicada.
* **$R^2$ en entrenamiento:** **0.4839 (48.4 %)**, confirmando **ausencia de sobreajuste (overfitting)**.
* **Error Cuadrático Medio (MSE):** **$4.5779\text{ h}^2$**
* **Raíz del MSE (RMSE):** **$2.1396\text{ horas}$** (margen típico de dispersión de $\pm 2.14$ h).
* **Error Absoluto Medio (MAE):** **$1.4503\text{ horas}$**

Los resultados con valores reales, valores estimados y residuos se encuentran registrados en SQLite en la tabla `predicciones_regresion`.

---

## 8. Dificultades Encontradas y Plan de Trabajo hacia la Semana 8

| Componente | Estado Actual | Tarea Planificada hacia la Entrega Final |
| :--- | :---: | :--- |
| **Integración SQL** | **100 % Completado** | Validado en SQLite con 0 huérfanas y vista `vw_analitica`. |
| **Diagnóstico y Limpieza** | **100 % Completado** | `dataset_limpio` con 453 órdenes (97.2 % de retención). |
| **Regresión Lineal Simple** | **100 % Completado** | Modelo 80/20 evaluado con $R^2 = 0.526$ y guardado en `predicciones_regresion`. |
| **Fase 4: Segmentación K-Means** | **0 % Pendiente (Semana 7)** | Estandarización (`StandardScaler`), optimización de $k$ (curva del codo y silueta) y tipificación de perfiles de vehículos. |
| **Regresión Lineal Múltiple** | **0 % Pendiente (Semana 7)** | Probar inclusión de kilometraje y piezas para superar el 70 % de $R^2$. |
| **Informe Técnico Final (16 pts)** | **0 % Pendiente (Semana 8)** | Completar las 14 secciones de la plantilla de Tecsup incorporando los resultados de clustering. |
| **Sustentación del Proyecto** | **0 % Pendiente (Semana 8)** | Preparar diapositivas de exposición (15 min) y balotario de defensa individual. |

---

## 9. Instrucciones de Reproducción y Ejecución

### Opción 1: Ejecutar directamente en Google Colab
1. Abrir el cuaderno haciendo clic en el badge superior:  
   [![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/erickparionayauricasa6/TRABAJO_FUNDAMENTOS/blob/main/avance/PMD1_Checkpoint_Semana06_Grupo07_Caso_Taller.ipynb)
2. En el menú superior de Colab, seleccionar `Entorno de ejecución` -> `Ejecutar todas`.
3. El script descargará automáticamente el archivo `07_Taller_PMD1.xlsx` desde GitHub, creará la base de datos `caso_pmd1_taller.db` y ejecutará todas las etapas sin requerir configuración manual.

### Opción 2: Ejecución local
```bash
# Clonar el repositorio
git clone https://github.com/erickparionayauricasa6/TRABAJO_FUNDAMENTOS.git
cd TRABAJO_FUNDAMENTOS

# Instalar dependencias requeridas
pip install pandas numpy matplotlib seaborn scikit-learn openpyxl

# Ejecutar Jupyter Notebook
jupyter notebook PMD1_Checkpoint_Semana06_Grupo07_Caso_Taller.ipynb
```
