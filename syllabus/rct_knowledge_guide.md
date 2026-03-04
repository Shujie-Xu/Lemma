# Comprehensive RCT Knowledge Guide

*A systematic reference for Randomized Controlled Trials in Development Economics*

*Core reference: Duflo, Glennerster, Kremer (2007) "Using Randomization in Development Economics Research: A Toolkit"; Glennerster & Takavarasha (2013) "Running Randomized Evaluations"; Athey & Imbens (2017) "The Econometrics of Randomized Experiments"*

---

## Part I: Foundations — Why Randomize?

### 1.1 The Fundamental Problem of Causal Inference

The core challenge: we can never observe the same unit in both the treatment and control state simultaneously (Holland, 1986). For individual $i$, we want to know $\tau_i = Y_i(1) - Y_i(0)$, but we only observe one of these potential outcomes.

**Selection bias in observational studies.** If we simply compare outcomes of those who received treatment vs. those who did not:

$$E[Y | D=1] - E[Y | D=0] = \underbrace{E[Y(1) - Y(0) | D=1]}_{\text{ATT}} + \underbrace{E[Y(0)|D=1] - E[Y(0)|D=0]}_{\text{Selection Bias}}$$

The second term is nonzero whenever the treated and untreated differ in baseline characteristics. For example, comparing income of people who attend college vs. those who don't confounds the return to education with ability, motivation, and family background.

**Why randomization solves this.** Random assignment makes $D$ independent of potential outcomes: $(Y(0), Y(1)) \perp D$. Therefore $E[Y(0)|D=1] = E[Y(0)|D=0]$, and the simple difference in means is an unbiased estimator of the Average Treatment Effect (ATE).

### 1.2 What Randomization Does and Does Not Guarantee

**Does guarantee (in expectation):**
- Balance on all covariates (observed AND unobserved) between treatment and control
- Unbiased estimation of the ATE
- Valid statistical inference under standard assumptions

**Does not guarantee:**
- Perfect balance in any single realization (finite sample); this is why we do balance checks
- External validity (results may not generalize to other populations or settings)
- That the estimand is policy-relevant (depends on compliance, spillovers, equilibrium effects)
- Correct standard errors automatically (still need to account for clustering, stratification, etc.)

### 1.3 Limitations of RCTs

- **Ethical constraints**: Cannot randomly assign harmful treatments; equipoise requirement
- **External validity**: Results from one setting may not generalize. Deaton (2010) and Heckman (2010) raise concerns about RCTs as "gold standard"
- **Hawthorne effects**: Subjects change behavior because they know they are being studied
- **John Henry effects**: Control group exerts extra effort to compensate
- **General equilibrium effects**: Partial equilibrium estimates may not scale; e.g., a job training program that works for 100 people may not work for 1 million because of labor market equilibrium
- **Cost and feasibility**: Some questions simply cannot be randomized
- **Site selection bias**: Experiments tend to be run where they are feasible, not necessarily where they are most policy-relevant (Allcott, 2015)

---

## Part II: Experimental Design

### 2.1 Unit of Randomization

**Individual-level randomization**
- Maximum statistical power (each individual is an independent observation)
- Clean identification; no ICC penalty
- Feasible when treatment is individual-specific (e.g., a personal financial incentive, a pill)
- Risk: spillovers between treated and control within the same community

**Cluster-level randomization**
- Randomize at the group level: villages, schools, clinics, firms
- Necessary when: (a) treatment is inherently group-level (e.g., infrastructure, school curriculum); (b) spillovers are a concern within clusters; (c) administrative/ethical reasons
- Cost: significant loss of statistical power due to intra-cluster correlation (see Power Analysis section)

**Choosing the unit**: The unit should be the level at which the treatment operates or the level below which spillovers are expected to occur. If treatment is a village-level water purification system, randomize at village level. If treatment is an individual cash transfer but neighbors might share, consider randomizing at neighborhood or village level.

### 2.2 Randomization Methods

#### 2.2.1 Simple (Pure) Randomization
Each unit independently assigned with fixed probability (e.g., coin flip with $p = 0.5$).

- **Pro**: Simplest; transparent; easy to verify
- **Con**: No guarantee of exact group sizes; possible covariate imbalance in small samples
- **When to use**: Large samples ($N > 500$) where the law of large numbers ensures approximate balance

#### 2.2.2 Stratified Randomization (Blocking)
Divide sample into strata based on pre-treatment covariates, then randomize within each stratum.

