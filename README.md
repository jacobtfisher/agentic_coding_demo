# COM 903: Agentic Coding for Social Scientists

## Overview

This repository contains the materials for the tutorial session on **Agentic Coding**.

In the era of Large Language Models (LLMs), the barrier to entry for computational social science has shifted. The primary challenge is no longer **syntax retrieval** (memorizing how to write a `for` loop or the specific arguments for `ggplot2`), but **architectural logic** (knowing _what_ steps are necessary to solve a problem validly).

This tutorial introduces a workflow using AI-assisted tools (Cursor, Copilot) to shift your cognitive load from "writing code" to "designing analysis."

---

## Architect vs. Contractor

Traditional coding often feels like laying bricks. You must manually place every semicolon, bracket, and function name. If you miss one, the wall falls down.

**Agentic Coding** changes your role from **Bricklayer** to **Site Manager (Architect)**.

- **The AI (The Contractor):** Handles the syntax, API calls, and boilerplate code. It builds the wall.
- **You (The Architect):** Provides the blueprint (logic), enforces building codes (validity), and inspects the work (verification).

### Connection to Course Readings

- **Cognitive Offloading (Mensh & Kording):** Just as good paper structure frees up working memory for the reader, agentic coding frees up your working memory to focus on the _logic_ of your analysis rather than the implementation details.
- **Stability (Yu & Kumbier):** Writing code to test 50 different analytical choices (e.g., thresholding) is tedious by hand. AI makes rigorous stability testing "cheap" in terms of effort, allowing us to easily simulate the "multiverse" of analytical choices.

---

## Hand Coding vs. Agentic Coding

| Feature               | **Traditional Hand Coding**                      | **Agentic Coding**                                                    |
| :-------------------- | :----------------------------------------------- | :-------------------------------------------------------------------- |
| **Starting Point**    | Empty file; typing `library(tidyverse)`          | **Context file**; writing plain English comments describing the goal. |
| **Cognitive Load**    | High on **Syntax** (grammar, punctuation).       | High on **Logic** (step-by-step process).                             |
| **Primary Friction**  | Syntax errors ("Unexpected symbol in...").       | **Context errors** (AI assumes variables that don't exist).           |
| **Development Style** | Bottom-up (write line by line).                  | Top-down (describe the whole function, then refine).                  |
| **Debugging**         | Googling error messages; checking StackOverflow. | "Add to Chat" -> "Fix this error" -> Verify fix.                      |
| **Verification**      | Implicit (if it runs, it's probably right).      | **Explicit** (it runs, but did it do what I asked?).                  |

---

## The Stack

I recommend using **[Cursor](https://cursor.sh/)** (a fork of VS Code) for this workflow.

- **Why Cursor?** Unlike standard ChatGPT, Cursor has **Context Awareness**. It can see your entire file folder, your datasets, and your documentation.
- **The Models:**
  - **Tab-Autocomplete:** (Fast) Predicts the next few words based on cursor position.
  - **Chat (Cmd+L):** (Smart) Uses Claude 4.5 Sonnet or GPT-5.1-mini to solve logic problems and refactor code.
  - **Inline Generation (Cmd+K):** Generates code blocks based on highlighted comments.

---

## Safety Protocols

Agent-written code is prone to "silent failures"—code that runs without error but produces mathematically incorrect results. You must build **Code Checks** into your workflow.

### 1. Context Check (Grounding)

AI models often hallucinate when they guess your data structure. Never ask an agent to code blindly.

- **Technique:** Each dataset folder contains a `README.md` file describing the columns, variable types, and data structure. Feed this to the AI using `@data/[dataset]/README.md` before asking for code.

### 2. Sanity Check (Verification)

After the AI generates a data transformation, immediately ask for a summary view to prove it worked.

- **Bad:** Running the code and moving to the next step.
- **Good:** Running the code, then running `glimpse()`, `table()`, or `summary()` to prove the NAs were handled correctly.

### 3. Logic Check (Step-by-Step)

Use **Comment-Driven Development**. Write the logic in comments first, then let the AI fill it in.

- _Comment:_ `# Pivot data to long format, keeping ID and Condition fixed.`
- _AI Action:_ Generates the `pivot_longer` code.
- _Your Job:_ Read the generated arguments. Did it pick the right columns to pivot?

### 4. Cross-Model Check (Peer Review)

Different AI models have different strengths and may catch errors that another model missed. Use multiple models to review critical code sections.

- **Technique:** After one model generates code, switch to a different model (e.g., from Claude to GPT, or vice versa) and ask it to review the code for logic errors, edge cases, or potential bugs.
- **When to Use:** Especially valuable for complex transformations, statistical calculations, or code that will be used in multiple analyses.
- **Example Workflow:** Generate code with Claude → Switch to GPT → Ask "Review this code for logical errors. Does the pivot operation correctly handle missing values?"

---

## The Datasets

We will use two "messy" datasets to demonstrate how Agentic Coding handles data cleaning and stability testing.

### 1. Star Wars Survey

- **Source:** FiveThirtyEight.
- **The Challenge:** A "wide" dataset with two header rows and inconsistent text encoding.
- **The Goal:** Use agents to clean the headers, recode Likert scales to numeric values, and visualize the "nostalgia gap" between generations.
- **Context File:** `data/starwars/README.md`

### 2. Reddit AITA ("Am I the Asshole?")

- **Source:** Hugging Face / Reddit.
- **The Challenge:** Unstructured text data, community verdicts, and potential moderator tags mixed in.
- **The Goal:** Use agents to perform basic Feature Engineering (text length, sentiment analysis) and visualize how these features predict the community verdict.
- **Context File:** `data/aita/README.md`

---

## Quick Start Guide (The "Pseudo-Code" Workflow)

1.  **Open Cursor** and load your project folder.
2.  **Open the `README.md`** file within your chosen dataset folder (e.g., `data/starwars/README.md` or `data/aita/README.md`) to understand your data structure and research questions.
3.  **Create a new script** (e.g., `analysis.R` or `analysis.py`).
4.  **Provide context to the AI:** Use `@data/[dataset]/README.md` in the chat to give the AI context about your data structure before asking for code.
5.  **Write a comment** describing your first step: `# Load the tidyverse and read the Star Wars CSV. Note that the header is on row 2.`
6.  **Hit `Cmd+K`** (Generate) or use Chat (`Cmd+L`) to generate code.
7.  **Review and Run.**
8.  **Iterate:** If it errors, click "Add to Chat" and ask the agent to fix it.
