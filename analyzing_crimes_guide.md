# Analyzing Crime in Los Angeles — Workshop Teaching Guide

---

## The Task

> We have `crimes.csv` with columns: `Date Rptd`, `DATE OCC`, `TIME OCC` (string like `"1620"`), `AREA NAME`, `Crm Cd Desc`, `Vict Age`, `Vict Sex`.

**Question 1:** Which hour has the highest frequency of crimes? → `peak_crime_hour`

**Question 2:** Which area has the most night crimes (10 PM – 3:59 AM)? → `peak_night_crime_location`

**Question 3:** Count crimes by victim age group (0-17, 18-25, 26-34, 35-44, 45-54, 55-64, 65+) → `victim_ages`

---

## How to Lead Students to Each Question

### Question 1 — "Which HOUR has the HIGHEST FREQUENCY"
- **What it's asking:** The time column has values like `"1620"`. We need the **hour** part (first 2 digits). Then count how many crimes happened in each hour across the whole dataset, and find which hour has the most.
- **Lead-in:** "Look at TIME OCC — it's a 4-digit string. Where is the hour hiding? Once you have hours, how do you count which one shows up the most?"

### Question 2 — "Which AREA has the most NIGHT crimes"
- **What it's asking:** Night = 10 PM to 3:59 AM = hours 22, 23, 0, 1, 2, 3. Filter for those hours only, then group by area and find which area has the highest count.
- **Lead-in:** "First — how do you define 'night' as hour numbers? Then once you have only night crimes, how do you find which area has the most?"

### Question 3 — "Count crimes by AGE GROUP"
- **What it's asking:** Ages are raw numbers (28, 45, 72). We need to put them into labeled bins like "18-25", "26-34", etc. Then count how many victims fall into each bin.
- **Lead-in:** "Ages are continuous numbers but the question wants groups. How do you turn a number like 28 into a label like '26-34'?"

---

## Cell 1: Load the Data

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

crimes = pd.read_csv("crimes.csv",
                      parse_dates=["Date Rptd", "DATE OCC"],
                      dtype={"TIME OCC": str})
crimes.head()
```

| Part | What it does |
|---|---|
| `parse_dates=[...]` | Converts date columns from strings to proper datetime objects |
| `dtype={"TIME OCC": str}` | Keeps `TIME OCC` as string so `"0900"` doesn't lose its leading zero |
| `.head()` | Preview first 5 rows — sanity check only, not for conclusions |

---

## Cell 2: Extract the Hour

```python
crimes["HOUR OCC"] = crimes["TIME OCC"].str[:2].astype(int)
crimes.head()
```

| Part | What it does |
|---|---|
| `.str[:2]` | Takes first 2 characters — `"1620"` → `"16"` |
| `.astype(int)` | Converts string to integer — `"16"` → `16`, `"09"` → `9` |
| New column `HOUR OCC` | Values from 0 (midnight) to 23 (11 PM) |

---

## Cell 3: Find Peak Crime Hour

```python
sns.countplot(data=crimes, x="HOUR OCC")
plt.show()

peak_crime_hour = 12
```

| Part | What it does |
|---|---|
| `sns.countplot()` | Bar chart counting how many crimes per hour across **all** rows |
| Tallest bar at 12 | Noon has the most crimes |
| `peak_crime_hour = 12` | Store the answer from reading the chart |

---

## Cell 4: Filter Night-Time Crimes

```python
night_time = crimes[crimes["HOUR OCC"].isin([22, 23, 0, 1, 2, 3])]
```

| Part | What it does |
|---|---|
| `.isin([22,23,0,1,2,3])` | Keeps rows where hour matches any of these values |
| Why not `BETWEEN 22 AND 3`? | 22 > 3, so a range doesn't work — need a list |
| Why no hour 4? | 3:59 AM is the cutoff; hour 4 = 4:00 AM onward |

---

## Cell 5: Find Peak Night Crime Area

```python
peak_night_crime_location = (
    night_time
    .groupby("AREA NAME", as_index=False)["HOUR OCC"]
    .count()
    .sort_values("HOUR OCC", ascending=False)
    .iloc[0]["AREA NAME"]
)
print(f"The area with the largest volume of night crime is {peak_night_crime_location}")
```

| Part | What it does |
|---|---|
| `.groupby("AREA NAME")` | Groups night crimes by LAPD area |
| `["HOUR OCC"].count()` | Counts rows per group (any column works, just counting rows) |
| `.sort_values(ascending=False)` | Sorts highest count first |
| `.iloc[0]["AREA NAME"]` | Grabs the area name from the top row |

---

## Cell 6: Define Age Bins

```python
age_bins   = [0, 17, 25, 34, 44, 54, 64, np.inf]
age_labels = ["0-17", "18-25", "26-34", "35-44", "45-54", "55-64", "65+"]
```

| Part | What it does |
|---|---|
| `age_bins` | Boundaries: 0–17, 18–25, 26–34, etc. |
| `age_labels` | Human-readable name for each bin |
| `np.inf` | Infinity — catches everyone age 65 and above |

---

## Cell 7: Bin Ages & Count

```python
crimes["Age Bracket"] = pd.cut(crimes["Vict Age"],
                               bins=age_bins,
                               labels=age_labels)

victim_ages = crimes["Age Bracket"].value_counts()
print(victim_ages)
```

| Part | What it does |
|---|---|
| `pd.cut()` | Assigns each age to a labeled bin — age 28 → "26-34" |
| `.value_counts()` | Counts victims per age group, sorted most to least |
