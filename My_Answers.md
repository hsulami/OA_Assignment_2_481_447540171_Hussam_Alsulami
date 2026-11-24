# My Answers - Assignment 2 (CFLP: Tabu Search vs Genetic Algorithm vs Gurobi)

**Student Information:**
- **Student ID:** 447540171
- **Student Name:** Hussam Alsulami
- **Gurobi Version Used:** [x] Academic [ ] Free
- **Date Completed:** 25/11/2026

> Use specific numbers from **YOUR** results (`results_student_<ID>.csv/.xlsx`) and reference the plots by name (e.g., "As shown in `gap_by_size.png`..."). Explain **why**, not only **what**.

---

## Section A: Solution Quality Analysis (4 marks)

### A1 (1 mark) - Overall Mean Optimality Gap

| | Tabu Search | Genetic Algorithm |
|---|---|---|
| Number of instances used (Gurobi optimal) | 144 | 144 |
| Mean gap (%) | 9.99 | 10.60 |

**Method with the lower overall gap:** [x] TS [ ] GA

**Comment:**

TS has a slightly lower mean gap than GA (9.99% vs 10.60%). The difference is small: both methods found exactly the same cost on 68 of the 144 instances, and neither reached the optimum on any instance. Both stop about 10% above Gurobi, probably because both use the same greedy client assignment.

---

### A2 (1 mark) - Gap by Instance Size

| | Small | Medium | Large |
|---|---|---|---|
| TS mean gap (%) | 9.99 | 9.98 | 9.99 |
| GA mean gap (%) | 10.61 | 10.58 | 10.61 |

**Does the gap increase with size?** TS: [ ] Yes [x] No  GA: [ ] Yes [x] No

**Comparison of the trends (TS vs GA):**

As shown in `gap_by_size.png`, the gap is almost flat for both methods, and TS stays about 0.6 points better than GA in every size. The gap does not grow because my instance sizes are close to each other (medium has 18-24 facilities, large has 20-27, with a similar number of clients). Difficulty affects the gap much more than size (see A3).

---

### A3 (1 mark) - Gap by Difficulty Level

| | Easy | Moderate | Expensive |
|---|---|---|---|
| TS mean gap (%) | 6.99 | 13.43 | 9.55 |
| GA mean gap (%) | 7.22 | 14.28 | 10.30 |

- Hardest difficulty for TS: Moderate
- Hardest difficulty for GA: Moderate

**Explanation:**

As shown in `gap_by_difficulty.png`, moderate is the hardest for both methods. Moderate instances have tight capacity (only 10-30% slack) and balanced fixed and service costs. With tight capacity many clients cannot go to their nearest facility and many open/close moves become infeasible, so the search gets stuck in local optima. Easy instances have loose capacity, so the greedy solution is already close to the optimum.

---

### A4 (1 mark) - TS vs GA Head-to-Head (`ts_vs_ga_scatter.png`)

- Fraction of instances where TS has a lower gap than GA: 75 / 144 ( 52.1% )
- Fraction of instances where GA has a lower gap than TS: 1 / 144 ( 0.7% )

**Instance types where one method consistently dominates:**

In `ts_vs_ga_scatter.png` almost all points are on or above the diagonal, and the other 68 instances (47.2%) are ties. GA wins only once (instance 095, medium-expensive). TS dominates on moderate and expensive instances (better on 59 of 96), while on easy instances the two methods are mostly equal (32 ties out of 48).

---

## Section B: Computational Efficiency Analysis (4 marks)

### B1 (1 mark) - Computation Time and TS Speedup

| Mean time (s) | Small | Medium | Large |
|---|---|---|---|
| Gurobi | 0.90 | 4.11 | 2.03 |
| Tabu Search | 0.56 | 0.59 | 0.74 |
| Genetic Algorithm | 0.57 | 0.60 | 0.74 |

**TS speedup over Gurobi on large instances:** 2.54x

**Comment:**

As shown in `computation_time.png`, TS and GA need less than 1 s on average for all sizes. Gurobi is also fast on these sizes, so the speedup is small. Gurobi is slowest on medium because of a few hard instances (instance 095 alone took 71.9 s).

---

### B2 (1 mark) - GA Speedup vs TS Speedup

- Mean GA speedup over Gurobi (all sizes): 3.52x
- Mean TS speedup over Gurobi (all sizes): 4.31x
- Faster metaheuristic and by how much: TS, 4.31 / 3.52 = 1.22x higher speedup

**Comment:**

As shown in `speedup_factor.png`, TS has the higher speedup, but the real time difference is tiny: mean time 0.63 s for TS vs 0.64 s for GA. On large instances GA speedup is even a bit higher (2.93x vs 2.54x). So both have about the same speed, and TS wins mainly on quality.

---

