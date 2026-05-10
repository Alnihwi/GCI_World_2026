# Homework 3 

## Question
Using the MyAnimeList dataset, perform a Pareto analysis (decile analysis) on a selected metric column.

The function should:

- Sort the anime in descending order based on the given metric column.
- Divide the sorted data into `n` equal groups using `pd.qcut()` on the ranked values with `method='first'`.
- Calculate the proportion each group contributes to the total value of the metric.
- Sort the group proportions in descending order.
- Rename the groups as `"Group 1"` to `"Group n"`.

Return the result as a `pd.Series`.

---

## Inputs/Outputs

```python
homework(anime_data: pd.DataFrame, metric_column: str, n: int) -> pd.Series
```
---

## Solution

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

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

# Load the anime data
anime_data_raw = pd.read_csv('MyAnimeList-Database-master/data/anime.csv')

# Preprocessing
columns_to_keep = [
    'MAL_ID',
    'Name',
    'Score',
    'Type',
    'Episodes',
    'Members',
    'Completed',
    'Watching',
    'Dropped',
    'Popularity'
]

anime_data = anime_data_raw[columns_to_keep].copy()

# Remove rows with missing or zero values
anime_data = anime_data.dropna(
    subset=['Members', 'Completed', 'Watching']
)

anime_data = anime_data[
    (anime_data['Members'] > 0) &
    (anime_data['Completed'] > 0) &
    (anime_data['Watching'] > 0)
]

anime_data = anime_data.reset_index(drop=True)

# Solution
def homework(anime_data, metric_column, n):

    # Sort data descending by the selected metric
    sorted_data = anime_data.sort_values(
        by=metric_column,
        ascending=False
    ).copy()

    # Rank values while preserving duplicate order
    ranks = sorted_data[metric_column].rank(
        method='first',
        ascending=False
    )

    # Divide data into n equal groups
    sorted_data['group'] = pd.qcut(
        ranks,
        q=n,
        labels=False
    )

    # Calculate total metric value
    total = sorted_data[metric_column].sum()

    # Compute group proportions
    proportions = (
        sorted_data
        .groupby('group')[metric_column]
        .sum()
        / total
    )

    # Sort proportions descending
    proportions = proportions.sort_values(
        ascending=False
    )

    # Rename index labels
    proportions.index = [
        f'Group {i+1}'
        for i in range(len(proportions))
    ]

    return proportions
```

--- 

# Example

```python
result = homework(anime_data, 'Completed', 5)
print(result)
```

```python
Group 1    0.94XXXX
Group 2    0.04XXXX
Group 3    0.009XXX
Group 4    0.001XXX
Group 5    0.0004XX
dtype: float64
