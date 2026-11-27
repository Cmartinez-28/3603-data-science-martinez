# 📝 Quiz: 06 - Working with Data (Advanced)

47 questions covering Pandas Series/DataFrames, loading/exploring data, indexing/selection, cleaning, column operations, sorting/filtering/searching, grouping and combining tables, and a "going further" bonus. Mix of multiple choice, true/false, code-tracing, and short answer.

Try every question on your own first — the Answer Key is at the bottom, no peeking. If you're using an AI tutor, paste in *your answer* and ask it to check your reasoning rather than asking it to solve the question first.

---

## Section A — Series and DataFrames

**1.** (Short Answer) What's the difference between a Pandas `Series` and a `DataFrame`?\
Series is one dimensional. A dataFrame is a two dimensional table of rows and columns.

**2.** (Code Tracing) What prints?
```python
s = pd.Series({'a': 1, 'b': 2})
print(s['a'])
```
1

**3.** (Short Answer) What does `df.shape` return?\
Returns a tuple - (# of rows, # of cols)

**4.** (Short Answer) If you add two `Series` together and one has a label the other doesn't, what happens at that label?\
Pandas aligns by label. If a label exists in one series but not the other, the result is NaN

**5.** (Short Answer) You have a list of dictionaries, one per student (the way you stored records in Module 03b). What does `pd.DataFrame(that_list)` turn each dictionary into?\
Each dictionary becomes a row in DataFrame

**6.** (Code Tracing) What prints?
```python
scores = pd.Series([70, 95, 88])
print(scores[scores > 80].tolist())
```
[95,88]

---

## Section B — Loading and Exploring Data

**7.** (Multiple Choice) Which method shows summary statistics (mean, std, min/max) for numeric columns?
A) `.head()`  B) `.columns`  C) `.describe()`  D) `.shape`\
Answer is C - describe()

**8.** (Short Answer) What's the difference between what `.head()` and `.info()` each tell you about a DataFrame?\
head() shows first few rows of data\
info() shows column names, dtypes, non-null counts

**9.** (Short Answer) How do you count how many missing values are in *each column* of a DataFrame?\
Use isnull().sum()

**10.** (Short Answer) What do `df.duplicated().sum()` and `df['col'].value_counts(dropna=False)` each tell you?\
duplicated().sum() shows number of duplicate rows\
['col'].value_counts(dropna=False) shows frequency of each value

**11.** (Short Answer) Titanic's `Age` column holds whole numbers, but its dtype is `float64`. Why?\
Because the column contains missing values (NaN)

---

## Section C — Indexing and Selection

**12.** (Short Answer) What's the difference between `.loc[]` and `.iloc[]`?\
loc[] selects by index name\
iloc[] selects by integer position

**13.** (Short Answer) Is `df.loc[0:4]` inclusive or exclusive of row `4`? What about `df.iloc[0:4]` and position `4`?\
Inclusive of label 4\
Exclusive of position 4

**14.** (Short Answer) What symbols does Pandas require for combining conditions when filtering a DataFrame (instead of the `and`/`or` keywords)?\
& for AND\
| for OR\
~ for NOT

**15.** (Short Answer) What does `df[df['Age'] < 10]` return?\
Returns all rows where Age column is less than 10

**16.** (Short Answer) Rewrite `df[(df['Embarked'] == 'C') | (df['Embarked'] == 'Q')]` using `.isin()`. Does `df['Age'].between(13, 19)` include ages 13 and 19?\
df[df['Embarked'].isin(['C','Q'])]\
.between includes both 13 and 19

**17.** (Multiple Choice) Which one returns a `DataFrame` rather than a `Series`?
A) `df['Age']`  B) `df[['Age']]`  C) `df.Age`  D) `df.loc[0]`\
Answer is B

**18.** (Short Answer) After `by_id = df.set_index('PassengerId')`, what does `by_id.loc[2]` select, and how is that different from `df.loc[2]`?\
Selects the row whose PassengerId equals 2.\
df.loc[2] selects row with index label 2.

**19.** (Code Tracing) After `by_id = titanic.set_index('PassengerId')`, do `by_id.loc[3]` and `by_id.iloc[3]` select the same passenger? Why or why not?\
They do select the same thing

**20.** (Short Answer) Write a filter for passengers who did **not** board at Southampton (`'S'`), using `~` and `.isin()`.\
df[~df['Embarked'].isin(['S'])]

**21.** (Short Answer) What's wrong with `df[df['Age'] < 10]['child'] = True`, and what should you write instead?\
This creates a temporary filtered DataFrame and can cause a SettingWithCopyWarning.\
df.loc[df['Age'] < 10, 'child'] = True

---

## Section D — Cleaning

**22.** (Short Answer) What's the difference between `.dropna()` and `.fillna()`?\
.dropna() removes rows or columns containing missing values.\
.fillna() keeps them and replaces missing values with something else.

**23.** (Short Answer) What does `pd.to_numeric(col, errors='coerce')` do to a value like `'ninety'` in that column?\
Converts it into 'NaN'

**24.** (True/False) `NaN` and an empty string `""` are always treated identically by `.isnull()`.\
False

**25.** (Short Answer) Two rows have the same `order_id` but different `amount`s. Does `df.drop_duplicates()` remove one of them? What would you write to keep only the last row for each `order_id`?\
No, it only removes rows that are identical in all columns\
df.drop_duplicates(subset=['order_id'], keep='last')