### B3 (1 mark) - Growth of Gurobi Time vs Metaheuristic Time

| | Small -> Medium (x) | Medium -> Large (x) |
|---|---|---|
| Gurobi time growth | 4.11 / 0.90 = 4.57 | 2.03 / 4.11 = 0.49 |
| TS time growth | 0.59 / 0.56 = 1.06 | 0.74 / 0.59 = 1.24 |
| GA time growth | 0.60 / 0.57 = 1.06 | 0.74 / 0.60 = 1.22 |

**Comparison:**

TS and GA times grow slowly (about 1.1-1.2x per step) because they run a limited number of iterations. Gurobi time does not follow the size: it jumps 4.57x and then drops to 0.49x, because it depends more on difficulty (0.23 s for easy vs 4.90 s for expensive on average). For much larger instances I expect Gurobi time to grow much faster than TS and GA, because an exact search grows very fast with the problem size.

---

### B4 (1 mark) - Quality-Speed Trade-off

**Method with the best balance of low gap and low time:** [ ] TS [ ] GA [x] Gurobi

**Justification (with numbers):**

Gurobi gives 0% gap in 2.35 s on average, while TS saves only about 1.7 s (0.63 s) but loses 9.99% in quality, and GA loses 10.60% in 0.64 s. So for these instance sizes Gurobi gives the best balance. Between the two metaheuristics TS is better: the same time as GA and a lower gap. TS is useful only if we need a result in under 1 s or the instances become too big for Gurobi.

---

## Section C: Scalability Analysis (3 marks)

### C1 (1 mark) - Gap Heatmap (`gap_heatmap.png`)

- Highest TS gap: (size, difficulty) = (large, moderate) with 13.87%
- Highest GA gap: (size, difficulty) = (medium, moderate) with 14.65%

**Interpretation:**

In `gap_heatmap.png` the moderate column is the darkest in all sizes for both methods (TS 13.02-13.87%, GA 13.73-14.65%), and easy is the lightest. So tight capacity affects the gap much more than size: inside the moderate column the sizes differ by less than 1 point.

---

### C2 (1 mark) - Expected Scalability to Very Large Instances

**Method expected to scale better:** [x] TS [ ] GA

**Justification (algorithmic complexity and your observed results):**

Both methods spend most of their time assigning clients to facilities, and this cost grows with clients x facilities. TS checks a fixed number of neighbors per iteration (`max_candidates`), while GA must repair and evaluate the whole population every generation, and with many facilities crossover and mutation change many genes at once, so it needs more generations. In my results TS already gives a lower gap in the same time, and its time grew only 1.24x from medium to large, so I expect TS to scale better (with a larger `max_candidates` for very large instances).

---

### C3 (1 mark) - Validity of Gap Measurements When Gurobi Hits the Time Limit

- Instances where Gurobi did not prove optimality (from `gurobi_status`): 0 out of 144

**Effect on the validity of the gaps:**

In my run all gaps are valid: the longest Gurobi time was 71.9 s, far below the 300 s limit (I checked `gurobi_time` too, because the code writes "Optimal" also when the time limit is hit). If Gurobi stops at the limit, its cost is only the best solution found, not the optimum, so the gap is underestimated and can even become negative.

**Better benchmark:**

Compare against the lower bound that Gurobi reports instead of its best solution, or against the best known solution from a longer Gurobi run.

---

## Section D: Quality-Time Tradeoff Analysis (3 marks)

### D1 (1 mark) - 10-Second Time Budget (Medium Instances)

**Recommended method:** [x] TS [ ] GA

- Mean TS time / gap on medium instances: 0.59 s / 9.98%
- Mean GA time / gap on medium instances: 0.60 s / 10.58%

**Justification:**

Both methods are far below 10 s (maximum on medium: TS 1.50 s, GA 1.27 s), so time is not a problem. TS gives the lower gap (9.98% vs 10.58%) and was better on 25 of the 48 medium instances, GA only on 1. Gurobi is not safe for a 10 s limit because instance 095 needed 71.9 s.

---

### D2 (1 mark) - Quality Is the Top Priority

| Size | Recommended approach (TS / GA / Gurobi) | Justification (numbers) |
|---|---|---|
| Small | Gurobi | 0% gap in 0.90 s mean, vs TS 9.99% and GA 10.61% |
| Medium | Gurobi | 0% gap in 4.11 s mean (max 71.9 s), vs TS 9.98% and GA 10.58% |
| Large | Gurobi | 0% gap in 2.03 s mean, vs TS 9.99% and GA 10.61% |

---

### D3 (1 mark) - Convergence Timing (3-5 convergence plots, pushed in `sample_convergence_plots/`)

