# Predicción de tiempos y costos de atención mediante regresión lineal

## Metadatos

| Elemento | Valor |
|---|---|
| Duración | 105 minutos |
| Complejidad | Media |
| Nivel de Bloom | Aplicar |

## Descripción general

En este laboratorio se inicia un proyecto reproducible de aprendizaje automático supervisado para un centro de atención. A partir de datos históricos, se construirá una regresión lineal univariable para estimar el tiempo de atención y una regresión lineal multivariable para estimar el costo operativo.

Se implementarán de forma vectorizada la hipótesis lineal, la función de costo y el descenso del gradiente. Finalmente, se compararán los resultados con `LinearRegression` de scikit-learn, evaluando métricas, residuos, coeficientes y limitaciones operativas.

## Objetivos de aprendizaje

Al finalizar el laboratorio, podrá:

- [ ] Explorar y preparar datos históricos de atención sin modificar el archivo de origen.
- [ ] Implementar una regresión lineal univariable para predecir `tiempo_atencion_minutos`.
- [ ] Implementar de forma vectorizada la función de costo y el descenso del gradiente con NumPy.
- [ ] Entrenar una regresión lineal multivariable con escalamiento e ingeniería de características para predecir `costo_atencion_usd`.
- [ ] Evaluar, interpretar y comparar modelos manuales con `LinearRegression` mediante MAE, MSE, RMSE y R².

## Requisitos previos

### Conocimientos

Se requiere comprensión básica de los siguientes temas:

- Aprendizaje automático supervisado, características y variable objetivo.
- Regresión lineal simple y múltiple.
- Hipótesis lineal: \(\hat{y} = X\theta\).
- Residuo: \(y - \hat{y}\).
- Error cuadrático medio y descenso del gradiente.
- Arreglos de NumPy, `DataFrame` de pandas y gráficos básicos.
- Uso de celdas de código y Markdown en JupyterLab.

### Acceso y recursos

- Computador de 64 bits con al menos 8 GB de RAM; se recomiendan 16 GB.
- Al menos 5 GB de espacio libre en disco.
- Acceso a Internet durante la instalación inicial.
- Archivo oficial disponible en:

```text
data/raw/atenciones_historicas.csv
```

> **Importante:** no modifique ni sobrescriba el archivo `data/raw/atenciones_historicas.csv`. Todas las transformaciones se guardarán en `data/processed/atenciones_preparadas.csv`.

## Entorno del laboratorio

### Hardware recomendado

| Recurso | Mínimo | Recomendado |
|---|---:|---:|
| Procesador | 2 núcleos, 2.0 GHz, 64 bits | 4 núcleos o superior |
| Memoria RAM | 8 GB | 16 GB |
| Espacio en disco | 5 GB libres | 10 GB libres |
| Resolución | 1366×768 | 1920×1080 |

### Software requerido

| Software | Versión |
|---|---|
| Python | 3.12.3 |
| pip | 24.2 |
| JupyterLab | 4.2.5 |
| IPython | 8.27.0 |
| NumPy | 2.0.1 |
| pandas | 2.2.2 |
| Matplotlib | 3.9.2 |
| scikit-learn | 1.5.1 |
| seaborn | 0.13.2 |

### Estructura obligatoria del proyecto

El directorio global obligatorio es:

- Linux/macOS: `~/taller_ml_supervisado_parte1`
- Windows: `C:\Users\<usuario>\taller_ml_supervisado_parte1`

La estructura final debe ser:

```text
taller_ml_supervisado_parte1/
├── data/
│   ├── raw/
│   │   └── atenciones_historicas.csv
│   └── processed/
│       └── atenciones_preparadas.csv
├── notebooks/
│   └── 01_prediccion_tiempos_costos_regresion_lineal.ipynb
├── requirements/
│   └── requirements.txt
├── results/
│   ├── metricas_modelos.csv
│   ├── predicciones_prueba.csv
│   ├── coeficientes_modelos.csv
│   ├── curva_costo_univariable.png
│   ├── recta_regresion_univariable.png
│   ├── residuos_multivariable.png
│   └── resumen_modelos.json
└── venv-ml-regresion/
```

## Procedimiento paso a paso

### Paso 1. Crear el proyecto y el entorno virtual

**Objetivo:** crear una estructura reproducible de trabajo, instalar versiones bloqueadas de dependencias y verificar el entorno.

**Instrucciones:**

1. Abra una terminal.

2. Cree el directorio del proyecto y sus subdirectorios.

   En Linux o macOS:

   ```bash
   mkdir -p ~/taller_ml_supervisado_parte1/{data/raw,data/processed,notebooks,results,requirements}
   cd ~/taller_ml_supervisado_parte1
   ```

   En Windows PowerShell:

   ```powershell
   mkdir C:\Users\$env:USERNAME\taller_ml_supervisado_parte1
   cd C:\Users\$env:USERNAME\taller_ml_supervisado_parte1
   mkdir data, data\raw, data\processed, notebooks, results, requirements
   ```

3. Copie el archivo oficial entregado por la persona instructora a:

   ```text
   data/raw/atenciones_historicas.csv
   ```

4. Cree el entorno virtual obligatorio llamado `venv-ml-regresion`.

   En Linux o macOS:

   ```bash
   python3.12 -m venv venv-ml-regresion
   source venv-ml-regresion/bin/activate
   ```

   En Windows PowerShell:

   ```powershell
   py -3.12 -m venv venv-ml-regresion
   .\venv-ml-regresion\Scripts\Activate.ps1
   ```

   Si PowerShell bloquea la activación, ejecute una vez:

   ```powershell
   Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
   .\venv-ml-regresion\Scripts\Activate.ps1
   ```

5. Cree el archivo `requirements/requirements.txt` con el contenido exacto siguiente:

   ```text
   jupyterlab==4.2.5
   ipython==8.27.0
   numpy==2.0.1
   pandas==2.2.2
   matplotlib==3.9.2
   scikit-learn==1.5.1
   seaborn==0.13.2
   ```

6. Instale las dependencias:

   ```bash
   python -m pip install --upgrade pip==24.2
   python -m pip install -r requirements/requirements.txt
   ```

7. Verifique las versiones instaladas:

   ```bash
   python -c "import numpy, pandas, matplotlib, sklearn, seaborn, jupyterlab; print('NumPy:', numpy.__version__); print('pandas:', pandas.__version__); print('Matplotlib:', matplotlib.__version__); print('scikit-learn:', sklearn.__version__); print('seaborn:', seaborn.__version__); print('JupyterLab:', jupyterlab.__version__)"
   ```

**Resultado esperado:**

