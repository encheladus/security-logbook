# Python: Simple Demo — TryHackMe (Programming Basics)

**Date:** 2026-02-28  
**Type:** TryHackMe  
**Scope:** Lab / Authorized only  

---

## 1) Context

This room introduces the fundamentals of Python through a simple “Guess the Number” game.  
The objective is to understand core programming concepts such as variables, user input, conditional logic, and loops.

---

## 2) Initial hypothesis

Before starting, I expected to:

- Learn how Python handles variables and memory.
- Understand how conditional statements control program logic.
- See how loops allow repetition until a condition is met.
- Discover how randomness can make a program dynamic.

---

## 3) Tools used

- Python 3 interpreter  
- Basic text editor  

---

## 4) Approach (high level)

The program was built progressively, following structured development steps:

- Initialize variables and generate a random secret number.
- Collect user input and convert it to the correct data type.
- Introduce conditional statements to compare values.
- Implement a loop to allow repeated attempts until success.

The logic flow follows a simple structure:
1. Setup (initialization).
2. Input handling.
3. Decision-making.
4. Iteration until the winning condition is met.

---

## 5) Results / Evidence (sanitized)

The final program successfully:

- Generates a random number between 1 and 20.
- Prompts the user for guesses.
- Validates range boundaries.
- Provides feedback (“Too low”, “Too high”, “Out of range”).
- Tracks the total number of attempts.
- Stops only when the correct number is guessed.

Final state example:
- Multiple incorrect attempts allowed.
- Correct guess ends the loop.
- Displays total attempts without exposing internal state beyond intended output.

---

## 6) Recommended remediation

Although this is an educational program, good development practices include:

- Adding input validation to prevent crashes (e.g., handling non-numeric input).
- Using exception handling (`try/except`) for robustness.
- Structuring logic into reusable functions.
- Clearly separating initialization, logic, and output layers.

---

## 7) Lessons learned

- Variables store and manage state during execution.
- `input()` always returns a string and requires type conversion for numeric comparisons.
- The `random` module introduces unpredictability into programs.
- Conditional statements (`if / elif / else`) control execution flow.
- Comparison operators (`<`, `>`, `!=`, `==`) define logical relationships.
- A `while` loop repeats execution until a condition becomes false.
- Building programs incrementally improves clarity and debugging efficiency.

---

## 8) Links / Resources

- Room: TryHackMe – Programming Basics  
- Official documentation: https://docs.python.org/3/  
- Personal notes: Notion (detailed write-up)

---