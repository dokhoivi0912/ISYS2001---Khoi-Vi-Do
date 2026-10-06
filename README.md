# 🎓 Student Budget Coach & Personal Finance Assistant

**ISYS2001 Assessment 2: Programming Project**
* **Student Name:** Khoi Vi Do
* **Student ID:** 23391554
* **Course:** ISYS2001 - Introduction to Business Programming (Curtin University)
* **Instructor / Collaborator:** Kevin Blasiak (`kevin-blasiak-curtin`)
* **Repository Link:** [https://github.com/dokhoivi0912/ISYS2001---Khoi-Vi-Do](https://github.com/dokhoivi0912/ISYS2001---Khoi-Vi-Do)

---

## 📌 Project Overview & Problem Statement

### The Problem
University students often struggle with managing limited monthly allowances, part-time wages, and daily living expenses. Without a structured budget, students frequently overspend on discretionary items ("Wants") before covering essential needs ("Essentials") or setting aside long-term savings.

### The Solution
The **Student Budget Coach** is a user-friendly, interactive Python web application built using **Google Colab** and **Gradio**. It automates the widely recommended **50/30/20 financial rule** (allocating 50% of income to Essentials, 30% to Wants, and 20% to Savings). The application accepts income figures either via **manual numerical entry** or by analysing a real transaction history from a **CSV file** using **Pandas**, and delivers personalised advice powered by **Google Gemini AI**, grounded in the user's actual category-by-category spending rather than a single generic number.

---

## ✨ Requirements Alignment (R1 – R6)

This project adheres to all six requirements outlined in the ISYS2001 Assessment 2 Specification:

| Requirement | Implementation Detail | Status |
| :--- | :--- | :---: |
| **R1: Conversational AI Assistant** | Calls Google's Gemini API (`gemini-flash-latest`, with automatic fallback to other free models if one is temporarily overloaded) directly via the **`requests`** library, with a system instruction defining an empathetic "Student Finance Advisor" persona that politely declines off-topic questions and redirects to budgeting. | ✅ Pass |
| **R2: Grounded in User Data** | Uses **Pandas** (`pd.read_csv`) to load `student_transactions.csv`, then buckets every transaction into Essentials / Wants / Savings / Income with a `CATEGORY_MAP` dictionary and a plain loop. Reports **actual dollars and % spent per category** against the 50/30/20 target, and both the Gemini advisor's reply and the Gradio CSV tab reflect this real breakdown — not a flat re-split of one summed total. | ✅ Pass |
| **R3: Custom Analytical Tool** | Implemented `calculate_budget(income)` to compute exact 50/30/20 dollar breakdowns, handling non-numeric, zero, and negative income safely without crashing. | ✅ Pass |
| **R4: Interactive Web Interface** | Designed a 3-tab user interface using **Gradio Blocks** (`gr.Blocks`) separating Manual Calculation, CSV File Processing, and Gemini AI Consulting, each wired to the current versions of the underlying functions. | ✅ Pass |
| **R5: Robust Assertion Testing** | Verified business logic with `assert` statements across all four core functions (`calculate_budget`, `load_transactions_df`, `analyze_transactions`, `compare_to_recommended`): standard figures, boundary values ($0), negative input (-$50), floating decimals, a missing CSV file, a CSV missing required columns, non-numeric transaction amounts, an unrecognised category, and overspending (net income ≤ 0). | ✅ Pass |
| **R6: Six-Step Problem-Solving Method** | Documented fully in `Assessment_2.ipynb` under explicit headings: Step 1 Understand the Problem, Step 2 Inputs/Outputs, Step 3 Worked Example, Step 4 Pseudocode, Step 5 Convert to Python, Step 6 Test with a Variety of Data — each step present and specific to this project. | ✅ Pass |

---

## 📊 Sample Inputs & Outputs

### 1. Manual Budget Calculation (Tab 1)
* **Input:** Income = `$1000.00`
* **Output:** `Essentials (50%): $500.00 | Wants (30%): $300.00 | Savings (20%): $200.00`

### 2. CSV Transaction Analysis (Tab 2)
* **Input File:** `student_transactions.csv` (15 transactions including weekly allowance, part-time cafe wages, rent, groceries, coffee, textbooks, transport, and a refund).
* **Analysis Output:**
* Net income this period: $765.00

` Actual vs. recommended 50/30/20 split:`
` Essentials: spent $690.95 (90.3% of income) vs. 50% target`
` Wants: spent $13.50 (1.8% of income) vs. 30% target`
` Savings: spent $0.00 (0.0% of income) vs. 20% target`

  This grounds the output in the student's real category spending — two CSVs with the same total
  income would no longer produce an identical result, because each category is tracked separately.

### 3. Gemini AI Financial Consultation (Tab 3)
* **Input:** Income = `$1000.00` | **User Question:** *"What's the weather like in Perth today?"* (off-topic test)
* **Grounded AI Output:** *"I'm happy to chat, but I can only help you with your budget here! Let's keep our focus on your finances—would you like to review your $500 essentials, $300 wants, or $200 savings?"*

---

## 🛠️ Execution Guide for Google Colab

1. **Open the Notebook:** Open `Assessment_2.ipynb` in Google Colab.
2. **Setup Gemini API Key:**
   * Acquire a free API key from [Google AI Studio](https://aistudio.google.com/apikey).
   * On Colab's left sidebar, click the **Secrets 🔑** icon.
   * Add a new secret named `GEMINI_API_KEY` and paste your key into the value field.
   * Enable the **Notebook access** toggle for it.
3. **Upload the CSV File:**
   * On Colab's left sidebar, click the **Files 📁** icon.
   * Upload `student_transactions.csv` to the session storage (or your own CSV with the same
     columns: `Date, Description, Amount, Category`).
4. **Launch the Application:**
   * Run every cell from top to bottom (`Runtime` → `Run all`). Cells depend on each other in
     order — a later cell will throw `NameError` if an earlier cell defining a function it calls
     hasn't run yet.
   * Interact with the app using the embedded Gradio panel or the generated public link
     (`https://xxxx.gradio.live`).

---

## 🐛 Debugging & Reliability Notes

Several real runtime issues were found and fixed during development, all documented with prompts
and screenshots in `Developer_Diary.md`:

* **Stale/duplicate function definitions (`NameError`):** Running a later cell without first
  running the cell that defines a function it depends on threw `NameError: name 'calculate_budget'
  is not defined`. Fixed by making dependent cells self-contained and always running the notebook
  top to bottom.
* **Deprecated model (`404 Not Found`):** `gemini-2.0-flash` had been removed from the available
  model list. Diagnosed using the Gemini `ListModels` endpoint and switched to the
  `gemini-flash-latest` alias, as the spec recommends, so the app automatically tracks Google's
  current free Flash model.
* **Transient server overload (`503 Service Unavailable`):** Added exponential backoff retries,
  then a fallback across a short list of alternate free-tier models, so a temporarily busy model
  doesn't fail the whole request.
* **Free-tier rate limit (`429 Too Many Requests`):** Learned this needs different handling from a
  503 — retrying immediately only burns more of the same quota — so the app surfaces a clear
  "please wait" message instead of retrying blindly.
* **Ungrounded CSV analysis:** The original `process_csv_income()` summed every transaction
  (income minus expenses) into one number and re-applied the 50/30/20 split to it, so two
  different CSVs with the same total produced identical results. Rewrote it as
  `load_transactions_df()` + `analyze_transactions()` + `compare_to_recommended()` to compare
  **actual spending per category** against the target — a genuine fix to meet R2.
* **Stale Gradio interface:** After rewriting the R1 and R2 functions, the R4 Gradio cell still
  called the old function names (`process_csv_income`, the old Gemini SDK pattern), which would
  have thrown `NameError` the moment a user clicked a button. Fixed by syncing `run_csv_processor`
  and `run_ai_advisor` to call the current functions.
* **Scope check:** An early draft of the category-analysis code used `pandas .map()` with a
  lambda function — technically correct, but outside what this unit actually taught. Rewritten
  using a plain `for` loop with `if`/`elif` and dictionaries instead, so every line is something
  explainable using only this unit's syllabus.

---

## 📂 Repository File Structure

```text
ISYS2001---Khoi-Vi-Do/
├── Assessment_2.ipynb        # Main Colab notebook (6-step method, code, assert tests & Gradio app)
├── student_transactions.csv  # Sample student financial transaction dataset for Pandas processing
├── Developer_Diary.md        # Weekly development log tracking AI prompts, critiques, screenshots & debugs
└── README.md                 # Executive project documentation & operational instructions
```