- Existe el directorio `venv-ml-regresion`.
- El archivo `requirements/requirements.txt` contiene exactamente siete dependencias bloqueadas.
- El archivo de datos oficial se encuentra en `data/raw/atenciones_historicas.csv`.
- Las versiones mostradas corresponden a las especificadas en el laboratorio.

**Verificación:**

Ejecute uno de los siguientes comandos.

En Linux o macOS:

```bash
find . -maxdepth 3 -type d | sort
```

En Windows PowerShell:

```powershell
Get-ChildItem -Recurse -Directory | Select-Object FullName
```

Confirme que existen `data/raw`, `data/processed`, `notebooks`, `results`, `requirements` y `venv-ml-regresion`.

---

### Paso 2. Iniciar JupyterLab y crear el notebook oficial

**Objetivo:** crear el notebook documentado que centralizará el flujo reproducible de exploración, preparación, entrenamiento y evaluación.

**Instrucciones:**

1. Con el entorno virtual activo y desde la raíz del proyecto, ejecute:

   ```bash
   jupyter lab
   ```

2. En JupyterLab, abra la carpeta `notebooks`.

3. Cree un notebook de Python 3.

4. Guárdelo exactamente con el nombre:

   ```text
   01_prediccion_tiempos_costos_regresion_lineal.ipynb
   ```

5. Inserte como primera celda Markdown:

   ```markdown
   # Predicción de tiempos y costos de atención mediante regresión lineal

   **Laboratorio:** 01-00-01  
   **Objetivo:** construir y evaluar modelos de regresión lineal para estimar tiempos de atención y costos operativos a partir de datos históricos.  
   **Archivo fuente:** `../data/raw/atenciones_historicas.csv`  
   **Archivo preparado:** `../data/processed/atenciones_preparadas.csv`

   > El archivo original no se modifica. Todas las transformaciones se guardan en la carpeta `data/processed`.
   ```

6. Inserte una celda de código para importar las bibliotecas y configurar el entorno visual:

   ```python
   from pathlib import Path
   import json

   import numpy as np
   import pandas as pd
   import matplotlib.pyplot as plt
   import seaborn as sns

   from sklearn.model_selection import train_test_split
   from sklearn.preprocessing import StandardScaler
   from sklearn.linear_model import LinearRegression
   from sklearn.metrics import (
       mean_absolute_error,
       mean_squared_error,
       root_mean_squared_error,
       r2_score,
   )

   sns.set_theme(style="whitegrid", context="notebook")
   pd.set_option("display.max_columns", None)
   pd.set_option("display.float_format", lambda value: f"{value:,.3f}")

   RUTA_PROYECTO = Path.cwd().parent
   RUTA_RAW = RUTA_PROYECTO / "data" / "raw" / "atenciones_historicas.csv"
   RUTA_PROCESSED = RUTA_PROYECTO / "data" / "processed" / "atenciones_preparadas.csv"
   RUTA_RESULTS = RUTA_PROYECTO / "results"

   RUTA_RESULTS.mkdir(parents=True, exist_ok=True)

   print(f"Proyecto: {RUTA_PROYECTO}")
   print(f"Archivo fuente: {RUTA_RAW}")
   ```

> **Nota:** el notebook se ejecuta desde `notebooks`; por ello `Path.cwd().parent` identifica la raíz del proyecto. Si ejecuta el notebook desde otra ubicación, ajuste las rutas antes de continuar.

**Resultado esperado:**

La celda muestra las rutas absolutas del proyecto y del archivo de entrada. No deben aparecer errores de importación.

**Verificación:**

Ejecute la celda y confirme que la ruta impresa para el archivo fuente termina en:

```text
data/raw/atenciones_historicas.csv
```

---

### Paso 3. Cargar e inspeccionar los datos históricos

**Objetivo:** conocer la estructura, tipos de datos, valores faltantes, distribución básica y columnas disponibles antes de entrenar cualquier modelo.

**Instrucciones:**

1. Agregue una celda Markdown:

   ```markdown
   ## 1. Carga e inspección inicial

   Cada fila representa una atención histórica finalizada. Las variables predictoras deben estar disponibles antes de realizar una predicción; las variables objetivo son los resultados reales que se desea estimar.
   ```

2. Agregue y ejecute la siguiente celda de código:

   ```python
   if not RUTA_RAW.exists():
       raise FileNotFoundError(
           f"No se encontró el archivo oficial en: {RUTA_RAW}\n"
           "Verifique que el archivo fue copiado a data/raw/atenciones_historicas.csv"
       )

   datos_originales = pd.read_csv(RUTA_RAW)

   print("Dimensiones originales:", datos_originales.shape)
   display(datos_originales.head())
   ```

3. Inspeccione nombres de columnas, tipos y valores faltantes:

   ```python
   print("Columnas disponibles:")
   print(datos_originales.columns.tolist())

   print("\nInformación general:")
   datos_originales.info()

   faltantes = (
       datos_originales.isna()
       .sum()
       .sort_values(ascending=False)
       .to_frame("valores_faltantes")
   )
   faltantes["porcentaje_faltantes"] = (
       faltantes["valores_faltantes"] / len(datos_originales) * 100
   )

   print("\nValores faltantes:")
   display(faltantes)
   ```

4. Revise estadísticas descriptivas de columnas numéricas y categóricas:

   ```python
   print("Estadísticas de variables numéricas:")
   display(datos_originales.describe().T)

   print("Resumen de variables categóricas:")
   display(datos_originales.describe(include="object").T)
   ```

5. Estandarice nombres de columnas, si el archivo contiene mayúsculas, espacios o nombres alternativos. Ejecute esta celda:

   ```python
   datos = datos_originales.copy()

   datos.columns = (
       datos.columns
       .str.strip()
       .str.lower()
       .str.replace(" ", "_")
       .str.replace("-", "_")
   )

   aliases = {
       "numero_solicitudes": "cantidad_solicitudes",
       "solicitudes": "cantidad_solicitudes",
       "n_solicitudes": "cantidad_solicitudes",
       "complejidad": "complejidad_caso",
       "nivel_complejidad": "complejidad_caso",
       "experiencia_agente": "experiencia_agente_anios",
       "experiencia_agente_años": "experiencia_agente_anios",
       "anios_experiencia_agente": "experiencia_agente_anios",
       "años_experiencia_agente": "experiencia_agente_anios",
       "tiempo_atencion": "tiempo_atencion_minutos",
       "tiempo_minutos": "tiempo_atencion_minutos",
       "costo_atencion": "costo_atencion_usd",
       "costo_operativo_usd": "costo_atencion_usd",
       "canal": "canal_atencion",
   }

   datos = datos.rename(columns={
       columna: aliases[columna]
       for columna in datos.columns
       if columna in aliases
   })

   columnas_requeridas = [
       "cantidad_solicitudes",
       "complejidad_caso",
       "experiencia_agente_anios",
       "tiempo_atencion_minutos",
       "costo_atencion_usd",
   ]

   faltantes_esquema = [
       columna for columna in columnas_requeridas
       if columna not in datos.columns
   ]

   if faltantes_esquema:
       raise ValueError(
           "Faltan columnas requeridas para este laboratorio: "
           f"{faltantes_esquema}\n"
           f"Columnas encontradas: {datos.columns.tolist()}"
       )

   print("Columnas normalizadas:")
   print(datos.columns.tolist())
   ```

