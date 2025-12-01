# Star Wars Survey Dataset

## Overview

This dataset comes from a survey conducted by FiveThirtyEight (Walt Hickey) to determine the popularity of the _Star Wars_ franchise, individual films, and characters among the American public. It is a classic example of "messy" survey data involving Likert scales, rankings, and check-all-that-apply questions.

## Source & Citation

- **Original Article:** ["America’s Favorite ‘Star Wars’ Movies (And Least Favorite Characters)"](https://fivethirtyeight.com/features/americas-favorite-star-wars-movies-and-least-favorite-characters/)
- **Repository:** [FiveThirtyEight GitHub](https://github.com/fivethirtyeight/data/tree/master/star-wars-survey)
- **Raw Data Link:** `https://raw.githubusercontent.com/fivethirtyeight/data/master/star-wars-survey/StarWars.csv`

## Data Description

The dataset contains 1,186 responses. The raw CSV is in a "wide" format with human-readable questions as column headers, often requiring significant cleaning to be usable for analysis.

### Key Variables

- **RespondentID:** Unique identifier.
- **Have you seen any...?**: Filter question (Yes/No).
- **Have you seen...?**: Checkbox-style columns for Episodes I–VI.
- **Rankings:** Respondents ranked Episodes I–VI from 1 (Most Favorite) to 6 (Least Favorite).
- **Favorability:** Likert-scale ratings ("Very favorably" to "Very unfavorably") for key characters (Han Solo, Jar Jar Binks, etc.).
- **Demographics:** Gender, Age, Household Income, Education, Location.

## Tutorial Research Questions (Visual & Transformation Focused)

**1. The "Nostalgia" Hypothesis (Pivoting & Interaction Plot)**

- _Theory:_ Older generations prefer the Original Trilogy (Ep 4-6), while younger generations might be more forgiving of the Prequels (Ep 1-3).
- _Agentic Task:_
  1.  Pivot the 6 "Ranking" columns from Wide to Long format.
  2.  Create a new column `trilogy` (Original vs. Prequel).
  3.  Group by `Age` and `trilogy` to calculate the average Rank (lower rank = better).
- _Visualization:_ Create an **Interaction Plot** (Line Chart) with Age on the X-axis, Average Rank on the Y-axis, and lines colored by Trilogy.

**2. The "Polarization" Map (Recoding & Scatterplot)**

- _Theory:_ Some characters are universally loved (Han Solo), while others are polarizing (Jar Jar Binks).
- _Agentic Task:_
  1.  Convert the character Favorability columns (text) into numeric scores (e.g., Very Unfavorably = -2, Neutral = 0, Very Favorably = +2).
  2.  Calculate the **Mean** (Popularity) and **Standard Deviation** (Divisiveness) for each character.
- _Visualization:_ Create a **Scatterplot** where X = Mean Popularity, Y = Standard Deviation. Add text labels for each point so we can see who the "controversial" characters are.

**3. The "Gatekeeper" Stability Test (Filtering & Bar Chart)**

- _Theory:_ "True Fans" have different opinions than the general public. Does the data result change depending on how we define the sample?
- _Agentic Task:_ Calculate the % of people who rated "Jar Jar Binks" favorably (Somewhat or Very). Calculate this number for two different subsets:
  - **Subset A (Casual):** Anyone who answered "Yes" to seeing _any_ film.
  - **Subset B (Hardcore):** Only people who ticked the box for seeing _all 6_ films.
- _Visualization:_ Create a **Bar Chart** comparing the Jar Jar Favorability % between Casual vs. Hardcore fans. (This demonstrates the Yu & Kumbier "Stability" principle).

---

### 🤖 Notes for AI Context (Read this before coding)

**1. The Header Problem:**

- This CSV has **two** header rows.
  - Row 1: The Question (e.g., "Please rank the Star Wars films...")
  - Row 2: The Option (e.g., "Star Wars: Episode I")
- _Tip for AI:_ Tell the AI to "Reload the dataset using the second row as the header, or manually rename the columns to be snake_case (e.g., `rank_ep_1`, `rank_ep_2`)."

**2. Data Types:**

- **Rankings:** These might import as character strings because of the header rows. Ensure they are converted to numeric.
- **Likert Scales:** These are text strings. You will need to explicitly map them to numbers if doing math (e.g., `case_when` in R or `map` in Python).

**3. Missing Data:**

- Respondents who answered "No" to the first question ("Have you seen any films?") will have `NA` for all subsequent ranking/opinion columns. This is expected.