- **Pro**: Guarantees balance on stratification variables; improves precision (can reduce variance by 20–30% or more if stratification variables are predictive of outcome)
- **Con**: Need to choose stratification variables ex ante; too many strata with small cells defeat the purpose; analysis should include strata fixed effects
- **When to use**: Almost always recommended; especially when sample is small or covariates are strongly predictive of outcome
- **Best practice**: Stratify on 2–3 variables most correlated with the outcome; Bruhn & McKenzie (2009) show stratification rarely hurts and often substantially helps

#### 2.2.3 Pairwise Randomization (Matched-Pair Design)
Extreme version of stratification: create pairs of similar units, then randomize one to treatment within each pair.

- **Pro**: Maximum balance; highest precision for given sample size
- **Con**: If one unit in a pair attrites, you lose the entire pair; analysis must account for pair fixed effects; Bruhn & McKenzie (2009) show it can be less robust than stratification when pairs are imperfect
- **When to use**: Very small samples (e.g., 20 clusters); when you have rich baseline data to form good pairs

#### 2.2.4 Cluster Randomization
Randomize groups rather than individuals (detailed in Section 2.1 above).

- **Pro**: Avoids within-cluster spillovers; administratively simpler; politically more acceptable
- **Con**: Major power loss; requires more clusters (not more individuals per cluster) to recover power
- **When to use**: When treatment operates at group level or spillovers within groups are a primary concern

#### 2.2.5 Phase-In (Stepped Wedge) Design
All units eventually receive treatment, but the order/timing is randomized. Early recipients serve as treatment, later recipients as control.

- **Pro**: Ethically appealing (everyone gets treatment); politically feasible; useful when rollout is logistically staggered anyway
- **Con**: Cannot estimate long-run effects easily (control eventually becomes treated); potential anticipation effects; time effects can confound treatment effects
- **When to use**: When withholding treatment permanently is unethical or infeasible; when the implementing organization plans to scale anyway

#### 2.2.6 Encouragement Design
Cannot force treatment take-up, so randomize an "encouragement" (e.g., information, subsidy, invitation) and use it as an instrument for actual take-up.

- **Pro**: Ethical when you cannot mandate treatment; avoids coercion
- **Con**: Estimates LATE (effect on compliers only), not ATE; requires strong first stage; exclusion restriction must hold
- **When to use**: When treatment is voluntary (e.g., health insurance enrollment, school attendance)

#### 2.2.7 Factorial Design
Multiple treatments (or treatment arms) crossed with each other. For two binary treatments A and B, you get four groups: control, A only, B only, A+B.

- **Pro**: Tests multiple interventions simultaneously; tests interactions/complementarities; more efficient than separate experiments
- **Con**: Requires larger sample; interpretation complex if interactions are present; power for interaction term is typically low
- **When to use**: When you want to understand whether two interventions are complements or substitutes

#### 2.2.8 Regression Discontinuity (as quasi-experimental complement)
Not randomization per se, but useful to discuss: treatment assigned based on a threshold of a running variable. Units near the threshold are "as good as randomly assigned."

- **When relevant in RCT context**: Sometimes combined with RCT (e.g., RCT below a poverty line, RD at the line); useful when full randomization is not feasible

### 2.3 Treatment Arms

**Multiple treatment arms**: Often useful to test different versions or intensities of an intervention. For example, different subsidy levels, different information framings.

- Increases sample size requirements (must power each pairwise comparison)
- Pre-specify primary comparisons in pre-analysis plan to avoid multiple testing concerns
- Consider whether you need to detect differences between treatment arms or only between each arm and control

**Pure control vs. active control**: Pure control receives nothing; active control receives a placebo or alternative treatment. Active control helps isolate the specific mechanism of treatment vs. general attention/novelty effects.

### 2.4 Sampling and Recruitment

- **Target population**: Define clearly who is eligible. Inclusion/exclusion criteria affect external validity.
- **Sampling frame**: The list from which you draw. If incomplete, non-listed individuals are excluded, creating potential bias in generalizability.
- **Consent and IRB**: Informed consent is required. The consent process itself can cause differential attrition or Hawthorne effects. IRB approval needed before any data collection.
- **Baseline survey**: Conduct before randomization to measure pre-treatment covariates. Serves three purposes: (a) verify balance, (b) improve precision through ANCOVA, (c) provide baseline for difference-in-differences within the experiment.

---

## Part III: Threats to Validity

### 3.1 Internal Validity Threats

#### 3.1.1 Attrition (Loss to Follow-up)
Units drop out of the study between baseline and endline.