6. Cree gráficos exploratorios para examinar relaciones entre las variables cuantitativas:

   ```python
   columnas_numericas = [
       "cantidad_solicitudes",
       "complejidad_caso",
       "experiencia_agente_anios",
       "tiempo_atencion_minutos",
       "costo_atencion_usd",
   ]

   sns.pairplot(
       datos[columnas_numericas].dropna(),
       corner=True,
       diag_kind="hist",
       plot_kws={"alpha": 0.55, "s": 30},
   )
   plt.suptitle("Relaciones entre variables numéricas", y=1.02)
   plt.show()
   ```

7. Visualice específicamente la relación para el modelo univariable:

   ```python
   plt.figure(figsize=(8, 5))
   sns.scatterplot(
       data=datos,
       x="cantidad_solicitudes",
       y="tiempo_atencion_minutos",
       alpha=0.7,
   )
   plt.title("Solicitudes asociadas y tiempo de atención")
   plt.xlabel("Cantidad de solicitudes")
   plt.ylabel("Tiempo de atención (minutos)")
   plt.show()
   ```

**Resultado esperado:**

- Se observa el número de filas y columnas del conjunto original.
- Se identifican posibles valores faltantes y tipos de datos inconsistentes.
- Se confirma la disponibilidad de las variables requeridas.
- El gráfico de dispersión permite observar si existe una tendencia aproximadamente lineal entre cantidad de solicitudes y tiempo de atención.

**Verificación:**

Responda en una celda Markdown del notebook:

```markdown
### Observación inicial

- Variable objetivo del modelo univariable: `tiempo_atencion_minutos`.
- Característica principal del modelo univariable: `cantidad_solicitudes`.
- Variable objetivo del modelo multivariable: `costo_atencion_usd`.
- Posibles características multivariables: `cantidad_solicitudes`, `complejidad_caso` y `experiencia_agente_anios`.
- Las variables seleccionadas están disponibles antes del cierre de la atención: sí/no, justificar.
```

> No use como predictor una variable conocida únicamente después de finalizar la atención, por ejemplo el costo final, el tiempo real final u otra variable derivada directamente del objetivo.

---

### Paso 4. Preparar y guardar el conjunto de datos procesado

**Objetivo:** construir un conjunto limpio, documentar las decisiones de preparación y guardar la versión procesada sin alterar los datos de origen.

**Instrucciones:**

1. Agregue una celda Markdown:

   ```markdown
   ## 2. Preparación de datos

   Se eliminarán registros duplicados y filas sin valores en las variables necesarias para este laboratorio. Esta decisión evita imputar objetivos de regresión y mantiene la trazabilidad del proceso.
   ```

2. Ejecute la siguiente celda para convertir tipos y revisar rangos:

   ```python
   columnas_numericas = [
       "cantidad_solicitudes",
       "complejidad_caso",
       "experiencia_agente_anios",
       "tiempo_atencion_minutos",
       "costo_atencion_usd",
   ]

   for columna in columnas_numericas:
       datos[columna] = pd.to_numeric(datos[columna], errors="coerce")

   print("Valores mínimos:")
   display(datos[columnas_numericas].min().to_frame("mínimo"))

   print("Valores máximos:")
   display(datos[columnas_numericas].max().to_frame("máximo"))
   ```

3. Limpie duplicados, faltantes y valores operativamente imposibles:

   ```python
   filas_iniciales = len(datos)

   datos_preparados = datos.copy()
   datos_preparados = datos_preparados.drop_duplicates()

   columnas_modelado = columnas_numericas
   datos_preparados = datos_preparados.dropna(subset=columnas_modelado)

   condiciones_validas = (
       (datos_preparados["cantidad_solicitudes"] >= 0)
       & (datos_preparados["complejidad_caso"] >= 0)
       & (datos_preparados["experiencia_agente_anios"] >= 0)
       & (datos_preparados["tiempo_atencion_minutos"] > 0)
       & (datos_preparados["costo_atencion_usd"] > 0)
   )

   datos_preparados = datos_preparados.loc[condiciones_validas].copy()

   filas_finales = len(datos_preparados)

   print(f"Filas originales: {filas_iniciales}")
   print(f"Filas preparadas: {filas_finales}")
   print(f"Filas excluidas: {filas_iniciales - filas_finales}")

   if filas_finales < 20:
       raise ValueError(
           "Quedaron menos de 20 registros preparados. Revise las reglas de limpieza "
           "y la calidad del archivo de entrada."
       )

   display(datos_preparados.head())
   ```

4. Guarde el conjunto procesado:

   ```python
   datos_preparados.to_csv(RUTA_PROCESSED, index=False)

   print(f"Archivo preparado guardado en: {RUTA_PROCESSED}")
   print(f"Dimensiones finales: {datos_preparados.shape}")
   ```

5. Compruebe que el archivo puede cargarse correctamente:

   ```python
   verificacion_preparados = pd.read_csv(RUTA_PROCESSED)

   assert len(verificacion_preparados) == len(datos_preparados)
   assert list(verificacion_preparados.columns) == list(datos_preparados.columns)

   print("Verificación completada: archivo procesado legible y consistente.")
   ```

**Resultado esperado:**

- Se crea `data/processed/atenciones_preparadas.csv`.
- El archivo procesado no contiene valores nulos en las columnas de modelado.
- Los valores de tiempo y costo son positivos.
- El archivo original permanece intacto en `data/raw`.

**Verificación:**

Ejecute:

```python
print(datos_preparados[columnas_numericas].isna().sum())
print(datos_preparados[columnas_numericas].describe().T)
```

Todas las columnas de modelado deben mostrar cero valores faltantes.

---

### Paso 5. Definir una partición reproducible de entrenamiento y prueba

**Objetivo:** separar datos para entrenamiento y prueba antes de ajustar escaladores o modelos, evitando fuga de información.

**Instrucciones:**

1. Agregue una celda Markdown:

   ```markdown
   ## 3. Partición reproducible

   El conjunto de entrenamiento se utiliza para aprender parámetros y calcular estadísticas de escalamiento. El conjunto de prueba se reserva para una evaluación final sobre casos no vistos por el modelo.
   ```