**26.** (Short Answer) A survey stores missing ages as `-1`. Why is that a problem for `df['age'].mean()`, and how do you fix it?\
-1 gets treated as a real age and lowers the mean.\
df['age'] = df['age'].replace(-1, np.nan)

**27.** (Short Answer) A date column holds `'2022-01-01'`, `'2022/03/01'`, and `'April 5, 2022'`. Why can `pd.to_datetime(col, errors='coerce')` turn two perfectly valid dates into `NaT`, and how do you fix it?\
Pandas may infer a single date format and fail on rows using a different format, turning valid dates into NaT.

**28.** (Short Answer) Why does `.astype('int64')` fail on a column with missing values, while `.astype('Int64')` works?\
int64 cannot store missing values (NaN).

**29.** (Short Answer) Before deleting rows with a repeated `order_id`, why would you look at `df[df.duplicated(subset=['order_id'], keep=False)]` first?\
To inspect what you're about to delete.

---

## Section E — Column Operations, Sorting, Filtering, and Searching

**30.** (Short Answer) How do you create a new column that's the sum of two existing numeric columns?\
df['total'] = df['price'] + df['tax']

**31.** (Short Answer) What does `.apply()` do that you could also do with a `for` loop over rows — and why is `.apply()` usually preferred?\
.apply() applies a function across a Series or DataFrame.

**32.** (Short Answer) What does `ascending=False` do inside `df.sort_values(by='score', ascending=False)`?\
Sorts from highest score to lowest score.

**33.** (Short Answer) What's the main advantage of `.query()` over bracket-based boolean filtering (`df[df['col'] > 5]`)?\
Main advantage is readability

**34.** (Code Tracing) `bonus` is a brand-new column. What value do the rows with `score >= 80` get in it?
```python
df.loc[df['score'] < 80, 'bonus'] = 5
```
Rows with score < 80 get 5\
Rows with score >= 80 get NaN

**35.** (Short Answer) To compute `price * qty` for every row, why should you write `df['price'] * df['qty']` instead of looping with `.iterrows()`? When *is* a loop (preferably `.itertuples()`) the right tool?\
Vectorized operations are much faster

**36.** (Short Answer) What do `case=False` and `na=False` each do in `df['name'].str.contains('an', case=False, na=False)`?\
case=False ignores capitlization\
na=False treats missing values as False

**37.** (Code Tracing) A student has `math = 95` and `average = 91`. What is their `award` after these three lines, and what would it be if the last two lines were swapped?
```python
df['award'] = 'None'
df.loc[df['average'] >= 85, 'award'] = 'Honor Roll'
df.loc[df['math'] >= 90, 'award'] = 'Math Star'
```
Award is 'Math Star'\
'Honor Roll' if lines are swapped

**38.** (Short Answer) What does `pd.cut(df['score'], bins=[0, 70, 90, 101], labels=['Low', 'Mid', 'High'], right=False)` do, and why is it usually better than `.apply()` with an `if`/`elif` function for the same job?\
It creates the different categories

**39.** (Short Answer) What's the difference between `df.query('math_score >= minimum')` and `df.query('math_score >= @minimum')`?\
First one looks for column named minimum\
Second one uses Python variable minimum


**40.** (Code Tracing) What does each line return, and why are they different?
```python
books = pd.Series(['C++ Primer', 'C Programming'])
books.str.contains('C++').tolist()
books.str.contains('C++', regex=False).tolist()
```


---

## Section F — Grouping and Combining

**41.** (Code Tracing) What does this return?
```python
df = pd.DataFrame({'team': ['A', 'B', 'A', 'B'], 'points': [10, 4, 20, 6]})
df.groupby('team')['points'].mean()
```
Returns
```python
team
A   15.0
B   5.0
Name: points, dtype: float64
```

**42.** (Short Answer) Write a `groupby` with `.agg()` that gives, for each `team`, the number of rows and the total `points`, as columns named `games` and `total`.\
```python
df.groupby('team').agg(
    games=('team', 'size'),
    total=('points', 'sum')
)
```

**43.** (Short Answer) When would you use `pd.concat()` instead of `.merge()`? What does `ignore_index=True` do?\
When stacking DataFrames

**44.** (Short Answer) In `orders.merge(prices, on='product_id', how='left')`, what happens to an order whose `product_id` isn't in `prices`? What would `how='inner'` do instead? And why does a merge on state codes fail when one table uses `'tx'` and the other uses `'TX'`?\
If it isn't found in prices, the order stays and matching columns become NaN

**45.** (Short Answer) In Titanic, `groupby('Pclass').size()` gives 216 for 1st class, but `groupby('Pclass')['Age'].count()` gives 186. Why are they different?\
First one counts all rows, other one counts only non-missing Age values

**46.** (Short Answer) `purchases` has 6 rows. After `purchases.merge(buyers, on='buyer_id', how='left')`, it has 8. What probably happened, and how can `validate=` protect you?\
Many-to-one merge was expected, but duplicate buyer_id values exist in buyers.

---

## Section G — Going Further (Bonus)

**47.** (Short Answer) What do the `usecols` and `nrows` arguments to `pd.read_csv()` do, and when would you reach for them?\
usecols read only selected columns, nrows reads only the first N rows.

