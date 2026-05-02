# Homework 2 – Cleaning Data Using Pandas

## Question
Using the MyAnimeList dataset, compute the mean Score for each Type, and return the results sorted in descending order.

---

## Inputs/Outputs

```python
homework(anime_data_extracted: pd.DataFrame) -> pd.Series
```

---

## Solution

```python
import numpy as np
import pandas as pd
from pandas import DataFrame

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

def homework(anime_data_extracted):
    result = anime_data_extracted.groupby("Type")["Score"].mean()
    return result.sort_values(ascending=False)
```

---

Example:

```python
result = homework(anime_data_extracted)
print(result)
```
```python
Type
TV         6.8XXXXX
Special    6.5XXXXX
Movie      6.4XXXXX
OVA        6.3XXXXX
ONA        6.1XXXXX
Music      5.8XXXXX
Name: Score, dtype: float64