- **Why it matters**: If attrition is correlated with treatment assignment (differential attrition), the remaining sample is no longer balanced, and ITT is biased.
- **Detection**: (a) Compare attrition rates between treatment and control; (b) Compare baseline characteristics of attritors vs. non-attritors across groups; (c) Use Lee (2009) bounds to provide worst-case estimates.
- **Prevention**: Track participants aggressively; keep surveys short; provide incentives for follow-up; collect multiple contact methods at baseline.
- **Remediation**: Lee bounds (trimming); inverse probability weighting; bounding exercises assuming worst-case outcomes for attritors.

#### 3.1.2 Non-Compliance
Assigned treatment ≠ received treatment.

- **One-sided**: Only treatment group can fail to comply (never-takers in treatment group). Control group has no access to treatment. LATE = effect on compliers. Cleaner interpretation.
- **Two-sided**: Both groups can cross over. Treatment-assigned may not take up; control-assigned may access treatment elsewhere. More complex; LATE still identified under monotonicity.
- **Always-takers**: Would receive treatment regardless of assignment. Their behavior is the same in both groups, so they don't contribute to identifying the treatment effect.
- **Never-takers**: Would not receive treatment regardless. Same logic.
- **Compliers**: Take treatment if and only if assigned to treatment. LATE is the effect for this subgroup.
- **Defiers**: Would do the opposite of assignment. Monotonicity assumption rules these out.

#### 3.1.3 Spillovers (Interference)
Treatment of one unit affects outcomes of other units. Violates SUTVA (Stable Unit Treatment Value Assumption).

- **Types**: Information spillovers (treated tell control what they learned); resource spillovers (treated share benefits); market equilibrium effects (treatment changes prices); behavioral spillovers (social norms shift)
- **Detection**: Randomize treatment intensity at cluster level and measure outcomes for untreated within treated clusters vs. pure control clusters; use "spatial" or "network" distance to nearest treated unit
- **Design solutions**: Cluster randomize at a level above which spillovers are unlikely; create buffer zones; multi-level randomization (randomize clusters, then within clusters measure spillovers)
- **Miguel & Kremer (2004)**: Classic example showing deworming spillovers through school-level externalities

#### 3.1.4 Hawthorne and John Henry Effects
- **Hawthorne**: Treatment group changes behavior because of observation/attention, not the treatment itself
- **John Henry**: Control group compensates by trying harder
- **Solutions**: Placebo treatments for control; blind or double-blind design when feasible; measure outcomes using administrative data rather than self-reports

#### 3.1.5 Experimenter Demand Effects
Participants infer the "desired" result and adjust behavior accordingly.

#### 3.1.6 Contamination
Control group gains access to the treatment through informal channels. Essentially a spillover problem. Reduces the treatment-control contrast and attenuates ITT estimates.

### 3.2 External Validity Threats

- **Site selection**: Experiments run in NGO-friendly areas may not represent government-scale implementation
- **Partner effects**: Results depend on implementing organization's capacity
- **Volunteer bias**: Participants who consent may differ from general population
- **Scalability**: General equilibrium effects at scale differ from partial equilibrium in a trial
- **Temporal validity**: Effects may differ across time periods due to changing contexts
- **Replication across contexts**: Increasingly valued; "state of the art" is to test in multiple sites

---

## Part IV: Statistical Framework

### 4.1 Estimands

- **ATE** (Average Treatment Effect): $E[Y(1) - Y(0)]$ over the entire population
- **ATT** (Average Treatment Effect on the Treated): $E[Y(1) - Y(0) | D = 1]$; the effect for those who actually received treatment
- **LATE** (Local Average Treatment Effect): $E[Y(1) - Y(0) | \text{compliers}]$; the effect for those whose treatment status is changed by the instrument
- **ITT** (Intention to Treat): $E[Y | Z = 1] - E[Y | Z = 0]$; the effect of assignment, regardless of compliance. With perfect compliance, ITT = ATE. With imperfect compliance, ITT < ATE (attenuated toward zero).
- **CATE** (Conditional Average Treatment Effect): $E[Y(1) - Y(0) | X = x]$; heterogeneity in treatment effects by subgroup

### 4.2 Estimation

#### 4.2.1 Simple Difference in Means
$$\hat{\tau} = \bar{Y}_T - \bar{Y}_C$$

Unbiased for ATE under random assignment. Standard error:
$$SE = \sqrt{\frac{s_T^2}{n_T} + \frac{s_C^2}{n_C}}$$

#### 4.2.2 Regression Framework
$$Y_i = \alpha + \tau D_i + \epsilon_i$$

OLS estimate of $\tau$ is numerically identical to the difference in means. Adding baseline covariates $X_i$:

