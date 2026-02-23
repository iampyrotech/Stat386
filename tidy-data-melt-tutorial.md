# From Wide to Tidy: Reshaping DataFrames with `pd.melt`

Most real-world datasets do not arrive in tidy form. Text files often organize values horizontally for readability (e.g., one column per day or device), but this “wide” structure makes plotting, grouping, and modeling painful.

In this tutorial, we will convert messy wide tables into clean, tidy DataFrames using `pandas.melt`. This is one of the best methods in data wrangling and is essential for modeling, visualization, and reproducibility.

---

## What Does “Tidy” Mean?

A dataset is **tidy** when:

- Each **variable** has its own column  
- Each **observation** has its own row  
- Each **value** has its own cell  

Wide datasets violate this by spreading one variable across many columns.

![Diagram illustrating tidy data structure: vertical arrows showing variables, horizontal arrows showing observations, and circles showing individual values.](tidy-1.png)

---

## Wide vs. Tidy: A Simple Example

Suppose you get student quiz scores in this format:

### Wide Format

| student | quiz1 | quiz2 | quiz3 |
|--------|-------|-------|-------|
| Alice  | 8     | 9     | 10    |
| Bob    | 7     | 6     | 9     |
| Chloe  | 10    | 10    | 10    |

The “quiz number” is encoded in the column names, acting as a variable hiding inside the headers.

### Tidy Format (What We Want)

| student | quiz | score |
|--------|------|-------|
| Alice  | 1    | 8     |
| Alice  | 2    | 9     |
| Alice  | 3    | 10    |
| Bob    | 1    | 7     |
| Bob    | 2    | 6     |
| Bob    | 3    | 9     |
| Chloe  | 1    | 10    |
| Chloe  | 2    | 10    |
| Chloe  | 3    | 10    |

This structure works much better with:

- `groupby`
- seaborn visualizations
- modeling functions
- joins & filtering

---

## Step 1: Load the Data

```python
import pandas as pd

df_wide = pd.DataFrame({
    "student": ["Alice", "Bob", "Chloe"],
    "quiz1": [8, 7, 10],
    "quiz2": [9, 6, 10],
    "quiz3": [10, 9, 10],
})

df_wide
```

---

## Step 2: Identify ID Columns vs Value Columns

Before melting, separate:

- **ID columns**: stay the same  
  - `student`

- **Value columns**: will be stacked  
  - `quiz1`, `quiz2`, `quiz3`

---

## Step 3: Use `pandas.melt` to Tidy the Data

```python
df_long = df_wide.melt(
    id_vars="student",
    value_vars=["quiz1", "quiz2", "quiz3"],
    var_name="quiz",
    value_name="score"
)

df_long
```

Output:

```
  student   quiz  score
0   Alice  quiz1      8
1     Bob  quiz1      7
2   Chloe  quiz1     10
3   Alice  quiz2      9
4     Bob  quiz2      6
5   Chloe  quiz2     10
6   Alice  quiz3     10
7     Bob  quiz3      9
8   Chloe  quiz3     10
```

### Clean the quiz column

```python
df_long["quiz"] = df_long["quiz"].str.replace("quiz", "").astype(int)
df_long
```

---

## Here's a more realistic example from a Stat 469 (Analysis of Correlated Data) project on Youtube Views

```python
df = pd.DataFrame({
    "date": ["2025-01-01", "2025-01-02", "2025-01-03"],
    "views_desktop": [100, 120, 150],
    "views_mobile": [200, 220, 260],
    "views_tv": [50, 60, 80],
})
```

Wide:

| date       | views_desktop | views_mobile | views_tv |
|------------|---------------|--------------|----------|
| 2025-01-01 | 100           | 200          | 50       |
| 2025-01-02 | 120           | 220          | 60       |
| 2025-01-03 | 150           | 260          | 80       |

Convert to tidy:

```python
df_long = df.melt(
    id_vars="date",
    value_vars=["views_desktop", "views_mobile", "views_tv"],
    var_name="device",
    value_name="views"
)

df_long["device"] = df_long["device"].str.replace("views_", "")
df_long
```

---

## Common Mistakes and How to Avoid Them

### - Forgetting `id_vars`
```python
df.melt()  # melts everything — usually wrong
```

### - Melting multiple measurement types together  
If variables represent different concepts (temp_high vs temp_low), clean them after melt.

---

## Example: High vs Low Temperature

```python
weather = pd.DataFrame({
    "date": ["2025-01-01", "2025-01-02"],
    "temp_high": [50, 55],
    "temp_low": [30, 32],
})

weather_long = weather.melt(
    id_vars="date",
    var_name="measure",
    value_name="temperature"
)

weather_long["measure"] = weather_long["measure"].replace({
    "temp_high": "high",
    "temp_low": "low"
})

weather_long
```

---

## Checklist for Tidy Data

- One row = one observation  
- One column = one variable  
- Column names do **not** contain variable info  
- Data is easy to group, plot, or model  

---

## Learn More

- [Pandas Melt Documentation](https://pandas.pydata.org/pandas-docs/stable/user_guide/reshaping.html#melt)  
- [Hadley Wickham — “Tidy Data”](https://vita.had.co.nz/papers/tidy-data.pdf)

---

## Conclusion & Call to Action

Reshaping wide data into tidy format is one of the most important skills in data science. With `melt()`, your tables become easier to analyze, visualize, and model.

**Try this next:**

1. Pick a dataset from a recent Stat 386 assignment  
2. Identify ID columns vs value columns  
3. Use `melt()` to tidy it  
4. Add your analysis and results to a public GitHub repo 

If your dataset becomes easier to plot and summarize afterward, you’ve mastered one of the core principles of tidy data.