2. Ejecute la partición reproducible:

   ```python
   INDICE_ENTRENAMIENTO, INDICE_PRUEBA = train_test_split(
       datos_preparados.index,
       test_size=0.20,
       random_state=42,
   )

   datos_entrenamiento = datos_preparados.loc[INDICE_ENTRENAMIENTO].copy()
   datos_prueba = datos_preparados.loc[INDICE_PRUEBA].copy()

   print("Registros de entrenamiento:", len(datos_entrenamiento))
   print("Registros de prueba:", len(datos_prueba))
   print(
       "Proporción de prueba:",
       round(len(datos_prueba) / len(datos_preparados), 3)
   )
   ```

3. Verifique que no existen registros compartidos entre las particiones:

   ```python
   indices_compartidos = set(datos_entrenamiento.index).intersection(
       set(datos_prueba.index)
   )

   assert len(indices_compartidos) == 0
   assert len(datos_entrenamiento) + len(datos_prueba) == len(datos_preparados)

   print("Partición válida: no hay índices compartidos.")
   ```

**Resultado esperado:**

Aproximadamente 80 % de los registros pertenecen al conjunto de entrenamiento y 20 % al conjunto de prueba.

**Verificación:**

Confirme que la salida indica:

```text
Partición válida: no hay índices compartidos.
```

> La semilla `random_state=42` permite que todas las personas obtengan la misma partición a partir del mismo archivo preparado.

---

### Paso 6. Implementar regresión lineal univariable con NumPy

**Objetivo:** predecir `tiempo_atencion_minutos` a partir de `cantidad_solicitudes`, implementando hipótesis, costo y descenso del gradiente vectorizados.

**Instrucciones:**

1. Agregue una celda Markdown:

   ```markdown
   ## 4. Regresión lineal univariable implementada manualmente

   Se modelará el tiempo de atención a partir de la cantidad de solicitudes asociadas.

   \[
   \hat{y} = \theta_0 + \theta_1x
   \]

   Se minimizará la función:

   \[
   J(\theta) = \frac{1}{2m}\sum_{i=1}^{m}(\hat{y}^{(i)} - y^{(i)})^2
   \]

   El factor \(1/2\) simplifica la derivada y no cambia el mínimo de la función.
   ```

2. Prepare los arreglos de entrenamiento y prueba. La columna de unos representa el intercepto \(\theta_0\):

   ```python
   x_train_uni = datos_entrenamiento["cantidad_solicitudes"].to_numpy(
       dtype=float
   ).reshape(-1, 1)

   y_train_uni = datos_entrenamiento["tiempo_atencion_minutos"].to_numpy(
       dtype=float
   ).reshape(-1, 1)

   x_test_uni = datos_prueba["cantidad_solicitudes"].to_numpy(
       dtype=float
   ).reshape(-1, 1)

   y_test_uni = datos_prueba["tiempo_atencion_minutos"].to_numpy(
       dtype=float
   ).reshape(-1, 1)

   X_train_uni = np.c_[np.ones((len(x_train_uni), 1)), x_train_uni]
   X_test_uni = np.c_[np.ones((len(x_test_uni), 1)), x_test_uni]

   print("Forma de X entrenamiento:", X_train_uni.shape)
   print("Forma de y entrenamiento:", y_train_uni.shape)
   ```

3. Implemente la hipótesis, la función de costo y el descenso del gradiente:

   ```python
   def predecir_lineal(X, theta):
       """Calcula y_estimado = X @ theta."""
       return X @ theta


   def costo_mse(X, y, theta):
       """
       Calcula J(theta) = 1/(2m) * suma((X @ theta - y)^2)
       de forma vectorizada.
       """
       m = len(y)
       errores = predecir_lineal(X, theta) - y
       return float((errores.T @ errores) / (2 * m))


   def descenso_gradiente(X, y, alpha=0.01, iteraciones=3000):
       """
       Ajusta theta mediante descenso del gradiente vectorizado.
       Devuelve parámetros finales e historial de costo.
       """
       m, n = X.shape
       theta = np.zeros((n, 1))
       historial_costo = []

       for _ in range(iteraciones):
           errores = predecir_lineal(X, theta) - y
           gradiente = (X.T @ errores) / m
           theta = theta - alpha * gradiente
           historial_costo.append(costo_mse(X, y, theta))

       return theta, historial_costo
   ```

4. Entrene el modelo. Si el costo aumenta o llega a `nan`, reduzca `alpha` a `0.001`.

   ```python
   theta_uni, historial_uni = descenso_gradiente(
       X_train_uni,
       y_train_uni,
       alpha=0.001,
       iteraciones=10000,
   )

   print(f"Intercepto theta_0: {theta_uni[0, 0]:.4f}")
   print(f"Pendiente theta_1: {theta_uni[1, 0]:.4f}")
   print(f"Costo inicial: {historial_uni[0]:.4f}")
   print(f"Costo final: {historial_uni[-1]:.4f}")
   ```

5. Grafique la evolución del costo:

   ```python
   plt.figure(figsize=(8, 5))
   plt.plot(historial_uni, color="tab:blue")
   plt.title("Evolución de la función de costo: modelo univariable")
   plt.xlabel("Iteración")
   plt.ylabel("Costo J(θ)")
   plt.tight_layout()
   plt.savefig(
       RUTA_RESULTS / "curva_costo_univariable.png",
       dpi=150,
       bbox_inches="tight",
   )
   plt.show()
   ```

6. Grafique la recta aprendida sobre los datos de entrenamiento:

   ```python
   orden = np.argsort(x_train_uni[:, 0])
   x_ordenado = x_train_uni[orden]
   X_ordenado = np.c_[np.ones((len(x_ordenado), 1)), x_ordenado]
   y_recta = predecir_lineal(X_ordenado, theta_uni)

   plt.figure(figsize=(8, 5))
   plt.scatter(
       x_train_uni,
       y_train_uni,
       alpha=0.65,
       label="Observaciones de entrenamiento",
   )
   plt.plot(
       x_ordenado,
       y_recta,
       color="crimson",
       linewidth=2,
       label="Recta de regresión",
   )
   plt.title("Regresión univariable: solicitudes y tiempo de atención")
   plt.xlabel("Cantidad de solicitudes")
   plt.ylabel("Tiempo de atención (minutos)")
   plt.legend()
   plt.tight_layout()
   plt.savefig(
       RUTA_RESULTS / "recta_regresion_univariable.png",
       dpi=150,
       bbox_inches="tight",
   )
   plt.show()
   ```

