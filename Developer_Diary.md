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

---
