# Actividad 1. Python: fuentes de datos, EDA y limpieza

## 1. Idea seleccionada y objetivo

### 1.1. Descripción del proyecto

**Equity Intelligence: análisis financiero y evaluación de oportunidades de inversión mediante Machine Learning** es un proyecto orientado al análisis fundamental de empresas tecnológicas estadounidenses cotizadas en bolsa.

El objetivo es desarrollar una herramienta que permita evaluar la situación económica y financiera de distintas compañías a partir de sus estados financieros históricos, proporcionando información objetiva que facilite la comprensión de su salud financiera y la identificación de posibles oportunidades de inversión.

Se trabajará principalmente con cuatro dimensiones financieras:

- **Posición de caja:** efectivo y equivalentes disponibles para hacer frente a las necesidades de la empresa.
- **Endeudamiento:** nivel de deuda de la compañía y su relación con otros elementos de su estructura financiera.
- **Solvencia:** capacidad de la empresa para respaldar sus obligaciones financieras a largo plazo.
- **Liquidez:** capacidad para afrontar los compromisos financieros a corto plazo.

Adicionalmente, se estudiará la evolución del margen operativo TTM (*Trailing Twelve Months*) con el objetivo de desarrollar posteriormente modelos de Machine Learning capaces de estimar su valor a cuatro trimestres vista.


### 1.2. Objetivo de esta actividad

En esta actividad se pretende comprobar la viabilidad de obtener y utilizar información financiera de empresas tecnológicas estadounidenses.

Para ello, se utilizará como fuente principal SEC EDGAR, el sistema oficial de consulta de información financiera de la Comisión de Bolsa y Valores de Estados Unidos (SEC).

Se realizará una primera extracción de datos, un análisis exploratorio (EDA) y un proceso de limpieza reproducible utilizando Python.

El resultado será una primera muestra de datos financieros preparada (capa silver), que servirá como punto de partida para las siguientes fases del proyecto.


---


## 2. Datos necesarios y finalidad

### 2.1. Información necesaria

Para desarrollar las funcionalidades iniciales de Equity Intelligence necesitamos información procedente de los estados financieros de las empresas seleccionadas.

Las principales variables que se pretenden obtener son:

| Variable | Descripción | Utilidad |
|---|---|---|
| Revenue | Ingresos totales | Analizar la evolución de la actividad empresarial. |
| Operating Income | Beneficio operativo | Calcular el margen operativo. |
| Cash and Cash Equivalents | Efectivo y equivalentes | Analizar la posición de caja. |
| Total Debt | Deuda total | Evaluar el endeudamiento. |
| Current Assets | Activos corrientes | Calcular indicadores de liquidez. |
| Current Liabilities | Pasivos corrientes | Evaluar las obligaciones a corto plazo. |
| Total Assets | Activos totales | Analizar la solvencia. |
| Total Liabilities | Pasivos totales | Evaluar el peso de las obligaciones financieras. |

También necesitaremos variables de identificación y contexto temporal:

| Variable | Descripción |
|---|---|
| Company | Nombre de la empresa. |
| CIK | Identificador de la empresa en la SEC. |
| Fiscal Year | Ejercicio fiscal. |
| Fiscal Period | Periodo fiscal (trimestre o ejercicio anual). |
| Start | Fecha de inicio del periodo, cuando proceda. |
| End | Fecha de finalización del periodo. |
| Filed | Fecha de presentación del informe. |
| Accession Number | Identificador único del documento presentado. |
| Form | Tipo de informe (10-K, 10-Q, etc.). |
| Frame | Referencia temporal normalizada, cuando esté disponible. |


### 2.2. Relación entre los datos y el producto

La información obtenida permitirá calcular diferentes indicadores financieros y estudiar su evolución histórica.

Por ejemplo:

- Los ingresos y el beneficio operativo permitirán calcular el margen operativo.
- El efectivo disponible permitirá estudiar la posición de caja.
- La deuda y los recursos propios permitirán evaluar el endeudamiento.
- Los activos y pasivos totales permitirán analizar la solvencia.
- Los activos y pasivos corrientes permitirán calcular indicadores de liquidez.

Además, la información histórica permitirá construir posteriormente variables explicativas para los modelos predictivos.


### 2.3. Datos obtenidos y datos pendientes

En esta fase se plantea utilizar SEC EDGAR como fuente principal.

**Datos obtenidos:**
- Informes financieros 10-K y 10-Q localizados para distintas empresas.
- Identificadores CIK de las compañías seleccionadas.
- Referencias a la API oficial de SEC EDGAR.