7. Interprete los parámetros en una celda Markdown. Use los valores obtenidos por su ejecución:

   ```markdown
   ### Interpretación del modelo univariable

   - El intercepto estimado es `theta_0 = ...`. Representa el tiempo estimado cuando la cantidad de solicitudes es cero. Debe interpretarse con cautela si el valor cero no está representado en los datos históricos.
   - La pendiente estimada es `theta_1 = ...`. El modelo estima que una solicitud adicional se asocia, en promedio, con un cambio de aproximadamente `theta_1` minutos en el tiempo de atención.
   - Esta asociación no demuestra causalidad y puede estar afectada por complejidad, experiencia del agente, canal u otras variables no incluidas.
   ```

**Resultado esperado:**

- El costo disminuye progresivamente durante el entrenamiento.
- Se obtiene un intercepto y una pendiente numéricos.
- La recta de regresión representa la tendencia lineal aprendida.
- Se crean los archivos `results/curva_costo_univariable.png` y `results/recta_regresion_univariable.png`.

**Verificación:**

Ejecute:

```python
assert np.isfinite(theta_uni).all()
assert np.isfinite(historial_uni).all()
assert historial_uni[-1] < historial_uni[0]

print("Modelo univariable entrenado correctamente.")
```

---

### Paso 7. Evaluar el modelo univariable sobre datos no vistos

**Objetivo:** calcular métricas de error sobre el conjunto de prueba e interpretar el rendimiento operativo del modelo de tiempo.

**Instrucciones:**

1. Defina una función reutilizable para calcular métricas:

   ```python
   def calcular_metricas(y_real, y_predicho):
       """Devuelve métricas de regresión en un diccionario."""
       y_real = np.ravel(y_real)
       y_predicho = np.ravel(y_predicho)

       return {
           "MAE": mean_absolute_error(y_real, y_predicho),
           "MSE": mean_squared_error(y_real, y_predicho),
           "RMSE": root_mean_squared_error(y_real, y_predicho),
           "R2": r2_score(y_real, y_predicho),
       }
   ```

2. Genere predicciones de prueba y calcule métricas:

   ```python
   y_pred_uni_test = predecir_lineal(X_test_uni, theta_uni)

   metricas_uni = calcular_metricas(y_test_uni, y_pred_uni_test)

   print("Métricas del modelo univariable en prueba:")
   for nombre, valor in metricas_uni.items():
       print(f"{nombre}: {valor:.4f}")
   ```

3. Cree un gráfico de valores reales frente a predicciones:

   ```python
   y_real_uni = y_test_uni.ravel()
   y_pred_uni = y_pred_uni_test.ravel()

   limite_inferior = min(y_real_uni.min(), y_pred_uni.min())
   limite_superior = max(y_real_uni.max(), y_pred_uni.max())

   plt.figure(figsize=(6, 6))
   plt.scatter(y_real_uni, y_pred_uni, alpha=0.7)
   plt.plot(
       [limite_inferior, limite_superior],
       [limite_inferior, limite_superior],
       linestyle="--",
       color="crimson",
       label="Predicción perfecta",
   )
   plt.title("Tiempo real frente a tiempo predicho")
   plt.xlabel("Tiempo real (minutos)")
   plt.ylabel("Tiempo predicho (minutos)")
   plt.legend()
   plt.tight_layout()
   plt.show()
   ```

4. Documente la interpretación:

   ```markdown
   ### Interpretación de métricas del modelo de tiempo

   - **MAE:** error absoluto promedio en minutos. Es una medida directamente comunicable a operación.
   - **MSE:** promedio de errores al cuadrado; penaliza con mayor intensidad errores grandes.
   - **RMSE:** error típico aproximado en minutos, conservando la unidad del objetivo.
   - **R²:** proporción de variabilidad del tiempo de atención explicada por el modelo respecto a predecir siempre el promedio de entrenamiento.

   Un R² bajo, negativo o una RMSE alta sugieren que `cantidad_solicitudes` por sí sola no resume toda la complejidad operativa de una atención.
   ```

**Resultado esperado:**

Se muestran MAE, MSE, RMSE y R² del modelo univariable sobre datos reservados para prueba.

**Verificación:**

Compruebe que todas las métricas son finitas:

```python
assert all(np.isfinite(valor) for valor in metricas_uni.values())
print("Métricas univariables calculadas correctamente.")
```

---

### Paso 8. Crear características multivariables y escalar sin fuga de información

**Objetivo:** preparar características numéricas para predecir costo, incorporando una interacción justificada y estandarizando únicamente con estadísticas del conjunto de entrenamiento.

**Instrucciones:**

1. Agregue una celda Markdown:

   ```markdown
   ## 5. Modelo multivariable para costo operativo

   Se predecirá `costo_atencion_usd` usando características disponibles antes de la finalización del caso.

   Además de las variables originales, se creará la interacción:

   \[
   \text{solicitudes\_por\_complejidad} =
   \text{cantidad\_solicitudes} \times \text{complejidad\_caso}
   \]

   Esta característica representa que el efecto de tener más solicitudes puede ser mayor cuando el caso es más complejo. Es una hipótesis operativa que debe contrastarse con las métricas de prueba.
   ```

2. Defina una función para crear la misma ingeniería de características en entrenamiento y prueba:

   ```python
   def crear_caracteristicas_costo(df):
       """Crea características numéricas reproducibles para el modelo de costo."""
       resultado = df[
           [
               "cantidad_solicitudes",
               "complejidad_caso",
               "experiencia_agente_anios",
           ]
       ].copy()

       resultado["solicitudes_por_complejidad"] = (
           resultado["cantidad_solicitudes"]
           * resultado["complejidad_caso"]
       )

       return resultado
   ```

3. Cree matrices de características y objetivos:

   ```python
   X_train_multi_df = crear_caracteristicas_costo(datos_entrenamiento)
   X_test_multi_df = crear_caracteristicas_costo(datos_prueba)

   y_train_multi = datos_entrenamiento["costo_atencion_usd"].to_numpy(
       dtype=float
   ).reshape(-1, 1)

   y_test_multi = datos_prueba["costo_atencion_usd"].to_numpy(
       dtype=float
   ).reshape(-1, 1)

   nombres_caracteristicas_multi = X_train_multi_df.columns.tolist()

   print("Características del modelo multivariable:")
   print(nombres_caracteristicas_multi)
   display(X_train_multi_df.head())
   ```

4. Ajuste `StandardScaler` exclusivamente con los datos de entrenamiento:

   ```python
   escalador = StandardScaler()

   X_train_multi_escalado = escalador.fit_transform(X_train_multi_df)
   X_test_multi_escalado = escalador.transform(X_test_multi_df)

   X_train_multi = np.c_[
       np.ones((X_train_multi_escalado.shape[0], 1)),
       X_train_multi_escalado,
   ]

   X_test_multi = np.c_[
       np.ones((X_test_multi_escalado.shape[0], 1)),
       X_test_multi_escalado,
   ]

   print("Promedios de entrenamiento después del escalamiento:")
   print(np.round(X_train_multi_escalado.mean(axis=0), 8))

   print("\nDesviaciones estándar de entrenamiento después del escalamiento:")
   print(np.round(X_train_multi_escalado.std(axis=0), 8))
   ```

