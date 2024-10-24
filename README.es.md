# Plantilla de Proyecto de Ciencia de Datos

Esta plantilla está diseñada para impulsar proyectos de ciencia de datos proporcionando una configuración básica para conexiones de base de datos, procesamiento de datos, y desarrollo de modelos de aprendizaje automático. Incluye una organización estructurada de carpetas para tus conjuntos de datos y un conjunto de paquetes de Python predefinidos necesarios para la mayoría de las tareas de ciencia de datos.

## Estructura

El proyecto está organizado de la siguiente manera:

- `app.py` - El script principal de Python que ejecutas para tu proyecto.
- `explore.py` - Un notebook para que puedas hacer tus exploraciones, idealmente el codigo de este notebook se migra hacia app.py para subir a produccion.
- `utils.py` - Este archivo contiene código de utilidad para operaciones como conexiones de base de datos.
- `requirements.txt` - Este archivo contiene la lista de paquetes de Python necesarios.
- `models/` - Este directorio debería contener tus clases de modelos SQLAlchemy.
- `data/` - Este directorio contiene los siguientes subdirectorios:
  - `interim/` - Para datos intermedios que han sido transformados.
  - `processed/` - Para los datos finales a utilizar para el modelado.
  - `raw/` - Para datos brutos sin ningún procesamiento.

## Configuración

**Prerrequisitos**

Asegúrate de tener Python 3.11+ instalado en tu máquina. También necesitarás pip para instalar los paquetes de Python.

**Instalación**

Clona el repositorio del proyecto en tu máquina local.

Navega hasta el directorio del proyecto e instala los paquetes de Python requeridos:

```bash
pip install -r requirements.txt
```

**Crear una base de datos (si es necesario)**

Crea una nueva base de datos dentro del motor Postgres personalizando y ejecutando el siguiente comando: `$ createdb -h localhost -U <username> <db_name>`
Conéctate al motor Postgres para usar tu base de datos, manipular tablas y datos: `$ psql -h localhost -U <username> <db_name>`
NOTA: Recuerda revisar la información del archivo ./.env para obtener el nombre de usuario y db_name.

¡Una vez que estés dentro de PSQL podrás crear tablas, hacer consultas, insertar, actualizar o eliminar datos y mucho más!

**Variables de entorno**

Crea un archivo .env en el directorio raíz del proyecto para almacenar tus variables de entorno, como tu cadena de conexión a la base de datos:

```makefile
DATABASE_URL="your_database_connection_url_here"
```

## Ejecutando la Aplicación

Para ejecutar la aplicación, ejecuta el script app.py desde la raíz del directorio del proyecto:

```bash
python app.py
```

## Añadiendo Modelos

Para añadir clases de modelos SQLAlchemy, crea nuevos archivos de script de Python dentro del directorio models/. Estas clases deben ser definidas de acuerdo a tu esquema de base de datos.

Definición del modelo de ejemplo (`models/example_model.py`):

```py
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy import Column, Integer, String

Base = declarative_base()

class ExampleModel(Base):
    __tablename__ = 'example_table'
    id = Column(Integer, primary_key=True)
    name = Column(String)

```
## Optimizando el modelo Naive Bayes

### MultinomialNB

El modelo Multinomial Naive Bayes es adecuado para datos que representan frecuencias de eventos (como el conteo de palabras en un texto). Es muy utilizado en tareas de clasificación de texto.

#### **Hiperparametros**

```py
from sklearn.naive_bayes import MultinomialNB

model = MultinomialNB(alpha=1.0)
model = MultinomialNB(alpha=1.0, fit_prior=True, class_prior=None)

```

`alpha`:

  - Descripción: Este es el parámetro de suavizado de Laplace o suavizado de Lidstone. Controla la cantidad de suavizado que se aplica para evitar que probabilidades se vuelvan cero por la ausencia de algunas características.
  - Valores típicos:
    - alpha=1.0 (por defecto): Aplica suavizado de Laplace.
    - Si alpha=0.0, no hay suavizado (pero esto puede causar problemas si alguna palabra o característica no aparece en los datos de entrenamiento).
    - Valores menores a 1 reducen el nivel de suavizado, valores mayores a 1 aumentan el suavizado.
  - Uso: Se ajusta para evitar problemas con datos escasos o de baja frecuencia. En clasificación de texto, ayuda a que el modelo maneje mejor las palabras raras.