**Datos pendientes:**
- Descarga automatizada de los datos financieros mediante la API.
- Extracción de las partidas contables necesarias.
- Comprobación de la cobertura histórica disponible.
- Homogeneización de las variables entre empresas.
- Construcción de una muestra consolidada.


---


## 3. Fuentes de datos

### 3.1. Fuente principal: SEC EDGAR

**Origen y acceso**

La fuente principal del proyecto será SEC EDGAR, sistema de información de la Securities and Exchange Commission (SEC) de Estados Unidos.

Esta plataforma permite consultar los informes financieros y otra documentación presentada por las empresas cotizadas.

Para obtener los datos se utilizarán principalmente las siguientes herramientas:

- **Company Facts:** proporciona información financiera estructurada de las empresas mediante datos XBRL en formato JSON.
- **Submissions:** proporciona información sobre los documentos presentados por cada empresa, incluyendo fechas y tipos de informes.

La adquisición se realizará mediante la API oficial de SEC EDGAR.

Documentación oficial:
- SEC EDGAR Application Programming Interfaces (APIs).
- SEC EDGAR Accessing EDGAR Data.

**Método de obtención:** API REST.

**Formato:** JSON.

**Fecha de acceso:** pendiente de completar tras realizar la primera descarga.

**Empresas iniciales:**

| Empresa | CIK |
|---|---|
| Microsoft | 789019 |
| Nvidia | 1045810 |
| Tesla | 1318605 |
| AMD | 2488 |
| Intel | 50863 |
| Meta | 1326801 |

La selección definitiva de empresas dependerá de la disponibilidad y calidad de sus datos históricos.

**Condiciones de uso**

La información procede de una fuente pública oficial de la SEC. No obstante, se revisarán las condiciones de acceso, reutilización y redistribución aplicables a los datos y documentos utilizados.

Se respetarán las políticas de acceso de la API, incluyendo los límites de solicitudes y la identificación del cliente mediante la cabecera User-Agent.

La posibilidad de compartir los datos descargados en el repositorio se comprobará antes de incorporar muestras.

**Protección y privacidad**

Los datos utilizados corresponden a información financiera corporativa publicada por empresas cotizadas.

No se prevé trabajar con datos personales de inversores ni información financiera privada de particulares.

Se conservarán únicamente los campos necesarios para el análisis financiero.

No se incorporarán credenciales ni información privada al repositorio.

**Frecuencia y cobertura**

SEC EDGAR recibe información de las empresas conforme estas presentan sus informes financieros.

Los principales documentos utilizados serán:

- **10-K:** informes anuales.
- **10-Q:** informes trimestrales.

Se pretende construir una serie histórica cercana a diez años, aunque la cobertura real dependerá de cada empresa y de la disponibilidad de las partidas contables.

La frecuencia de publicación es adecuada para el análisis fundamental histórico y la futura construcción de modelos predictivos trimestrales.

**Granularidad y variables**

Los datos de Company Facts representan observaciones de conceptos contables correspondientes a una empresa y a un periodo o fecha determinados.

Es importante distinguir entre:

- Variables de flujo, como ingresos y beneficio operativo, que representan cantidades acumuladas durante un periodo.
- Variables de balance, como efectivo, activos y pasivos, que representan una situación financiera en una fecha concreta.

Se conservarán las fechas de los periodos y las fechas de presentación de los informes para evitar utilizar información que todavía no era pública en el momento de una futura predicción.


---


## 4. EDA: análisis exploratorio de datos

### 4.1. Carga e inspección inicial

Una vez descargados los datos de SEC EDGAR, se realizará una primera inspección utilizando Python y pandas.

Se analizarán:

- Número de filas y columnas.
- Tipos de datos.
- Nombres de las variables.
- Empresas disponibles.
- Periodos históricos.
- Primeras y últimas observaciones.

**Resultados:** pendiente de completar tras la descarga.


### 4.2. Análisis de calidad

Se realizarán los siguientes controles:

| Control | Objetivo |
|---|---|
| Valores ausentes | Identificar partidas contables sin información. |
| Duplicados | Detectar observaciones repetidas. |
| Tipos de datos | Comprobar que fechas y valores numéricos tienen el formato correcto. |
| Fechas | Identificar periodos inconsistentes. |
| Valores fuera de rango | Detectar posibles errores o valores inusuales. |
| Unidades | Comprobar la consistencia de las unidades monetarias. |
| Cobertura histórica | Identificar periodos sin información suficiente. |
| Consistencia contable | Comprobar relaciones básicas entre partidas financieras. |