5. Registre las estadísticas de escalamiento como evidencia de que el conjunto de prueba no fue usado para `fit`:

   ```python
   estadisticas_escalamiento = pd.DataFrame({
       "caracteristica": nombres_caracteristicas_multi,
       "media_entrenamiento": escalador.mean_,
       "desviacion_estandar_entrenamiento": escalador.scale_,
   })

   display(estadisticas_escalamiento)
   ```

**Resultado esperado:**

- Se crea la característica `solicitudes_por_complejidad`.
- Las características de entrenamiento escaladas tienen media cercana a cero y desviación estándar cercana a uno.
- El escalador se ajusta con `fit_transform` únicamente en entrenamiento y se aplica a prueba con `transform`.

**Verificación:**

Ejecute:

```python
assert X_train_multi.shape[1] == len(nombres_caracteristicas_multi) + 1
assert X_test_multi.shape[1] == X_train_multi.shape[1]
assert np.isfinite(X_train_multi).all()
assert np.isfinite(X_test_multi).all()

print("Escalamiento y matrices multivariables verificados.")
```

---

### Paso 9. Entrenar y evaluar la regresión multivariable manual

**Objetivo:** usar descenso del gradiente vectorizado para estimar el costo operativo y analizar errores de predicción.

**Instrucciones:**

1. Entrene el modelo multivariable usando las funciones definidas en el Paso 6:

   ```python
   theta_multi, historial_multi = descenso_gradiente(
       X_train_multi,
       y_train_multi,
       alpha=0.05,
       iteraciones=5000,
   )

   print("Costo inicial:", f"{historial_multi[0]:.6f}")
   print("Costo final:", f"{historial_multi[-1]:.6f}")
   print("\nParámetros aprendidos:")
   print("Intercepto:", f"{theta_multi[0, 0]:.4f}")

   for nombre, coeficiente in zip(
       nombres_caracteristicas_multi,
       theta_multi[1:, 0],
   ):
       print(f"{nombre}: {coeficiente:.4f}")
   ```

2. Genere predicciones y métricas de prueba:

   ```python
   y_pred_multi_test = predecir_lineal(X_test_multi, theta_multi)

   metricas_multi = calcular_metricas(y_test_multi, y_pred_multi_test)

   print("Métricas del modelo multivariable en prueba:")
   for nombre, valor in metricas_multi.items():
       print(f"{nombre}: {valor:.4f}")
   ```

3. Construya una tabla de residuos y ordene los casos con mayor error absoluto:

   ```python
   resultados_prueba = datos_prueba.copy()

   resultados_prueba["tiempo_real_minutos"] = y_test_uni.ravel()
   resultados_prueba["tiempo_predicho_univariable"] = y_pred_uni
   resultados_prueba["residuo_tiempo"] = (
       resultados_prueba["tiempo_real_minutos"]
       - resultados_prueba["tiempo_predicho_univariable"]
   )

   resultados_prueba["costo_real_usd"] = y_test_multi.ravel()
   resultados_prueba["costo_predicho_multivariable"] = y_pred_multi_test.ravel()
   resultados_prueba["residuo_costo"] = (
       resultados_prueba["costo_real_usd"]
       - resultados_prueba["costo_predicho_multivariable"]
   )
   resultados_prueba["error_absoluto_costo"] = (
       resultados_prueba["residuo_costo"].abs()
   )

   casos_error_alto = resultados_prueba.sort_values(
       "error_absoluto_costo",
       ascending=False,
   ).head(10)

   display(
       casos_error_alto[
           [
               "cantidad_solicitudes",
               "complejidad_caso",
               "experiencia_agente_anios",
               "costo_real_usd",
               "costo_predicho_multivariable",
               "residuo_costo",
               "error_absoluto_costo",
           ]
       ]
   )
   ```

4. Grafique los residuos del modelo de costo:

   ```python
   plt.figure(figsize=(8, 5))
   sns.scatterplot(
       data=resultados_prueba,
       x="costo_predicho_multivariable",
       y="residuo_costo",
       alpha=0.7,
   )
   plt.axhline(0, color="crimson", linestyle="--")
   plt.title("Residuos del modelo multivariable de costo")
   plt.xlabel("Costo predicho (USD)")
   plt.ylabel("Residuo: costo real - costo predicho (USD)")
   plt.tight_layout()
   plt.savefig(
       RUTA_RESULTS / "residuos_multivariable.png",
       dpi=150,
       bbox_inches="tight",
   )
   plt.show()
   ```

5. Agregue una interpretación en Markdown:

   ```markdown
   ### Interpretación de residuos

   - Un residuo positivo indica que el costo real fue mayor que el costo predicho: el modelo subestimó el costo.
   - Un residuo negativo indica que el costo real fue menor que el costo predicho: el modelo sobrestimó el costo.
   - Una nube de residuos sin patrón claro alrededor de cero es más consistente con una relación lineal adecuada.
   - Patrones curvos, abanicos de dispersión o grupos separados pueden indicar no linealidad, heterocedasticidad, segmentos operativos distintos o variables relevantes ausentes.
   ```

**Resultado esperado:**

- El costo de entrenamiento disminuye y permanece finito.
- Se obtienen métricas de costo sobre datos de prueba.
- Se identifican los diez casos con mayor error absoluto.
- Se crea `results/residuos_multivariable.png`.

**Verificación:**

Ejecute:

```python
assert np.isfinite(theta_multi).all()
assert np.isfinite(historial_multi).all()
assert historial_multi[-1] < historial_multi[0]
assert all(np.isfinite(valor) for valor in metricas_multi.values())

print("Modelo multivariable manual evaluado correctamente.")
```

---

### Paso 10. Comparar con `LinearRegression` de scikit-learn

**Objetivo:** verificar que la implementación manual converge hacia una solución compatible con la regresión lineal de scikit-learn.

**Instrucciones:**

1. Entrene el modelo de referencia con las mismas características escaladas. No incluya manualmente la columna de unos, porque `LinearRegression` calcula el intercepto mediante `fit_intercept=True`.

   ```python
   modelo_sklearn = LinearRegression(fit_intercept=True)

   modelo_sklearn.fit(
       X_train_multi_escalado,
       y_train_multi.ravel(),
   )

   y_pred_sklearn_test = modelo_sklearn.predict(X_test_multi_escalado)

   metricas_sklearn = calcular_metricas(
       y_test_multi,
       y_pred_sklearn_test,
   )

   print("Métricas de scikit-learn en prueba:")
   for nombre, valor in metricas_sklearn.items():
       print(f"{nombre}: {valor:.4f}")
   ```

