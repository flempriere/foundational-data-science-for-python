# Chapter 9: Pandas

- [Notes](#notes)
  - [About DataFrames](#about-dataframes)
  - [Creating DataFrames](#creating-dataframes)
    - [Creating a DataFrame from a
      Dictionary](#creating-a-dataframe-from-a-dictionary)
    - [Creating a DataFrame from a List of
      Lists](#creating-a-dataframe-from-a-list-of-lists)
    - [Creating a DataFrame from a
      File](#creating-a-dataframe-from-a-file)
  - [Interacting with DataFrame Data](#interacting-with-dataframe-data)
    - [Heads and Tails](#heads-and-tails)
    - [Descriptive Statistics](#descriptive-statistics)
    - [Accessing Data](#accessing-data)
      - [Bracket Syntax](#bracket-syntax)
      - [Optimised Access by Label](#optimised-access-by-label)
      - [Optimised Access by Index](#optimised-access-by-index)
    - [Masking and Filtering](#masking-and-filtering)
      - [Pandas Boolean Operators](#pandas-boolean-operators)
  - [Manipulating DataFrames](#manipulating-dataframes)
    - [Manipulating Data](#manipulating-data)
    - [The `replace` Method](#the-replace-method)
- [Summary](#summary)
- [Questions](#questions)

## Notes

- Pandas uses the NumPy array to create *dataframes*
- Dataframes are a specialised data structure for handling
  comma-separated data

### About DataFrames

- Dataframes consist of columns and rows
  - Each column is a `pandas.Series` object
- Dataframes can be viewed as two-dimensional NumPy array
  - Columns and indices are labelled
  - However, each column can have a different datatype
- More accurate to think of a dataframe as a collection of individual
  one-dimensional arrays
  - Each column is a series
  - Each series has a fixed data-type
- Most `numpy.ndarray` methods will work on a `pandas.Series` object
  - e.g. `min`, `max`, `mean`, `median`
- The usual `import` convention for pandas is `pd`

``` python
import pandas as pd

df = pd.DataFrame()
print(df)
```

    Empty DataFrame
    Columns: []
    Index: []

- You can create an empty dataframe but it’s generally better to create
  them with data directly

### Creating DataFrames

#### Creating a DataFrame from a Dictionary

- Dataframes can be created from a list of dictionaries or from a
  dictionary directly
  - Dictionary key’s are used to define the column names

``` python
import pandas as pd

first_names = ["Shanda", "Rolly", "Molly", "Frank", "Rip", "Steven", "Gwen", "Arthur"]
last_names = ["Smith", "Brocker", "Stein", "Bach", "Spencer", "De Wilde", "Mason", "Davis"]

ages = [43, 23, 78, 56, 26, 14, 46, 92]
data = {"first": first_names, "last": last_names, "age": ages}

participants = pd.DataFrame(data)
print(participants)
```

        first      last  age
    0  Shanda     Smith   43
    1   Rolly   Brocker   23
    2   Molly     Stein   78
    3   Frank      Bach   56
    4     Rip   Spencer   26
    5  Steven  De Wilde   14
    6    Gwen     Mason   46
    7  Arthur     Davis   92

#### Creating a DataFrame from a List of Lists

- Another creation mechanisms is via a list of lists
  - Each sub-list contains the data for one row
- This can then be passed to create a dataframe
  - The column’s are unlabelled
  - They can be supplied manually
  - Can do the same for the indices

``` python
import pandas as pd

data = [["Shanda", "Smith", 42],
        ["Rolly", "Brocker", 23],
        ["Molly", "Stein", 78],
        ["Frank", "Bach", 56],
        ["Rip", "Spencer", 26],
        ["Steven", "De Wilde", 14],
        ["Gwen", "Mason", 46],
        ["Arthur", "Davis", 92]]

column_names = ["First", "Last", "Ages"]
index_labels = ["a", "b", "c", "d", "e", "f", "g", "h"]
participants = pd.DataFrame(data, columns=column_names, index=index_labels)
print(participants)
```

        First      Last  Ages
    a  Shanda     Smith    42
    b   Rolly   Brocker    23
    c   Molly     Stein    78
    d   Frank      Bach    56
    e     Rip   Spencer    26
    f  Steven  De Wilde    14
    g    Gwen     Mason    46
    h  Arthur     Davis    92

#### Creating a DataFrame from a File

- Typically dataframes are created by loading in data from a file
- Pandas supports several different input file formats
  - CSV,
  - Excel,
  - HTML,
  - JSON,
  - SQL
- For example, to open a tab-separated data file
  - Note the use of `sep='\t'` is used to specify that tabs are the
    separator
  - By default, comma’s are used as the separator

``` python
import pandas as pd

gapminder = pd.read_csv("Data/gapminder.tsv", sep="\t")
print(gapminder)
```

              country continent  year  lifeExp       pop   gdpPercap
    0     Afghanistan      Asia  1952   28.801   8425333  779.445314
    1     Afghanistan      Asia  1957   30.332   9240934  820.853030
    2     Afghanistan      Asia  1962   31.997  10267083  853.100710
    3     Afghanistan      Asia  1967   34.020  11537966  836.197138
    4     Afghanistan      Asia  1972   36.088  13079460  739.981106
    ...           ...       ...   ...      ...       ...         ...
    1699     Zimbabwe    Africa  1987   62.351   9216418  706.157306
    1700     Zimbabwe    Africa  1992   60.377  10704340  693.420786
    1701     Zimbabwe    Africa  1997   46.809  11404948  792.449960
    1702     Zimbabwe    Africa  2002   39.989  11926563  672.038623
    1703     Zimbabwe    Africa  2007   43.487  12311143  469.709298

    [1704 rows x 6 columns]

> [!IMPORTANT]
>
> **Gap Minder**
>
> The [gapminder.tsv](./Data/gapminder.tsv) used as an example in this
> chapter is sourced from the [gapminder
> dataset](https://github.com/jennybc/gapminder) hosted on GitHub.

### Interacting with DataFrame Data

- Pandas offers a range of techniques for inspecting data
  - By row,
  - By column,
  - By cell
- Can also extract data by value
- A good way to start is by peeking at data, and generating some summary
  statistics

#### Heads and Tails

- You can examine the top rows of a dataset with the `head` method
  - By default, shows the top five rows, but the number can be specified

``` python
import pandas as pd

gapminder = pd.read_csv("Data/gapminder.tsv", sep="\t")
gapminder.head()
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
&#10;    .dataframe tbody tr th {
        vertical-align: top;
    }
&#10;    .dataframe thead th {
        text-align: right;
    }
</style>

|     | country     | continent | year | lifeExp | pop      | gdpPercap  |
|-----|-------------|-----------|------|---------|----------|------------|
| 0   | Afghanistan | Asia      | 1952 | 28.801  | 8425333  | 779.445314 |
| 1   | Afghanistan | Asia      | 1957 | 30.332  | 9240934  | 820.853030 |
| 2   | Afghanistan | Asia      | 1962 | 31.997  | 10267083 | 853.100710 |
| 3   | Afghanistan | Asia      | 1967 | 34.020  | 11537966 | 836.197138 |
| 4   | Afghanistan | Asia      | 1972 | 36.088  | 13079460 | 739.981106 |

</div>

- You can also show the bottom rows with the `tail` method
  - As with `head` defaults to five, but can be specified

``` python
import pandas as pd

gapminder = pd.read_csv("Data/gapminder.tsv", sep="\t")
gapminder.tail(3)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
&#10;    .dataframe tbody tr th {
        vertical-align: top;
    }
&#10;    .dataframe thead th {
        text-align: right;
    }
</style>

|      | country  | continent | year | lifeExp | pop      | gdpPercap  |
|------|----------|-----------|------|---------|----------|------------|
| 1701 | Zimbabwe | Africa    | 1997 | 46.809  | 11404948 | 792.449960 |
| 1702 | Zimbabwe | Africa    | 2002 | 39.989  | 11926563 | 672.038623 |
| 1703 | Zimbabwe | Africa    | 2007 | 43.487  | 12311143 | 469.709298 |

</div>

#### Descriptive Statistics

- `describe` produces summary statistics about a dataset
  - By default it can be called with no arguments

``` python
import pandas as pd

gapminder = pd.read_csv("Data/gapminder.tsv", sep="\t")
gapminder.describe()
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
&#10;    .dataframe tbody tr th {
        vertical-align: top;
    }
&#10;    .dataframe thead th {
        text-align: right;
    }
</style>

|       | year       | lifeExp     | pop          | gdpPercap     |
|-------|------------|-------------|--------------|---------------|
| count | 1704.00000 | 1704.000000 | 1.704000e+03 | 1704.000000   |
| mean  | 1979.50000 | 59.474439   | 2.960121e+07 | 7215.327081   |
| std   | 17.26533   | 12.917107   | 1.061579e+08 | 9857.454543   |
| min   | 1952.00000 | 23.599000   | 6.001100e+04 | 241.165876    |
| 25%   | 1965.75000 | 48.198000   | 2.793664e+06 | 1202.060309   |
| 50%   | 1979.50000 | 60.712500   | 7.023596e+06 | 3531.846988   |
| 75%   | 1993.25000 | 70.845500   | 1.958522e+07 | 9325.462346   |
| max   | 2007.00000 | 82.603000   | 1.318683e+09 | 113523.132900 |

</div>

- Also accepts optional arguments to control what data is reported
- e.g. `percentiles` to set which quantiles are shown

``` python
import pandas as pd

gapminder = pd.read_csv("Data/gapminder.tsv", sep="\t")
gapminder.describe(percentiles=[0.1, 0.9])
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
&#10;    .dataframe tbody tr th {
        vertical-align: top;
    }
&#10;    .dataframe thead th {
        text-align: right;
    }
</style>

|       | year       | lifeExp     | pop          | gdpPercap     |
|-------|------------|-------------|--------------|---------------|
| count | 1704.00000 | 1704.000000 | 1.704000e+03 | 1704.000000   |
| mean  | 1979.50000 | 59.474439   | 2.960121e+07 | 7215.327081   |
| std   | 17.26533   | 12.917107   | 1.061579e+08 | 9857.454543   |
| min   | 1952.00000 | 23.599000   | 6.001100e+04 | 241.165876    |
| 10%   | 1957.00000 | 41.510800   | 9.463671e+05 | 687.718361    |
| 90%   | 2002.00000 | 75.097000   | 5.480137e+07 | 19449.138209  |
| max   | 2007.00000 | 82.603000   | 1.318683e+09 | 113523.132900 |

</div>

- What if we want to calculate statistics for non-numeric objects?
  - Like the number of unique strings in a column
  - Can specify data types to include via the `include` parameter
    - Should be a sequence of datatypes (including)
      [NumPy](../Chapter_07/Chapter_07.qmd) types
      - In older NumPy ($\leq 1.20$), the internal `np.object` is used
        instead of the built-in `object`
    - Newer Pandas represents strings as `str` types
      - Older Pandas represents them as `object` so to include `string`
        columns, use `object`

``` python
import numpy as np
import pandas as pd

gapminder = pd.read_csv("Data/gapminder.tsv", sep="\t")
gapminder.describe(include=[str])
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
&#10;    .dataframe tbody tr th {
        vertical-align: top;
    }
&#10;    .dataframe thead th {
        text-align: right;
    }
</style>

|        | country     | continent |
|--------|-------------|-----------|
| count  | 1704        | 1704      |
| unique | 142         | 5         |
| top    | Afghanistan | Africa    |
| freq   | 12          | 624       |

</div>

- To include statistics for all column types use `"all"`

``` python
import pandas as pd

gapminder = pd.read_csv("Data/gapminder.tsv", sep="\t")
gapminder.describe(include="all")
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
&#10;    .dataframe tbody tr th {
        vertical-align: top;
    }
&#10;    .dataframe thead th {
        text-align: right;
    }
</style>

|        | country     | continent | year       | lifeExp     | pop          | gdpPercap     |
|--------|-------------|-----------|------------|-------------|--------------|---------------|
| count  | 1704        | 1704      | 1704.00000 | 1704.000000 | 1.704000e+03 | 1704.000000   |
| unique | 142         | 5         | NaN        | NaN         | NaN          | NaN           |
| top    | Afghanistan | Africa    | NaN        | NaN         | NaN          | NaN           |
| freq   | 12          | 624       | NaN        | NaN         | NaN          | NaN           |
| mean   | NaN         | NaN       | 1979.50000 | 59.474439   | 2.960121e+07 | 7215.327081   |
| std    | NaN         | NaN       | 17.26533   | 12.917107   | 1.061579e+08 | 9857.454543   |
| min    | NaN         | NaN       | 1952.00000 | 23.599000   | 6.001100e+04 | 241.165876    |
| 25%    | NaN         | NaN       | 1965.75000 | 48.198000   | 2.793664e+06 | 1202.060309   |
| 50%    | NaN         | NaN       | 1979.50000 | 60.712500   | 7.023596e+06 | 3531.846988   |
| 75%    | NaN         | NaN       | 1993.25000 | 70.845500   | 1.958522e+07 | 9325.462346   |
| max    | NaN         | NaN       | 2007.00000 | 82.603000   | 1.318683e+09 | 113523.132900 |

</div>

> [!TIP]
>
> **`NaN` and Summary Statistics**
>
> When a statistic is not appropriate for a given data column, such as
> the standard deviation of a string, a `NaN` is instead produced

#### Accessing Data

- `head`, `tail`, `describe` and `print` give useful methods for
  examining part of a dataframe as a whole
- The next step is to be able to access and examine select rows, columns
  or cells
- Pandas provides a couple of different mechanisms for data access

##### Bracket Syntax

- Columns and rows are accessed via bracket syntax
- Columns are accessed like dictionary keys
  - Specify the column name in the brackets

``` python
import pandas as pd

gapminder = pd.read_csv("Data/gapminder.tsv", sep="\t")
gapminder["pop"]
```

    0        8425333
    1        9240934
    2       10267083
    3       11537966
    4       13079460
              ...
    1699     9216418
    1700    10704340
    1701    11404948
    1702    11926563
    1703    12311143
    Name: pop, Length: 1704, dtype: int64

- This returns the `pd.Series` object
- Columns with names that don’t contain dashes, spaces and special
  characters can be accessed as attributes
  - Also can’t be accessed as an attribute if it would conflict with an
    existing attribute for the dataframe
  - For example we can see accessing `lifeExp` works but accessing `pop`
    fails because it conflicts with an existing method

``` python
import pandas as pd

gapminder = pd.read_csv("Data/gapminder.tsv", sep="\t")

print("gapminder.lifeExp:\n", gapminder.lifeExp)
print("gapminder.pop:\n", gapminder.pop)
```

    gapminder.lifeExp:
     0       28.801
    1       30.332
    2       31.997
    3       34.020
    4       36.088
             ...
    1699    62.351
    1700    60.377
    1701    46.809
    1702    39.989
    1703    43.487
    Name: lifeExp, Length: 1704, dtype: float64
    gapminder.pop:
     <bound method DataFrame.pop of           country continent  year  lifeExp       pop   gdpPercap
    0     Afghanistan      Asia  1952   28.801   8425333  779.445314
    1     Afghanistan      Asia  1957   30.332   9240934  820.853030
    2     Afghanistan      Asia  1962   31.997  10267083  853.100710
    3     Afghanistan      Asia  1967   34.020  11537966  836.197138
    4     Afghanistan      Asia  1972   36.088  13079460  739.981106
    ...           ...       ...   ...      ...       ...         ...
    1699     Zimbabwe    Africa  1987   62.351   9216418  706.157306
    1700     Zimbabwe    Africa  1992   60.377  10704340  693.420786
    1701     Zimbabwe    Africa  1997   46.809  11404948  792.449960
    1702     Zimbabwe    Africa  2002   39.989  11926563  672.038623
    1703     Zimbabwe    Africa  2007   43.487  12311143  469.709298

    [1704 rows x 6 columns]>

- To access multiple columns, rather than passing a single column name,
  we pass a list of column names

``` python
import pandas as pd

gapminder = pd.read_csv("Data/gapminder.tsv", sep="\t")
gapminder[["pop", "year"]]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
&#10;    .dataframe tbody tr th {
        vertical-align: top;
    }
&#10;    .dataframe thead th {
        text-align: right;
    }
</style>

|      | pop      | year |
|------|----------|------|
| 0    | 8425333  | 1952 |
| 1    | 9240934  | 1957 |
| 2    | 10267083 | 1962 |
| 3    | 11537966 | 1967 |
| 4    | 13079460 | 1972 |
| ...  | ...      | ...  |
| 1699 | 9216418  | 1987 |
| 1700 | 10704340 | 1992 |
| 1701 | 11404948 | 1997 |
| 1702 | 11926563 | 2002 |
| 1703 | 12311143 | 2007 |

<p>1704 rows × 2 columns</p>
</div>

- The bracket syntax is also used for row selection
  - Utilises the slice syntax similar to NumPy
- integer slices are interpreted as slices over the rows

``` python
import pandas as pd

gapminder = pd.read_csv("Data/gapminder.tsv", sep="\t")
gapminder[3:6]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
&#10;    .dataframe tbody tr th {
        vertical-align: top;
    }
&#10;    .dataframe thead th {
        text-align: right;
    }
</style>

|     | country     | continent | year | lifeExp | pop      | gdpPercap  |
|-----|-------------|-----------|------|---------|----------|------------|
| 3   | Afghanistan | Asia      | 1967 | 34.020  | 11537966 | 836.197138 |
| 4   | Afghanistan | Asia      | 1972 | 36.088  | 13079460 | 739.981106 |
| 5   | Afghanistan | Asia      | 1977 | 38.438  | 14880372 | 786.113360 |

</div>

- If row’s have labels then they can also be sliced
  - The endpoint is excluded (unlike label-based slices)

``` python
import pandas as pd

data = [
    ["Shanda", "Smith", 42],
    ["Rolly", "Brocker", 23],
    ["Molly", "Stein", 78],
    ["Frank", "Bach", 56],
    ["Rip", "Spencer", 26],
    ["Steven", "De Wilde", 14],
    ["Gwen", "Mason", 46],
    ["Arthur", "Davis", 92],
]

column_names = ["First", "Last", "Ages"]
index_labels = ["a", "b", "c", "d", "e", "f", "g", "h"]
participants = pd.DataFrame(data, columns=column_names, index=index_labels)
participants["a":"c"]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
&#10;    .dataframe tbody tr th {
        vertical-align: top;
    }
&#10;    .dataframe thead th {
        text-align: right;
    }
</style>

|     | First  | Last    | Ages |
|-----|--------|---------|------|
| a   | Shanda | Smith   | 42   |
| b   | Rolly  | Brocker | 23   |
| c   | Molly  | Stein   | 78   |

</div>

- The last option is to provide a list of Boolean masks
  - Must be one Boolean value for each row in the dataframe

``` python
import pandas as pd

data = [
    ["Shanda", "Smith", 42],
    ["Rolly", "Brocker", 23],
    ["Molly", "Stein", 78],
    ["Frank", "Bach", 56],
    ["Rip", "Spencer", 26],
    ["Steven", "De Wilde", 14],
    ["Gwen", "Mason", 46],
    ["Arthur", "Davis", 92],
]

column_names = ["First", "Last", "Ages"]
index_labels = ["a", "b", "c", "d", "e", "f", "g", "h"]
participants = pd.DataFrame(data, columns=column_names, index=index_labels)

mask = [False, True, True, False, False, True, False, False]
participants[mask]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
&#10;    .dataframe tbody tr th {
        vertical-align: top;
    }
&#10;    .dataframe thead th {
        text-align: right;
    }
</style>

|     | First  | Last     | Ages |
|-----|--------|----------|------|
| b   | Rolly  | Brocker  | 23   |
| c   | Molly  | Stein    | 78   |
| f   | Steven | De Wilde | 14   |

</div>

- Bracket syntax is useful for interactive sessions
- Convenient and easy to read syntax
- Not optimised for efficiency or large dataset interrogation
- For production code or large sets instead the `loc` and `iloc`
  indexers are preferred
  - These use a similar bracket-based indexing
  - `loc` is for label-based locating
  - `iloc` is for index-based locating

##### Optimised Access by Label

- `loc` accepts a single label
  - Associated values for that row are returned

``` python
import pandas as pd

data = [
    ["Shanda", "Smith", 42],
    ["Rolly", "Brocker", 23],
    ["Molly", "Stein", 78],
    ["Frank", "Bach", 56],
    ["Rip", "Spencer", 26],
    ["Steven", "De Wilde", 14],
    ["Gwen", "Mason", 46],
    ["Arthur", "Davis", 92],
]

column_names = ["First", "Last", "Ages"]
index_labels = ["a", "b", "c", "d", "e", "f", "g", "h"]
participants = pd.DataFrame(data, columns=column_names, index=index_labels)

participants.loc["c"]
```

    First    Molly
    Last     Stein
    Ages        78
    Name: c, dtype: object

- `loc` also supports slice-style indexing
  - Last label is included

``` python
import pandas as pd

data = [
    ["Shanda", "Smith", 42],
    ["Rolly", "Brocker", 23],
    ["Molly", "Stein", 78],
    ["Frank", "Bach", 56],
    ["Rip", "Spencer", 26],
    ["Steven", "De Wilde", 14],
    ["Gwen", "Mason", 46],
    ["Arthur", "Davis", 92],
]

column_names = ["First", "Last", "Ages"]
index_labels = ["a", "b", "c", "d", "e", "f", "g", "h"]
participants = pd.DataFrame(data, columns=column_names, index=index_labels)

participants.loc["c":"f"]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
&#10;    .dataframe tbody tr th {
        vertical-align: top;
    }
&#10;    .dataframe thead th {
        text-align: right;
    }
</style>

|     | First  | Last     | Ages |
|-----|--------|----------|------|
| c   | Molly  | Stein    | 78   |
| d   | Frank  | Bach     | 56   |
| e   | Rip    | Spencer  | 26   |
| f   | Steven | De Wilde | 14   |

</div>

- Or provide a Boolean mask

``` python
import pandas as pd

data = [
    ["Shanda", "Smith", 42],
    ["Rolly", "Brocker", 23],
    ["Molly", "Stein", 78],
    ["Frank", "Bach", 56],
    ["Rip", "Spencer", 26],
    ["Steven", "De Wilde", 14],
    ["Gwen", "Mason", 46],
    ["Arthur", "Davis", 92],
]

column_names = ["First", "Last", "Ages"]
index_labels = ["a", "b", "c", "d", "e", "f", "g", "h"]
participants = pd.DataFrame(data, columns=column_names, index=index_labels)

mask = [False, True, True, False, False, True, False, False]
participants.loc[mask]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
&#10;    .dataframe tbody tr th {
        vertical-align: top;
    }
&#10;    .dataframe thead th {
        text-align: right;
    }
</style>

|     | First  | Last     | Ages |
|-----|--------|----------|------|
| b   | Rolly  | Brocker  | 23   |
| c   | Molly  | Stein    | 78   |
| f   | Steven | De Wilde | 14   |

</div>

- To also select a subset of columns can provide an optional second
  argument to `loc`
- For example, to select all rows and the column `"First"`

``` python
import pandas as pd

data = [
    ["Shanda", "Smith", 42],
    ["Rolly", "Brocker", 23],
    ["Molly", "Stein", 78],
    ["Frank", "Bach", 56],
    ["Rip", "Spencer", 26],
    ["Steven", "De Wilde", 14],
    ["Gwen", "Mason", 46],
    ["Arthur", "Davis", 92],
]

column_names = ["First", "Last", "Ages"]
index_labels = ["a", "b", "c", "d", "e", "f", "g", "h"]
participants = pd.DataFrame(data, columns=column_names, index=index_labels)

participants.loc[:, "First"]
```

    a    Shanda
    b     Rolly
    c     Molly
    d     Frank
    e       Rip
    f    Steven
    g      Gwen
    h    Arthur
    Name: First, dtype: str

- To select all rows up to row `"c"` associated with the columns
  `"ages"` and `"last"`
  - Take a slice against the rows, then
  - a list of column names

``` python
import pandas as pd

data = [
    ["Shanda", "Smith", 42],
    ["Rolly", "Brocker", 23],
    ["Molly", "Stein", 78],
    ["Frank", "Bach", 56],
    ["Rip", "Spencer", 26],
    ["Steven", "De Wilde", 14],
    ["Gwen", "Mason", 46],
    ["Arthur", "Davis", 92],
]

column_names = ["First", "Last", "Ages"]
index_labels = ["a", "b", "c", "d", "e", "f", "g", "h"]
participants = pd.DataFrame(data, columns=column_names, index=index_labels)

participants.loc[:"c", ["Ages", "Last"]]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
&#10;    .dataframe tbody tr th {
        vertical-align: top;
    }
&#10;    .dataframe thead th {
        text-align: right;
    }
</style>

|     | Ages | Last    |
|-----|------|---------|
| a   | 42   | Smith   |
| b   | 23   | Brocker |
| c   | 78   | Stein   |

</div>

- Or provide a list of Booleans corresponding to each column

``` python
import pandas as pd

data = [
    ["Shanda", "Smith", 42],
    ["Rolly", "Brocker", 23],
    ["Molly", "Stein", 78],
    ["Frank", "Bach", 56],
    ["Rip", "Spencer", 26],
    ["Steven", "De Wilde", 14],
    ["Gwen", "Mason", 46],
    ["Arthur", "Davis", 92],
]

column_names = ["First", "Last", "Ages"]
index_labels = ["a", "b", "c", "d", "e", "f", "g", "h"]
participants = pd.DataFrame(data, columns=column_names, index=index_labels)

participants.loc[:"c", [False, True, True]]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
&#10;    .dataframe tbody tr th {
        vertical-align: top;
    }
&#10;    .dataframe thead th {
        text-align: right;
    }
</style>

|     | Last    | Ages |
|-----|---------|------|
| a   | Smith   | 42   |
| b   | Brocker | 23   |
| c   | Stein   | 78   |

</div>

##### Optimised Access by Index

- `iloc` works like `loc` but is designed to work with numeric indices
- To specify a single row, provide a single number

``` python
import pandas as pd

data = [
    ["Shanda", "Smith", 42],
    ["Rolly", "Brocker", 23],
    ["Molly", "Stein", 78],
    ["Frank", "Bach", 56],
    ["Rip", "Spencer", 26],
    ["Steven", "De Wilde", 14],
    ["Gwen", "Mason", 46],
    ["Arthur", "Davis", 92],
]

column_names = ["First", "Last", "Ages"]
index_labels = ["a", "b", "c", "d", "e", "f", "g", "h"]
participants = pd.DataFrame(data, columns=column_names, index=index_labels)

participants.iloc[3]
```

    First    Frank
    Last      Bach
    Ages        56
    Name: d, dtype: object

- Specify multiple rows using slice syntax
  - Does *not* include the endpoint

``` python
import pandas as pd

data = [
    ["Shanda", "Smith", 42],
    ["Rolly", "Brocker", 23],
    ["Molly", "Stein", 78],
    ["Frank", "Bach", 56],
    ["Rip", "Spencer", 26],
    ["Steven", "De Wilde", 14],
    ["Gwen", "Mason", 46],
    ["Arthur", "Davis", 92],
]

column_names = ["First", "Last", "Ages"]
index_labels = ["a", "b", "c", "d", "e", "f", "g", "h"]
participants = pd.DataFrame(data, columns=column_names, index=index_labels)

participants.iloc[1:4] # takes rows 1, 2, 3
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
&#10;    .dataframe tbody tr th {
        vertical-align: top;
    }
&#10;    .dataframe thead th {
        text-align: right;
    }
</style>

|     | First | Last    | Ages |
|-----|-------|---------|------|
| b   | Rolly | Brocker | 23   |
| c   | Molly | Stein   | 78   |
| d   | Frank | Bach    | 56   |

</div>

- Columns can also be specified by a second slice

``` python
import pandas as pd

data = [
    ["Shanda", "Smith", 42],
    ["Rolly", "Brocker", 23],
    ["Molly", "Stein", 78],
    ["Frank", "Bach", 56],
    ["Rip", "Spencer", 26],
    ["Steven", "De Wilde", 14],
    ["Gwen", "Mason", 46],
    ["Arthur", "Davis", 92],
]

column_names = ["First", "Last", "Ages"]
index_labels = ["a", "b", "c", "d", "e", "f", "g", "h"]
participants = pd.DataFrame(data, columns=column_names, index=index_labels)

participants.iloc[1:4, :2]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
&#10;    .dataframe tbody tr th {
        vertical-align: top;
    }
&#10;    .dataframe thead th {
        text-align: right;
    }
</style>

|     | First | Last    |
|-----|-------|---------|
| b   | Rolly | Brocker |
| c   | Molly | Stein   |
| d   | Frank | Bach    |

</div>

#### Masking and Filtering

- Pandas data can be selected by value
- Can examine columns based on conditional expressions
- For example, might want to see which rows correspond to the year
  $2002$

``` python
import pandas as pd

gapminder = pd.read_csv("Data/gapminder.tsv", sep="\t")
gapminder.year == 2002
```

    0       False
    1       False
    2       False
    3       False
    4       False
            ...
    1699    False
    1700    False
    1701    False
    1702     True
    1703    False
    Name: year, Length: 1704, dtype: bool

- The result is a `pd.Series` object
  - Boolean result for each column indicating if it satisfied the value
- These can be passed to the `loc` indexer to filter results
  - Every `true` value is selected

``` python
import pandas as pd

gapminder = pd.read_csv("Data/gapminder.tsv", sep="\t")
year_mask = gapminder.loc[:, "pop"] > 25_000_000
data_pop_above_25mil = gapminder.loc[year_mask]
print(data_pop_above_25mil)
```

              country continent  year  lifeExp       pop    gdpPercap
    10    Afghanistan      Asia  2002   42.129  25268405   726.734055
    11    Afghanistan      Asia  2007   43.828  31889923   974.580338
    32        Algeria    Africa  1992   67.744  26298373  5023.216647
    33        Algeria    Africa  1997   69.152  29072015  4797.295051
    34        Algeria    Africa  2002   70.994  31287142  5288.040382
    ...           ...       ...   ...      ...       ...          ...
    1651      Vietnam      Asia  1987   62.820  62826491   820.799445
    1652      Vietnam      Asia  1992   67.662  69940728   989.023149
    1653      Vietnam      Asia  1997   70.672  76048996  1385.896769
    1654      Vietnam      Asia  2002   73.017  80908147  1764.456677
    1655      Vietnam      Asia  2007   74.249  85262356  2441.576404

    [350 rows x 6 columns]

- `describe` can be used on individual columns
  - For example, we might want to filter down to the bottom quartile of
    life expectancy

``` python
import pandas as pd

gapminder = pd.read_csv("Data/gapminder.tsv", sep="\t")
gapminder.lifeExp.describe()
```

    count    1704.000000
    mean       59.474439
    std        12.917107
    min        23.599000
    25%        48.198000
    50%        60.712500
    75%        70.845500
    max        82.603000
    Name: lifeExp, dtype: float64

- Can create a mask using this condition

``` python
import pandas as pd

gapminder = pd.read_csv("Data/gapminder.tsv", sep="\t")
life_exp_mask = gapminder.loc[:, "lifeExp"] <= 48.198000
print(life_exp_mask)
```

    0        True
    1        True
    2        True
    3        True
    4        True
            ...
    1699    False
    1700    False
    1701     True
    1702     True
    1703     True
    Name: lifeExp, Length: 1704, dtype: bool

- This mask might then be used to create a deduced dataframe

``` python
import pandas as pd

gapminder = pd.read_csv("Data/gapminder.tsv", sep="\t")
life_exp_mask = gapminder.loc[:, "lifeExp"] <= 48.198000

life_exp_df = gapminder.loc[life_exp_mask]
print(life_exp_df)
```

              country continent  year  lifeExp       pop    gdpPercap
    0     Afghanistan      Asia  1952   28.801   8425333   779.445314
    1     Afghanistan      Asia  1957   30.332   9240934   820.853030
    2     Afghanistan      Asia  1962   31.997  10267083   853.100710
    3     Afghanistan      Asia  1967   34.020  11537966   836.197138
    4     Afghanistan      Asia  1972   36.088  13079460   739.981106
    ...           ...       ...   ...      ...       ...          ...
    1690       Zambia    Africa  2002   39.193  10595811  1071.613938
    1691       Zambia    Africa  2007   42.384  11746035  1271.211593
    1701     Zimbabwe    Africa  1997   46.809  11404948   792.449960
    1702     Zimbabwe    Africa  2002   39.989  11926563   672.038623
    1703     Zimbabwe    Africa  2007   43.487  12311143   469.709298

    [426 rows x 6 columns]

- We might then use `unique` on the `country` column to see which
  countries show up the lowest quartile for life expectancy at some
  point over the data collection period

``` python
import pandas as pd

gapminder = pd.read_csv("Data/gapminder.tsv", sep="\t")
life_exp_mask = gapminder.loc[:, "lifeExp"] <= 48.198000

life_exp_df = gapminder.loc[life_exp_mask]
life_exp_df.country.unique()
```

    <StringArray>
    [             'Afghanistan',                  'Algeria',
                       'Angola',               'Bangladesh',
                        'Benin',                  'Bolivia',
                     'Botswana',             'Burkina Faso',
                      'Burundi',                 'Cambodia',
                     'Cameroon', 'Central African Republic',
                         'Chad',                    'China',
                      'Comoros',         'Congo, Dem. Rep.',
                  'Congo, Rep.',            'Cote d'Ivoire',
                     'Djibouti',       'Dominican Republic',
                        'Egypt',              'El Salvador',
            'Equatorial Guinea',                  'Eritrea',
                     'Ethiopia',                    'Gabon',
                       'Gambia',                    'Ghana',
                    'Guatemala',                   'Guinea',
                'Guinea-Bissau',                    'Haiti',
                     'Honduras',                    'India',
                    'Indonesia',                     'Iran',
                         'Iraq',                   'Jordan',
                        'Kenya',              'Korea, Rep.',
                      'Lesotho',                  'Liberia',
                        'Libya',               'Madagascar',
                       'Malawi',                     'Mali',
                   'Mauritania',                 'Mongolia',
                      'Morocco',               'Mozambique',
                      'Myanmar',                  'Namibia',
                        'Nepal',                'Nicaragua',
                        'Niger',                  'Nigeria',
                         'Oman',                 'Pakistan',
                         'Peru',              'Philippines',
                       'Rwanda',    'Sao Tome and Principe',
                 'Saudi Arabia',                  'Senegal',
                 'Sierra Leone',                  'Somalia',
                 'South Africa',                    'Sudan',
                    'Swaziland',                    'Syria',
                     'Tanzania',                     'Togo',
                      'Tunisia',                   'Turkey',
                       'Uganda',                  'Vietnam',
           'West Bank and Gaza',              'Yemen, Rep.',
                       'Zambia',                 'Zimbabwe']
    Length: 80, dtype: str

##### Pandas Boolean Operators

- Pandas supports the boolean operators AND (`&`), OR (`|`), and NOT
  (`~`) on conditions

- `&` and `|` allow for combining conditions into complex expressions

- `~` is useful for inverting masks

- For example, we might want to create a mask that shows us highly
  populated countries with low life expectancies

  - We can combine the two masks together

``` python
import pandas as pd

gapminder = pd.read_csv("Data/gapminder.tsv", sep="\t")

# mask for bottom quartile of life expectancy
life_exp_mask = gapminder.loc[:, "lifeExp"] <= 48.198000

# population mask for top quartile of life expectancy
pop_mask = gapminder.loc[:, "pop"] >= 1.958522e7

# combined mask
total_mask = life_exp_mask & pop_mask
print(total_mask)
```

    0       False
    1       False
    2       False
    3       False
    4       False
            ...
    1699    False
    1700    False
    1701    False
    1702    False
    1703    False
    Length: 1704, dtype: bool

- We can then pass this into the dataframe to get a new dataframe that
  matches both conditions

``` python
import pandas as pd

gapminder = pd.read_csv("Data/gapminder.tsv", sep="\t")

# mask for bottom quartile of life expectancy
life_exp_mask = gapminder.loc[:, "lifeExp"] <= 48.198000

# population mask for top quartile of life expectancy
pop_mask = gapminder.loc[:, "pop"] >= 1.958522e7

# combined mask
total_mask = life_exp_mask & pop_mask
gapminder.loc[total_mask]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
&#10;    .dataframe tbody tr th {
        vertical-align: top;
    }
&#10;    .dataframe thead th {
        text-align: right;
    }
</style>

|      | country     | continent | year | lifeExp | pop      | gdpPercap  |
|------|-------------|-----------|------|---------|----------|------------|
| 9    | Afghanistan | Asia      | 1997 | 41.763  | 22227415 | 635.341351 |
| 10   | Afghanistan | Asia      | 2002 | 42.129  | 25268405 | 726.734055 |
| 11   | Afghanistan | Asia      | 2007 | 43.828  | 31889923 | 974.580338 |
| 96   | Bangladesh  | Asia      | 1952 | 37.484  | 46886859 | 684.244172 |
| 97   | Bangladesh  | Asia      | 1957 | 39.348  | 51365468 | 661.637458 |
| ...  | ...         | ...       | ...  | ...     | ...      | ...        |
| 1594 | Uganda      | Africa    | 2002 | 47.813  | 24739869 | 927.721002 |
| 1644 | Vietnam     | Asia      | 1952 | 40.412  | 26246839 | 605.066492 |
| 1645 | Vietnam     | Asia      | 1957 | 42.887  | 28998543 | 676.285448 |
| 1646 | Vietnam     | Asia      | 1962 | 45.363  | 33796140 | 772.049160 |
| 1647 | Vietnam     | Asia      | 1967 | 47.838  | 39463910 | 637.123289 |

<p>70 rows × 6 columns</p>
</div>

- The `~` lets us create inverse masks
- For example, to select all rows that are *not* in the bottom quartile
  of life expectancy

``` python
import pandas as pd

gapminder = pd.read_csv("Data/gapminder.tsv", sep="\t")

# mask for bottom quartile of life expectancy
life_exp_mask = gapminder.loc[:, "lifeExp"] <= 48.198000
higher_life_exp_mask = ~life_exp_mask

gapminder.loc[higher_life_exp_mask]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
&#10;    .dataframe tbody tr th {
        vertical-align: top;
    }
&#10;    .dataframe thead th {
        text-align: right;
    }
</style>

|      | country  | continent | year | lifeExp | pop      | gdpPercap   |
|------|----------|-----------|------|---------|----------|-------------|
| 12   | Albania  | Europe    | 1952 | 55.230  | 1282697  | 1601.056136 |
| 13   | Albania  | Europe    | 1957 | 59.280  | 1476505  | 1942.284244 |
| 14   | Albania  | Europe    | 1962 | 64.820  | 1728137  | 2312.888958 |
| 15   | Albania  | Europe    | 1967 | 66.220  | 1984060  | 2760.196931 |
| 16   | Albania  | Europe    | 1972 | 67.690  | 2263554  | 3313.422188 |
| ...  | ...      | ...       | ...  | ...     | ...      | ...         |
| 1696 | Zimbabwe | Africa    | 1972 | 55.635  | 5861135  | 799.362176  |
| 1697 | Zimbabwe | Africa    | 1977 | 57.674  | 6642107  | 685.587682  |
| 1698 | Zimbabwe | Africa    | 1982 | 60.363  | 7636524  | 788.855041  |
| 1699 | Zimbabwe | Africa    | 1987 | 62.351  | 9216418  | 706.157306  |
| 1700 | Zimbabwe | Africa    | 1992 | 60.377  | 10704340 | 693.420786  |

<p>1278 rows × 6 columns</p>
</div>

- If we wanted to select rows that were *either* in the lowest life
  expectancy quartile *or* the highest population quartile

``` python
import pandas as pd

gapminder = pd.read_csv("Data/gapminder.tsv", sep="\t")

# mask for bottom quartile of life expectancy
life_exp_mask = gapminder.loc[:, "lifeExp"] <= 48.198000

# population mask for top quartile of life expectancy
pop_mask = gapminder.loc[:, "pop"] >= 1.958522e7

# combined mask
total_mask = life_exp_mask | pop_mask
gapminder.loc[total_mask]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
&#10;    .dataframe tbody tr th {
        vertical-align: top;
    }
&#10;    .dataframe thead th {
        text-align: right;
    }
</style>

|      | country     | continent | year | lifeExp | pop      | gdpPercap   |
|------|-------------|-----------|------|---------|----------|-------------|
| 0    | Afghanistan | Asia      | 1952 | 28.801  | 8425333  | 779.445314  |
| 1    | Afghanistan | Asia      | 1957 | 30.332  | 9240934  | 820.853030  |
| 2    | Afghanistan | Asia      | 1962 | 31.997  | 10267083 | 853.100710  |
| 3    | Afghanistan | Asia      | 1967 | 34.020  | 11537966 | 836.197138  |
| 4    | Afghanistan | Asia      | 1972 | 36.088  | 13079460 | 739.981106  |
| ...  | ...         | ...       | ...  | ...     | ...      | ...         |
| 1690 | Zambia      | Africa    | 2002 | 39.193  | 10595811 | 1071.613938 |
| 1691 | Zambia      | Africa    | 2007 | 42.384  | 11746035 | 1271.211593 |
| 1701 | Zimbabwe    | Africa    | 1997 | 46.809  | 11404948 | 792.449960  |
| 1702 | Zimbabwe    | Africa    | 2002 | 39.989  | 11926563 | 672.038623  |
| 1703 | Zimbabwe    | Africa    | 2007 | 43.487  | 12311143 | 469.709298  |

<p>782 rows × 6 columns</p>
</div>

### Manipulating DataFrames

- Often more than *masking* and *filtering* data, we might also want to
  *modify* the dataframe
  - Rename columns or row indices
  - Add new columns or rows
  - Delete columns or rows
- `rename` is a straightforward method for relabelling columns
  - Can first get the columns via the `.columns` attribute

``` python
import pandas as pd

gapminder = pd.read_csv("Data/gapminder.tsv", sep="\t")
gapminder.columns
```

    Index(['country', 'continent', 'year', 'lifeExp', 'pop', 'gdpPercap'], dtype='str')

- Renaming columns is then done by providing a dictionary mapping old
  column names (`key`) to the new name (`value`)
  - Columns that are not specified as keys are left untouched
  - By default, a *new* dataframe is supplied with the new column names
  - This is a common paradigm for Pandas
    - This allows for *method chaining*
    - To modify the current dataframe use the `inplace=True` keyword
      argument

``` python
import pandas as pd

data = [
    ["Shanda", "Smith", 42],
    ["Rolly", "Brocker", 23],
    ["Molly", "Stein", 78],
    ["Frank", "Bach", 56],
    ["Rip", "Spencer", 26],
    ["Steven", "De Wilde", 14],
    ["Gwen", "Mason", 46],
    ["Arthur", "Davis", 92],
]

column_names = ["First", "Last", "Ages"]
index_labels = ["a", "b", "c", "d", "e", "f", "g", "h"]
participants = pd.DataFrame(data, columns=column_names, index=index_labels)

# creates a new Pandas dataframe
print(participants.rename(columns={"Ages": "Age"}))

print("Original dataframe:")
print(participants)

# modifies the current one inplace
participants.rename(columns={"Ages": "Age"}, inplace=True)
print("Original dataframe:")
print(participants)
```

        First      Last  Age
    a  Shanda     Smith   42
    b   Rolly   Brocker   23
    c   Molly     Stein   78
    d   Frank      Bach   56
    e     Rip   Spencer   26
    f  Steven  De Wilde   14
    g    Gwen     Mason   46
    h  Arthur     Davis   92
    Original dataframe:
        First      Last  Ages
    a  Shanda     Smith    42
    b   Rolly   Brocker    23
    c   Molly     Stein    78
    d   Frank      Bach    56
    e     Rip   Spencer    26
    f  Steven  De Wilde    14
    g    Gwen     Mason    46
    h  Arthur     Davis    92
    Original dataframe:
        First      Last  Age
    a  Shanda     Smith   42
    b   Rolly   Brocker   23
    c   Molly     Stein   78
    d   Frank      Bach   56
    e     Rip   Spencer   26
    f  Steven  De Wilde   14
    g    Gwen     Mason   46
    h  Arthur     Davis   92

- Indexer syntax can be used to insert new columns much like inserting
  keys into a dictionary

``` python
import pandas as pd

data = [
    ["Shanda", "Smith", 42],
    ["Rolly", "Brocker", 23],
    ["Molly", "Stein", 78],
    ["Frank", "Bach", 56],
    ["Rip", "Spencer", 26],
    ["Steven", "De Wilde", 14],
    ["Gwen", "Mason", 46],
    ["Arthur", "Davis", 92],
]

column_names = ["First", "Last", "Ages"]
index_labels = ["a", "b", "c", "d", "e", "f", "g", "h"]
participants = pd.DataFrame(data, columns=column_names, index=index_labels)

participants["Zip Code"] = [94702, 97402, 94223, 94705, 97503, 94705, 94111, 95333]
print(participants)
```

        First      Last  Ages  Zip Code
    a  Shanda     Smith    42     94702
    b   Rolly   Brocker    23     97402
    c   Molly     Stein    78     94223
    d   Frank      Bach    56     94705
    e     Rip   Spencer    26     97503
    f  Steven  De Wilde    14     94705
    g    Gwen     Mason    46     94111
    h  Arthur     Davis    92     95333

- We can use NumPy-style array-operations to combine columns together
  - For example concatenating the `"First"` and `"Last"` columns into a
    `"Full Name"` column

``` python
import pandas as pd

data = [
    ["Shanda", "Smith", 42],
    ["Rolly", "Brocker", 23],
    ["Molly", "Stein", 78],
    ["Frank", "Bach", 56],
    ["Rip", "Spencer", 26],
    ["Steven", "De Wilde", 14],
    ["Gwen", "Mason", 46],
    ["Arthur", "Davis", 92],
]

column_names = ["First", "Last", "Ages"]
index_labels = ["a", "b", "c", "d", "e", "f", "g", "h"]
participants = pd.DataFrame(data, columns=column_names, index=index_labels)

participants["Full Name"] = participants.loc[:, "First"] + participants.loc[:, "Last"]
print(participants)
```

        First      Last  Ages       Full Name
    a  Shanda     Smith    42     ShandaSmith
    b   Rolly   Brocker    23    RollyBrocker
    c   Molly     Stein    78      MollyStein
    d   Frank      Bach    56       FrankBach
    e     Rip   Spencer    26      RipSpencer
    f  Steven  De Wilde    14  StevenDe Wilde
    g    Gwen     Mason    46       GwenMason
    h  Arthur     Davis    92     ArthurDavis

- Column’s can also be overwritten the same way
  - For example if we latter decided to add the whitespace between the
    first and last names

``` python
import pandas as pd

data = [
    ["Shanda", "Smith", 42],
    ["Rolly", "Brocker", 23],
    ["Molly", "Stein", 78],
    ["Frank", "Bach", 56],
    ["Rip", "Spencer", 26],
    ["Steven", "De Wilde", 14],
    ["Gwen", "Mason", 46],
    ["Arthur", "Davis", 92],
]

column_names = ["First", "Last", "Ages"]
index_labels = ["a", "b", "c", "d", "e", "f", "g", "h"]
participants = pd.DataFrame(data, columns=column_names, index=index_labels)

participants["Full Name"] = participants.loc[:, "First"] + participants.loc[:, "Last"]
print(participants)

print("Redefining the 'Full Name' Column")
participants["Full Name"] = (
    participants.loc[:, "First"] + " " + participants.loc[:, "Last"]
)
print(participants)
```

        First      Last  Ages       Full Name
    a  Shanda     Smith    42     ShandaSmith
    b   Rolly   Brocker    23    RollyBrocker
    c   Molly     Stein    78      MollyStein
    d   Frank      Bach    56       FrankBach
    e     Rip   Spencer    26      RipSpencer
    f  Steven  De Wilde    14  StevenDe Wilde
    g    Gwen     Mason    46       GwenMason
    h  Arthur     Davis    92     ArthurDavis
    Redefining the 'Full Name' Column
        First      Last  Ages        Full Name
    a  Shanda     Smith    42     Shanda Smith
    b   Rolly   Brocker    23    Rolly Brocker
    c   Molly     Stein    78      Molly Stein
    d   Frank      Bach    56       Frank Bach
    e     Rip   Spencer    26      Rip Spencer
    f  Steven  De Wilde    14  Steven De Wilde
    g    Gwen     Mason    46       Gwen Mason
    h  Arthur     Davis    92     Arthur Davis

#### Manipulating Data

- There are more complex techniques for modifying data
  - Let us be more specific, e.g. to the row, column or cell
  - Create new rows or columns from multiple rows and columns
- We can use an indexer to change data
  - Just need to select the subset we want to change and define the new
    value

``` python
import pandas as pd

data = [
    ["Shanda", "Smith", 42],
    ["Rolly", "Brocker", 23],
    ["Molly", "Stein", 78],
    ["Frank", "Bach", 56],
    ["Rip", "Spencer", 26],
    ["Steven", "De Wilde", 14],
    ["Gwen", "Mason", 46],
    ["Arthur", "Davis", 92],
]

column_names = ["First", "Last", "Ages"]
index_labels = ["a", "b", "c", "d", "e", "f", "g", "h"]
participants = pd.DataFrame(data, columns=column_names, index=index_labels)


participants["Zip Code"] = [94702, 97402, 94223, 94705, 97503, 94705, 94111, 95333]
participants["Full Name"] = (
    participants.loc[:, "First"] + " " + participants.loc[:, "Last"]
)
print(participants)

print("Redefining one row h, column first...")
participants.loc["h", "first"] = "Paul"
print(participants)
```

        First      Last  Ages  Zip Code        Full Name
    a  Shanda     Smith    42     94702     Shanda Smith
    b   Rolly   Brocker    23     97402    Rolly Brocker
    c   Molly     Stein    78     94223      Molly Stein
    d   Frank      Bach    56     94705       Frank Bach
    e     Rip   Spencer    26     97503      Rip Spencer
    f  Steven  De Wilde    14     94705  Steven De Wilde
    g    Gwen     Mason    46     94111       Gwen Mason
    h  Arthur     Davis    92     95333     Arthur Davis
    Redefining one row h, column first...
        First      Last  Ages  Zip Code        Full Name first
    a  Shanda     Smith    42     94702     Shanda Smith   NaN
    b   Rolly   Brocker    23     97402    Rolly Brocker   NaN
    c   Molly     Stein    78     94223      Molly Stein   NaN
    d   Frank      Bach    56     94705       Frank Bach   NaN
    e     Rip   Spencer    26     97503      Rip Spencer   NaN
    f  Steven  De Wilde    14     94705  Steven De Wilde   NaN
    g    Gwen     Mason    46     94111       Gwen Mason   NaN
    h  Arthur     Davis    92     95333     Arthur Davis  Paul

- We could also achieve a similar result through numeric indices with
  `iloc`

``` python
import pandas as pd

data = [
    ["Shanda", "Smith", 42],
    ["Rolly", "Brocker", 23],
    ["Molly", "Stein", 78],
    ["Frank", "Bach", 56],
    ["Rip", "Spencer", 26],
    ["Steven", "De Wilde", 14],
    ["Gwen", "Mason", 46],
    ["Arthur", "Davis", 92],
]

column_names = ["First", "Last", "Ages"]
index_labels = ["a", "b", "c", "d", "e", "f", "g", "h"]
participants = pd.DataFrame(data, columns=column_names, index=index_labels)


participants["Zip Code"] = [94702, 97402, 94223, 94705, 97503, 94705, 94111, 95333]
participants["Full Name"] = (
    participants.loc[:, "First"] + " " + participants.loc[:, "Last"]
)
print(participants)

print("Redefining row 3, column 2")
participants.iloc[3, 2] = 99
print(participants)
```

        First      Last  Ages  Zip Code        Full Name
    a  Shanda     Smith    42     94702     Shanda Smith
    b   Rolly   Brocker    23     97402    Rolly Brocker
    c   Molly     Stein    78     94223      Molly Stein
    d   Frank      Bach    56     94705       Frank Bach
    e     Rip   Spencer    26     97503      Rip Spencer
    f  Steven  De Wilde    14     94705  Steven De Wilde
    g    Gwen     Mason    46     94111       Gwen Mason
    h  Arthur     Davis    92     95333     Arthur Davis
    Redefining row 3, column 2
        First      Last  Ages  Zip Code        Full Name
    a  Shanda     Smith    42     94702     Shanda Smith
    b   Rolly   Brocker    23     97402    Rolly Brocker
    c   Molly     Stein    78     94223      Molly Stein
    d   Frank      Bach    99     94705       Frank Bach
    e     Rip   Spencer    26     97503      Rip Spencer
    f  Steven  De Wilde    14     94705  Steven De Wilde
    g    Gwen     Mason    46     94111       Gwen Mason
    h  Arthur     Davis    92     95333     Arthur Davis

- We’ve seen that we can use operations between columns to construct new
  values
- We can also use the inplace operators to modify a column inplace

``` python
import pandas as pd

data = [
    ["Shanda", "Smith", 42],
    ["Rolly", "Brocker", 23],
    ["Molly", "Stein", 78],
    ["Frank", "Bach", 56],
    ["Rip", "Spencer", 26],
    ["Steven", "De Wilde", 14],
    ["Gwen", "Mason", 46],
    ["Arthur", "Davis", 92],
]

column_names = ["First", "Last", "Ages"]
index_labels = ["a", "b", "c", "d", "e", "f", "g", "h"]
participants = pd.DataFrame(data, columns=column_names, index=index_labels)

print(participants)
print("Decreasing 'Ages' Column by 10")
participants.Ages -= 10
print(participants)
```

        First      Last  Ages
    a  Shanda     Smith    42
    b   Rolly   Brocker    23
    c   Molly     Stein    78
    d   Frank      Bach    56
    e     Rip   Spencer    26
    f  Steven  De Wilde    14
    g    Gwen     Mason    46
    h  Arthur     Davis    92
    Decreasing 'Ages' Column by 10
        First      Last  Ages
    a  Shanda     Smith    32
    b   Rolly   Brocker    13
    c   Molly     Stein    68
    d   Frank      Bach    46
    e     Rip   Spencer    16
    f  Steven  De Wilde     4
    g    Gwen     Mason    36
    h  Arthur     Davis    82

#### The `replace` Method

- `replace` performs a find and replace across a dataframe
  - For example changing `"Rolly"` to `"Smiley"`

``` python
import pandas as pd

data = [
    ["Shanda", "Smith", 42],
    ["Rolly", "Brocker", 23],
    ["Molly", "Stein", 78],
    ["Frank", "Bach", 56],
    ["Rip", "Spencer", 26],
    ["Steven", "De Wilde", 14],
    ["Gwen", "Mason", 46],
    ["Arthur", "Davis", 92],
]

column_names = ["First", "Last", "Ages"]
index_labels = ["a", "b", "c", "d", "e", "f", "g", "h"]
participants = pd.DataFrame(data, columns=column_names, index=index_labels)

print(participants)
print("Replacing 'Rolly' by 'Smiley'")
participants.replace("Rolly", "Smiley")
```

        First      Last  Ages
    a  Shanda     Smith    42
    b   Rolly   Brocker    23
    c   Molly     Stein    78
    d   Frank      Bach    56
    e     Rip   Spencer    26
    f  Steven  De Wilde    14
    g    Gwen     Mason    46
    h  Arthur     Davis    92
    Replacing 'Rolly' by 'Smiley'

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
&#10;    .dataframe tbody tr th {
        vertical-align: top;
    }
&#10;    .dataframe thead th {
        text-align: right;
    }
</style>

|     | First  | Last     | Ages |
|-----|--------|----------|------|
| a   | Shanda | Smith    | 42   |
| b   | Smiley | Brocker  | 23   |
| c   | Molly  | Stein    | 78   |
| d   | Frank  | Bach     | 56   |
| e   | Rip    | Spencer  | 26   |
| f   | Steven | De Wilde | 14   |
| g   | Gwen   | Mason    | 46   |
| h   | Arthur | Davis    | 92   |

</div>

- `replace` supports regular expressions for more advanced pattern
  matching
  - For example, to match words starting with a lowercase s and replace
    with a capital S

``` python
import pandas as pd

data = [
    ["shanda", "smith", 42],
    ["Rolly", "Brocker", 23],
    ["Molly", "stein", 78],
    ["Frank", "Bach", 56],
    ["Rip", "spencer", 26],
    ["steven", "De Wilde", 14],
    ["Gwen", "Mason", 46],
    ["Arthur", "Davis", 92],
]

column_names = ["First", "Last", "Ages"]
index_labels = ["a", "b", "c", "d", "e", "f", "g", "h"]
participants = pd.DataFrame(data, columns=column_names, index=index_labels)

print(participants)
print("Using regex to replace lowercase s with uppercase S")
participants.replace(r"(s)([a-z]+)", r"S\2", regex=True)
```

        First      Last  Ages
    a  shanda     smith    42
    b   Rolly   Brocker    23
    c   Molly     stein    78
    d   Frank      Bach    56
    e     Rip   spencer    26
    f  steven  De Wilde    14
    g    Gwen     Mason    46
    h  Arthur     Davis    92
    Using regex to replace lowercase s with uppercase S

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
&#10;    .dataframe tbody tr th {
        vertical-align: top;
    }
&#10;    .dataframe thead th {
        text-align: right;
    }
</style>

|     | First  | Last     | Ages |
|-----|--------|----------|------|
| a   | Shanda | Smith    | 42   |
| b   | Rolly  | Brocker  | 23   |
| c   | Molly  | Stein    | 78   |
| d   | Frank  | Bach     | 56   |
| e   | Rip    | Spencer  | 26   |
| f   | Steven | De Wilde | 14   |
| g   | Gwen   | MaSon    | 46   |
| h   | Arthur | Davis    | 92   |

</div>

- For more advanced functions Pandas provides the `apply` method
  - Calls a function of the values
  - For `pd.Series` apply calls the function on each value in the
    `Series`
- For example, we could pass a lambda or function to capitalise every
  word

``` python
import pandas as pd

data = [
    ["shanda", "smith", 42],
    ["rolly", "brocker", 23],
    ["molly", "stein", 78],
    ["frank", "bach", 56],
    ["rip", "spencer", 26],
    ["steven", "de wilde", 14],
    ["gwen", "mason", 46],
    ["arthur", "davis", 92],
]

column_names = ["First", "Last", "Ages"]
index_labels = ["a", "b", "c", "d", "e", "f", "g", "h"]
participants = pd.DataFrame(data, columns=column_names, index=index_labels)

print(participants)
print("Using `apply` to capitalise first names")

participants.loc[:, "First"].apply(lambda w: w.capitalize())
```

        First      Last  Ages
    a  shanda     smith    42
    b   rolly   brocker    23
    c   molly     stein    78
    d   frank      bach    56
    e     rip   spencer    26
    f  steven  de wilde    14
    g    gwen     mason    46
    h  arthur     davis    92
    Using `apply` to capitalise first names

    a    Shanda
    b     Rolly
    c     Molly
    d     Frank
    e       Rip
    f    Steven
    g      Gwen
    h    Arthur
    Name: First, dtype: str

- When `apply` is called on a DataFrame, rather than operating on every
  value, the function is applied to each *row*
  - Means we can look at the value of all the columns in a given row

``` python
import pandas as pd

data = [
    ["shanda", "smith", 42],
    ["rolly", "brocker", 23],
    ["molly", "stein", 78],
    ["frank", "bach", 56],
    ["rip", "spencer", 26],
    ["steven", "de wilde", 14],
    ["gwen", "mason", 46],
    ["arthur", "davis", 92],
]

column_names = ["First", "Last", "Ages"]
index_labels = ["a", "b", "c", "d", "e", "f", "g", "h"]
participants = pd.DataFrame(data, columns=column_names, index=index_labels)

print(participants)
print("Using `apply` to generate a new column")

participants.apply(
    lambda row: f"{row['First']} is {row['Ages']} years old.", axis=1
)
```

        First      Last  Ages
    a  shanda     smith    42
    b   rolly   brocker    23
    c   molly     stein    78
    d   frank      bach    56
    e     rip   spencer    26
    f  steven  de wilde    14
    g    gwen     mason    46
    h  arthur     davis    92
    Using `apply` to generate a new column

    a    shanda is 42 years old.
    b     rolly is 23 years old.
    c     molly is 78 years old.
    d     frank is 56 years old.
    e       rip is 26 years old.
    f    steven is 14 years old.
    g      gwen is 46 years old.
    h    arthur is 92 years old.
    dtype: str

- `axis` lets us specify how you want to the function to operate on the
  dataframe
  - For example we could instead have the function accept columns

## Summary

- Pandas DataFrame objects are designed for working with
  spreadsheet-like data
- DataFrames are most commonly constructed from files
- DataFrames can be extended with new rows and columns
- Pandas provides specialised indexers for accessing subsets of columns
  and rows
  - They can also be used to set data
- Datasets provide mechanisms to explore and manipulate data

## Questions

Use the following table to answer the following questions

| Sample Size (mg) | %P  | %Q  |
|------------------|-----|-----|
| 0.24             | 40  | 60  |
| 2.34             | 34  | 66  |
| 0.0234           | 12  | 88  |

1. Create a DataFrame representing this table

    - There are a few techniques we could use
    - We’ll use the dictionary of lists technique

    ``` python
     import pandas as pd

     sample_size = [0.24, 2.34, 0.0234]
     percent_P = [40, 34, 12]
     percent_Q = [60, 66, 88]

     data = {"Sample Size (mg)": sample_size, "%P": percent_P, "%Q": percent_Q}
     df = pd.DataFrame(data)
    ```

2. Add a new column Total Q that contains the amount of Q (in mg) for
    each sample

    - We can do this through a simple `apply` function
    - This calculates a new `pd.Series` from the rows in our dataframe
      - We then need to add this new column into the dataframe

    ``` python
     import pandas as pd

     def calculate_Q(row):
         return row.loc["Sample Size (mg)"] * row.loc["%Q"] / 100

     df.loc[:, "Total Q"] = df.apply(calculate_Q, axis=1)
    ```

3. Divide the columns `%P` and `%Q` by $100$

    - The question is awkwardly framed, but we’ll assume the intention
      is to do this division in-place
    - For these we could repeat the process of using `apply` and a
      function
    - However, for simple division this is overkill
    - We can just use in-place division
      - This has a subtle caveat
      - Since it’s in-place we must respect the types of the existing
        columns
        - However, the `"%P"` and `"%Q"` columns were implicitly defined
          as `int64`
        - So we can’t assign back, since that the division gives us
          floating point values
        - There are two solutions,
          1. We coerce the columns into floating point values using
              `astype(flaot)`
          2. We redefine our DataFrame to store `"%P` and `"%Q"` as
              floats
              - This is easily achieved by adding `.0` to each value
        - We’ll use option 2 for now since it’s the most straightforward

    ``` python
     import pandas as pd

     df.loc[:, ["%P", "%Q"]] /= 100
    ```

- We can put this all together

``` python
    import pandas as pd

    sample_size = [0.24, 2.34, 0.0234]
    percent_P = [40.0, 34.0, 12.0]
    percent_Q = [60.0, 66.0, 88.0]

    data = {"Sample Size (mg)": sample_size, "%P": percent_P, "%Q": percent_Q}
    df = pd.DataFrame(data)

    def calculate_Q(row):
        return row.loc["Sample Size (mg)"] * row.loc["%Q"] / 100

    df.loc[:, "Total Q"] = df.apply(calculate_Q, axis=1)
    df.loc[:, ["%P", "%Q"]] /= 100

    print(df)
```

       Sample Size (mg)    %P    %Q   Total Q
    0            0.2400  0.40  0.60  0.144000
    1            2.3400  0.34  0.66  1.544400
    2            0.0234  0.12  0.88  0.020592
