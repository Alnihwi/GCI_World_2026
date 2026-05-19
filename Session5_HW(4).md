# Homework 4 – Supervised Learning

## Question
Using a dataset from MyAnimeList, run a simple linear regression where the target column name is `Y_column` and the explanatory column name is `X_column`, both passed as function arguments, and compute the coefficient of determination (R-squared). Do not transform variables or preprocess data; use the data as-is.

---

## Inputs/Outputs

```python
homework(anime_data_extracted: pd.DataFrame, X_column: str, Y_column: str) -> float
```

---

## Solution

```python
import numpy as np
import pandas as pd
from sklearn import linear_model

# Libraries for retrieving data from the web and handling zip files
import requests, zipfile
import io

# Specify the url with data
url = 'https://github.com/Hernan4444/MyAnimeList-Database/archive/refs/heads/master.zip'

# Acquire data from the url
r = requests.get(url, stream=True)

# Read and extract the zipfile
z = zipfile.ZipFile(io.BytesIO(r.content))
z.extractall()

# Load and clean the data
anime_data = pd.read_csv('MyAnimeList-Database-master/data/anime.csv')
anime_data_extracted = anime_data[anime_data['Score'] != 'Unknown'].copy()
anime_data_extracted['Score'] = pd.to_numeric(anime_data_extracted['Score'])

def homework(anime_data_extracted, X_column, Y_column):

    # Select X and Y
    X = anime_data_extracted[[X_column]]
    Y = anime_data_extracted[Y_column]

    # Create linear regression model
    model = linear_model.LinearRegression()

    # Fit the model
    model.fit(X, Y)

    # Compute R-squared
    result = model.score(X, Y)

    return result
```

---

Example:

```python
result = homework(anime_data_extracted,
                  X_column='Members',
                  Y_column='Completed')

print(result)
```

```python
0.954XXXXXXXX
```