$$Y_i = \alpha + \tau D_i + X_i'\beta + \epsilon_i$$

This does not change the unbiasedness of $\hat{\tau}$ (since $D$ is randomly assigned), but reduces residual variance, thereby improving precision. Freedman (2008) shows that with stratified randomization you should include strata FE for correct inference; Lin (2013) shows that adding interactions $D_i \times (X_i - \bar{X})$ provides robust efficiency gains.

#### 4.2.3 ANCOVA (Analysis of Covariance)
$$Y_{i,\text{post}} = \alpha + \tau D_i + \gamma Y_{i,\text{pre}} + \epsilon_i$$

Controls for baseline outcome. McKenzie (2012) shows ANCOVA is generally more efficient than difference-in-differences when autocorrelation of outcome is moderate. Particularly useful when you have a baseline measure of the outcome.

#### 4.2.4 IV/2SLS for Non-Compliance

**First stage**: $D_i = \pi_0 + \pi_1 Z_i + \nu_i$ (where $Z$ is assignment, $D$ is actual treatment)

**Second stage**: $Y_i = \alpha + \tau_{IV} \hat{D}_i + \epsilon_i$

$\hat{\tau}_{IV} = \frac{\text{Cov}(Y, Z)}{\text{Cov}(D, Z)} = \frac{\text{ITT}}{\text{First Stage}} = \frac{E[Y|Z=1] - E[Y|Z=0]}{E[D|Z=1] - E[D|Z=0]}$

This is the Wald estimator. It identifies LATE under relevance, exclusion restriction, and monotonicity.

**Weak instrument concern**: If $\pi_1$ is small (low compliance rate), IV estimates are noisy and potentially biased. Rule of thumb: first-stage F-statistic > 10. With very low compliance, consider whether ITT alone is more informative.

#### 4.2.5 Difference-in-Differences within an RCT
$$Y_{it} = \alpha + \tau (D_i \times \text{Post}_t) + \gamma_i + \delta_t + \epsilon_{it}$$

Uses within-unit variation over time. Removes time-invariant unobservables. Can improve precision when baseline data is available, though McKenzie (2012) shows ANCOVA is often preferred.

### 4.3 Inference

#### 4.3.1 Hypothesis Testing Basics
- **Null hypothesis** $H_0$: $\tau = 0$ (no treatment effect)
- **Alternative** $H_1$: $\tau \neq 0$ (two-sided) or $\tau > 0$ (one-sided)
- **p-value**: Probability of observing a test statistic at least as extreme as the one observed, assuming $H_0$ is true. It is NOT the probability that $H_0$ is true.
- **Type I error** ($\alpha$): Rejecting $H_0$ when it is true (false positive). Controlled by significance level.
- **Type II error** ($\beta$): Failing to reject $H_0$ when it is false (false negative). Related to power: $1 - \beta$.
- **Confidence interval**: Range of values of $\tau$ that would not be rejected at the given significance level. A 95% CI contains the true $\tau$ in 95% of repeated samples (frequentist interpretation).

#### 4.3.2 Clustered Standard Errors
When randomization is at the cluster level (or when errors are correlated within clusters), standard OLS standard errors are biased downward (too small), leading to over-rejection of $H_0$.

**Cluster-robust standard errors**: $V = (X'X)^{-1} \left( \sum_{g=1}^G X_g' \hat{u}_g \hat{u}_g' X_g \right) (X'X)^{-1}$

- Rule of thumb: cluster at the level of treatment assignment
- Small number of clusters ($G < 50$) makes cluster-robust SEs unreliable. Solutions: wild bootstrap (Cameron, Gelbach, Miller, 2008); randomization inference; aggregation to cluster level
- **Cameron & Miller (2015)**: Comprehensive guide to clustering in practice

#### 4.3.3 Randomization Inference (Fisher Exact Test)
Non-parametric alternative to standard inference. Compute the test statistic under all (or many) possible random assignments. The p-value is the fraction of permutations that produce a test statistic as extreme as the observed one.

- Does not rely on large-sample approximations
- Valid with any number of clusters
- Exact test under the sharp null hypothesis ($\tau_i = 0$ for all $i$)
- Computationally intensive for large samples; use Monte Carlo approximation

#### 4.3.4 Multiple Hypothesis Testing
When testing $K$ hypotheses, the probability of at least one false positive under the global null is $1 - (1-\alpha)^K$, which grows quickly.