**Selected plots:** 103_cflp_large_easy_fac23_cli68_convergence.png, 108_cflp_large_easy_fac22_cli65_convergence.png, 121_cflp_large_moderate_fac25_cli68_convergence.png, 139_cflp_large_expensive_fac22_cli67_convergence.png

| Instance | TS: iteration of best solution / total iterations | GA: generation of best solution / total generations |
|---|---|---|
| 103 (large, easy) | 32 / 72 = 0.44 | 11 / 41 = 0.27 |
| 108 (large, easy) | 6 / 46 = 0.13 | 15 / 45 = 0.33 |
| 121 (large, moderate) | 110 / 150 = 0.73 | 0 / 30 = 0.00 |
| 139 (large, expensive) | 50 / 90 = 0.56 | 31 / 61 = 0.51 |

**What this implies for early stopping:**

The total iterations/generations are taken from the notebook log, because the CSV columns `ts_iterations` and `ga_generations` give the iteration of the last improvement. On all 48 large instances, TS found its best solution at about 23% of its iterations and GA at about 28% of its generations. So most of the run after that is wasted, and early stopping saves time without losing quality. But instance 121 shows that TS can still improve late (iteration 110), so `max_no_improve` should not be too small.

---

## Section E: Algorithm Understanding (3 marks)

### E1 (1 mark) - Tabu Search: Tabu List and Aspiration Criterion

**How the tabu list prevents cycling:**

After a move is applied, its inverse move is forbidden for `tabu_tenure` = 10 iterations. So TS cannot go straight back to the solution it just left, and it is forced to try new solutions instead of going back and forth between the same two solutions.

**Role of the aspiration criterion:**

A tabu move is still allowed if it gives a cost better than the best solution found so far. A new best solution cannot be a cycle, so there is no reason to block it.

**Concrete CFLP example** (a specific tabu move, and when aspiration would override it):

If TS applies `('close', 7)` (close facility 7), the inverse move `('open', 7)` becomes tabu for the next 10 iterations. If during this time opening facility 7 only improves the current solution, it stays blocked. But if opening facility 7 gives a total cost lower than the best cost found so far, aspiration overrides the tabu and the move is accepted.

---

### E2 (1 mark) - Genetic Algorithm: Repair Operator and Crossover

**How the repair operator ensures feasibility, and what would happen without it:**

If a child does not have enough open capacity for all the demand, the repair operator keeps opening the closed facility with the largest capacity until all clients can be assigned. Without it, many children would be infeasible (especially in moderate instances with tight capacity), and the GA would waste its generations on invalid solutions.

**How uniform crossover combines two parent CFLP solutions:**

For each facility, the child takes the gene from parent 1 or parent 2 with probability 0.5. Example with 6 facilities: P1 = `1 0 1 0 1 0`, P2 = `0 1 1 0 0 1`, mask (1 = take from P1) = `1 1 0 0 1 0`, child = `1 0 1 0 1 1`. Facilities where both parents agree (facility 3 open, facility 4 closed) are always kept, the others are mixed. Then the child is repaired and the clients are assigned greedily.

---

### E3 (1 mark) - Trajectory-Based vs Population-Based Exploration

**Meaning for the exploration of the solution space:**

TS moves one solution step by step to a neighbor, so it searches deeply around one path. GA keeps 30 solutions and mixes them, so it searches many areas of the space at once but less deeply in each one.

| | Advantages for the CFLP | Disadvantages for the CFLP |
|---|---|---|
| Tabu Search (trajectory) | Small moves (reassign, swap) directly fix the solution; the tabu list helps it escape local optima; lower gap in my results (9.99%) | Depends on the greedy start; can stay in one area (8 instances never improved the start) |
| Genetic Algorithm (population) | Explores many facility combinations at once; keeps diversity | Only chooses open facilities, client assignment is always greedy; crossover can break good solutions; no improvement over the initial population in 28 of 144 instances (10.60% gap) |

---

## Section F: Critical Reflection (3 marks)

### F1 (1 mark) - Most Important TS Hyperparameters

1. `max_no_improve` (40) - stops TS after 40 iterations without improvement. It decides the real run length: the mean total was only 58 iterations, and only 1 of 144 instances reached `max_iterations` = 200.

2. `tabu_tenure` (10) - how long an inverse move stays forbidden. It is enough to stop short cycles, but may be too short to push the search far from a local optimum.

3. `max_candidates` (20) - number of neighbors checked per move type. Only a sample of the neighborhood is checked, so good moves can be missed; the median best solution was found at iteration 7.

**What I would change and why:**

I would increase `max_no_improve` (for example to 80), because on instances 121 and 144 TS still improved after 90-110 iterations, and increase `max_candidates` to check more of the neighborhood. TS is fast (0.63 s mean), so the extra time is not a problem.

---

### F2 (1 mark) - Most Important GA Hyperparameters

