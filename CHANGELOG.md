# Changelog: Ari2018 benchmark family

All notable changes to the Ari2018 family are recorded here, the most recent first. The entry "2026-11: family repository" records the creation of this repository. The sections after it hold the changelogs of the 2 former repositories. They keep every entry, fact, date, count and commit reference, with the wording edited for style and the headings moved 2 levels down.

## 2026-11-25: optimality claims withdrawn from edition r1

- The 40 solver-reported optimality claims (`metadata.optimality`) of this family's best-known solutions are withdrawn, all 40 in TDVRPTW. They targeted the historical `td-fold/1` evaluation of earlier Kayros versions, not the `td-binary64-exact-v1` profile of edition `r1`. New claims will be derived with Kayros 2.0 under `td-binary64-exact-v1`.
- The 40 content-addressed evidence files under `.mamut/history/` that only these claims referenced are removed. The archive of the original pre-migration best-known solutions, indexed by `.mamut/archives/r1-original-bks.json`, is unchanged.
- No route, cost, route witness, manifest or instance file changed.

## 2026-11: family repository

- The TDVRPTW and TDVRP variants of Ari2018, kept until now in the 2 repositories `MAMUT-routing-TDVRPTW-Ari2018` and `MAMUT-routing-TDVRP-Ari2018`, form 1 repository, `MAMUT-routing-Ari2018`, mounted in MAMUT-routing at `benchmarks/Ari2018`. The histories of both former repositories are imported, each under the directory of its problem type, and joined by a merge commit. `history/` holds the maps from the former to the new commit identifiers.
- Instances and best-known solutions keep their former path after the problem type, for example `TDVRP/n=15/Ari-A10-pB-d70-w50.vrp.json`.
- The ATF sidecars move to `sidecars/`, stored once for the 2 variants: 160 sidecars kept, 160 duplicate copies removed. In 320 instances the sidecar path field now holds a path relative to the repository root, for example `sidecars/n=15/Ari-A10-pB-d70-w50.atf.json.gz`. No sidecar content changed, and the sidecar SHA-256 recorded in each instance is unchanged.
- The SHA-256 of the 320 instance files changed because of the rewritten path field. No instance identifier changed.
- 2 metadata strings of 1 BKS file, `TDVRP/n=30/Ari-A3-pB-d95-w0.bks.Duration.json`, named the former path of its TDVRPTW twin. They now name the new path. No other best-known solution changed.
- `mamut-collection.json` at the root marks the repository as a family collection (`"layout_version": 2`).
- This layout is read by `mamut-routing-lib` 0.13 and later, which is in development and not released yet. `mamut-routing-lib` 0.12 and earlier read the former layout, which stays available read-only in the 2 archived former repositories.
- The README, LICENSE, CHANGELOG and CITATION.cff files of the 2 variants are replaced by 1 file each at the root, and a NOTICE file is added.

## Former TDVRPTW repository

### Changelog of the Ari2018 TDVRPTW BKS

All notable changes to the curated `Ari2018` TDVRPTW best-known solutions (BKS) are recorded here. The objective is **Duration** (duration minimization), and the depot departure time of each route is a decision variable. Costs are the authoritative output of the canonical checker (`mamut_routing_lib.td.check_td_solution`): exact IEEE-754 double arithmetic, no epsilon thresholds, routes in canonical order (sorted by first customer), total summed in that order. Any strict improvement is therefore real. Reminder: this family is the VRP variant curated by Onyr (see README.md), so these BKS are not comparable with published TD-TSPTW results on the underlying raw files.

#### 2026-09-24

**Re-priced under the `td-fold/2` checker contract (mamut-routing-lib 0.12.0), no route changed.** mamut-routing-lib 0.12.0 replaces the TD checker's route fold (checker contract `td-fold/1` -> `td-fold/2`). Waiting and service at a vertex are now applied exactly to the accumulated arrival times instead of being composed through a ratio interpolation, the departure window restricts the first arc without interpolation, and travel on slope-one pieces is computed by addition. Under `td-fold/1` a ready time could be off by an ulp, and on stepwise travel-time functions such an ulp could land past a step and read its upper branch. All 160 BKS were re-priced with `mamut-routing bks reprice-td`: 92 files were rewritten, 31 of them because the cost moved (by at most 4.1e-16 relative, float rounding only), the other 61 only to refresh `route_durations` / `route_departure_times` by ulps. Each BKS whose cost moved records `metadata.repriced` (previous cost, checker, contract, date). 7 optimality-stamped BKS moved (by at most 9.1e-13): their `proven_optimum` now equals the re-priced cost, and a `note` records that the proof was obtained under `td-fold/1`, that `dual_bound` is the prover's value under that arithmetic, and that re-certification under `td-fold/2` is pending.