**Corrections:**
- **Bonferroni**: Set individual threshold to $\alpha / K$. Very conservative; controls Family-Wise Error Rate (FWER).
- **Holm step-down**: Less conservative than Bonferroni; still controls FWER. Order p-values from smallest to largest; compare $k$-th smallest to $\alpha / (K - k + 1)$.
- **Benjamini-Hochberg**: Controls False Discovery Rate (FDR) instead of FWER. More powerful; appropriate when you're willing to tolerate some false positives among rejections.
- **Anderson (2008) sharpened q-values**: Common in development economics. Controls FDR with improved power.
- **Index approach**: Combine related outcomes into a single index (e.g., standardized weighted average) to reduce the number of tests. Kling, Liebman, Katz (2007) use this approach.

**Best practice**: Pre-specify primary outcome(s) in pre-analysis plan. Distinguish between primary (confirmatory) and secondary (exploratory) analyses.

### 4.4 Heterogeneous Treatment Effects

#### 4.4.1 Subgroup Analysis
Interact treatment with baseline characteristics:
$$Y_i = \alpha + \tau D_i + \beta X_i + \gamma (D_i \times X_i) + \epsilon_i$$

$\gamma$ captures differential treatment effect by $X$.

**Warnings**: Subgroup analyses are prone to false discoveries (multiple comparisons). Always pre-specify; report all subgroup tests, not just significant ones; interpret with caution.

#### 4.4.2 Machine Learning for Heterogeneity
- **Causal forests** (Wager & Athey, 2018): Non-parametric estimation of CATE using random forests adapted for causal inference
- **Generic ML approach** (Chernozhukov et al., 2018): Use ML to predict treatment effects, then validate using sample splitting
- **LASSO for covariate selection**: Belloni, Chernozhukov, Hansen (2014) double-LASSO for selecting controls

---

## Part V: Power Analysis

### 5.1 Core Concepts

**Power** = $P(\text{reject } H_0 | H_1 \text{ is true}) = 1 - \beta$

Conventional target: 80% (some use 90% for higher-stakes evaluations).

**Four interrelated quantities** (know any three, solve for the fourth):
1. Sample size ($N$)
2. Effect size ($\delta$, or standardized effect size $\delta / \sigma$)
3. Significance level ($\alpha$, usually 0.05)
4. Power ($1 - \beta$, usually 0.80)

### 5.2 Sample Size Formulas

#### Individual-level randomization, two groups, continuous outcome:
$$N = \frac{(z_{1-\alpha/2} + z_{1-\beta})^2 \cdot 2\sigma^2}{\delta^2} \cdot \frac{1}{P(1-P)}$$

where $P$ is the proportion assigned to treatment (maximized at $P = 0.5$, giving $\frac{1}{P(1-P)} = 4$).

With $\alpha = 0.05$ and power $= 0.80$: $(1.96 + 0.84)^2 = 7.84$

$$N \approx \frac{7.84 \cdot 2\sigma^2}{\delta^2} = \frac{15.68 \sigma^2}{\delta^2}$$

So $N \approx 16 / (\delta/\sigma)^2$ per group, or about 16 times the inverse squared standardized effect size.

For standardized effect size of 0.2 (small): need $\approx 400$ per group
For 0.5 (medium): need $\approx 64$ per group
For 0.8 (large): need $\approx 25$ per group

#### With covariates (ANCOVA):
$$N_{\text{adjusted}} = N \cdot (1 - R^2)$$

where $R^2$ is the proportion of outcome variance explained by covariates. A covariate explaining 30% of variance reduces required sample by 30%. This is "free" power.

#### Binary outcome:
Replace $\sigma^2$ with $p(1-p)$ where $p$ is the average probability. Effect size $\delta$ is in terms of percentage points.

### 5.3 Cluster Randomization Power

$$J = \frac{(z_{1-\alpha/2} + z_{1-\beta})^2 \cdot 2 \cdot \text{DEFF}}{\delta^2 / \sigma^2}$$

where $J$ is the number of clusters needed and $\text{DEFF} = 1 + (m-1)\rho$ is the design effect, $m$ is cluster size, $\rho$ is ICC.

**Effective sample size**: $N_{\text{eff}} = \frac{N}{1 + (m-1)\rho} = \frac{J \cdot m}{1 + (m-1)\rho}$

**Key insight**: When $m$ is large, $N_{\text{eff}} \approx J / \rho$. Adding more individuals per cluster has diminishing returns. The binding constraint is the number of clusters.

**ICC ($\rho$) typical values** (development economics):
- Health outcomes: 0.01–0.05
- Education test scores: 0.10–0.25
- Income/consumption: 0.02–0.10
- Agricultural yields: 0.03–0.15

Even a seemingly small $\rho = 0.05$ with $m = 100$ gives DEFF $= 5.95$, meaning you need about 6 times as many observations as under individual randomization.

