# Developer's Diary - ISYS2001 Assessment 2

## Week 1: Problem Definition & Initial Setup

* **Date:** 2026-09-22
* **Goal:** Define the finance project concept and complete the first 4 steps of the Six-Step Problem Solving Method.

### AI Interaction #1
* **My Request:** "I am working on ISYS2001 Assessment 2. Help me design a simple personal finance assistant for university students. Draft the first 4 steps of the Six-Step Problem Solving Method (Problem, Inputs/Outputs, Worked Example, Pseudocode) using the 50/30/20 budget rule."
* **AI's Response:** Suggested the "Student Budget Coach" application and generated Step 1 to Step 4 incorporating logic for edge cases (negative/zero income).
* **My Critique & Decision:** I accepted the 50/30/20 budget calculation rule because it uses fundamental concepts from Modules 2–5 (variables, conditionals, and formatting). I directed the AI to include strict error checking (`income <= 0`) in the pseudocode to handle invalid input properly.
* **Screenshot:**


---
<img width="804" height="716" alt="image" src="https://github.com/user-attachments/assets/8c4f755e-c008-4627-9c67-e95d752ab85c" />

## Week 1: Custom Tool Implementation & Testing

* **Date:** 2026-09-22
* **Goal:** Implement the Python calculation function `calculate_budget` and verify its behavior using assertion test cases.

### AI Interaction #2: Custom Tool & Assert Testing
* **My Request:** "I have defined my pseudocode for the 50/30/20 budget calculator. Now write the Python function calculate_budget(income) based on Module 5. Include edge-case checks for zero or negative values using if/else, format output with f-strings, and write assert statements to test standard, zero, negative, and decimal inputs."
* **AI's Response:** Generated the Python function `calculate_budget` and four `assert` test cases covering positive values, zero, negative numbers, and float decimals.
* **My Critique & Decision:** The solution is clear and simple. I verified that the code uses standard Python formatting (`.2f`) learned in Module 2 and basic conditionals (`if/else`) from Module 3. All four `assert` statements passed successfully, confirming robust handling of bad user input (Requirement R3 & R5).
* **Screenshot:**

<img width="700" height="845" alt="image" src="https://github.com/user-attachments/assets/ee146711-3358-433e-bbf8-9307b80ee78c" />

* Note: While testing, I fixed an AssertionError in Test Case 4 caused by a typo in the expected string ($301.10 instead of $300.10 for 20% of $1500.50). Correcting this allowed all tests to pass successfully.
<img width="1209" height="936" alt="image" src="https://github.com/user-attachments/assets/d92f4ff3-1ce8-4745-8a5d-e36c1f0975b4" />

---

### AI Interaction #1 (Update)
* **Date:** 2026-09-24
* **My Request:** "I want to expand my project design in Step 2 to support processing an uploaded CSV transaction file via Pandas (Module 7) alongside direct income input."
* **AI's Response:** Updated Step 1 to Step 3 of the design to explicitly include optional CSV file inputs for transaction history analysis.
* **My Critique & Decision:** I decided to adopt this hybrid input model (direct `income` or CSV file) because it strengthens compliance with Requirement R2 (Grounding AI responses in user transaction data) while maintaining full compatibility with my existing 50/30/20 calculation logic.

<img width="555" height="702" alt="image" src="https://github.com/user-attachments/assets/d0a3198e-8c3d-439e-8f66-00541a00d2e4" />


---
## Week 1: Gemini API Integration & Grounding (R1 & R2)

* **Date:** 2026-09-24
* **Goal:** Connect Google Gemini API with system instructions to provide grounded financial advice based on user data and Pandas CSV input.

### AI Interaction #3: Gemini API & Persona Setup
* **Date:** 2026-09-24
* **My Request:** "Help me integrate Google Gemini API (gemini-flash-latest) into Python based on Module 8. Define a system instruction for a Student Finance Advisor persona (R1). Create a function ask_gemini_advisor that takes income (calculated from manual input or Pandas CSV processing in Module 7) and user_question, calls calculate_budget(income) to ground the AI response in user data (R2), and handles missing API keys securely using Colab Secrets."
* **AI's Response:** Provided Python code initializing `gemini-flash-latest` with a custom finance advisor persona, securely retrieving credentials via `userdata.get()`, processing transaction CSV data via Pandas, and constructing a grounded prompt.
* **My Critique & Decision:** I accepted this structure because it enforces security best practices by keeping the API key out of code (Module 8). By passing the output of `calculate_budget()` (fed by either direct user input or Pandas CSV sums from `student_transactions.csv`) into the prompt, the model is strictly grounded in actual user figures rather than offering generic advice (Requirement R1 & R2).
* **Screenshot:**

