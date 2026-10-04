# My Answers - Assignment 1 (CFLP: VNS vs Gurobi)

**Student Information:**
- **Student ID:** ___________________________
- **Student Name:** ___________________________
- **Gurobi Version Used:** [ ] Academic [ ] Free
- **Date Completed:** ___________________________

---

## Section A: Solution Quality Analysis (4 marks)

### A.1 (2 marks) - Gap Analysis by Categories

**Analysis of Plot 01:**

[Your answer here - analyze gap patterns by size and difficulty]

**Specific Observations:**
- Highest median gap size: ___________
- Median gap value: ___________%
- Effect of difficulty on gap: ___________

**Outliers identified:**

[Explain outliers and their possible causes]

**Interpretation:**

[Explain WHY these patterns occur]

---

### A.2 (2 marks) - Overall Solution Quality Statistics

**From Excel File (Sheet "All Results"):**

- Total instances analyzed: ___________
- Instances with gap < 5%: ___________ ( _____% )
- Instances with gap < 10%: ___________ ( _____% )
- Mean gap across all instances: ___________%
- Standard deviation of gap: ___________%

**Interpretation:**

[Analyze what these statistics tell you about VNS consistency and quality]

[Is VNS providing good solutions consistently? Explain]

---

## Section B: Computational Efficiency Analysis (4 marks)

### B.1 (2 marks) - Time Comparison

**Analysis of Plot 02:**

**Size at which VNS becomes significantly faster:**

[Your answer with specific size category and time values]

**Most challenging configurations:**
- For VNS: ___________ (size-difficulty) with time: _____ seconds
- For Gurobi: ___________ (size-difficulty) with time: _____ seconds

**Detailed Comparison:**

[Explain the time differences and their implications]

---

### B.2 (2 marks) - Speedup Analysis

**Analysis of Plot 03:**

**Average speedup factors:**
- Small instances: ______x faster
- Medium instances: ______x faster
- Large instances: ______x faster

**Instances where Gurobi is faster (speedup < 1):**

[List if any exist, describe their characteristics]

**Practical Implications:**

[Discuss what these speedup factors mean for real applications]

---

## Section C: Scalability Analysis (3 marks)

### C.1 (1.5 marks) - Scalability Behavior

**Analysis of Plot 04:**

**VNS Scalability:**

[Describe how VNS time grows with problem size]

**Gurobi Scalability:**

[Describe how Gurobi time grows with problem size]

**Comparison:**

[Which scales better and why?]

---

### C.2 (1.5 marks) - Growth Rate and Predictions

**Observed Growth of Average Runtime (from your results):**

| | Small | Medium | Large | Small -> Medium (x) | Medium -> Large (x) |
|---|---|---|---|---|---|
| VNS avg time (s) | | | | | |
| Gurobi avg time (s) | | | | | |

**Shape of growth in Plot 04:**
- VNS: [ ] Roughly steady [ ] Steadily increasing [ ] Sharply increasing
- Gurobi: [ ] Roughly steady [ ] Steadily increasing [ ] Sharply increasing

**Justification:**

[Explain your choice using the growth ratios above]

**Rough Prediction for Extra Large Instance (n=50, m=20):**
- Predicted VNS time: ________ seconds
- Predicted Gurobi time: ________ seconds
- How I computed it (e.g., applying the Medium -> Large ratio again): ___________

**Suitability for Very Large Instances:**

[Would you recommend VNS for very large problems? Why?]

---

## Section D: Quality-Time Tradeoff Analysis (3 marks)

### D.1 (1.5 marks) - Sweet Spot Identification

**Analysis of Plot 05:**

**Sweet Spot:**

[Describe the optimal balance point between quality and time]

**Time Savings for High-Quality Solutions (gap < 5%):**
- Average VNS time: ________ seconds
- Average Gurobi time: ________ seconds
- Absolute savings: ________ seconds
- Relative savings: ________%

**Interpretation:**

[What does this mean for practitioners?]

---

### D.2 (1.5 marks) - Practical Decision Making

**Scenarios Justifying 10-15% Gap:**

1. [Scenario 1 with justification]

2. [Scenario 2 with justification]

3. [Scenario 3 with justification]

**Recommendation for Real-World Logistics Company:**

[VNS or Gurobi? Explain your recommendation considering various factors]

---

## Section E: Algorithm Understanding (3 marks)

### E.1 (1.5 marks) - VNS Neighborhood Structures

