# Analisis-Credito-Bancos
Code used to analyze ecuadorian financial system.

Modelos de riesgo crediticio para bancos privados del Ecuador
Este repositorio contiene el notebook y la base de datos utilizados para ejecutar el proceso analítico de la tesis sobre determinantes de la cartera vencida en bancos privados del Ecuador. El flujo incluye limpieza de datos, análisis descriptivo, validación estadística, estimación de un modelo de datos de panel y entrenamiento de un modelo XGBoost para predicción.
Contenido del repositorio
`Code_Capstone Analisis Credito Bancos.ipynb`: notebook principal con el código para preparar los datos, ejecutar los modelos y generar resultados.
`DB_BANKS_UDLA.csv`: base de datos en formato de panel con información de bancos privados del Ecuador y variables macroeconómicas asociadas.
Objetivo del análisis
Evaluar la relación entre la cartera vencida y variables internas de los bancos, junto con variables macroeconómicas, para explicar y predecir el comportamiento del riesgo crediticio en el sistema bancario privado ecuatoriano.
La variable dependiente principal es:
`C_VENC_PCM`: cartera vencida utilizada como indicador de morosidad o deterioro crediticio.
Variables utilizadas
El notebook trabaja con variables bancarias y macroeconómicas, entre ellas:
Crecimiento de crédito.
Ratio de cobertura.
Liquidez de corto plazo.
ROA.
Cartera bruta.
Variación interanual del PIB real.
Desempleo.
Subempleo.
Inflación interanual.
Dummy COVID.
Variables rezagadas para el modelo predictivo XGBoost.
Flujo metodológico del notebook
1. Carga de librerías y base de datos
Se importan las librerías necesarias para el análisis, entre ellas:
`pandas`
`numpy`
`matplotlib`
`seaborn`
`statsmodels`
`linearmodels`
`scikit-learn`
`xgboost`
Luego se carga el archivo `DB_BANKS_UDLA.csv` y se prepara el DataFrame principal.
2. Limpieza y preparación de datos
El notebook realiza las siguientes tareas:
Limpieza de nombres de columnas.
Conversión de variables porcentuales y numéricas a formato `float`.
Conversión de la variable de fecha a formato temporal.
Selección de variables relevantes para el análisis.
Construcción de la variable objetivo `C_VENC_PCM`.
Eliminación de columnas auxiliares o no utilizadas.
Revisión de valores faltantes.
Exclusión de bancos específicos cuando es necesario para la consistencia del panel.
3. Análisis exploratorio y estadística descriptiva
Se generan tablas y gráficos para entender el comportamiento de la base:
Vista inicial de la base de datos.
Tipos de datos.
Conteo de valores faltantes.
Estadística descriptiva.
Matriz de correlaciones.
Gráficos de dispersión entre `C_VENC_PCM` y las variables explicativas.
Esta sección permite identificar relaciones preliminares, posibles problemas de multicolinealidad y patrones entre variables internas y macroeconómicas.
4. Validación previa de variables
Antes de estimar los modelos, el notebook incluye pruebas de validación como:
Revisión de correlaciones.
Cálculo del factor de inflación de varianza, VIF.
Evaluación de multicolinealidad.
Ajustes en la selección de variables explicativas.
El propósito es reducir problemas estadísticos y mejorar la interpretación de los resultados.
5. Modelo OLS preliminar
Se estima un modelo OLS como aproximación inicial para observar la relación entre la cartera vencida y las variables explicativas.
Variables utilizadas en esta etapa:
`credit_growth (%)`
`coverage_ratio (provisiones / cartera improductiva) (%)`
`FONDOS DISPONIBLES / TOTAL DEPOSITOS A CORTO PLAZO (%)`
`ROA (%)`
`CARTERA BRUTA`
`Var PIB REAL INTERANUAL (%)`
`VAR_ANUAL_PETROLEO (%)`
`Desempleo (%)`
`Subempleo (%)`
`Inflacion mensual (%)`
`Inflacion interanual (%)`
6. Modelo de datos de panel
El notebook estima un modelo de datos de panel con efectos fijos por entidad bancaria usando `PanelOLS`.
La estructura del panel se define con:
Entidad: `ID BANCO`
Tiempo: `Fecha`
El modelo usa efectos fijos por banco y errores estándar agrupados para considerar diferencias no observadas entre entidades y mejorar la robustez de la inferencia.
Variables utilizadas en el modelo de panel:
Ratio de cobertura.
Liquidez de corto plazo.
ROA.
Cartera bruta.
Variación interanual del PIB real.
Desempleo.
Subempleo.
Inflación interanual.
Dummy COVID.
Los resultados permiten evaluar la significancia estadística, el signo de los coeficientes y la capacidad explicativa del modelo.
7. Preparación del modelo XGBoost
Para el modelo predictivo se construye una base de machine learning a partir del panel. Se generan variables rezagadas para capturar efectos temporales:
`credit_growth (%)_lag24`
`coverage_ratio (%)_lag24`
`Var PIB REAL INTERANUAL (%)_lag12`
`Subempleo (%)_lag12`
La base se divide en conjunto de entrenamiento y prueba usando una partición 80/20.
8. Modelo XGBoost inicial
Se entrena un modelo `XGBRegressor` para predecir `C_VENC_PCM`.
El notebook calcula las siguientes métricas:
MSE.
RMSE.
R cuadrado.
Gráfico de valores reales vs. valores predichos.
Importancia de variables por ganancia.
9. Optimización del modelo XGBoost
Se aplica `GridSearchCV` para buscar los mejores hiperparámetros del modelo. Los parámetros evaluados incluyen:
`n_estimators`
`max_depth`
`learning_rate`
`subsample`
`colsample_bytree`
El modelo optimizado se evalúa nuevamente con MSE, RMSE y R cuadrado.
10. Validación de resultados del XGBoost
El notebook incluye análisis de residuos del modelo ajustado:
Comparación entre valores reales y predichos.
Revisión gráfica de residuos.
Evaluación visual de posibles sesgos o errores sistemáticos.
Identificación de variables con mayor contribución predictiva.
11. Comparación de modelos
Finalmente, el notebook compara el desempeño del modelo de datos de panel y el modelo XGBoost. El modelo de panel se utiliza principalmente con fines explicativos, mientras que XGBoost se orienta a la predicción y detección de patrones no lineales.
Cómo ejecutar el notebook
Clonar el repositorio:
```bash
git clone <URL_DEL_REPOSITORIO>
cd <NOMBRE_DEL_REPOSITORIO>
```
Instalar las dependencias principales:
```bash
pip install pandas numpy matplotlib seaborn statsmodels linearmodels scikit-learn xgboost
```
Abrir el notebook:
```bash
jupyter notebook "Code_Capstone Analisis Credito Bancos.ipynb"
```
Verificar que el archivo `DB_BANKS_UDLA.csv` esté en la misma carpeta del notebook.
Ajustar la ruta del archivo si se ejecuta fuera de Google Colab. En lugar de una ruta de Google Drive, se puede usar:
```python
file_path = "DB_BANKS_UDLA.csv"
df = pd.read_csv(file_path)
```
Ejecutar las celdas en orden, desde la carga de datos hasta la comparación final de modelos.
Resultados esperados
Al ejecutar el notebook se obtienen:
Base depurada en formato de panel.
Estadísticas descriptivas.
Matriz de correlación.
Gráficos exploratorios.
Diagnóstico de multicolinealidad mediante VIF.
Resultados del modelo OLS preliminar.
Resultados del modelo de datos de panel.
Métricas de desempeño del modelo XGBoost.
Importancia de variables del modelo XGBoost.
Análisis de residuos.
Comparación general entre enfoque explicativo y predictivo.
Nota metodológica
El modelo de datos de panel permite interpretar la relación entre variables bancarias y macroeconómicas con la cartera vencida, controlando diferencias estructurales entre bancos. El modelo XGBoost complementa este análisis al capturar relaciones no lineales y mejorar la capacidad predictiva sobre la variable objetivo.

Autor: Milton Eduardo Freire Sosa

Proyecto desarrollado como parte de una tesis de maestría en análisis de datos e inteligencia de negocios, enfocada en riesgo crediticio y morosidad bancaria en Ecuador.
