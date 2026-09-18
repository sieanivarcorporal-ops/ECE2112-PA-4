# ECE2112-PA-4

**Made by Siean Ivar B. Corporal | 2ECE - A**

**Objectives**
  1. filter tabular data using several categorical and numerical conditions;
  2. construct focused DataFrames by selecting relevant features;
  3. summarize the relationship between categorical features and a numerical variable; and
  4. communicate a data comparison using clear and correctly labeled plots.

## Task Breakdown & Syntax Used

### A. Visayas Communication DataFrame
```python
df = pd.read_csv('board2.csv')
viscom = df[(df['Track'] == 'Communication') & (df['Hometown'] == 'Visayas')]
viscom = viscom.loc[:, ['Name', 'Gender', 'Math', 'Electronics', 'Average']]
```
**Syntax highlights:**
- `pd.read_csv()` — loads the dataset into a DataFrame
- **Boolean indexing with `&`** — combines two conditions (`Track` and `Hometown`) in a single filter; each condition is wrapped in parentheses, which is required when chaining with `&`/`|`
- `.loc[:, [...]]` — selects a specific, ordered list of columns after filtering rows
- `.shape` — reports `(rows, columns)` of the result

### B. Visayas Female DataFrame
```python
VisFemale = df[(df['Gender'] == 'Female') & (df['Hometown'] == 'Visayas')]
VisFemale = VisFemale.loc[:, ['Name', 'Track', 'GEAS', 'Electronics', 'Average']]
display(VisFemale.loc[VisFemale['Average'] >= 60])
```
**Syntax highlights:**
- Same boolean-indexing pattern as Task A, applied to different columns
- A **second, independent filter** (`Average >= 60`) applied via `.loc[]` on the already-filtered DataFrame, without overwriting the original filtered result

### C. Category-Average Visualization
```python
mean_track    = df.groupby('Track')['Average'].mean()
mean_gender   = df.groupby('Gender')['Average'].mean()
mean_hometown = df.groupby('Hometown')['Average'].mean()

fig, ax = plt.subplots(1, 3, figsize=(20, 4))
ax[0].bar(mean_track.index, mean_track.values)
...
plt.show()

top_track, top_track_val = mean_track.idxmax(), mean_track.max()
print(f"... {top_track} ... {top_track_val:.2f}")
```
**Syntax highlights:**
- `.groupby(col)[target].mean()` — computes the mean of a numeric column per category, returning a Series indexed by category
- `plt.subplots(1, 3, figsize=...)` — creates one figure with three side-by-side Axes objects (`ax[0]`, `ax[1]`, `ax[2]`), rather than three separate figures
- `ax[i].bar(x, height)` — draws a bar chart into a specific subplot, using a Series' `.index` (category labels) as `x` and `.values` (means) as `height`
- `.set_title()`, `.set_xlabel()`, `.set_ylabel()` — per-subplot labeling, called on the Axes object itself (not on `plt`)
- `.idxmax()` / `.max()` — pull the category label and value of the largest group mean directly from the Series, instead of hardcoding numbers
- **f-strings** (`f"... {var} ..."`) with format specifiers (`{val:.2f}`) — insert variable values into text, rounded to 2 decimal places

## Author
Siean Ivar B. Corporal, 2ECE-A — ECE 2112, University of Santo Tomas