**Practical implication**: With a fixed budget, it is almost always better to increase the number of clusters than to increase cluster size.

### 5.4 Minimum Detectable Effect (MDE)

Given sample size and desired power, the smallest effect you can detect:

$$MDE = (z_{1-\alpha/2} + z_{1-\beta}) \cdot \sqrt{\frac{2\sigma^2}{N}}$$

For clustered designs:
$$MDE = (z_{1-\alpha/2} + z_{1-\beta}) \cdot \sigma \sqrt{\frac{2[1 + (m-1)\rho]}{J \cdot m}}$$

After calculating MDE, ask: Is this plausible? If MDE is larger than any reasonable effect, the study is underpowered and should not proceed as designed.

### 5.5 Strategies to Increase Power

1. **Increase number of clusters** (most effective for cluster RCTs)
2. **Add baseline covariates** (ANCOVA reduces residual variance; equivalent to increasing $N$ by factor $1/(1-R^2)$)
3. **Stratified randomization** on variables correlated with outcome
4. **Multiple follow-up rounds** (panel data; within-subject designs)
5. **Use more precise outcome measures** (e.g., administrative data vs. self-reports)
6. **Relax significance level** (from 0.05 to 0.10, though controversial)
7. **One-sided test** (if direction of effect is known ex ante; rarely used in practice)
8. **Reduce number of treatment arms** (concentrate sample on fewer comparisons)
9. **Unequal allocation** (if treatment is cheap but measurement is expensive, over-sample treatment group; optimal ratio depends on relative costs)

### 5.6 Power Calculation in Practice

**When to do it**: Before the experiment, during the design phase. Required for most grant applications and IRB submissions.

**Inputs you need**:
- Expected effect size (from pilot, literature, or theoretical prediction)
- Variance of outcome (from baseline data, pilot, or prior studies)
- ICC (for cluster designs; estimate from baseline data or similar contexts)
- Number of clusters and cluster sizes
- Significance level and desired power
- $R^2$ of covariates (from baseline)

**Tools**: Stata (`power`, `sampsi`, `clustersampsi`), R (`pwr` package, `clusterPower`), Optimal Design software, PowerUp

**Sensitivity analysis**: Always report power for a range of assumptions (vary $\rho$, effect size, $R^2$). Present a power curve showing how power changes with sample size.

---

## Part VI: Implementation

### 6.1 Pre-Analysis Plan (PAP)

A pre-registered document specifying the analysis plan before seeing the data. Filed on registries like AEA RCT Registry or OSF.

**Should include:**
- Primary hypotheses and outcomes
- Secondary / exploratory outcomes (clearly distinguished)
- Sample definition and inclusion/exclusion criteria
- Regression specifications (exact equations, control variables, fixed effects)
- Approach to heterogeneity analysis (which subgroups, pre-specified)
- Correction for multiple testing
- Approach to attrition, missing data, and outliers
- Power calculations

**Why it matters**: Prevents p-hacking, outcome switching, and selective reporting. Increasingly required by top journals. Casey, Glennerster, Miguel (2012) provide a comprehensive discussion.

### 6.2 Baseline Data Collection

- Collect data on key covariates and pre-treatment outcome measures
- Verify sampling frame and eligibility
- Measure ICC for cluster designs
- Collect contact information and tracking identifiers for follow-up
- Timing: must be completed before randomization

### 6.3 Balance Checks

After randomization, verify that treatment and control groups are balanced on baseline covariates.

**Methods:**
- Compare means using t-tests for each covariate
- F-test (joint test) for all covariates simultaneously: regress treatment dummy on all baseline covariates and test joint significance
- Normalized differences (Imbens & Rubin): more informative than t-tests for large samples

**If imbalance is found:**
- Imbalance can occur by chance (with 20 covariates at 5%, expect one "significant" difference)
- Do not re-randomize repeatedly until balanced (this invalidates inference)
- Include imbalanced covariates as controls in regression
- Report balance table transparently

### 6.4 Monitoring and Process Data

- Track compliance rates continuously
- Monitor attrition and implement tracking protocols
- Collect process data (take-up, attendance, dosage) to understand mechanisms
- Document any deviations from protocol

### 6.5 Endline Data Collection

- Blind enumerators to treatment status when possible
- Use same instruments/methods as baseline for comparability
- Collect data on all randomized units (even non-compliers) to enable ITT analysis
- Timing should be specified ex ante; consider short-run vs. long-run effects

---

## Part VII: Analysis

### 7.1 Standard Analysis Pipeline

