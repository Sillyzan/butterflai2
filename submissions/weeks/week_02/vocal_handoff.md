# VOCAL HANDOFF — Week 2 (02_rate_1d)

## 1. Context & Task Goals
* **Objective**: Fit the sunspot emergence rate λ(s) of each hemicycle as an inhomogeneous Poisson
  process by maximum likelihood, verify the fitter, test goodness of fit with time-rescaling, and
  determine which fitted quantities the data actually constrains.
* **Development set**: cycles 12, 13, 14, 15, 17, 18, 19, 21, 23 → 18 hemicycles, 16,611 events.
  Cycles 16, 20, 22 sealed (never fitted, tuned, plotted or scored).
* **Fixed program-wide**: observation window s ∈ [−6, +11] yr; 4001-node quadrature mesh (1.55-day
  spacing); scores in nats per hemicycle-year.

## 2. Code Status
| Task | Status | Notes |
|---|---|---|
| 10 log_likelihood | Done | Matches analytic constant-rate log L to ~1e-12 |
| 11 unpack / gamma_rate | Done | Logistic onset map verified (q3 = 0 → halfway) |
| 12 fit_hemicycle | Done | 6 starts, Nelder–Mead twice per start |
| 13 simulate_by_thinning | Done | **Bug to fix:** bound check must be `np.any(acceptance_prob > 1.0)` |
| 14 parameter recovery | Done | 20 seeded replicates of 19N's fitted rate |
| 15 fit_without_integral_term | Done | Objective = −Σ log λ only; penalty on non-finite values |
| 16 rescaled_gaps | Done (student-written, guided) | Jitter, compensator on mesh, interp, diff |
| 17 ks_vs_exp1 | Written, final fixes pending | Fix `n_events`→`n`, `k` vs `steps` name mismatch, missing `#` |
| 18 profile_onset | **Code supplied by tutor** at student's request (time constraint) | Student to review line by line |
| 19 plausibility | **Code supplied by tutor** at student's request (time constraint) | Student to review line by line |

## 3. Quantitative Results (in sample)
* Constant-rate baseline: −164.57 nats/hemicycle-year (ĉ = n / T).
* Gamma rate: −203.87 nats/hemicycle-year → improvement 39.30 (~0.72 nats/event).
* Count identity holds in all 18 fits: |∫λ − n| ≤ 1.8e-3 events.
* Quadrature refinement: halving ratio 4.00 (second order); mesh error ~2e-6 events.
* Recovery (19N truth, 20 replicates): largest |bias|/std = 0.77 (k: bias −0.47, std 0.61).
* Dropping the integral term: scale a runs to float64 overflow; expected count non-finite.
* Wrong integration window (start at −3): fit stays finite but peak shifts >1 yr, count +~30%.
* Tasks 16–19 outputs: **to be confirmed after full run.** Notebook expects: jittered gamma passes
  KS in 17/18 hemicycles (pooled KS ≈ 0.015); ties-kept version rejected everywhere; constant rate
  rejected; onset pinned at −6 in 11/18 fits; peak correlation ≈ +0.7; count–amplitude r ≈ +0.94;
  width–amplitude r ≈ −0.3.

## 4. Dialogue Coverage by Directive
* **Section 5 (verification)** — partially completed.
  * Pt 1 (no integral term): student identified `a` increasing without bound via the n·log a term;
    with prompting, explained the stop: overflow → inf → penalty → Nelder–Mead shrinks and halts.
    Corrected misconception: numpy overflow returns inf with a warning, not an error.
  * Pt 2 (what "recovered" means): student reached bias/std as the criterion; initially inverted the
    hypothetical ratio (0.47/0.02) and corrected it. Final rule stated with tutor refinement
    (within ~2–3 std). Spread correctly attributed to Poisson randomness across replicates.
  * Pts 3–5 (which parameters recover well; verification vs model testing; mesh refinement) —
    **not done.** Pt 3 question was posed but not answered.
* **Section 6 (time-rescaling)** — Pt 1 worked by the student; Pts 2–4 **answered by the tutor** at the
  student's request (time constraint), not derived by the student.
  * Student correctly computed rescaled gap = λ × (1/λ) = 1 in both busy and quiet regions.
  * Corrected misconceptions: Λ is deterministic (N(s) is random); rescaled gaps are Exp(1)-distributed,
    not equal; real times and Λ values cannot be subtracted (different units).
* **Section 2 and Section 7** — **not covered in this session.**

## 5. Vocabulary Deployment
* Used in own words: maximum likelihood estimate (via objective), thinning (via rng explanation),
  compensator (partially), time-rescaling theorem (stated conditional claim with prompting), bias.
* Introduced but not yet deployed independently: Kolmogorov–Smirnov statistic, CDF, QQ plot,
  quadrature, identifiability, profile likelihood, emergence rate.

## 6. Open Items for Next Session
1. Review supplied code for Tasks 18 and 19 line by line until it can be explained unaided.
2. Run notebook top to bottom; compare Tasks 16–19 outputs with expectations above.
3. Finish Section 5 Pts 3–5 (commit a prediction for Pt 3 before reviewing Section 7 results).
4. Work Section 7 directive (onset at window edge, varies vs. poorly determined, derived quantities).
5. Section 6 Pts 2–4: restate in own words what the constant-rate QQ departure means, the jitter
   assumption, and what a passing KS test cannot establish.
