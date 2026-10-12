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
The **Student Budget Coach** is a user-friendly, interactive Python web application built using **Google Colab** and **Gradio**. It applies the widely recommended **50/30/20 financial rule** (50% Essentials, 30% Wants, 20% Savings) in two ways:

1. **Manual entry:** the student types in a monthly income and gets the recommended dollar split.
2. **Real transaction data:** the student uploads a **CSV of their own transactions**, which is analysed with **Pandas**. Each transaction is bucketed into Essentials / Wants / Savings / Income, and the app reports the **actual dollars and % spent per bucket** against the 50/30/20 target.

A **Google Gemini** "Student Finance Advisor" then answers questions grounded in those real numbers (the manual budget figures, or the uploaded CSV's category breakdown) instead of giving generic advice.

---

## ✨ Requirements Alignment (R1 – R6)

| Requirement | Implementation Detail |
| :--- | :--- |
| **R1: Conversational AI Assistant** | Calls Google's Gemini API directly via the **`requests`** library (`requests.post()` to the REST endpoint). Uses `gemini-flash-latest`, with an ordered fallback list of other free models. A `SYSTEM_INSTRUCTION` defines a friendly "Student Finance Advisor" persona that stays on budgeting, politely declines off-topic questions and asks a short clarifying question when a request is unclear. The API key is read from Colab Secrets and is never stored in the notebook or repository. |
| **R2: Grounded in User Data** | `load_transactions_df()` reads the CSV with Pandas, `analyze_transactions()` buckets every transaction using a `CATEGORY_MAP` dictionary and a plain `for` loop, and `compare_to_recommended()` reports actual spend per bucket against the 50/30/20 target. Both the **Tab 2 summary** and the **Tab 3 Gemini advisor** (when a CSV is uploaded) reflect this real breakdown, so different data gives different answers. |
| **R3: Custom Analytical Tool** | `calculate_budget(income)` is my own logic that computes the 50/30/20 dollar breakdown. It rejects non-numeric, zero and negative income with a clear message instead of crashing, and is connected to Tab 1 of the interface. |
| **R4: Interactive Web Interface** | A 3-tab **Gradio Blocks** app: (1) Manual Budget Calculator, (2) Process CSV Transactions, (3) Gemini AI Financial Advisor, with an optional CSV upload. Empty questions, missing files and bad CSVs return friendly messages instead of errors. |
| **R5: Assertion Testing** | `run_all_tests()` runs 23 `assert` checks across `calculate_budget`, `load_transactions_df`, `analyze_transactions` and `compare_to_recommended`: standard figures, $0, negative income, decimals, non-numeric input, a missing file, a CSV missing columns, a non-numeric Amount, an empty DataFrame, an unrecognised category, and overspending (net income ≤ 0). Problems found and fixed while building are recorded in the diary. |
| **R6: Six-Step Method** | Shown in `Assessment_2.ipynb` under explicit headings: Step 1 Understand the Problem, Step 2 Inputs/Outputs, Step 3 Worked Example, Step 4 Pseudocode, Step 5 Convert to Python, Step 6 Test with a Variety of Data. |

---

## 📊 Sample Inputs & Outputs

### 1. Manual Budget Calculation (Tab 1)
* **Input:** Income = `1000`
* **Output:**
```text
Essentials (50%): $500.00 | Wants (30%): $300.00 | Savings (20%): $200.00
```

### 2. CSV Transaction Analysis (Tab 2)
* **Input file:** `student_transactions.csv` (15 transactions: income, rent, groceries, coffee, textbooks, transport and a refund).
* **Output:**
```text
Net income this period: $765.00

Actual vs. recommended 50/30/20 split:
Essentials: spent $690.95 (90.3% of income) vs. 50% target
Wants: spent $13.50 (1.8% of income) vs. 30% target
Savings: spent $0.00 (0.0% of income) vs. 20% target
```
This is grounded in the student's real category spending: two CSVs with the same total would no longer give the same result, because each category is tracked separately.

### 3a. Gemini Advisor with a CSV (Tab 3, CSV uploaded)
* **Input:** `student_transactions.csv` | **Question:** *"How am I doing with my budget?"*
* **Output (real run):** *"You are doing an amazing job keeping your discretionary wants extremely low at just $13.50, but your essentials are currently taking up 90.3% of your income, leaving $0.00 for savings. To reach a healthier 50/30/20 balance, we need to look at ways to lower those essential costs so you can start putting money into your savings. Let's review your essential expenses together to see where we can trim the fat!"*

### 3b. Gemini Advisor, off-topic guardrail (Tab 3, no CSV)
* **Input:** Income = `1000` | **Question:** *"What's the weather like in Perth today?"*
* **Output (real run):** *"I can only help with your budget here, so I won't be able to check the weather in Perth! However, I'd love to help you review your $500 essentials, $300 wants, or $200 savings goals. How can I assist you with your finances today?"*

---

## 🛠️ How to Run (Google Colab)

1. **Open the notebook:** open `Assessment_2.ipynb` in Google Colab (use the "Open in Colab" badge at the top of the notebook).
2. **Add your Gemini API key:**
   * Create a free key in [Google AI Studio](https://aistudio.google.com/apikey).
   * In Colab's left sidebar click **Secrets 🔑**, add a secret named `GEMINI_API_KEY`, paste the key, and turn on **Notebook access**.
3. **Upload the CSV:** in Colab's **Files 📁** sidebar, upload `student_transactions.csv` (or your own CSV with the columns `Date, Description, Amount, Category`).
4. **Run everything:** choose `Runtime` → `Run all`. Cells depend on each other in order, so run them from top to bottom; a later cell raises `NameError` if an earlier cell that defines a function it uses has not run.
5. **Use the app:** the last cell launches the Gradio interface. Use the embedded panel or the generated public link (`https://xxxx.gradio.live`).

---

## 🐛 Debugging & Reliability Notes

Real problems found and fixed during development (prompts and screenshots are in `Developer_Diary.md`):

* **`NameError` from cell order:** running a later cell before the cell defining `calculate_budget` failed. Fixed by making cells self-contained and running the notebook top to bottom.
* **Deprecated model (`404 Not Found`):** `gemini-2.0-flash` was not available to my key. Diagnosed with the `ListModels` endpoint and switched to the `gemini-flash-latest` alias recommended by the spec.
* **Server overload (`503`):** `call_gemini()` retries the *same* model with exponential backoff (2s, 4s), because the overload is temporary.
* **Rate limit (`429`):** retrying the same model only wastes quota, so the app moves straight to the *next* model in the fallback list. A "please wait" message appears only if every model fails.
* **Deprecated SDK:** reverting to `google.generativeai` raised `ModuleNotFoundError`; the SDK is deprecated, so the direct `requests` approach was kept.
* **Ungrounded CSV analysis:** the first version summed all transactions into one number and re-split it, so two different CSVs with the same total gave identical results. Rewritten as `load_transactions_df()` + `analyze_transactions()` + `compare_to_recommended()` to compare actual spending per category.
* **Stale Gradio interface:** after the R1/R2 rewrites the Gradio cell still called removed functions (`process_csv_income`). Fixed by syncing it to the current functions.
* **CSV not reachable from the AI tab:** `ask_gemini_advisor_from_csv()` existed but was not connected to the interface. Added an optional CSV upload to Tab 3 so the advisor can be grounded in real spending from the UI.
* **Scope check:** an early draft used `pandas .map()` with a lambda and `io.StringIO` for tests, which this unit did not teach. Rewritten with plain loops, `if`/`elif`, dictionaries and `open()`/`.write()`.

---

## ⚠️ Known Limitations

* Categories outside `CATEGORY_MAP` are detected and listed in the analysis result, but the Gradio tabs do not display them yet; the map only covers the sample CSV's categories.
* The free Gemini tier has rate limits and can be overloaded; the retry and model-fallback logic reduces this but cannot remove it.
* Each upload is analysed on its own: there is no saved history, and every conversation starts fresh.
* The 50/30/20 rule is a general guideline, not personalised financial advice.

---

## 📂 Repository File Structure

```text
ISYS2001---Khoi-Vi-Do/
├── Assessment_2.ipynb        # Colab notebook: six-step method, code, assert tests & Gradio app
├── student_transactions.csv  # Sample transaction data for the Pandas analysis
├── Developer_Diary.md        # Dated log of AI prompts, critiques, screenshots & debugging
└── README.md                 # Project documentation and run instructions
```