1. **Balance table**: Compare baseline characteristics across groups (Table 1)
2. **Attrition analysis**: Compare attrition rates; test for differential attrition
3. **First stage** (if applicable): Estimate compliance rate; F-statistic
4. **ITT estimation**: Difference in means or regression, with and without controls
5. **TOT/IV estimation** (if non-compliance): 2SLS using assignment as instrument
6. **Heterogeneity analysis**: Pre-specified subgroups only
7. **Robustness checks**: Alternative specifications, controls, clustering levels, trimming
8. **Multiple testing corrections**: Apply to secondary outcomes

### 7.2 Reporting Standards

Follow CONSORT guidelines adapted for social science (adapted by J-PAL):
- Flow diagram showing enrollment, randomization, follow-up, analysis
- Report all pre-specified outcomes, not just significant ones
- Distinguish between pre-specified and post-hoc analyses
- Report effect sizes and confidence intervals, not just p-values
- Report compliance rates and first-stage results
- Discuss limitations and external validity

### 7.3 Cost-Effectiveness Analysis

- Compare cost per unit of outcome improvement across interventions
- Report incremental cost-effectiveness ratios
- Consider both direct program costs and indirect costs (participant time, opportunity costs)
- Useful for policy comparison even when individual RCTs have different outcome measures
- Dhaliwal et al. (2013) provide a framework for cost-effectiveness in education

---

## Part VIII: Advanced Topics

### 8.1 Mechanisms and Mediation

- **Mediation analysis**: Decompose total effect into direct and indirect channels. $Y = \alpha + \tau D + \gamma M + \epsilon$, where $M$ is a mediator. Standard mediation analysis (Baron & Kenny style) has identification problems because $M$ is endogenous.
- **Experimental manipulation of mediators**: Best approach is to experimentally vary the mediator (design-based mediation). Requires additional randomization.
- **Bounds on direct/indirect effects**: Imai, Keele, Yamamoto (2010) provide sensitivity analysis for mediation under sequential ignorability.

### 8.2 General Equilibrium and Scaling

- RCT estimates are partial equilibrium effects for marginal units
- At scale, treatment may change prices, norms, or institutional responses
- Muralidharan & Niehaus (2017): Framework for thinking about GE effects in development experiments
- Saturation designs: Randomize treatment intensity at market/region level to capture GE effects

### 8.3 Structural Estimation Combined with RCTs

- Use experimental variation to estimate structural parameters (e.g., risk aversion, discount rates, demand elasticities)
- Todd & Wolpin (2006): Use RCT to validate structural model, then use model for counterfactual analysis
- Attanasio, Meghir, Santiago (2012): Combine RCT with structural model in PROGRESA/Oportunidades

### 8.4 Adaptive Experiments

- **Multi-armed bandit**: Allocate more subjects to better-performing treatments over time. Thompson sampling, UCB algorithms.
- **Bayesian adaptive designs**: Update randomization probabilities as data accumulates
- **Platform experiments (A/B testing)**: Tech companies run thousands of simultaneous experiments; different statistical challenges (multiple testing, interference, network effects)
- Kasy & Sautmann (2021): Optimal adaptive experiments for policy

### 8.5 Long-Run Follow-Up

- Track participants years after intervention (e.g., Chetty et al. on STAR experiment; Gertler et al. on early childhood)
- Challenges: attrition grows over time; costs increase; treatment may have sleeper effects or fade-out
- Administrative data linkage (tax records, health records) is increasingly used for long-run tracking

### 8.6 Machine Learning in RCT Analysis

- **Heterogeneous treatment effects**: Causal forests, Bayesian Additive Regression Trees (BART)
- **Covariate selection**: LASSO/post-LASSO for choosing controls (Belloni, Chernozhukov, Hansen)
- **Optimal treatment rules**: Estimate which subgroups benefit most and design targeting rules
- **Prediction for targeting**: Train ML model on experimental data to predict treatment benefit for new individuals
- **Text as data in RCTs**: Use NLP to process qualitative implementation data, open-ended survey responses

### 8.7 Ethics of Randomization

- **Equipoise**: Genuine uncertainty about which arm is better; ethical foundation for randomization
- **Informed consent**: Participants must understand they may be in control group
- **Do no harm**: Control group should not be made worse off than status quo (though being denied a beneficial treatment is a gray area)
- **Fair subject selection**: Randomization itself can be seen as fair allocation when resources are scarce
- **Post-trial access**: Ethical obligation to provide effective treatment to control group after trial
- **Community engagement**: Involve local stakeholders in design and implementation

---

