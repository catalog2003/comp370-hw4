# COMP 370/570 Homework 4

## Dataset

Dataset used: My Little Pony Transcript — `clean_dialog.csv`

The dataset was downloaded from the My Little Pony Transcript Kaggle dataset and the `clean_dialog.csv` file was used for this analysis.

## Dataset Size

Command:

```bash
wc -l clean_dialog.csv
```

Result:

```text
36860 clean_dialog.csv
```

The CSV contains one header row, so the number of dialogue records is:

```bash
echo $((36860 - 1))
```

Result:

```text
36859
```

Therefore, the dataset contains **36,859 dialogue records**, excluding the header.

The number of records was also verified using `csvtool`:

```bash
csvtool col 1 clean_dialog.csv | tail -n +2 | wc -l
```

Result:

```text
36859
```

## Dataset Structure

Command:

```bash
head -n 5 clean_dialog.csv
```

The dataset has four fields:

* `title` — the title of the episode
* `writer` — the writer associated with the episode
* `pony` — the speaker or speakers associated with the dialogue
* `dialog` — the spoken dialogue

The header is:

```text
"title","writer","pony","dialog"
```

Because the CSV contains commas inside quoted fields, `csvtool` was used instead of relying on a simple `cut -d','` command.

## Number of Episodes

Command:

```bash
csvtool col 1 clean_dialog.csv | tail -n +2 | sort -u | wc -l
```

Result:

```text
197
```

Therefore, the dataset contains **197 unique episodes**.

## Unexpected Dataset Property

Command:

```bash
csvtool col 3 clean_dialog.csv | tail -n +2 | grep -i " and " | head -10
```

Example results:

```text
Narrator and Twilight Sparkle
Twilight Sparkle and Rainbow Dash
Applejack and Twilight Sparkle
Rarity and Pinkie Pie
Rainbow Dash and Gilda
Gilda and Rainbow Dash
Snips and Snails
Snips and Snails
Snips and Snails
Snips and Snails
```

A further count was performed using:

```bash
csvtool col 3 clean_dialog.csv | tail -n +2 | grep -i " and " | wc -l
```

Result:

```text
294
```

An unexpected property of the dataset is that the `pony` field does not always contain a single speaker. At least **294 speaker entries contain "and"**, indicating that multiple characters can be associated with a single dialogue record.

For example, the dataset contains:

```text
Narrator and Twilight Sparkle
Twilight Sparkle and Rainbow Dash
Rarity and Pinkie Pie
```

This could create issues during speaker-frequency analysis because it is not always clear how a dialogue record involving multiple speakers should be attributed to individual characters.

## Speaker Frequency

The speaker column was extracted with `csvtool`, and `grep` was used to count exact speaker labels.

### Twilight Sparkle

Command:

```bash
csvtool col 3 clean_dialog.csv | tail -n +2 | grep -i -x "Twilight Sparkle" | wc -l
```

Result:

```text
4745
```

### Rarity

Command:

```bash
csvtool col 3 clean_dialog.csv | tail -n +2 | grep -i -x "Rarity" | wc -l
```

Result:

```text
2660
```

### Pinkie Pie

Command:

```bash
csvtool col 3 clean_dialog.csv | tail -n +2 | grep -i -x "Pinkie Pie" | wc -l
```

Result:

```text
2833
```

### Rainbow Dash

Command:

```bash
csvtool col 3 clean_dialog.csv | tail -n +2 | grep -i -x "Rainbow Dash" | wc -l
```

Result:

```text
3072
```

### Fluttershy

Command:

```bash
csvtool col 3 clean_dialog.csv | tail -n +2 | grep -i -x "Fluttershy" | wc -l
```

Result:

```text
2109
```

The resulting speaker frequencies are:

| Pony             | Line count |
| ---------------- | ---------: |
| Twilight Sparkle |      4,745 |
| Rarity           |      2,660 |
| Pinkie Pie       |      2,833 |
| Rainbow Dash     |      3,072 |
| Fluttershy       |      2,109 |

## Line Percentages

The percentage of all dialogue records for each main pony was calculated using:

```text
pony line count / total dialogue records × 100
```

The total number of dialogue records is **36,859**.

The calculated percentages are:

| Pony             | Lines | Percentage of all lines |
| ---------------- | ----: | ----------------------: |
| Twilight Sparkle | 4,745 |                  12.87% |
| Rarity           | 2,660 |                   7.22% |
| Pinkie Pie       | 2,833 |                   7.69% |
| Rainbow Dash     | 3,072 |                   8.33% |
| Fluttershy       | 2,109 |                   5.72% |

The final results are stored in `Line_percentages.csv`.