2. Compare parámetros manuales y parámetros de scikit-learn:

   ```python
   comparacion_coeficientes = pd.DataFrame({
       "parametro": ["intercepto"] + nombres_caracteristicas_multi,
       "manual_descenso_gradiente": theta_multi.ravel(),
       "sklearn_linear_regression": np.r_[
           modelo_sklearn.intercept_,
           modelo_sklearn.coef_,
       ],
   })

   comparacion_coeficientes["diferencia_absoluta"] = (
       comparacion_coeficientes["manual_descenso_gradiente"]
       - comparacion_coeficientes["sklearn_linear_regression"]
   ).abs()

   display(comparacion_coeficientes)
   ```

3. Compare predicciones:

   ```python
   diferencia_maxima_predicciones = np.max(
       np.abs(y_pred_multi_test.ravel() - y_pred_sklearn_test)
   )

   print(
       "Diferencia máxima absoluta entre predicciones:",
       f"{diferencia_maxima_predicciones:.8f}",
   )
   ```

4. Documente una explicación en una celda Markdown:

   ```markdown
   ### Comparación entre implementaciones

   `LinearRegression` calcula una solución de mínimos cuadrados mediante métodos numéricos optimizados. La implementación manual utiliza descenso del gradiente, que se aproxima a esa solución de manera iterativa.

   Las pequeñas diferencias pueden explicarse por:

   - número limitado de iteraciones;
   - tasa de aprendizaje seleccionada;
   - tolerancias numéricas;
   - precisión de punto flotante.

   Si las métricas y predicciones son cercanas, la implementación manual es consistente con la referencia de scikit-learn.
   ```
   
**Resultado esperado:**

Las métricas y predicciones del modelo manual y de `LinearRegression` son cercanas. El modelo de scikit-learn puede mostrar diferencias pequeñas por la estrategia de optimización utilizada.

**Verificación:**

Ejecute:

```python
assert np.isfinite(diferencia_maxima_predicciones)

comparacion_metricas = pd.DataFrame([
    {"modelo": "manual_multivariable", **metricas_multi},
    {"modelo": "sklearn_multivariable", **metricas_sklearn},
])

display(comparacion_metricas)
```

Si la diferencia entre resultados es grande, aumente iteraciones o reduzca moderadamente la tasa de aprendizaje, sin modificar la partición ni el escalamiento.

---

### Paso 11. Guardar métricas, coeficientes y predicciones reproducibles

**Objetivo:** conservar resultados reutilizables para prácticas posteriores y facilitar la auditoría del experimento.

**Instrucciones:**

1. Guarde métricas de todos los modelos:

   ```python
   metricas_modelos = pd.DataFrame([
       {
           "modelo": "univariable_manual_tiempo",
           "objetivo": "tiempo_atencion_minutos",
           "conjunto_evaluacion": "prueba",
           **metricas_uni,
       },
       {
           "modelo": "multivariable_manual_costo",
           "objetivo": "costo_atencion_usd",
           "conjunto_evaluacion": "prueba",
           **metricas_multi,
       },
       {
           "modelo": "multivariable_sklearn_costo",
           "objetivo": "costo_atencion_usd",
           "conjunto_evaluacion": "prueba",
           **metricas_sklearn,
       },
   ])

   metricas_modelos.to_csv(
       RUTA_RESULTS / "metricas_modelos.csv",
       index=False,
   )

   display(metricas_modelos)
   ```

2. Guarde coeficientes de los modelos:

   ```python
   coeficientes_univariable = pd.DataFrame({
       "modelo": "univariable_manual_tiempo",
       "parametro": ["intercepto", "cantidad_solicitudes"],
       "coeficiente": theta_uni.ravel(),
   })

   coeficientes_multivariable_manual = pd.DataFrame({
       "modelo": "multivariable_manual_costo",
       "parametro": ["intercepto"] + nombres_caracteristicas_multi,
       "coeficiente": theta_multi.ravel(),
   })

   coeficientes_multivariable_sklearn = pd.DataFrame({
       "modelo": "multivariable_sklearn_costo",
       "parametro": ["intercepto"] + nombres_caracteristicas_multi,
       "coeficiente": np.r_[
           modelo_sklearn.intercept_,
           modelo_sklearn.coef_,
       ],
   })

   coeficientes_modelos = pd.concat(
       [
           coeficientes_univariable,
           coeficientes_multivariable_manual,
           coeficientes_multivariable_sklearn,
       ],
       ignore_index=True,
   )

   coeficientes_modelos.to_csv(
       RUTA_RESULTS / "coeficientes_modelos.csv",
       index=False,
   )

   display(coeficientes_modelos)
   ```

3. Guarde las predicciones de prueba:

   ```python
   resultados_prueba.to_csv(
       RUTA_RESULTS / "predicciones_prueba.csv",
       index=False,
   )

   print(
       "Archivo de predicciones guardado en:",
       RUTA_RESULTS / "predicciones_prueba.csv",
   )
   ```

4. Guarde un resumen técnico en formato JSON:

   ```python
   resumen_modelos = {
       "laboratorio": "01-00-01",
       "archivo_entrada": str(RUTA_RAW),
       "archivo_procesado": str(RUTA_PROCESSED),
       "semilla_particion": 42,
       "proporcion_prueba": 0.20,
       "modelo_univariable": {
           "objetivo": "tiempo_atencion_minutos",
           "caracteristica": "cantidad_solicitudes",
           "alpha": 0.001,
           "iteraciones": 10000,
           "metricas_prueba": {
               clave: float(valor)
               for clave, valor in metricas_uni.items()
           },
       },
       "modelo_multivariable": {
           "objetivo": "costo_atencion_usd",
           "caracteristicas": nombres_caracteristicas_multi,
           "escalamiento": "StandardScaler ajustado solo con entrenamiento",
           "alpha": 0.05,
           "iteraciones": 5000,
           "metricas_prueba_manual": {
               clave: float(valor)
               for clave, valor in metricas_multi.items()
           },
           "metricas_prueba_sklearn": {
               clave: float(valor)
               for clave, valor in metricas_sklearn.items()
           },
       },
   }

   with open(
       RUTA_RESULTS / "resumen_modelos.json",
       "w",
       encoding="utf-8",
   ) as archivo_json:
       json.dump(
           resumen_modelos,
           archivo_json,
           ensure_ascii=False,
           indent=2,
       )

   print("Resumen JSON guardado correctamente.")
   ```

5. Guarde el notebook desde JupyterLab mediante **File → Save Notebook** o con `Ctrl+S` / `Cmd+S`.