`fit_prior`:

  - Descripción: Indica si se debe aprender las probabilidades previas (priors) de las clases a partir de los datos.
  - Valores:
    - True (por defecto): Se calculan las probabilidades de clase según la frecuencia de las clases en los datos.
    - False: Se asumen probabilidades de clase uniformes.

`class_prior`:

  - Descripción: Puedes definir manualmente las probabilidades previas de las clases.
  - Valores:
    - Por defecto es None, y se calculan automáticamente a partir de los datos. Si defines un arreglo de probabilidades aquí, se usará en lugar de los valores calculados.

### GaussianNB

El modelo Gaussian Naive Bayes es adecuado para datos continuos que siguen una distribución normal o gaussiana. Se usa mucho en aplicaciones donde las características son valores continuos en lugar de frecuencias o conteos.

#### **Hiperparametros**

```py
from sklearn.naive_bayes import GaussianNB

model = GaussianNB(priors=None)
model = GaussianNB(var_smoothing=1e-9)

```

`priors`:

  - Descripción: Puedes especificar las probabilidades previas para las clases. Si no se especifican, se calculan a partir de los datos.
  - Valores:
    - Por defecto es None (las probabilidades se estiman de los datos).
    - Si lo defines, debe ser un arreglo con las probabilidades para cada clase.

`var_smoothing`:

  - Descripción: Añade una pequeña cantidad a la varianza calculada de cada característica para evitar divisiones por cero en la probabilidad de la distribución gaussiana.
  - Valores típicos:
    - var_smoothing=1e-9 (por defecto): Se suma una pequeña constante a la varianza de cada característica.
    - Este valor puede ajustarse si los datos son muy sensibles o si las características tienen varianzas muy pequeñas.

### BernoulliNB

El modelo Bernoulli Naive Bayes es adecuado para datos binarios (0 y 1), donde las características representan la presencia o ausencia de algún evento (por ejemplo, si una palabra está presente o no en un documento).

#### **Hiperparametros**

```py
from sklearn.naive_bayes import BernoulliNB

model = BernoulliNB(alpha=1.0)
model = BernoulliNB(alpha=1.0, binarize=0.0)

```
`alpha`:

  - Descripción: Controla el suavizado de Laplace, similar al MultinomialNB.
  - Valores típicos:
    - alpha=1.0 (por defecto): Suavizado de Laplace.
    - Se puede ajustar para manejar características de baja frecuencia, igual que en MultinomialNB.

`binarize`:

  - Descripción: Este parámetro es importante para convertir automáticamente las características en valores binarios (0 y 1). Cualquier valor superior a binarize se convierte en 1, y cualquier valor inferior o igual se convierte en 0.
  - Valores:
    - binarize=0.0 (por defecto): Se asume que las características ya están binarizadas.
    - Puedes ajustar este valor si los datos no son estrictamente binarios.

`fit_prior`:

  - Igual que en MultinomialNB, indica si se deben ajustar las probabilidades previas de las clases.
  - Valores: True (por defecto) o False.

`class_prior`:

  - Igual que en MultinomialNB, puedes especificar manualmente las probabilidades previas de las clases.


## Trabajando con Datos

Puedes colocar tus conjuntos de datos brutos en el directorio data/raw, conjuntos de datos intermedios en data/interim, y los conjuntos de datos procesados listos para el análisis en data/processed.

Para procesar datos, puedes modificar el script app.py para incluir tus pasos de procesamiento de datos, utilizando pandas para la manipulación y análisis de datos.

## Contribuyentes

Esta plantilla fue construida como parte del [Data Science and Machine Learning Bootcamp](https://4geeksacademy.com/us/coding-bootcamps/datascience-machine-learning) de 4Geeks Academy por [Alejandro Sanchez](https://twitter.com/alesanchezr) y muchos otros contribuyentes. Descubre más sobre [los programas BootCamp de 4Geeks Academy](https://4geeksacademy.com/us/programs) aquí.

Otras plantillas y recursos como este se pueden encontrar en la página de GitHub de la escuela.