1. `population_size` (30) - number of solutions per generation; controls diversity.

2. `mutation_rate` (0.10) - chance to flip each gene; keeps diversity but too much can destroy good solutions.

3. `max_no_improve` (30) - early stopping; it decides the real run length (mean total 42 generations, never reached `max_generations` = 100).

**Effect of increasing `population_size` / `max_generations` on quality and runtime:**

A larger `population_size` gives more diversity and should lower the gap a little, but the runtime grows about linearly because every chromosome is repaired and evaluated each generation. A larger `max_generations` alone would change almost nothing in my case, because the GA always stopped earlier by `max_no_improve` (max 90 generations), so `max_no_improve` must be increased too.

---

### F3 (1 mark) - Proposed Algorithmic Improvements

**Improvement for Tabu Search:**

Diversification restart: when TS does not improve for some iterations, apply a strong random change (several facility swaps) and continue from there. TS is a single trajectory, so its main weakness is getting stuck in one area; in my results it often stopped near the greedy start (median best at iteration 7) and the gap is high on moderate instances (13.43%).

**Improvement for the Genetic Algorithm:**

Memetic GA: apply a short Tabu Search to the best chromosome in each generation. GA is good at global exploration but has no fine local improvement, and in 28 instances it never improved its initial population. Local search adds the missing fine improvement while GA keeps the diversity.

---

## BONUS A: Tabu Search Improvement (+5%, optional)

*Leave this section empty if you do not attempt the bonus. See Section 8 of the guide. Your improved notebook (`StudentID_FirstName_LastName_Assignment2_IMPROVED.ipynb`) must be pushed to your repository.*

**(a) Screening results (small/moderate subset):**

| Enhancement tested | Mean gap before (%) | Mean gap after (%) | Improvement |
|---|---|---|---|
| Adaptive tenure | 12.535 | 12.528 | +0.007 |
| Aspiration+ criterion | 12.535 | 12.524 | +0.011 |
| Frequency-based memory | 12.535 | 12.703 | -0.167 |
| Diversification restart | 12.535 | 12.005 | +0.531 |

**(b) Selected enhancement and justification:**

Diversification restart. I tested the 4 options on the 16 small/moderate instances, 3 runs each, with the same seeds for all options (results in `bonus_screening_student_447540171.csv`). Restart gave the biggest gain (0.531 points), while the other three gave almost nothing or were worse (frequency memory -0.167). After 20 iterations without improvement, it kicks the best solution with 2 random swaps (max 3 times) and continues from there. It works because the original TS gets stuck near the greedy start, and the kick moves it to a new area. The cost is about double the runtime.

**(c) Full 144-instance results:**

| Metric | Original TS | Improved TS |
|---|---|---|
| Mean gap (%) | 9.988 | 9.593 |
| Mean runtime (s) | 0.629 | 1.268 |

**Improved results files (names in your repository):** `447540171_Hussam_Alsulami_Assignment2_IMPROVED.ipynb`, `results_student_447540171_improved.csv`, `results_student_447540171_improved.xlsx`, `visualization_plots_student_447540171_improved/`, `bonus_screening_student_447540171.csv`

---

## BONUS B: Genetic Algorithm Improvement (+5%, optional)

*Leave this section empty if you do not attempt the bonus.*

**(a) Screening results (small/moderate subset):**

| Enhancement tested | Mean gap before (%) | Mean gap after (%) | Improvement |
|---|---|---|---|
| Adaptive mutation | 13.734 | 13.734 | 0.000 |
| Two-point crossover | 13.734 | 13.734 | 0.000 |
| Memetic GA (TS + GA) | 13.734 | 12.172 | +1.562 |
| Diversity preservation | 13.734 | 13.730 | +0.003 |

**(b) Selected enhancement and justification:**

Memetic GA. Same test as Bonus A (16 small/moderate instances, 3 runs, same seeds). Memetic gave the biggest gain (1.562 points). The other three changed almost nothing: two-point crossover gave the same final gap and the same convergence speed (best found at generation 2.06 vs 2.00 on average). The original GA finds its best solution at about generation 2 and then stops improving, because it only chooses which facilities are open and always uses the greedy client assignment. The memetic version runs 10 Tabu Search iterations on the best chromosome every generation, which adds the reassign and swap moves that GA does not have. The cost is a longer runtime.

**(c) Full 144-instance results:**

| Metric | Original GA | Improved GA |
|---|---|---|
| Mean gap (%) | 10.600 | 9.771 |
| Mean runtime (s) | 0.637 | 4.289 |

**Improved results files (names in your repository):** same files as Bonus A (one improved notebook for both TS and GA)

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

**Signature:** Hussam Alsulami

**Date:** 25/11/2026

---

**END OF MY ANSWERS**
