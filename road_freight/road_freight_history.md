---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.2'
      jupytext_version: 1.4.2
  kernelspec:
    display_name: eurostats
    language: python
    name: eurostats
---

## Road/Rail Freight

Million tonne-kilometres by Year

Data Source: ITF (2020), https://data.oecd.org/transport/freight-transport.htm

Alternative: ["Goods transport", ITF Transport Statistics (database), https://doi.org/10.1787/g2g5557d-en (accessed on 17 May 2020).](https://stats.oecd.org/BrandedView.aspx?oecd_bv_id=trsprt-data-en&doi=g2g5557d-en)

1. Annual data for million tonne-kilometers


```python
import pandas as pd
import missingno as msno
```

```python
import matplotlib.pyplot as plt
import seaborn as sns;sns.set()
```

```python
!ls assets
```

Load Data

```python
df =pd.read_csv('assets/DP_LIVE_17052020151422582.csv')
```

## Data Quality

```python
msno.matrix(df)
```

Flag codes indicates:
```
    1. B: Break
    2. E: Estimated value
    3. P: Provisional value
```

```python
df["Flag Codes"].unique()
```

```python
df.head()
```

Check the columns

```python
df.LOCATION.unique()
```

```python
df.INDICATOR.unique()
```

```python
df.SUBJECT.unique()
```

```python
df.MEASURE.unique()
```

```python

```

## Road Freight Time Series

```python

```

```python
fig, ax = plt.subplots(figsize=(10,6.18))

df_rf = df.loc[
    (
        df.SUBJECT == 'ROAD'
    ) & (
        df.LOCATION.isin(
            df.groupby('LOCATION').sum().Value.sort_values().tail(10).index
        )
    )
]

sns.lineplot(
    x='TIME',
    y='Value',
    data=df_rf,
    hue='LOCATION',
    marker='o'
)

ax.set_yscale('log')
ax.set_ylabel('Million Tonne-Kilometers')
ax.set_xlabel('Year')
```

```python

```

## Investments

```python
road_infra_investment_data_path = "assets/ITF_INV-MTN_DATA_17052020174930980.csv"
```

```python
df_invest = pd.read_csv(road_infra_investment_data_path)
```

```python
df_invest.head()
```

```python
msno.matrix(df_invest)
```

Check data quality

```python
df_invest.VARIABLE.value_counts()
```

```python
df_invest.MEASURE.value_counts()
```

```python
df_invest_ri = df_invest.loc[
    (
        df_invest.VARIABLE == 'I-INV-RD'
    ) & (
        df_invest.MEASURE == 'EUR'
    ) & (
        df_invest.COUNTRY.isin(
            df_invest.groupby('COUNTRY').sum().Value.sort_values().tail(10).index
        )
    )
]
```

```python
fig, ax = plt.subplots(figsize=(10,6.18))

sns.lineplot(
    x='YEAR',
    y='Value',
    data=df_invest_ri,
    hue='COUNTRY',
    marker='o'
)

ax.set_yscale('log')
ax.set_ylabel('Investment in Euros')
ax.set_xlabel('Year')
```

```python

```
