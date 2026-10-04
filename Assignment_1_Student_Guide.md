# Assignment: Capacitated Facility Location Problem - Student Guide

**Course:** CS616 - Optimization Algorithms  
**Semester:** 1, AY 2026-2027  
**Due Date:** November 07, 2026, 11:59 PM  
**Instructors:** Dr. Mahdi Khemakhem & Dr. Essra Aldessouki  
**Total Marks:** 20 + 3 (Bonus)

---

## 📋 Table of Contents

1. [Assignment Overview](#1-assignment-overview)
2. [Learning Objectives](#2-learning-objectives)
3. [Problem Description](#3-problem-description)
4. [Implementation Guide](#4-implementation-guide)
5. [How to Run the Notebook](#5-how-to-run-the-notebook)
6. [Output Files Structure](#6-output-files-structure)
7. [Analysis Questions](#7-analysis-questions)
8. [Bonus Question: VNS Improvement Challenge](#8-bonus-question-vns-improvement-challenge-3-bonus-marks)
9. [Grading Rubric](#9-grading-rubric)
10. [Submission Guidelines](#10-submission-guidelines)
11. [Academic Integrity](#11-academic-integrity)
12. [Getting Help](#12-getting-help)
13. [Your Analysis and Answers](#13-your-analysis-and-answers)

---

## 1. Assignment Overview

In this assignment, you will:
1. **Implement** a Variable Neighborhood Search (VNS) metaheuristic for the Capacitated Facility Location Problem
2. **Compare** VNS with an exact solver (Gurobi) on 144 test instances
3. **Analyze** the performance in terms of solution quality, computational time, and scalability
4. **Document** your findings in `My_Answers.md` with detailed analysis

### What You Need to Do:

✅ **Update your student ID and Gurobi license** in the notebook  
✅ **Run ALL cells** in the notebook (from top to bottom)  
✅ **Analyze the generated results**  
✅ **Answer all questions** in `My_Answers.md` with specific references to your results and plots.
✅ **Submit** the link of your GitHub fork (renamed with your ID and name) on Blackboard - see [Section 10](#10-submission-guidelines)

**IMPORTANT:** You do NOT need to implement anything! All code is provided. You only need to:
1. Update two variables (STUDENT_ID and GUROBI_VERSION) in the last cell
2. Run all cells in order
3. Analyze the results and answer the questions in `My_Answers.md`

**📊 REALISTIC BENCHMARK INSTANCES**
The instances use a **research-based ratio model** with α computed against **optimal service cost** to create realistic difficulty patterns:
- **Problem Sizes (Realistic for Educational Benchmarking):**
  - Small: 30-50 facilities, 150-300 clients (~4,500-15,000 variables)
  - Medium: 60-90 facilities, 400-700 clients (~24,000-63,000 variables)
  - Large: 100-150 facilities, 800-1500 clients (~80,000-225,000 variables)
- **Performance Expectations (approximate, depend on your hardware and ID):**
  - **Gurobi:** fractions of a second (small) up to several seconds (large)
  - **VNS:** usually faster than Gurobi, especially on medium and large instances
  - **Solution quality:** the provided VNS has a known weakness (see Section 8). Report what YOU observe - do not assume any gap values in advance.
- **Total experiment runtime: 10-20 minutes** (depending on your hardware)

---

## 2. Learning Objectives

After completing this assignment, you will be able to:

1. ✓ Formulate the Capacitated Facility Location Problem as a Mixed-Integer Programming model
2. ✓ Implement a metaheuristic algorithm (VNS) with multiple neighborhood structures
3. ✓ Use professional optimization software (Gurobi) for exact optimization
4. ✓ Conduct systematic computational experiments
5. ✓ Analyze algorithmic performance using statistical methods
6. ✓ Interpret optimization results and draw practical conclusions
7. ✓ Communicate technical findings effectively

---

## 3. Problem Description

### Capacitated Facility Location Problem (CFLP)

The Capacitated Facility Location Problem is a classic optimization problem where:
- A company must decide **which facilities to open** from a set of potential locations
- Each facility has a **fixed opening cost** and a **limited capacity**
- Each client has a **demand** that must be satisfied
- Each client must be **assigned to exactly one open facility**
- Each assignment has a **service cost**
- **Capacity constraints:** Total demand assigned to any facility cannot exceed its capacity
- **Objective:** Minimize total cost (facility opening + client assignment costs)

### Real-World Applications:
- Warehouse location and distribution planning
- Data center placement
- Healthcare facility location (hospitals, clinics)
- Retail store placement
- Telecommunication tower placement
- Emergency service station location

### Mathematical Formulation:

**Sets:**
- $I = \{1, 2, ..., n\}$: Set of clients
- $J = \{1, 2, ..., m\}$: Set of potential facility locations

**Parameters:**
- $f_j$: Fixed cost of opening facility $j \in J$
- $c_{ij}$: Cost of assigning client $i \in I$ to facility $j \in J$
- $q_j$: Capacity of facility $j \in J$
- $d_i$: Demand of client $i \in I$

**Decision Variables:**
- $y_j \in \{0,1\}$: Binary variable, 1 if facility $j$ is opened, 0 otherwise
- $x_{ij} \in \{0,1\}$: Binary variable, 1 if client $i$ is assigned to facility $j$, 0 otherwise

**Objective Function:**
$$\min \sum_{j \in J} f_j y_j + \sum_{i \in I} \sum_{j \in J} c_{ij} x_{ij}$$

**Subject to:**
1. Each client assigned to exactly one facility:
   $$\sum_{j \in J} x_{ij} = 1, \quad \forall i \in I$$

2. Clients only assigned to open facilities:
   $$x_{ij} \leq y_j, \quad \forall i \in I, \forall j \in J$$

3. Facility capacity constraints:
   $$\sum_{i \in I} d_i x_{ij} \leq q_j y_j, \quad \forall j \in J$$

### Problem Complexity:
- **NP-Hard** problem
- Exact methods (like Gurobi) can solve small-medium instances optimally
- Heuristics (like VNS) provide good solutions for large instances quickly
- Trade-off between **solution quality** and **computational time**

---

## 4. Implementation Guide

### ⚠️ IMPORTANT: NO CODING REQUIRED!

**All code is already implemented!** You do NOT need to write any code.

**Your only tasks are:**
1. Update **TWO variables** (STUDENT_ID and GUROBI_VERSION) in the last code cell
2. Run ALL cells in the notebook (from top to bottom)
3. Analyze the results and answer the questions in `My_Answers.md`

### Structure of the Notebook

The notebook `Assignment_1.ipynb` is organized into 9 numbered sections plus a final student-configuration cell:

#### **Section 1: Problem Description** ✅ PROVIDED
- Mathematical formulation
- Problem statement
- **Action: READ carefully**

#### **Section 2: Setup and Dependencies** ✅ PROVIDED
- Auto-installs required packages
- Imports libraries
- **Action: RUN the cell**

#### **Section 3: Instance Generation** ✅ PROVIDED
- Function: `generate_instances()`
- Generates 144 problem instances
- Uses your student ID as random seed
- **Action: RUN the cell**

#### **Section 4: Utility Functions** ✅ PROVIDED
- Core helper functions for FLP
- Functions: `load_flp_instance()`, `compute_total_cost()`, `is_feasible()`, etc.
- **Action: RUN the cell**

#### **Section 5: VNS Neighborhood Structures** ✅ PROVIDED
- Four neighborhood operators (N1, N2, N3, N4)
- Functions: `n1_open_facility()`, `n2_close_open_facility()`, etc.
- **Action: RUN the cell**

#### **Section 6: VNS Algorithm Components** ✅ PROVIDED
- Functions: `shaking()`, `local_search()`
- **Action: RUN the cell**

#### **Section 7: Complete VNS Algorithm** ✅ PROVIDED
- Function: `vns_solve_flp()` - Main VNS metaheuristic
- **Action: RUN the cell**

#### **Section 8: Gurobi Exact Solver** ✅ PROVIDED
- Function: `gurobi_solve_flp()` - MIP formulation
- Supports both academic and free licenses
- **Action: RUN the cell**

#### **Section 9: Experiments and Visualization** ✅ PROVIDED
- Functions: `plot_convergence()`, `run_experiments()`, `visualize_results()`, etc.
- **Action: RUN the cell**

#### **Student Configuration (last cells)** ⚠️ YOUR TURN!
- Variables to update: `STUDENT_ID`, `GUROBI_VERSION`
- **Action: UPDATE these two variables, then RUN the cell**

### What You Need to Do:

**There is NO coding required!** All functions are already implemented. Your tasks are:

1. ✅ **Read and understand** the code and mathematical formulation
2. ✅ **Run all cells** in the notebook sequentially
3. ✅ **Execute the main function** with YOUR student ID
4. ✅ **Analyze the generated results**
5. ✅ **Answer all questions** in `My_Answers.md`

---

## 5. How to Run the Notebook

### Prerequisites:

1. **Python Environment:** Python 3.8+
2. **Required Libraries:**
   - numpy
   - pandas
   - matplotlib
   - seaborn
   - gurobipy
   - openpyxl
   
   *(Will auto-install when you run Section 2)*

3. **Gurobi License:**
   - **Academic License** (recommended): Free for students, unlimited
     - Get it here: https://www.gurobi.com/academia/
   - **Free License:** Limited to 2000 variables and 2000 constraints
     - Large instances will be skipped (this is acceptable)

### Step-by-Step Execution:

#### Step 1: Open the Notebook
```bash
jupyter notebook Assignment_1.ipynb
```
or open in VS Code

#### Step 2: Update Your Student Information

Scroll to the **LAST CODE CELL** (after all the implementation sections).

You will see:
```python
# ============================================================================
# STUDENT INFORMATION - UPDATE THESE TWO VARIABLES ONLY
# ============================================================================

# TODO: Replace 12345 with YOUR actual student ID
STUDENT_ID = 12345

# TODO: Set your Gurobi license type
GUROBI_VERSION = "academic"  # Options: "academic" or "free"
```

**Update these two variables:**
- **STUDENT_ID**: Replace `12345` with YOUR actual student ID (e.g., `461234123`)
- **GUROBI_VERSION**: Set to `"academic"` if you have academic license, or `"free"` for free license

**Example:**
```python
STUDENT_ID = 20231234
GUROBI_VERSION = "academic"  # I have an academic license
```

#### Step 3: Run All Cells
- Select "Run All" from the notebook menu
- **OR** run cells one by one from top to bottom (Shift+Enter)
- The last cell will execute `main()` with your student ID

#### Step 4: Wait for Completion
- **Estimated Time: 10-20 minutes** (depending on hardware and Gurobi license)
- **Typical Runtime Breakdown:**
  - Gurobi: 0.1-3s per instance (most complete in under 1 second)
  - VNS: 0.05-0.5s per instance (very fast)
  - 144 instances typically complete in 10-20 minutes on modern hardware
- Progress displayed in real-time with instance-by-instance results
- **⚠️ Note:** The provided VNS implementation has a known bug that limits its performance on many instances. See Section 8 (Bonus Question) for details
- **⚠️ Important Note About VNS Performance:**
  - The provided VNS implementation has a bug that causes poor performance on many instances
  - You may observe gaps greater than 20% on multiple instances
  - This is intentional for educational purposes - see Section 8 (Bonus Question)
  - Your analysis should identify and explain these performance issues

#### Step 5: Check Generated Files
After completion, verify all output files are created (see next section)

#### Step 6: Analyze Results
Open the generated plots and Excel files to analyze the results

#### Step 7: Answer Questions
Complete `My_Answers.md` with your analysis

---

## 6. Output Files Structure

After running the main function, you will have the following files:

### Folder Structure:

```
📁 Your_Working_Directory/
│
├── 📄 Assignment_1.ipynb                           # Your notebook
├── 📄 Assignment_1_Student_Guide.md                # Guide (do not edit)
├── 📄 My_Answers.md                                # Your answers (to complete)
│
├── 📁 cflp_instances_student_{YOUR_ID}/            # 144 JSON instance files
│   ├── 001_cflp_small_easy_fac17_cli55.json
│   ├── 002_cflp_small_easy_fac17_cli54.json
│   ├── ...
│   └── 144_cflp_large_expensive_fac150_cli1500.json
│
├── 📁 convergence_plots_student_{YOUR_ID}/         # Convergence plots
│   ├── 001_cflp_small_easy_fac17_cli55_convergence.png
│   ├── 002_cflp_small_easy_fac17_cli54_convergence.png
│   ├── ...
│   └── 144_cflp_large_expensive_convergence.png
│   (Note: Fewer plots if using free Gurobi)
│
├── 📁 analysis_plots_student_{YOUR_ID}/            # 6 comprehensive analysis plots
│   ├── 01_gap_analysis_by_categories.png
│   ├── 02_time_analysis_detailed.png
│   ├── 03_speedup_analysis_comprehensive.png
│   ├── 04_scalability_analysis.png
│   ├── 05_quality_time_tradeoff.png
│   └── 06_statistical_summary_dashboard.png
│
├── 📊 comparison_analysis_student_{YOUR_ID}.png    # Main 6-plot visualization
├── 📊 results_student_{YOUR_ID}.csv                # Raw results (CSV)
├── 📊 results_student_{YOUR_ID}.xlsx               # Detailed results (5 sheets)
└── 📄 report_summary_student_{YOUR_ID}.txt         # Statistical summary
```

### File Descriptions:

#### 1. Instance Files (`cflp_instances_student_{YOUR_ID}/`)
- **Count:** 144 JSON files
- **Content:** CFLP problem instances with unique data based on your student ID
- **Categories (Research-Based Ratio Model for Realistic Difficulty):** 
  - **Sizes (Realistic Educational Benchmarks):** 
    - Small: 30-50 facilities, 150-300 clients (~4,500-15,000 variables)
    - Medium: 60-90 facilities, 400-700 clients (~24,000-63,000 variables)
    - Large: 100-150 facilities, 800-1500 clients (~80,000-225,000 variables)
  - **Difficulties (via α - Fixed-to-OPTIMAL-Service Cost RATIO):**
    - Easy: α ∈ [0.3, 0.6] → Fixed costs 30-60% of **optimal service** → Open many facilities
    - Moderate: α ∈ [0.8, 1.5] → Fixed ≈ **Optimal service** → Balanced trade-offs
    - Expensive: α ∈ [2.0, 4.0] → Fixed costs 2-4× **optimal service** → Open few facilities
  - **Noise:** Constant 15% proportional noise for ALL difficulties (difficulty comes from α, not noise!)
  - **Spatial Distribution:** 1000×1000 coordinate space with unique positions (no overlaps)
  - **16 instances per size-difficulty combination**
- **Format:** JSON with keys: num_facilities, num_clients, fixed_costs, service_costs, facility_capacities, client_demands, client_coordinates, facility_coordinates
- **🔬 DESIGN PRINCIPLES (Based on FLP Research Literature):** 
  - Computational hardness comes from **BALANCED COST TRADE-OFFS**, not high absolute values
  - **CRITICAL**: α computed against **optimal service cost** (minimum per client), NOT sum of all pairs
  - **α ≈ 1.0 creates interesting trade-off decisions** because opening facility costs ≈ optimal service savings
  - This creates **realistic business scenarios** where both exact and heuristic methods provide insights
  - Constant noise (15%) adds realism without artificially helping/hindering algorithms
  - All coordinates are guaranteed unique (no overlapping entities)
  - **Balanced for education**: Gurobi can solve these instances to optimality (academic license), which gives a reference for measuring the VNS gap

#### 2. Convergence Plots (`convergence_plots_student_{YOUR_ID}/`)
- **Count:** Up to 144 PNG files (depends on Gurobi version)
- **Content:** VNS optimization trajectory per instance
- **Shows:**
  - Blue line: VNS cost over iterations
  - Red dashed line: Gurobi optimal cost
  - Demonstrates convergence behavior
- **Resolution:** 150 DPI

#### 3. Analysis Plots (`analysis_plots_student_{YOUR_ID}/`)
- **Count:** 6 PNG files
- **Purpose:** Detailed analysis for report writing

**Plot 1: Gap Analysis by Categories**
- Boxplots of optimality gap by size and difficulty
- Use for: Understanding which factors affect solution quality

**Plot 2: Time Analysis Detailed**
- VNS vs Gurobi computation times
- Heatmaps showing time by size and difficulty
- Use for: Computational efficiency analysis

**Plot 3: Speedup Analysis Comprehensive**
- Speedup factor (Gurobi time / VNS time)
- Shows how much faster VNS is
- Use for: Quantifying efficiency gains

**Plot 4: Scalability Analysis**
- Time vs problem size scatter plots
- Use for: Understanding algorithm scalability

**Plot 5: Quality-Time Tradeoff**
- Gap vs time scatter plots
- Pareto frontier analysis
- Use for: Identifying optimal tradeoff points

**Plot 6: Statistical Summary Dashboard**
- Table with key metrics
- Overall statistics
- Use for: Quick reference of important numbers

#### 4. Main Visualization (`comparison_analysis_student_{YOUR_ID}.png`)
- **Content:** Combined 6-plot figure
- **Subplots:**
  1. Gap distribution by size
  2. Computation time comparison
  3. Speedup factor
  4. Scalability (time vs size)
  5. Gap vs problem size
  6. Summary statistics table
- **Resolution:** 300 DPI (publication quality)

#### 5. Results CSV (`results_student_{YOUR_ID}.csv`)
- **Format:** CSV (comma-separated values)
- **Columns:**
  - instance_id, instance_name
  - size, difficulty
  - num_facilities, num_clients
  - gurobi_cost, gurobi_time, gurobi_status
  - vns_cost, vns_time, vns_iterations, vns_status
  - gap_percent, speedup_factor

#### 6. Results Excel (`results_student_{YOUR_ID}.xlsx`)
- **Format:** Excel workbook with 5 sheets

**Sheet 1: All Results**
- Complete dataset with all instances and metrics

**Sheet 2: Summary by Size**
- Aggregated statistics by size (Small, Medium, Large)
- Mean and std for costs, times, gaps, speedups

**Sheet 3: Summary by Difficulty**
- Aggregated statistics by difficulty (Easy, Moderate, Expensive)
- Mean and std for all metrics

**Sheet 4: Gap Matrix**
- Pivot table: Size × Difficulty
- Shows average gap for each combination

**Sheet 5: Speedup Matrix**
- Pivot table: Size × Difficulty
- Shows average speedup for each combination

#### 7. Text Summary (`report_summary_student_{YOUR_ID}.txt`)
- **Format:** Plain text
- **Content:**
  - Overview (total instances, successes, failures)
  - Solution quality (mean/max/min gap, std deviation)
  - Computational efficiency (mean times, speedup)
  - Performance by size
  - Performance by difficulty
- **Purpose:** Quick reference for report writing

### Important Notes About File Names:

**All generated files (EXCEPT instance JSON files) include your student ID in the filename!**

This ensures that:
- ✅ Each student has unique results
- ✅ No conflicts when comparing results with classmates
- ✅ Easy identification of your submission
- ✅ Instructors can verify originality

**Examples for student ID 20231234:**
- `results_student_20231234.csv`
- `results_student_20231234.xlsx`
- `comparison_analysis_student_20231234.png`
- `report_summary_student_20231234.txt`
- `convergence_plots_student_20231234/` folder
- `analysis_plots_student_20231234/` folder
- `cflp_instances_student_20231234/` folder (instance files inside use standard names)

---

## 7. Analysis Questions

You must answer the following questions in **`My_Answers.md`**. Use the generated plots, Excel files, and your understanding of the algorithms to provide comprehensive answers.

### Important Notes:
- All questions require **detailed analysis** with **specific numbers** from YOUR results
- Reference specific plots when answering (e.g., "As shown in Plot 01...")
- Explain **WHY** things happen, not just **WHAT** you observe
- Your answers should demonstrate **deep understanding** of the algorithms

---

### Section A: Solution Quality Analysis (4 marks)

**A.1** (2 marks) Using Plot 01 (Gap Analysis by Categories), analyze the optimality gap patterns:
- Which problem size shows the highest median gap? Provide the specific percentage.
- How does difficulty level (Easy, Moderate, Expensive) affect VNS solution quality?
- Identify any outliers and explain possible reasons for their occurrence.

**A.2** (2 marks) Based on your results Excel file (Sheet "All Results"):
- What percentage of instances have gap < 5%?
- What percentage of instances have gap < 10%?
- Calculate the mean and standard deviation of gaps across all instances.
- Interpret these statistics: Is VNS providing consistently good solutions?

---

### Section B: Computational Efficiency Analysis (4 marks)

**B.1** (2 marks) Using Plot 02 (Time Analysis Detailed), compare VNS and Gurobi:
- At what problem size does VNS become significantly faster than Gurobi?
- Examine the heatmaps: Which size-difficulty combination is most challenging for:
  - VNS?
  - Gurobi?
- Provide specific time values to support your observations.

**B.2** (2 marks) Using Plot 03 (Speedup Analysis Comprehensive):
- Calculate the average speedup factor for each size (Small, Medium, Large).
- Are there any instances where Gurobi is faster than VNS (speedup < 1)?
- If yes, what characteristics do these instances share?
- Discuss the practical implications of these speedup factors.

---

### Section C: Scalability Analysis (3 marks)

**C.1** (1.5 marks) Using Plot 04 (Scalability Analysis):
- Describe how VNS computation time scales with problem size.
- Describe how Gurobi computation time scales with problem size.
- Which algorithm exhibits better scalability? Justify your answer.

**C.2** (1.5 marks) Analyze the growth rate (from your data only - no theory needed):
- From your results, compute how the average runtime changes when moving from Small to Medium and from Medium to Large (e.g., "Gurobi time is multiplied by about 4x"). Do this for both VNS and Gurobi.
- Describe the shape of the growth you observe in Plot 04: roughly steady, steadily increasing, or sharply increasing?
- Using these growth ratios, give a rough prediction of the runtime for an "Extra Large" instance (e.g., n=50, m=20) for both algorithms, and explain how you computed it.
- Based on your observations, would VNS be more suitable for very large instances? Why or why not?

---

### Section D: Quality-Time Tradeoff Analysis (3 marks)

**D.1** (1.5 marks) Using Plot 05 (Quality-Time Tradeoff):
- Identify the "sweet spot" where VNS provides the best balance between quality and time.
- For instances where VNS achieves < 5% gap, what is the typical time savings compared to Gurobi?
- Express this as both absolute time (seconds) and relative savings (percentage).

**D.2** (1.5 marks) Practical decision-making:
- Discuss scenarios where accepting a 10-15% gap would be justified for faster computation.
- Consider factors like: problem urgency, computational resources, solution requirements.
- Would you recommend VNS or Gurobi for a real-world logistics company? Why?

---

### Section E: Algorithm Understanding (3 marks)

**E.1** (1.5 marks) VNS Neighborhood Structures:
- Explain the role of each neighborhood structure (N1, N2, N3, N4) in the search process.
- Which neighborhoods contribute more to:
  - **Intensification** (local refinement)?
  - **Diversification** (exploring new areas)?
- How do these neighborhoods complement each other?

**E.2** (1.5 marks) Convergence Behavior:
- Select and analyze 3 convergence plots from `convergence_plots_student_{YOUR_ID}/`
- Identify:
  - Instances with smooth convergence
  - Instances with multiple improvement "jumps"
- What do these patterns tell you about the search landscape?
- Explain the role of the shaking mechanism in overcoming local optima.

---

### Section F: Critical Reflection (3 marks)

**F.1** (1 mark) Limitations and Improvements:
- What are the main limitations of the VNS implementation?
- Suggest 2-3 improvements that could enhance performance.
- Consider: parameter tuning, additional neighborhoods, stopping criteria, etc.

**F.2** (1 mark) Real-World Deployment:
- If deploying this in a real logistics company, what additional constraints or objectives would be needed?
- Consider: service time windows, vehicle routing, multi-period planning, stochastic demand, etc.

**F.3** (1 mark) Exact Method vs Heuristic in Practice:
- Check the `gurobi_status` column of your results: for how many instances did Gurobi prove optimality, and how many stopped at the time limit?
- Gurobi guarantees the optimal solution (when it finishes) while VNS does not. Using your results, explain what VNS gives up and what it gains.
- Discuss the practical value of heuristics like VNS in industrial settings, based on what you observed.

---

## 8. Bonus Question: VNS Improvement Challenge (3 Bonus Marks)

### 🎯 Challenge Overview

The VNS implementation provided in `Assignment_1.ipynb` contains a **subtle but critical bug** that significantly degrades its performance. If you examine your results carefully, you'll notice that VNS produces poor-quality solutions (gaps > 20%) for many instances, even though the algorithm appears to run normally.

**Your Challenge:** Identify the bug, fix it, and demonstrate improved performance!

### 📋 Requirements

To earn the 3 bonus marks, you must:

1. **Identify the Bug (1 mark)**
   - Describe the specific issue in the VNS implementation
   - Explain WHY this bug causes poor performance
   - Reference specific lines of code in the notebook

2. **Implement the Fix (1 mark)**
   - Modify the VNS code to fix the bug
   - Your fix should be minimal (changing 1-3 lines is sufficient)
   - Ensure your fix doesn't break other parts of the algorithm

3. **Demonstrate Improvement (1 mark)**
   - Run the fixed version on the same instances
   - Show that gaps significantly decrease (e.g., from 20%+ to < 5%)
   - Provide a comparison table showing before/after performance

### 📤 Submission for Bonus

Push an **additional improved notebook** to your forked repository, named:
```
StudentID_FirstName_LastName_Assignment1_IMPROVED.ipynb
```

**Example:** `20231234_Ali_Ben_Ahmed_Assignment1_IMPROVED.ipynb`

**The improved notebook should include:**
- ✅ The fixed VNS code (with comments explaining your fix)
- ✅ Results from running with the fixed version (push the results files too)

**Your written bonus answer goes in Section G of `My_Answers.md`:**
- Description of the bug you found
- Explanation of your fix
- Comparison table (original vs improved results)
- Analysis of why your fix works

### 💡 Hints to Get Started

<details>
<summary>Click to reveal hints (don't look unless you're stuck!)</summary>

**Hint 1:** The bug is related to how VNS accepts solutions during iterations. Look carefully at the acceptance criterion in the main VNS loop.

**Hint 2:** Pay attention to what happens when VNS doesn't find an improvement. Does it keep the best solution or does it accidentally accept a worse solution?

**Hint 3:** Check the lines in the `vns_solve_cflp` function around the `else` block of the acceptance criterion (around line 1640-1645).

**Hint 4:** Compare what happens to `current_solution` and `best_solution` in both the `if` and `else` branches.

**Diagnostic Tool:** If you're really stuck, create a file called `vns_diagnostic.py` that tracks all costs explored by VNS iteration by iteration. You should see if VNS is "cycling" between the same few costs repeatedly.

</details>

### 🎓 Learning Objectives for Bonus

By attempting this bonus challenge, you will:
- Develop debugging skills for metaheuristic algorithms
- Understand the importance of proper acceptance criteria in VNS
- Learn to diagnose performance issues through systematic analysis
- Gain deeper insight into VNS convergence behavior

### ⚖️ Evaluation Criteria

**Full Marks (3/3):**
- Correct identification of the bug
- Proper fix that significantly improves performance
- Clear explanation and good documentation
- Before/after comparison showing 15%+ gap improvement

**Partial Credit (1.5-2.5/3):**
- Partially correct identification or fix
- Some improvement but not optimal
- Incomplete documentation

**No Credit (0/3):**
- Incorrect bug identification
- Fix that doesn't improve performance
- No clear explanation

---

## 9. Grading Rubric

**Total: 20 marks (+ 3 bonus)**

| Component | Marks | Criteria |
|-----------|-------|----------|
| **A. Solution Quality Analysis** | 4 | Accurate gap analysis, proper interpretation of statistics, specific numerical values |
| **B. Computational Efficiency** | 4 | Correct time comparisons, speedup analysis, practical insights |
| **C. Scalability Analysis** | 3 | Understanding of how runtime grows with size (from your data), reasonable predictions |
| **D. Quality-Time Tradeoff** | 3 | Identification of tradeoffs, practical recommendations, justified decisions |
| **E. Algorithm Understanding** | 3 | Deep understanding of VNS mechanics, convergence analysis, neighborhood roles |
| **F. Critical Reflection** | 3 | Thoughtful limitations, realistic improvements, practical considerations |
| **BONUS: VNS Improvement** | +3 | Correct bug identification, proper fix implementation, demonstrated improvement |

### Detailed Criteria:

**Excellent (90-100%):**
- All questions answered comprehensively
- Specific numerical values provided from YOUR results
- Deep understanding demonstrated
- Clear explanations of WHY, not just WHAT
- Plots and data properly referenced
- Insightful critical analysis

**Good (75-89%):**
- Most questions answered well
- Some numerical values provided
- Good understanding shown
- Some explanations of reasoning
- Decent analysis depth

**Satisfactory (60-74%):**
- Questions answered but lacking depth
- Few specific numbers
- Basic understanding
- Mostly descriptive, less analytical
- Minimal critical thinking

**Needs Improvement (<60%):**
- Incomplete answers
- No specific data from results
- Surface-level understanding
- Copy-paste from plots without interpretation
- No critical analysis

---

## 10. Submission Guidelines

### Submission = Your Own GitHub Fork

You do **not** submit a ZIP file. Instead, you work in **your own fork** of the course repository and submit **the link to your fork** on Blackboard.

### Step 1: Fork and rename the repository

1. Sign in to GitHub (create a free account if needed).
2. Open the course repository: https://github.com/makopt/Assignment_1
3. Click **Fork** (top-right) -> **Create fork**.
4. In your fork, go to **Settings -> General -> Repository name**, rename it to:

   ```
   Assignment_1_StudentID_FirstName_LastName
   ```

   **Example:** `Assignment_1_20231234_Ali_Ben_Ahmed`

5. Clone **your fork** (not the original) to your computer:

   ```bash
   git clone https://github.com/<your-username>/Assignment_1_20231234_Ali_Ben_Ahmed.git
   ```

> Keep the fork **public** (GitHub does not allow a fork of a public repository to be made private), so that the instructors can open it without extra access.

### Step 2: Do the work inside your cloned fork

Complete the assignment as described in this guide. Save everything inside the cloned folder.

### Step 3: Commit and push your work

```bash
git add Assignment_1.ipynb My_Answers.md results_student_*.csv results_student_*.xlsx comparison_analysis_student_*.png analysis_plots_student_*
git commit -m "Assignment 1 - <StudentID> <FirstName LastName>"
git push origin main
```

Then refresh your repository page on GitHub and check that all files are visible and the notebook shows its outputs.

### What Your Repository Must Contain:

1. ✅ **Completed Jupyter Notebook:** `Assignment_1.ipynb`
   - All cells executed successfully
   - Output visible for all cells
   - Your student ID clearly used in main function

2. ✅ **Completed answers file:** `My_Answers.md`
   - All questions (Sections A-F) answered completely
   - Include your student ID and name at the top
   - Use proper markdown formatting
   - Do not modify `Assignment_1_Student_Guide.md`

3. ✅ **Results Files:**
   - `results_student_{YOUR_ID}.csv`
   - `results_student_{YOUR_ID}.xlsx`

4. ✅ **Key Visualizations:**
   - `comparison_analysis_student_{YOUR_ID}.png`
   - All 6 files from `analysis_plots_student_{YOUR_ID}/`

5. ❌ **DO NOT push:**
   - Instance files (`cflp_instances` folder - too large)
   - All convergence plots (`convergence_plots` folder - too large)
   - You may push just 3-5 sample convergence plots (folder `sample_convergence_plots/`) for question E.2

6. ✅ **BONUS (Optional - 3 marks):**
   - `StudentID_FirstName_LastName_Assignment1_IMPROVED.ipynb` (your improved VNS version)
   - Results from the improved version showing better performance

**Expected repository content:**
```
Assignment_1_20231234_Ali_Ben_Ahmed/
├── Assignment_1.ipynb
├── Assignment_1_Student_Guide.md (unchanged)
├── My_Answers.md (your answers)
├── results_student_20231234.csv
├── results_student_20231234.xlsx
├── comparison_analysis_student_20231234.png
├── analysis_plots_student_20231234/
│   ├── 01_gap_analysis_by_categories.png
│   ├── 02_time_analysis_detailed.png
│   ├── 03_speedup_analysis_comprehensive.png
│   ├── 04_scalability_analysis.png
│   ├── 05_quality_time_tradeoff.png
│   └── 06_statistical_summary_dashboard.png
└── sample_convergence_plots/ (optional - 3-5 plots for E.2)
```

> Tip: the repository already contains a `.gitignore` that excludes the generated instance and convergence-plot folders. Check `git status` before committing to be sure nothing large is added.

### Submission Checklist:

Before submitting the link, verify:

- [ ] You forked the repository and renamed it `Assignment_1_StudentID_FirstName_LastName`
- [ ] Your student ID is used in the notebook (NOT someone else's!)
- [ ] All cells in the notebook executed successfully
- [ ] All questions in this guide are answered completely
- [ ] Specific numerical values from YOUR results are included
- [ ] Plots are referenced in your answers
- [ ] All required files are committed and **pushed** (visible on GitHub in your browser)
- [ ] The repository is public and the link opens without logging in (test it in a private/incognito window)

### Submission Platform:

Submit via: **Blackboard on the dedicated rubric**

**What to submit on Blackboard:** the **link (URL) of your forked repository**, e.g.
`https://github.com/<your-username>/Assignment_1_20231234_Ali_Ben_Ahmed`

**Deadline:** November 07, 2026 11:59 PM

> The deadline applies to your **last commit**. Commits pushed after the deadline are considered late.

**Late Policy:** Late will be graded 0, no exceptions (plan ahead!)

---

## 11. Academic Integrity

### Important Reminders:

⚠️ **Your student ID ensures unique results!**
- Each student will have DIFFERENT numerical results
- Instances, costs, times, gaps will all be unique to you
- Copying results from classmates will be EASILY detected
- Focus on YOUR analysis and interpretation

✅ **Acceptable Collaboration:**
- Discussing general concepts and algorithms
- Helping each other with technical issues (installation, errors)
- Sharing understanding of the problem formulation

❌ **NOT Acceptable:**
- Copying numerical results from classmates
- Submitting identical analysis answers
- Sharing code modifications
- Using another student's generated files

### Consequences:
Violations of academic integrity will result in:
- Zero marks for the assignment
- Report to academic affairs
- Potential disciplinary action

---

## 12. Getting Help

### Technical Issues:

**Installation Problems:**
- Check Python version (3.8+)
- Try: `pip install --upgrade gurobipy numpy pandas matplotlib seaborn openpyxl`

**Gurobi License Issues:**
- Academic: https://www.gurobi.com/academia/
- Free version: Just use `gurobi_version='free'` parameter

**Memory Errors:**
- Close other applications
- Use a machine with 8GB+ RAM if possible

**Long Runtime:**
- Normal! 10-20 minutes is typical
- If it takes much longer, check your hardware or Gurobi license



---

# 13. YOUR ANALYSIS AND ANSWERS

Your answers go in a **separate file**, `My_Answers.md`, which is provided in the repository as a template (student information, Sections A-F, additional comments and declaration).

- Fill in `My_Answers.md` - do **not** write your answers in this guide.
- Keep this guide (`Assignment_1_Student_Guide.md`) unchanged.
- Fill in your student ID and name at the top of `My_Answers.md`.

---

**END OF ASSIGNMENT DOCUMENT**

*Please ensure `My_Answers.md` is completed before submission. Good luck!*
