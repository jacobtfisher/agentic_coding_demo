# Reddit AITA ("Am I the Asshole?") Dataset

## Overview

This dataset contains thousands of posts from the popular subreddit r/AmItheAsshole. Users submit stories about interpersonal conflicts and ask the community to judge who was in the wrong. It is an excellent resource for studying social norms, conflict resolution, and linguistic framing.

## Source & Citation

**Repository:** [Hugging Face: OsamaBsher/AITA-Reddit-Dataset](https://huggingface.co/datasets/OsamaBsher/AITA-Reddit-Dataset)

- **Original Context:** Reddit users post a situation, and the community votes on a verdict.

## Data Description

The dataset typically contains the text of the user's story and the final community verdict.

### Key Variables

- **id:** Unique Reddit post ID.
- **title:** The title of the post (often contains summaries like "AITA for yelling at my cat?").
- **text:** The full body text of the story.
- **verdict:** The final judgment tag assigned by the community.
- **comment1:** The top-voted comment on the post agreeing with the overall verdict.
- **comment2:** The second-top-voted comment on the post agreeing with the overall verdict.
- **score:** The net upvotes/downvotes.

### Verdict Labels (Crucial)

- `NTA`: **Not the Asshole** (The poster is in the right).
- `YTA`: **You're the Asshole** (The poster is in the wrong).
- `ESH`: **Everyone Sucks Here** (Everyone is wrong).
- `NAH`: **No Assholes Here** (Just a bad situation).

## Tutorial Research Questions (Simple & Visual)

**1. The "Defense" Hypothesis (Feature Engineering + Distribution Plot)**

- _Theory:_ People who know they are wrong might write longer, more convoluted stories to justify their behavior.
- _Agentic Task:_ Create a new column called `word_count` based on the `text` column.
- _Visualization:_ Create a **Raincloud Plot** (or a Violin plot with raw data points) comparing the distribution of `word_count` across the four `verdict` categories. Do YTA authors write more?

**2. The "Engagement" Hypothesis (Grouping + Bar Chart)**

- _Theory:_ The community might find "Asshole" stories more entertaining or engaging than "Not the Asshole" stories.
- _Agentic Task:_ Group the data by `verdict` and calculate the average (mean) and median `score`.
- _Visualization:_ Create a **Bar Chart** showing the mean Score for each Verdict. Ensure the bars have Error Bars (Standard Error) to show uncertainty.

**3. The "Sentiment" Hypothesis (Basic NLP + Boxplot)**

- _Theory:_ Titles that use negative language might predispose the community to vote "YTA".
- _Agentic Task:_ Use a simple sentiment analysis library (like `vader` or `textblob`) to calculate a `sentiment_score` for the `title` column only.
- _Visualization:_ Create a **Box Plot** showing the spread of Title Sentiment scores for each Verdict. Are YTA titles significantly more negative?

---

### 🤖 Notes for AI Context (Read this before coding)

**1. Data Loading:**

- You are reading a CSV file.
- **Check the Verdict Column:** Before plotting, check if the `verdict` column needs cleaning. If there are values other than NTA, YTA, ESH, NAH, filter the dataset to keep only those four main categories.

**2. Text Cleaning Requirements:**

- **Structure:** Many posts contain "EDIT:" or "UPDATE:" at the bottom. For a clean analysis, strip text appearing after the word "EDIT:".
- **Missing Data:** Check if `score` is numeric. If it is stored as a string (e.g., "1.2k"), convert it to a number.

**3. Visualization Rules (Visual Best Practices):**

- **Color:** Use a colorblind-friendly palette (e.g., Viridis or Okabe-Ito).
- **Clutter:** Remove the gray background grid (use `theme_classic()` or similar).
- **Labels:** Rename axis labels from variable names (e.g., `word_count`) to human-readable text (e.g., "Story Length (Words)").