<img width="368" height="786" alt="image" src="https://github.com/user-attachments/assets/ec551f06-91c6-4fae-9238-775b9e815ef2" />


---

### AI Interaction #3 (Update & Debugging)
* **Date:** 2026-09-24
* **Issue Encountered:** While running the Phase 3 code block for the Gemini API integration, Python threw a `NameError: name 'calculate_budget' is not defined`.
* **Root Cause Analysis:** Google Colab executes code in independent cells and volatile runtime memory. Because the Phase 3 cell was executed without re-running the Phase 2 cell in the active session, the runtime did not recognize the function `calculate_budget()`.
* **My Request:** "I got `NameError: name 'calculate_budget' is not defined` when running Phase 3. How can I fix this dependency error so Phase 3 runs independently?"
* **AI's Response:** Suggested combining the `calculate_budget()` custom tool definition directly inside the Phase 3 code block alongside Pandas CSV processing (`process_csv_income`) and the Gemini API call (`ask_gemini_advisor`).
* **My Critique & Decision:** I adopted this fix because consolidating the core calculation logic into the same cell ensures self-contained execution, preventing runtime memory errors when testing the AI advisor functionality. After updating the cell, all components executed seamlessly, successfully grounding Gemini's response in the processed CSV budget figure ($60.55).
* **Screenshot:**

<img width="1908" height="979" alt="image" src="https://github.com/user-attachments/assets/ba3b3346-eb4f-4b34-ace8-55acb1533878" />

<img width="1802" height="682" alt="image" src="https://github.com/user-attachments/assets/01f5d1c2-b972-40cc-83bd-f676557a2f82" />


---

## Week 1: Interactive Interface Development with Gradio (R4)

* **Date:** 2026-09-26
* **Goal:** Design and launch an interactive web UI using Gradio to integrate the budget calculator tool, CSV transaction processing, and the Gemini AI advisor.

### AI Interaction #4: Gradio Interface Integration
* **Date:** 2026-09-26
* **My Request:** "Help me build a user-friendly web interface using Gradio based on Module 9 for my Student Budget Coach application (Requirement R4). Create a tabbed interface (gr.Blocks) containing three tabs: 1) Manual 50/30/20 Budget Calculator, 2) CSV Transaction Processing via Pandas, and 3) Gemini AI Advisor Chatbot interface."
* **AI's Response:** Provided Python code leveraging `gradio.Blocks` to build a clean 3-tab user interface connecting `calculate_budget()`, `process_csv_income()`, and `ask_gemini_advisor()`.
* **My Critique & Decision:** I accepted the tabbed interface design because it separates input modes logically for non-technical users while fulfilling Requirement R4. Users can easily toggle between entering raw income amounts, uploading a CSV file of transaction records, or asking questions directly to the Gemini AI advisor.
* **Screenshot:**

<img width="260" height="944" alt="image" src="https://github.com/user-attachments/assets/c1415378-56e9-47fa-96f8-5c202363862c" />

---

* **Function 1:** Users enter raw income amounts

<img width="1895" height="935" alt="image" src="https://github.com/user-attachments/assets/83738b38-a9de-48a1-9717-fb9855fe9444" />

---

* **Function 2:** Users upload their own CSV file of transaction records

<img width="1914" height="941" alt="image" src="https://github.com/user-attachments/assets/f64477c5-4653-418f-8667-fb5cb1354764" />

---

* **Function 3:** Users ask questions directly to the Gemini AI for financial advice

<img width="1914" height="940" alt="image" src="https://github.com/user-attachments/assets/456944e4-1638-4968-88f7-f2ec012de464" />

## Week 2: Requirement Fixes Based on Self-Review

### AI Interaction #5: Replacing the SDK with a direct `requests.post()` call (R1)

**Date:** 2026-10-06