## Part IX: Key Formulas Quick Reference

| Quantity | Formula |
|----------|---------|
| ATE (difference in means) | $\hat{\tau} = \bar{Y}_T - \bar{Y}_C$ |
| SE (difference in means) | $\sqrt{s_T^2/n_T + s_C^2/n_C}$ |
| Wald/LATE estimator | $\hat{\tau}_{IV} = \frac{\bar{Y}_{Z=1} - \bar{Y}_{Z=0}}{\bar{D}_{Z=1} - \bar{D}_{Z=0}}$ |
| Design effect (cluster) | $\text{DEFF} = 1 + (m-1)\rho$ |
| Effective sample size | $N_{\text{eff}} = N / \text{DEFF}$ |
| Sample size (individual) | $N \approx 16\sigma^2 / \delta^2$ per group (at 80% power, $\alpha = 0.05$) |
| MDE | $(z_{1-\alpha/2} + z_{1-\beta}) \cdot \sqrt{2\sigma^2 / N}$ |
| ANCOVA adjustment | $N_{\text{adj}} = N(1 - R^2)$ |

---

## Part X: Essential Reading List

### Core Methodology
- Duflo, Glennerster, Kremer (2007). "Using Randomization in Development Economics Research: A Toolkit." *Handbook of Development Economics*.
- Glennerster & Takavarasha (2013). *Running Randomized Evaluations: A Practical Guide*. Princeton University Press.
- Athey & Imbens (2017). "The Econometrics of Randomized Experiments." *Handbook of Economic Field Experiments*.
- Bruhn & McKenzie (2009). "In Pursuit of Balance: Randomization in Practice in Development Field Experiments." *AEJ: Applied*.

### Design and Power
- McKenzie (2012). "Beyond Baseline and Follow-up: The Case for More T in Experiments." *JDE*.
- Cameron, Gelbach, Miller (2008). "Bootstrap-Based Improvements for Inference with Clustered Errors." *ReStat*.
- Cameron & Miller (2015). "A Practitioner's Guide to Cluster-Robust Inference." *JHR*.

### Inference and Multiple Testing
- Anderson (2008). "Multiple Inference and Gender Differences in the Effects of Early Intervention." *JASA*.
- Kling, Liebman, Katz (2007). "Experimental Analysis of Neighborhood Effects." *Econometrica*.
- Casey, Glennerster, Miguel (2012). "Reshaping Institutions: Evidence on Aid Impacts Using a Pre-Analysis Plan." *QJE*.
- Young (2019). "Channeling Fisher: Randomization Tests and the Statistical Insignificance of Seemingly Significant Experimental Results." *QJE*.

### Non-Compliance and IV
- Angrist, Imbens, Rubin (1996). "Identification of Causal Effects Using Instrumental Variables." *JASA*.
- Lee (2009). "Training, Wages, and Sample Selection: Estimating Sharp Bounds on Treatment Effects." *ReStat*.

### External Validity and Scaling
- Allcott (2015). "Site Selection Bias in Program Evaluation." *QJE*.
- Muralidharan & Niehaus (2017). "Experimentation at Scale." *JEP*.
- Deaton (2010). "Instruments, Randomization, and Learning about Development." *JEL*.

### Machine Learning + RCTs
- Athey & Imbens (2019). "Machine Learning Methods That Economists Should Know About." *ARE*.
- Wager & Athey (2018). "Estimation and Inference of Heterogeneous Treatment Effects Using Random Forests." *JASA*.
- Chernozhukov et al. (2018). "Generic Machine Learning Inference on Heterogeneous Treatment Effects." *NBER WP*.

### Landmark RCTs in Development Economics
- Miguel & Kremer (2004). "Worms: Identifying Impacts on Education and Health in the Presence of Treatment Externalities." *Econometrica*. [Spillovers, deworming]
- Banerjee et al. (2015). "A Multifaceted Program Causes Lasting Progress for the Very Poor." *Science*. [Graduation approach]
- Duflo, Hanna, Ryan (2012). "Incentives Work: Getting Teachers to Come to School." *AER*. [Monitoring + incentives]
- Kremer, Miguel, Thornton (2009). "Incentives to Learn." *ReStat*. [Merit scholarships]
- Dupas (2011). "Health Behavior in Developing Countries." *ARE*. [Health product pricing]
- Olken (2007). "Monitoring Corruption." *JPE*. [Audits vs. participation; governance]
- Blattman & Annan (2010). "The Consequences of Child Soldiering." *ReStat*. [Encouragement design]

---

*Last updated: March 2026. This guide is a living document; update as you encounter new material.*
