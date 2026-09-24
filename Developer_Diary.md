# Developer's Diary - ISYS2001 Assessment 2

## Week 1: Problem Definition & Initial Setup

* **Date:** 2026-09-22
* **Goal:** Define the finance project concept and complete the first 4 steps of the Six-Step Problem Solving Method.

### AI Interaction #1
* **My Request:** "I am working on ISYS2001 Assessment 2. Help me design a simple personal finance assistant for university students. Draft the first 4 steps of the Six-Step Problem Solving Method (Problem, Inputs/Outputs, Worked Example, Pseudocode) in English using the 50/30/20 budget rule."
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