**My Prompt:** "My Gemini integration currently uses the `google.generativeai` SDK, but the unit's tool list specifically names `requests` for any web API call, and R1 says I should be writing the code that calls the model myself. Rewrite `ask_gemini_advisor` to call the Gemini REST endpoint directly with `requests.post()`, and add a guardrail to the system instruction so the assistant handles off-topic or unclear questions sensibly instead of just answering anything."

**AI's Response:** Replaced `genai.GenerativeModel(...)` / `model.generate_content(...)` with a `call_gemini()` function that builds the JSON payload itself (`systemInstruction` + `contents`) and posts it to `https://generativelanguage.googleapis.com/v1beta/models/{GEMINI_MODEL}:generateContent`, wrapped in `try/except` for network errors and malformed responses. Added two sentences to `SYSTEM_INSTRUCTION` telling the model to decline off-topic questions politely and ask a clarifying question when a question is unclear.

**My Critique & Decision:** Accepted. Every line of the API call is now something I wrote and can explain, instead of SDK internals I'd have to guess at in Assessment 3.

**Screenshot:**

<img width="1463" height="611" alt="image" src="https://github.com/user-attachments/assets/615787ad-56ef-47c6-97fd-9d35a246666f" />




---

### AI Interaction #7: Grounding the budget analysis in actual category spending, not a single re-split total (R2)

**Date:** 2026-10-06

**My Prompt:** "My process_csv_income function just sums every Amount in the CSV into one net_income number, then re-runs the same 50/30/20 split on it. Two CSVs with the same total would produce an identical result - that doesn't actually ground the analysis in my real spending. How do I fix this to compare actual category spending against the 50/30/20 targets?"

**AI's Response:** Pointed out that summing every row (income minus expenses) and treating that as "income" for a 50/30/20 split applies the rule to leftover cash rather than gross income, which isn't how the rule is meant to work. Proposed splitting the logic into `load_transactions_df` (file I/O) and `analyze_transactions` (pure logic), with a `CATEGORY_MAP` dictionary bucketing each transaction's Category into Essentials/Wants/Savings/Income, so the app reports actual dollars and percentage spent per bucket against the 50/30/20 target.

**My Critique & Decision:** Accepted the overall approach. Running it against `student_transactions.csv` shows the student spent about 90% of income on Essentials and 0% on Savings - a result a flat re-split total could never surface. I also had the AI change how a zero/negative net income is handled: instead of blocking it as an "invalid input" error like the manual-entry `calculate_budget` path correctly does, it's now an overspending *warning*, since spending more than you earned is a real, useful thing to flag, not bad input to reject.

### AI Interaction #8: Keeping the analysis code within the unit's scope (ground rule)
**Screenshot:**

<img width="1777" height="581" alt="image" src="https://github.com/user-attachments/assets/9d61c5ac-79d3-40c5-9d67-7bd53d70894f" />

### AI Interaction #9: Testing the Pandas/CSV logic, not just calculate_budget (R5)

**Date:** 2026-10-06

**My Prompt:** "My only tests so far are on calculate_budget. The CSV-processing functions (load_transactions_df, analyze_transactions, compare_to_recommended) - the actual R2 logic - have zero tests. Add proper assert-based tests for them, covering normal cases and edge/invalid-input cases."

**AI's Response:** Added tests for: a missing CSV file, a CSV missing a required column (built in-memory with io.StringIO instead of a second file on disk), a CSV with a non-numeric Amount value (confirming the bad row is dropped and reported, not crashing the load), a small hand-checkable DataFrame to confirm analyze_transactions' bucket totals are correct, an empty DataFrame, a transaction with a category not in CATEGORY_MAP (confirmed it's reported as "uncategorised" rather than silently dropped), the normal case for compare_to_recommended, and both the negative and exactly-zero net income boundary cases.

**My Critique & Decision:** Kept all of them. Because load_transactions_df and analyze_transactions are split into an I/O layer and a pure logic layer (see Interaction #7), I could test the real decision logic directly with small hand-built DataFrames instead of only testing against the one sample CSV file. Running the full suite (18 assertions total, across all four functions) passes cleanly - this is real evidence of edge-case coverage for R5, not just a repeat of the same happy-path check four times.

**Screenshot:** 

<img width="1444" height="705" alt="image" src="https://github.com/user-attachments/assets/8c4aa8e7-4107-4d35-b8e2-ce48c367eefe" />