**Resultado esperado:**

La carpeta `results` contiene métricas, coeficientes, predicciones, gráficas y un resumen técnico del experimento.

**Verificación:**

Ejecute:

```python
archivos_esperados = [
    "metricas_modelos.csv",
    "predicciones_prueba.csv",
    "coeficientes_modelos.csv",
    "curva_costo_univariable.png",
    "recta_regresion_univariable.png",
    "residuos_multivariable.png",
    "resumen_modelos.json",
]

for archivo in archivos_esperados:
    ruta = RUTA_RESULTS / archivo
    assert ruta.exists(), f"No se encontró: {ruta}"

print("Todos los archivos de resultados fueron creados.")
```

## Validación y pruebas

Complete la siguiente lista antes de entregar el laboratorio:

- [ ] El archivo original sigue disponible en `data/raw/atenciones_historicas.csv`.
- [ ] El archivo `data/processed/atenciones_preparadas.csv` fue creado y puede cargarse con pandas.
- [ ] El notebook se guardó con el nombre exacto `01_prediccion_tiempos_costos_regresion_lineal.ipynb`.
- [ ] La partición utiliza `test_size=0.20` y `random_state=42`.
- [ ] La implementación manual contiene funciones vectorizadas para predicción, costo y descenso del gradiente.
- [ ] El costo final del descenso del gradiente es menor que el costo inicial en ambos modelos.
- [ ] El escalador fue ajustado solo con el conjunto de entrenamiento.
- [ ] El modelo multivariable incluye la interacción `solicitudes_por_complejidad`.
- [ ] Se calcularon MAE, MSE, RMSE y R² sobre el conjunto de prueba.
- [ ] Se comparó el modelo manual multivariable con `LinearRegression`.
- [ ] Se crearon los archivos requeridos en la carpeta `results`.

Ejecute además la siguiente celda final de validación:

```python
assert RUTA_RAW.exists()
assert RUTA_PROCESSED.exists()
assert (RUTA_PROYECTO / "notebooks" / "01_prediccion_tiempos_costos_regresion_lineal.ipynb").exists()
assert not datos_preparados[columnas_numericas].isna().any().any()
assert len(indices_compartidos) == 0
assert historial_uni[-1] < historial_uni[0]
assert historial_multi[-1] < historial_multi[0]
assert all(np.isfinite(valor) for valor in metricas_uni.values())
assert all(np.isfinite(valor) for valor in metricas_multi.values())
assert all(np.isfinite(valor) for valor in metricas_sklearn.values())

print("VALIDACIÓN FINAL SUPERADA: el flujo del laboratorio es reproducible.")
```

## Solución de problemas

### Problema 1. `FileNotFoundError` al cargar `atenciones_historicas.csv`

**Síntoma:** aparece un mensaje similar a:

```text
FileNotFoundError: No se encontró el archivo oficial en ...
```

**Causa:** el archivo no fue copiado a `data/raw/atenciones_historicas.csv`, tiene otro nombre, o el notebook se está ejecutando desde una ubicación distinta a `notebooks`.

**Solución:**

1. Confirme que el archivo se llama exactamente:

   ```text
   atenciones_historicas.csv
   ```

2. Confirme que se encuentra en:

   ```text
   ~/taller_ml_supervisado_parte1/data/raw/
   ```

3. Verifique la ruta actual dentro del notebook:

   ```python
   print(Path.cwd())
   print(RUTA_PROYECTO)
   print(RUTA_RAW)
   ```

4. Si el notebook no está dentro de `notebooks`, muévalo a esa carpeta o ajuste temporalmente `RUTA_PROYECTO` para que apunte a la raíz correcta del proyecto.

### Problema 2. El costo del descenso del gradiente aumenta, es `nan` o los coeficientes son extremadamente grandes

**Síntoma:** la curva de costo crece, aparecen valores `nan`, `inf` o predicciones poco realistas.

**Causa:** la tasa de aprendizaje (`alpha`) es demasiado alta, las características no fueron escaladas para el modelo multivariable, o existen valores atípicos/extremos que requieren revisión.

**Solución:**

1. Para el modelo univariable, reduzca la tasa de aprendizaje:

   ```python
   theta_uni, historial_uni = descenso_gradiente(
       X_train_uni,
       y_train_uni,
       alpha=0.0001,
       iteraciones=20000,
   )
   ```

2. Para el modelo multivariable, confirme que usa `X_train_multi_escalado` y `X_test_multi_escalado` generados por `StandardScaler`.

3. Verifique que no hay valores no finitos:

   ```python
   print(np.isfinite(X_train_multi).all())
   print(np.isfinite(y_train_multi).all())
   ```

4. Revise estadísticos y casos extremos antes de cambiar reglas de limpieza:

   ```python
   display(datos_preparados[columnas_numericas].describe(percentiles=[0.01, 0.50, 0.99]).T)
   ```

## Limpieza

1. Guarde el notebook con `Ctrl+S` o `Cmd+S`.

2. Cierre el servidor de JupyterLab desde la terminal con:

   ```text
   Ctrl+C
   ```

3. Cuando aparezca la confirmación, escriba:

   ```text
   y
   ```

4. Desactive el entorno virtual:

   ```bash
   deactivate
   ```

5. No elimine los directorios `data`, `notebooks`, `results` ni `requirements`, ya que serán reutilizados en prácticas posteriores.

6. No elimine ni modifique:

   ```text
   data/raw/atenciones_historicas.csv
   ```

## Resumen

En este laboratorio se construyó un flujo reproducible de regresión lineal para un contexto de atención. Se exploraron datos históricos, se generó un archivo preparado, se separaron conjuntos de entrenamiento y prueba, y se implementó una regresión univariable mediante descenso del gradiente para predecir tiempos de atención.

También se desarrolló un modelo multivariable para costos operativos usando escalamiento de características e ingeniería de una interacción entre solicitudes y complejidad. La evaluación mediante MAE, MSE, RMSE y R² permitió cuantificar el desempeño sobre casos no vistos, mientras que el análisis de residuos ayudó a identificar errores altos y posibles limitaciones del modelo.

Los coeficientes expresan asociaciones estimadas en los datos históricos, no relaciones causales garantizadas. Por ello, las predicciones deben utilizarse como apoyo para planificación y asignación de recursos, complementándose siempre con criterio profesional, reglas operativas, consideraciones éticas y políticas de atención.

### Recursos opcionales

- [Documentación de `LinearRegression` de scikit-learn](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html)
- [Documentación de `StandardScaler` de scikit-learn](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html)
- [Guía de métricas de regresión en scikit-learn](https://scikit-learn.org/stable/modules/model_evaluation.html#regression-metrics)
- [Documentación de NumPy](https://numpy.org/doc/)
