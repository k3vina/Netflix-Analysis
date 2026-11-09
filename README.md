# Netflix Movies Analysis: Tableau Dashboard

An interactive Tableau dashboard exploring a catalog of 16,000 movies (released 2010 to 2025) by genre, ratings, budget and revenue, language and country of origin.

## Project Overview

The goal of this project is to turn a raw movie dataset into a set of clear visualizations that answer a few simple questions:

- Which genres are most common, and which are rated highest?
- Has the rating of each genre changed over the years?
- Do bigger budgets lead to bigger revenue?
- Which languages and countries dominate the catalog?

## Dataset

- **File:** `netflix_movies_detailed_up_to_2025.csv`
- **Size:** 16,000 rows, 18 columns
- **Coverage:** Movies released from 2010 to 2025, exactly 1,000 per release year

| Column | Description |
|---|---|
| show_id | Unique ID for each movie |
| title, director, cast, description | Descriptive text fields |
| country, language | Production countries (can list several) and language code |
| date_added, release_year | When the title was added and when it was released |
| genres | Comma-separated list of genres |
| rating, vote_average, vote_count | Audience rating data (rating and vote_average are identical) |
| popularity | Popularity score |
| budget, revenue | Financial figures in dollars |

### Data notes

- `type` is "Movie" in every row, so it was not used.
- `duration` is empty for all rows, so it was not used.
- Budget and revenue are 0 for most movies. Only 3,540 movies (about 22%) have both, so the finance chart covers only that subset.
- 894 movies have zero votes, so rating averages exclude movies with fewer than 50 votes.
- `genres` and `country` can contain several values. The charts use the **first listed** value as the primary genre or country.

## Calculated Fields (Tableau)

| Field | Formula |
|---|---|
| Primary Genre | `TRIM(SPLIT([genres], ",", 1))` |
| Primary Country | `TRIM(SPLIT([country], ",", 1))` (geographic role: Country/Region) |
| Has Financials | `[budget] > 0 AND [revenue] > 0` |
| Enough Votes | `[vote_count] >= 50` |
| ROI | `IF [budget] > 0 AND [revenue] > 0 THEN ([revenue] - [budget]) / [budget] END` |

`Release Year` was converted to a discrete dimension so each year appears as its own column.

## Visualizations

1. **Genre Landscape (bar chart)**
   Number of movies per primary genre, colored by average vote. Filters: Enough Votes = True, top genres only, nulls excluded.

2. **Ratings Heatmap (genre by year)**
   Average vote for each genre in each release year, using a diverging color scale. Shows which genres consistently rate higher or lower.

3. **Budget vs Revenue (scatter plot)**
   One dot per movie, log-scale axes, colored by genre and sized by popularity, with a trend line. Filters: Has Financials = True, minimum budget and revenue of about $10,000, top 8 genres.

4. **Language and Country**
   A map of movies by primary country, plus a bar chart of the top 10 languages (aliases applied to the language codes).

### Dashboard

The four views are combined into one dashboard with a **Release Year** filter applied to all sheets, and the Genre Landscape chart set to act as a filter for the others.

## Key Findings

- **Animation and Documentary rate highest, Horror lowest.** Across all listed genres, Animation averages about 7.0 and Horror about 5.8.
- **Genre volume and quality don't match.** Horror is widely produced but rates lowest.
- **The catalog is heavily English and US-centered.** English accounts for 9,534 of 16,000 movies (about 60%), followed by French (1,054), Japanese (904), Korean (876) and Spanish (731). The US leads by country, with roughly 5,700 movies as primary country.
- **Revenue rises with budget, but with wide spread.** Big budgets do not guarantee big returns, and a few blockbusters (for example Avengers: Endgame, about $2.8B revenue) sit far above the trend.

## Limitations

- Genre and country charts use only the first value listed for each movie.
- The finance chart covers about 22% of the movies, and some budget or revenue values look like data-entry errors, so a minimum threshold was applied.
- Because every release year has exactly 1,000 movies, the dataset is not suited to analysing how catalog size changed over time.

## Tools

- Tableau (Desktop or Public)
- Python (pandas) for initial data profiling

## How to Open

1. Download the packaged workbook (`.twbx`) or open the Tableau Public link below.
2. If opening a `.twb`, point Tableau to `netflix_movies_detailed_up_to_2025.csv` when prompted.

**Tableau Public link:** _add your link here_

## Author

Kevina Jepkong'a