Los valores negativos o extremos no se considerarán automáticamente errores, ya que pueden tener sentido desde el punto de vista financiero.


### 4.3. Análisis de variables principales

Se estudiarán las principales partidas contables mediante estadísticas descriptivas:

- Media.
- Mediana.
- Desviación estándar.
- Valores mínimos y máximos.
- Frecuencias por empresa y periodo.

También se elaborarán gráficos para visualizar:

1. Evolución de los ingresos.
2. Evolución del beneficio operativo.
3. Evolución del efectivo disponible.
4. Distribución de observaciones por empresa.
5. Cobertura histórica de las principales variables.

**Resultados e interpretación:** pendiente de completar tras realizar el EDA.


---


## 5. Pipeline de limpieza con Python

### 5.1. Carga de los datos originales

Se descargarán los datos financieros desde SEC EDGAR y se conservará una copia original sin modificar en la carpeta `data/raw/`.

Los ficheros originales se utilizarán como referencia para poder reproducir las transformaciones posteriores.


### 5.2. Transformaciones

El proceso de limpieza incluirá las operaciones que resulten necesarias tras el análisis exploratorio:

- Normalización de nombres de variables.
- Conversión de fechas.
- Conversión de valores numéricos.
- Homogeneización de unidades monetarias.
- Identificación de observaciones duplicadas.
- Revisión de valores ausentes.
- Comprobación de periodos fiscales.
- Identificación de observaciones inconsistentes.

Cada transformación estará justificada por los problemas detectados durante el EDA.


### 5.3. Validación

Una vez realizado el proceso de limpieza se comprobará:

- Número de registros conservados.
- Número de registros eliminados y motivo.
- Valores ausentes pendientes.
- Duplicados restantes.
- Tipos de datos.
- Consistencia de las fechas.
- Coherencia de las unidades.
- Disponibilidad de las variables necesarias.


### 5.4. Salida: capa silver

Los datos resultantes se almacenarán en `data/silver/`.

Se utilizará un formato estructurado, preferiblemente CSV o Parquet, que permita su posterior utilización desde SQL.

Se conservarán las variables de identificación, procedencia y temporalidad necesarias para reconstruir el origen de cada observación.


---


## 6. Resultados de la limpieza

Se elaborará una comparación entre los datos originales y los datos preparados.

| Indicador | Antes de la limpieza | Después de la limpieza |
|---|---:|---:|
| Número de filas | Pendiente | Pendiente |
| Número de columnas | Pendiente | Pendiente |
| Valores ausentes | Pendiente | Pendiente |
| Registros duplicados | Pendiente | Pendiente |
| Tipos de datos incorrectos | Pendiente | Pendiente |
| Registros eliminados | - | Pendiente |

Las transformaciones realizadas y sus resultados se explicarán en este apartado.

También se documentarán los problemas que no hayan podido resolverse y sus posibles consecuencias para las siguientes fases del proyecto.


---


## 7. Conclusiones y viabilidad

El objetivo final de esta actividad es determinar si SEC EDGAR proporciona información suficiente y adecuada para desarrollar Equity Intelligence.

Se valorarán los siguientes aspectos:

- Disponibilidad de las partidas contables necesarias.
- Cobertura histórica de las empresas seleccionadas.
- Comparabilidad de los datos entre compañías.
- Calidad y consistencia de las observaciones.
- Posibilidad de automatizar la adquisición.
- Viabilidad de reconstruir series trimestrales.
- Limitaciones derivadas de la estructura de los informes financieros.

También se identificarán las necesidades de información adicionales y los posibles cambios en el alcance del proyecto.

**Conclusión final:** pendiente de completar una vez finalizados el EDA y el pipeline de limpieza.


---


## 8. Reproducibilidad y documentación

El código y los resultados de esta actividad estarán organizados en el repositorio de GitHub `TFM-Equity-Intelligence`.

La estructura prevista será:

- `data/raw/`: datos originales descargados.
- `data/silver/`: datos limpios y preparados.
- `notebooks/`: notebooks de adquisición, análisis exploratorio y limpieza.
- `data/README.md`: instrucciones para obtener y utilizar los datos.
- `requirements.txt`: dependencias necesarias.
- `docs/entregas/01_python.md`: documentación de esta actividad.

El proceso deberá poder ejecutarse desde el principio y en orden, sin depender de modificaciones manuales no documentadas.