**Role of Each Neighborhood:**

**N1 (Open Facility):**
- Purpose: ___________
- Contribution: [ ] Intensification [ ] Diversification
- Explanation: ___________

**N2 (Close Facility):**
- Purpose: ___________
- Contribution: [ ] Intensification [ ] Diversification
- Explanation: ___________

**N3 (Swap Facilities):**
- Purpose: ___________
- Contribution: [ ] Intensification [ ] Diversification
- Explanation: ___________

**N4 (Reassign Clients):**
- Purpose: ___________
- Contribution: [ ] Intensification [ ] Diversification
- Explanation: ___________

**How They Complement Each Other:**

[Explain the synergy between neighborhoods]

---

### E.2 (1.5 marks) - Convergence Behavior Analysis

**Selected Convergence Plots:**

**Plot 1:** [Instance name: ___________]
- Pattern: [ ] Smooth [ ] Multiple jumps
- Description: ___________
- What it reveals: ___________

**Plot 2:** [Instance name: ___________]
- Pattern: [ ] Smooth [ ] Multiple jumps
- Description: ___________
- What it reveals: ___________

**Plot 3:** [Instance name: ___________]
- Pattern: [ ] Smooth [ ] Multiple jumps
- Description: ___________
- What it reveals: ___________

**Role of Shaking Mechanism:**

[Explain how shaking helps VNS escape local optima with evidence from plots]

---

## Section F: Critical Reflection (3 marks)

### F.1 (1 mark) - Limitations and Improvements

**Main Limitations of Current VNS Implementation:**

1. [Limitation 1]

2. [Limitation 2]

3. [Limitation 3]

**Suggested Improvements:**

1. [Improvement 1 with explanation of expected benefit]

2. [Improvement 2 with explanation of expected benefit]

3. [Improvement 3 with explanation of expected benefit]

---

### F.2 (1 mark) - Real-World Deployment Considerations

**Additional Constraints for Real Logistics Company:**

1. [Constraint 1 and why it matters]

2. [Constraint 2 and why it matters]

3. [Constraint 3 and why it matters]

**Additional Objectives:**

[Discuss multi-objective considerations beyond cost minimization]

---

### F.3 (1 mark) - Exact Method vs Heuristic in Practice

**Gurobi Optimality Status (from the `gurobi_status` column):**
- Instances solved to proven optimality: ______ out of ______
- Instances stopped at the time limit: ______ out of ______
- Do the time-limit instances share a size/difficulty? ___________

**What VNS Gives Up and What It Gains:**

[Use your numbers: average gap vs. average time saved]

**Practical Value of Heuristics:**

[Discuss the industrial importance of VNS-like algorithms, based on what you observed]

---

## Section G: BONUS - VNS Improvement Challenge (+3 marks, optional)

*Leave this section empty if you do not attempt the bonus. See Section 8 of the guide. Your improved notebook (`StudentID_FirstName_LastName_Assignment1_IMPROVED.ipynb`) must be pushed to your repository.*

### G.1 (1 mark) - Bug Identification

**Where is the bug?** (cell / function / line numbers in `Assignment_1.ipynb`):

[Paste the faulty line(s) of code]

**What does the bug do?**

[Describe the specific issue]

**Why does it degrade VNS performance?**

[Explain the effect on the search and link it to the high gaps (>20%) in your results]

---

### G.2 (1 mark) - The Fix

**Corrected code:**

```python
# paste your fixed line(s) here
```

**Why does the fix work and why does it not break the rest of the algorithm?**

[Explanation]

---

### G.3 (1 mark) - Demonstrated Improvement

**Before vs After (same instances):**

| Metric | Original VNS | Improved VNS |
|---|---|---|
| Mean gap (%) | | |
| Std of gap (%) | | |
| Instances with gap < 5% | | |
| Instances with gap > 20% | | |
| Mean VNS time (s) | | |

**Analysis:**

[Discuss the improvement, and any change in running time]

**Improved results files (names in your repository):** ___________

---

## Additional Comments (Optional)

[Any additional observations, challenges faced, or insights gained]

---

## Declaration

I hereby declare that:
- I have completed this assignment independently
- All numerical results are from MY experiments using MY student ID
- All analysis and interpretations are MY own work
- I have not copied results or answers from other students
- I understand the consequences of academic dishonesty

**Signature:** _______________________

**Date:** ___________________________

---

**END OF MY ANSWERS**
