# Analyzing Students' Mental Health — Workshop Teaching Guide

---

## The Task

> *A Japanese international university surveyed its students in 2018. International students have a higher risk of mental health difficulties. Social connectedness and acculturative stress are predictive of depression.*
>
> *Explore the `students` data using PostgreSQL to see if the length of stay is a contributing factor for international students.*

### The Table

| Column | Description |
|---|---|
| `inter_dom` | `'Inter'` (international) or `'Dom'` (domestic) |
| `stay` | Length of stay in years |
| `todep` | Depression score (PHQ-9; higher = more depressed) |
| `tosc` | Social connectedness score (SCS; higher = more connected) |
| `toas` | Acculturative stress score (ASISS; higher = more stressed) |

---

## How to Lead Students to the Question

### What the task is really asking:
- We have international and domestic students with mental health scores
- The research says international students struggle more
- **Our job:** For international students only, do their scores change based on how long they've been staying?

### Lead-in:
- "Who do we care about? International students — so we need to filter."
- "What are we comparing across? Length of stay — so we need to group by stay."
- "What are we measuring? Average depression, connectedness, and stress per group."
- "How do we present it? Longest stay first, so we can see the trend."

---

## Cell 1: Preview the Data

```sql
SELECT *
FROM students;
```

| Part | What it does |
|---|---|
| `SELECT *` | Selects all columns |
| `FROM students` | From the students table |
| **Purpose** | See what the data looks like — column names, values, types |

---

## Cell 2: The Full Query

```sql
SELECT stay,
    COUNT(*)                  AS count_int,
    ROUND(AVG(todep), 2)      AS average_phq,
    ROUND(AVG(tosc), 2)       AS average_scs,
    ROUND(AVG(toas), 2)       AS average_as
FROM students
WHERE inter_dom = 'Inter'
GROUP BY stay
ORDER BY stay DESC;
```

### Clause-by-Clause Breakdown

| Clause | What it does | Why |
|---|---|---|
| `SELECT stay` | Shows the stay length | So we know which group each row is |
| `COUNT(*) AS count_int` | Counts students per stay length | Sample size — matters for reliability |
| `ROUND(AVG(todep), 2)` | Average depression score, 2 decimals | Key mental health metric |
| `ROUND(AVG(tosc), 2)` | Average social connectedness, 2 decimals | Higher = more connected |
| `ROUND(AVG(toas), 2)` | Average acculturative stress, 2 decimals | Higher = more culture-shock stress |
| `FROM students` | The table | Where the data lives |
| `WHERE inter_dom = 'Inter'` | International students only | Task says to focus on this group |
| `GROUP BY stay` | One row per stay length | So COUNT/AVG calculate per group |
| `ORDER BY stay DESC` | Longest stay first | Makes it easy to spot trends |

### Key Functions Explained

| Function | What it does |
|---|---|
| `COUNT(*)` | Counts how many rows are in each group |
| `AVG(column)` | Calculates the mean of a numeric column per group |
| `ROUND(value, 2)` | Rounds to 2 decimal places for clean output |
| `AS alias` | Gives the column a readable name in the results |