#### 2026-08-06

**All 40 optimality certificates re-derived from scratch and re-stamped** under the 4-solve agreement protocol (cold and warm starts crossed with the 2 labeling modes, an audited exact-pricing phase in every run, 0 checker-infeasible priced columns) on Grid'5000, on the repaired kayros 1.5.1 build. The campaign was motivated by the 2026-08-05 withdrawal of 1 certificate in the Vu2020 TDVRPTW family and the subsequent finding that the certifying builds carried since-repaired pricing defects (documented in the kayros 1.5.1 release notes and CHANGELOG). Every re-issued stamp certifies bit-exactly the previously stored value. No solution data changed, and the stamps' provenance now cites this campaign and build. 1 instance (Ari-B2-pB-d98-w100, n=15) exceeded the campaign's standard memory watermark and was re-derived in a dedicated high-memory run (peak resident set near 16.5 GB), certifying bit-exactly like the rest.

#### 2026-07-12

**8 further BKS stamped proven optimal, 2 stale stamps retracted.** The weekend top-up of the exact re-certification campaign (2026-07-11/12, Grid'5000; time limit 1800 s per run) certified Ari-B2-pB-d98-w100, Ari-C3-pA-d98-w50, Ari-C10-pB-d95-w50 (n=15); Ari-B10-pA-d90-w100 (n=20); Ari-B2-pB-d98-w100, Ari-B5-pB-d80-w100, Ari-C4-pB-d70-w100, Ari-C5-pB-d95-w100 (n=30). Protocol unchanged from the 2026-07-10/11 campaign: 4 independent exact solves (cold and warm starts crossed with the 2 labeling modes) agreeing on the value, an audited exact-pricing phase in every run, 0 checker-infeasible priced columns, and canonical-checker re-validation at stamping time. Separately, the last 2 stamps surviving from the superseded 2026-07-07 campaign (Ari-B10-pA-d90-w50 n=15, Ari-C10-pA-d98-w50 n=20, pre-audit prover metadata) are retracted: the pricing-ladder audit voided their trust basis and the audited campaigns leave both instances open, so their values return to ordinary best-known status. This family now carries 40 certified TDVRPTW BKS, all from the audited protocol.

#### 2026-07-11

**32 BKS stamped proven optimal** (`metadata.optimality`) under a re-certification of the whole family: the earlier certificates' producer carried a pricing-ladder termination defect (an exact-pricing phase could be skipped), so all stamps were regenerated from scratch under a stronger protocol. Each stamp certifies 4 independent exact solves (cold and warm starts x 2 labeling modes) agreeing on the value, an audited exact-pricing phase in every run, 0 checker-infeasible priced columns, and canonical-checker re-validation at stamping time. Instances whose runs disagreed, timed out, or priced a checker-infeasible column carry NO stamp.

#### 2026-07-08

59 of 160 BKS improved (mean -0.54%, largest single improvement -2.52%) by a 20,808-run anytime-strategy head-to-head campaign on Grid'5000: kayros 0.4.0.dev0 (TD-ILS, TD-ACO+LS, and an ACO-then-ILS budget split, all over the granular time-dependent local search), per-size time limits (120 s for n<=30, 300 s for n<=60, 600 s for n<=100), seeds {42, 123, 456}, single-threaded runs. Improve-only fold: for each instance the campaign-best solution was re-priced by the canonical checker before writing (checker cost authoritative). Stored BKS marked proven optimal were left untouched.

#### 2026-07-07

8 BKS improved by exact solves and **proven optimal** by kayros 0.3.0 lera branch-price-and-cut (HiGHS backend, warm-started from the previous BKS, TL 600 s), from the certification campaign over all families n≤50 (improvements −0.05% to −3.0% across n=15..40). Certificates: optimal under checker-exact route costs and standard LP/pricing tolerances, completeness modulo Lera epsilon dominance. The same campaign certified 30 of the other stored TDVRPTW BKS of this family optimal as stored.

Structured optimality metadata: all 38 BKS of this family proven optimal by that campaign now carry a machine-readable `metadata.optimality` object with the prover, certificate wording, proven optimum, dual bound and wall time (schema `OptimalityMetadata`, mamut-routing-lib ≥ 0.4.0).

#### 2026-07-06 (local-search sweep)

152 BKS improved by the first sweep of kayros 0.2.0.dev0 TD-ACO with time-dependent local search (tree-evaluated VND, every accepted move repriced by the checker-identical fold), 10 seeds per instance on Grid'5000.

#### 2026-07-06 (initial seeding sweep)

155 BKS added and 1 improved, reaching full 160/160 coverage, from the initial large-scale seeding sweep across all 4 TD families run on 2026-07-04 (kayros 0.0.1 TD-ACO, Grid'5000, 10 seeds per instance, 13 520 runs total).

#### 2026-07-03

Family populated (160 curated instances + ATF sidecars) with 5 BKS re-validated and re-priced from a former heuristic BKS store (2024–2026).

## Former TDVRP repository

### Changelog of the Ari2018 TDVRP BKS

All notable changes to the curated `Ari2018` TDVRP best-known solutions (BKS) are recorded here. The objective is **Duration** (duration minimization), and the depot departure time of each route is a decision variable. Costs are the authoritative output of the canonical checker (`mamut_routing_lib.td.check_td_solution`): exact IEEE-754 double arithmetic, no epsilon thresholds, routes in canonical order (sorted by first customer), total summed in that order. Any strict improvement is therefore real. Reminder: this family is the VRP variant curated by Onyr (see README.md), so these BKS are not comparable with published TD-TSPTW results on the underlying raw files.

#### 2026-09-24

**Re-priced under the `td-fold/2` checker contract (mamut-routing-lib 0.12.0), no route changed.** mamut-routing-lib 0.12.0 replaces the TD checker's route fold (checker contract `td-fold/1` -> `td-fold/2`). Waiting and service at a vertex are now applied exactly to the accumulated arrival times instead of being composed through a ratio interpolation, the departure window restricts the first arc without interpolation, and travel on slope-one pieces is computed by addition. Under `td-fold/1` a ready time could be off by an ulp, and on stepwise travel-time functions such an ulp could land past a step and read its upper branch. All 160 BKS were re-priced with `mamut-routing bks reprice-td`: 29 files were rewritten, 21 of them because the cost moved (by at most 5.7e-16 relative, float rounding only), the other 8 only to refresh `route_durations` / `route_departure_times` by ulps. Each BKS whose cost moved records `metadata.repriced` (previous cost, checker, contract, date).

**1 BKS improved by relaxation-twin cross-evaluation.** Every TDVRPTW route set is a feasible TDVRP solution of no larger duration (the TDVRP twin drops the time windows, and the arrival-time functions are FIFO), so the TDVRPTW BKS of the twin family were offered to this family through the improve-only store (`save_td_solution_as_bks_if_improved`, checker re-pricing authoritative). 1 was strictly better: n=30 Ari-A3-pB-d95-w0, 833.2900000000006 -> 829.900000000001 (-0.41 %), with 5 routes as before. The new record keeps the twin's authors and carries `method: twin-cross-evaluation`, a `derived_from` block naming the TDVRPTW instance, objective, cost and date, and `campaign: 2026-09 relaxation-twin cross-evaluation`. The other 159 twins were kept.

#### 2026-07-08

58 of 160 BKS improved (mean -0.17%, largest single improvement -0.97%) by a 20,808-run anytime-strategy head-to-head campaign on Grid'5000: kayros 0.4.0.dev0 (TD-ILS, TD-ACO+LS, and an ACO-then-ILS budget split, all over the granular time-dependent local search), per-size time limits (120 s for n<=30, 300 s for n<=60, 600 s for n<=100), seeds {42, 123, 456}, single-threaded runs. Improve-only fold: for each instance the campaign-best solution was re-priced by the canonical checker before writing (checker cost authoritative). Stored BKS marked proven optimal were left untouched.

#### 2026-07-06 (local-search sweep)

All 160 BKS improved by the first sweep of kayros 0.2.0.dev0 TD-ACO with time-dependent local search (tree-evaluated VND, every accepted move repriced by the checker-identical fold), 10 seeds per instance on Grid'5000.

#### 2026-07-06 (initial seeding sweep)

Initial BKS population, 160/160: the TDVRP variant had no earlier heuristic solutions, so all BKS come from the initial large-scale seeding sweep across all 4 TD families run on 2026-07-04 (kayros 0.0.1 TD-ACO, Grid'5000, 10 seeds per instance, 13 520 runs total).

#### 2026-07-03

Family populated (160 curated instances + ATF sidecars), no BKS yet.
