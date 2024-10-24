# Data Science Project Boilerplate

This boilerplate is designed to kickstart data science projects by providing a basic setup for database connections, data processing, and machine learning model development. It includes a structured folder organization for your datasets and a set of pre-defined Python packages necessary for most data science tasks.

## Structure

The project is organized as follows:

- `app.py` - The main Python script that you run for your project.
- `explore.py` - A notebook to explore data, play around, visualize, clean, etc. Ideally the notebook code should be migrated to the app.py when moving to production.
- `utils.py` - This file contains utility code for operations like database connections.
- `requirements.txt` - This file contains the list of necessary python packages.
- `models/` - This directory should contain your SQLAlchemy model classes.
- `data/` - This directory contains the following subdirectories:
  - `interin/` - For intermediate data that has been transformed.
  - `processed/` - For the final data to be used for modeling.
  - `raw/` - For raw data without any processing.
 
    
## Setup

**Prerequisites**

Make sure you have Python 3.11+ installed on your. You will also need pip for installing the Python packages.

**Installation**

Clone the project repository to your local machine.

Navigate to the project directory and install the required Python packages:

```bash
pip install -r requirements.txt
```

**Create a database (if needed)**

Create a new database within the Postgres engine by customizing and executing the following command: `$ createdb -h localhost -U <username> <db_name>`
Connect to the Postgres engine to use your database, manipulate tables and data: `$ psql -h localhost -U <username> <db_name>`
NOTE: Remember to check the ./.env file information to get the username and db_name.

Once you are inside PSQL you will be able to create tables, make queries, insert, update or delete data and much more!

**Environment Variables**

Create a .env file in the project root directory to store your environment variables, such as your database connection string:

```makefile
DATABASE_URL="your_database_connection_url_here"
```

## Running the Application

To run the application, execute the app.py script from the root of the project directory:

```bash
python app.py
```

## Adding Models

To add SQLAlchemy model classes, create new Python script files inside the models/ directory. These classes should be defined according to your database schema.

Example model definition (`models/example_model.py`):

```py
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy import Column, Integer, String

Base = declarative_base()

class ExampleModel(Base):
    __tablename__ = 'example_table'
    id = Column(Integer, primary_key=True)
    name = Column(String)

```

## Optimizing the Naive Bayes model

### MultinomialNB

The Multinomial Naive Bayes model is suitable for data that represents event frequencies (such as word counts in a text). It is widely used in text classification tasks.

#### **Hyperparameters**

```py
from sklearn.naive_bayes import MultinomialNB

model = MultinomialNB(alpha=1.0)
model = MultinomialNB(alpha=1.0, fit_prior=True, class_prior=None)

```

`alpha`:

- Description: This is the Laplace smoothing or Lidstone smoothing parameter. It controls the amount of smoothing that is applied to prevent probabilities from becoming zero due to the absence of some features.
- Typical values:
- alpha=1.0 (default): Applies Laplace smoothing.
- If alpha=0.0, there is no smoothing (but this can cause problems if some word or feature does not appear in the training data).
- Values ​​less than 1 reduce the level of smoothing, values ​​greater than 1 increase the smoothing.
- Usage: Fits to avoid problems with sparse or low-frequency data. In text classification, it helps the model better handle rare words.

`fit_prior`:

- Description: Indicates whether to learn class prior probabilities from the data.
- Values:
- True (default): Class probabilities are calculated based on the frequency of the classes in the data.
- False: Uniform class probabilities are assumed.

`class_prior`:

- Description: You can manually define class prior probabilities.
- Values:
- Default is None, and they are automatically calculated from the data. If you define an array of probabilities here, it will be used instead of the calculated values.

### GaussianNB

The Gaussian Naive Bayes model is suitable for continuous data that follows a normal or Gaussian distribution. It is widely used in applications where the features are continuous values ​​rather than frequencies or counts.

#### **Hyperparameters**

```py
from sklearn.naive_bayes import GaussianNB

model = GaussianNB(priors=None)
model = GaussianNB(var_smoothing=1e-9)

```

`priors`:

- Description: You can specify prior probabilities for classes. If not specified, they are calculated from the data.
- Values:
- Default is None (probabilities are estimated from the data).
- If you define it, it must be an array with the probabilities for each class.

`var_smoothing`:

- Description: Adds a small amount to the calculated variance of each feature to avoid divisions by zero in the Gaussian distribution probability.
- Typical values:
- var_smoothing=1e-9 (default): A small constant is added to the variance of each feature.
- This value can be adjusted if the data is very sensitive or if the features have very small variances.

### BernoulliNB

The Bernoulli Naive Bayes model is suitable for binary data (0 and 1), where the features represent the presence or absence of some event (for example, whether a word is present or not in a document).

#### **Hyperparameters**

```py
from sklearn.naive_bayes import BernoulliNB

model = BernoulliNB(alpha=1.0)
model = BernoulliNB(alpha=1.0, binarize=0.0)

```
`alpha`:

- Description: Controls Laplace smoothing, similar to MultinomialNB.
- Typical values:
- alpha=1.0 (default): Laplace smoothing.
- Can be tuned to handle low frequency features, same as MultinomialNB.

`binarize`:

- Description: This parameter is important to automatically convert features to binary values ​​(0 and 1). Any value greater than binarize becomes 1, and any value less than or equal to it becomes 0.
- Values:
- binarize=0.0 (default): Features are assumed to be already binarized.
- You can adjust this value if the data is not strictly binary.

`fit_prior`:

- Same as in MultinomialNB, indicates whether to adjust the prior probabilities of the classes.
- Values: True (default) or False.

`class_prior`:

- Same as in MultinomialNB, you can manually specify the prior probabilities of the classes.


## Working with Data

You can place your raw datasets in the data/raw directory, intermediate datasets in data/interim, and the processed datasets ready for analysis in data/processed.

To process data, you can modify the app.py script to include your data processing steps, utilizing pandas for data manipulation and analysis.

## Contributors

This template was built as part of the 4Geeks Academy [Data Science and Machine Learning Bootcamp](https://4geeksacademy.com/us/coding-bootcamps/datascience-machine-learning) by [Alejandro Sanchez](https://twitter.com/alesanchezr) and many other contributors. Find out more about [4Geeks Academy's BootCamp programs](https://4geeksacademy.com/us/programs) here.

Other templates and resources like this can be found on the school GitHub page.
