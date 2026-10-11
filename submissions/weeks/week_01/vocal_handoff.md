HANDOFF
turns: 7
learns by: definition, example, analogy
domain: point processes | level: 3 | milestone: reached | anchor: none
point process | claim: no | prior: none | end: deployed | "detect whether or not there is a pattern or correlation"
event time | claim: no | prior: correct | end: deployed | "find the event time for the sunspot group's emergence"
counting process | claim: no | prior: none | end: introduced | no quote
inter-arrival time | claim: no | prior: none | end: introduced | no quote
Poisson process | claim: no | prior: none | end: deployed | "emergences are a Poisson proccess"
independent increments | claim: no | prior: none | end: unused | no quote
memoryless | claim: no | prior: none | end: deployed | "determining whether the events are memoryless or not"
Fano factor | claim: no | prior: none | end: deployed | "calculating the Fano factor to see if we can rule out"
overdispersion | claim: no | prior: none | end: deployed | "see if we can rule out overdispersion"
domain: statistical inference | level: 2 | milestone: none | anchor: negative log-likelihood
negative log-likelihood | claim: no | prior: correct | end: deployed | "large negative log likelihood where our data varies a lot"
Earth Mover's Distance | claim: no | prior: correct | end: deployed | "arth mover's distance can take into consideration more other variables"


# VOCAL HANDOFF — Week 1 Completed

## 1. Context & Task Goals
* **Objective**: Determine whether sunspot emergence is consistent with a memoryless (Poisson) process, and identify the relevant timescales.
* **Analysis Set**: Selected sunspot groups from Cycles 12 to 23 across both hemispheres (24 hemicycles total), keeping only groups with peak corrected area > 30 MSH.
* **Event Convention**: Shifted from ButterflAI 1.0's "peak-area" time to the **first observation** as the event time to avoid systematic delays for larger groups. Total analysis set size: **22,122 events** across **253.6 total observed hemicycle-years** (mean rate of ~87.2 events/year).

## 2. Quantitative Scores & Findings
* **Task 1 — Per-Event NLL (Latitudes under 1.0 Model)**:
  * Mean finite NLL: **3.2024 nats/event**.
  * Zero-density (impossible) events: **110 events (0.50%)** occurred while the 1.0 model's hard latitude gate was shut ($

* **Task 2 — Earth Mover's Distance (EMD)**:
  * EMD ranges from **0.25° to 0.89°** of latitude across the active windows of $s$, providing a bounded, robust physical metric that handles "impossible" anomalies gracefully.

* **Task 6 — Inter-Arrival Times**:
  * Median gap: **2.01 days**; mean gap: **4.19 days**.
  * Exactly zero gaps: **3,633 gaps (16.4%)**, showing strong quantization and simultaneous detections in the daily catalog.

* **Tasks 7–9 — Fano Factor & Rate-Matched Null Models**:
  * At 1-day windows: $F_{\rm data} \approx 1.17$ versus $F_{\rm null} \approx 1.11$ (1-year null), revealing a minor but robust **~5% overdispersion** at the smallest physical timescale.
  * This small-scale clumping is physically anchored in simultaneous group emergences (the 16.4% zero-day gaps).
  * At wider timescales (days to years), $F(\Delta)$ scales up to ~70. However, this is largely dominated by the varying solar cycle envelope. The choice of smoothing bandwidth ($3$-month vs $1$-year) drastically alters the null comparison, showing that $F(\Delta)$ cannot cleanly decouple rate-variation from intrinsic memory at long timescales.

## 3. Key Arguments & Physics Realizations
* **Uniform drawing across the cycle** (without looping daily) would completely smear out the slow-varying solar cycle envelope, leading back to a flat, homogeneous Poisson process ($F(\Delta) \approx 1$). Doing a daily loop maintains the cycle's envelope while removing sub-day memory.
* **The bandwidth bottleneck** is why we must shift to Week 2's methodology: explicitly *fitting* a parametric rate model via maximum likelihood on the event times, followed by **time-rescaling** to map the arrivals to an Exp(1) Poisson process. This eliminates free bandwidth choices and shifts our metric to nats per hemicycle-year, enabling us to score *when* things emerge rather than just *where*.

## 4. Next Steps
* Proceed to **Week 2** to build and fit parametric intensity rate models using maximum likelihood and conduct time-rescaling tests.
null model | claim: no | prior: correct | end: deployed | "Null model is like the hypothesis we assume"
domain: solar cycle physics | level: 2 | milestone: none | anchor: none
emergence | claim: no | prior: correct | end: deployed | "event times of sunspot emergences are a Poisson proccess"
hemispheric cycle | claim: no | prior: none | end: introduced | no quote
alignment coordinate | claim: no | prior: none | end: unused | no quote
other deployed: none
other open: point process, counting process, inter-arrival time, hemispheric cycle
