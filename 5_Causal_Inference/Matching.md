## Designing and Analyzing Experiments

Sample Size Calculation

- Significant level: cutoff threshold value for p value to avoid **Type I error**, failue to detect 

- Power: for **type II error**, failure to detect .. when there is one



Post hoc randomization check, take treatment and control group  





## Matching

When we can't run expriments, such as scholarship vs years to graduate, we just cannot simply run expriment with treatment; Also, 

so we need matching 

Find pairs of treated unit and control unit that are **as similar as possible** on observed variables,  except for treatment assignment.  We believe in the matching, outcome variable can only be attributed to treatment

- Intuition: find matched pairs that are indifferent on observed variables, so that the treatment assignment is “as good as random”
- If no match is found, throw away the data



How to we define similarity, is that based on domain knowledge? (How do we define who's to match with who?)

**Propensity score,** a fancy name of probability of people getting treatment.

- Dimension reduction to PSM and only match based on PSM
- caliper is a threshold, control PSM +- caliper
- how many do we choose? 1,2  is the nearest neighbors



Coarsened Exact Matching (CEM): discretize varibles into bins, and only match control group in that bins











Big Assumption: When we can observe all the varibles that determine whether a person is treated or not. 