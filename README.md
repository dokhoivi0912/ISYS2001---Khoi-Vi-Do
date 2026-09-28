# 🎓 Student Budget Coach & Personal Finance Assistant

**ISYS2001 Assessment 2: Programming Project**  
* **Student Name:** Khoi Vi Do  
* **Student ID:** *23391554*  
* **Course:** ISYS2001 - Introduction to Business Programming (Curtin University)  
* **Instructor / Collaborator:** Kevin Blasiak (`kevin-blasiak-curtin`)[cite: 7]  
* **Repository Link:** [https://github.com/dokhoivi0912/ISYS2001---Khoi-Vi-Do](https://github.com/dokhoivi0912/ISYS2001---Khoi-Vi-Do)  

---

## 📌 Project Overview & Problem Statement

### The Problem
University students often struggle with managing limited monthly allowances, part-time wages, and daily living expenses[cite: 7]. Without a structured budget, students frequently overspend on discretionary items ("Wants") before covering essential needs ("Essentials") or setting aside long-term savings[cite: 7].

### The Solution
The **Student Budget Coach** is a user-friendly, interactive Python web application built using **Google Colab** and **Gradio**[cite: 7, 8]. It automates the widely recommended **50/30/20 financial rule** (allocating 50% of income to Essentials, 30% to Wants, and 20% to Savings)[cite: 8]. The application accepts income figures either via **manual numerical entry** or by analyzing bulk transaction logs from a **CSV file** using **Pandas**, and delivers personalized advice powered by **Google Gemini AI**[cite: 7, 8].

---

## ✨ Requirements Alignment (R1 – R6)

This project strictly adheres to all six requirements outlined in the ISYS2001 Assessment 2 Specification[cite: 7]:

| Requirement | Implementation Detail | Status |
| :--- | :--- | :---: |
| **R1: Conversational AI Assistant** | Integrated `gemini-flash-latest` via Google Gemini API (`google.generativeai`) using system instructions to enforce an empathetic, practical "Student Finance Advisor" persona[cite: 7, 8]. | ✅ Pass |
| **R2: Grounded in User Data** | Uses **Pandas** (`pd.read_csv`) to process transaction data (`student_transactions.csv`), ensuring Gemini AI advice is dynamically grounded in calculated net income ($60.55) rather than generic responses[cite: 7, 8]. | ✅ Pass |
| **R3: Custom Analytical Tool** | Implemented `calculate_budget(income)` to compute exact 50/30/20 dollar breakdowns and handle invalid inputs (zero/negative amounts) safely[cite: 7, 8]. | ✅ Pass |
| **R4: Interactive Web Interface** | Designed a 3-tab user interface using **Gradio Blocks** (`gr.Blocks`) separating Manual Calculation, CSV File Processing, and Gemini AI Consulting[cite: 7, 8]. | ✅ Pass |
| **R5: Robust Assertion Testing** | Verified core business logic with automated `assert` statements testing standard figures ($1000), boundary values ($0), negative edge cases (-$50), and floating decimals ($1500.50)[cite: 7, 8]. | ✅ Pass |
| **R6: Six-Step Problem-Solving Method** | Documented fully in `Assessment_2.ipynb`, detailing Problem Definition, Inputs/Outputs, Hand Calculation, Pseudocode, Python Conversion, and Edge-Case Testing[cite: 7, 8]. | ✅ Pass |

---

## 📊 Sample Inputs & Outputs

### 1. Manual Budget Calculation (Tab 1)
* **Input:** Income = `$1000.00`
* **Output:** `Essentials (50%): $500.00 | Wants (30%): $300.00 | Savings (20%): $200.00`[cite: 8]

### 2. CSV Transaction Analysis (Tab 2)
* **Input File:** `student_transactions.csv` (15 transactions including allowance, part-time wages, rent, groceries, and coffee)[cite: 8].
* **Pandas Analysis Output:**
  * Processed Net Income: `$60.55`[cite: 8]
  * Computed Allocations: `Essentials (50%): $30.28 | Wants (30%): $18.16 | Savings (20%): $12.11`[cite: 8]

### 3. Gemini AI Financial Consultation (Tab 3)
* **Input:** Income = `$60.55` | **User Question:** *"How much can I spend on wants based on this budget?"*[cite: 8]
* **Grounded AI Output:** *"Based on your current budget, you have $18.16 allocated for wants this month. While that might feel like a modest amount, setting aside this 30% ensures you can still enjoy a small treat—like a coffee or study snack—without overspending!"*[cite: 8]

---

## 🛠️ Execution Guide for Google Colab

Follow these steps to run the application directly inside Google Colab[cite: 7, 8]:

1. **Open the Notebook:** Open `Assessment_2.ipynb` in Google Colab[cite: 7, 8].
2. **Setup Gemini API Key:**
   * Acquire a free API Key from [Google AI Studio](https://aistudio.google.com)[cite: 7, 8].
   * On Colab's left sidebar, click the **Secrets 🔑** icon[cite: 8].
   * Add a new secret named `GEMINI_API_KEY` and paste your key into the value field[cite: 7, 8].
   * Enable the **Notebook Access** toggle[cite: 7, 8].
3. **Upload the CSV File:**
   * On Colab's left sidebar, click the **Files 📁** icon[cite: 8].
   * Upload `student_transactions.csv` to the session storage[cite: 7, 8].
4. **Launch the Application:**
   * Execute all code cells sequentially, or run the consolidated **Gradio Interface Cell** at the bottom[cite: 8].
   * Interact with the application using the embedded Gradio panel or via the generated public URL (`https://xxxx.gradio.live`)[cite: 7, 8].

---

## 🐛 Debugging & Reliability Notes

During development, two key runtime issues were encountered and resolved as documented in `Developer_Diary.md`[cite: 8]:
* **Runtime Dependency Issue (`NameError`):** Resolved by consolidating `calculate_budget()`, Pandas CSV reading, and Gemini API calls into a self-contained execution cell, eliminating Colab state volatility[cite: 8].
* **File Access Error (`FileNotFoundError`):** Handled gracefully with `try/except` blocks in `process_csv_income()`, ensuring clear user feedback if `student_transactions.csv` is missing from the active Colab runtime storage[cite: 8].

---

## 📂 Repository File Structure

```text
ISYS2001---Khoi-Vi-Do/
├── Assessment_2.ipynb        # Main Colab notebook (Code, 6-step method, assert tests & Gradio app)
├── student_transactions.csv  # Sample student financial transaction dataset for Pandas processing
├── Developer_Diary.md        # Weekly development log tracking AI prompts, critiques, screenshots & debugs
└── README.md                 # Executive project documentation & operational instructions
