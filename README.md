# Ari2018 benchmark family (TDVRPTW and TDVRP, Arigliano-generator TD travel times, curated VRP variant)

Benchmark family repository of [MAMUT-routing](https://github.com/ANR-MAMUT/MAMUT-routing) (ANR MAMUT, ANR-22-CE22-0016), mounted there as `benchmarks/Ari2018`. It holds the 2 problem variants of the family, TDVRPTW (time windows kept) and TDVRP (time windows removed). Self-contained: instances, canonical arrival-time-function (ATF) sidecars and best-known solutions ship together.

## What this family is, and what it is not

The raw data is the classic **TD-TSPTW** benchmark of the Arigliano et al. lineage: a single uncapacitated vehicle, no demands, no fleet size, no service times. Per instance it gives a symmetric distance matrix, integer time windows, and IGP time-dependent travel speeds (73 speed zones, 3 speed profiles shared by all arcs; congestion depth Delta ∈ {70, 80, 90, 95, 98}, traffic patterns A/B, TW-width parameter β·100 ∈ {0, 25, 50, 100}, 3 TW classes A/B/C, 10 instances each; sizes 15/20/30/40 customers). The result is a 4800-instance full factorial.

This canonical family is the **VRP variant curated by Onyr (2024–2026)**. Demands, vehicle capacities, fleet sizes and gaussian service times were generated once by a former generation pipeline and are pinned here verbatim. The generation was unseeded, so the values recorded per instance in `metadata.legacy_attribute_source` are the authoritative definition. The TDVRPTW variant keeps the raw Arigliano time windows verbatim. The TDVRP variant removes them (TDVRP twin). The family is therefore **not** comparable with published TD-TSPTW results on the underlying raw files.

## Curated subset (deterministic)

The family ships **160 instances** per variant, not the full 4800. It keeps one instance per structural cell `(n, Delta, pattern, width)`, chosen by a rule that is a pure function of the raw tree and a former heuristic BKS store. Candidates holding a best-known solution in that store win first, and remaining ties are decided by ascending SHA-256 of the canonical instance name (e.g. `sha256("Ari-C3-pA-d98-w50")`). Instance names `Ari-<Class><id>-p<Pattern>-d<Delta>-w<Width>` under `n=<customers>` carry all structural parameters. The TDVRPTW and TDVRP variants hold the same 160 instance names. The full factorial raw trees remain available upstream for anyone needing the complement.

## Layout

- `TDVRPTW/n=<customers>/` and `TDVRP/n=<customers>/` hold the instances (`<Name>.vrp.json`) and their best-known solutions (`<Name>.bks.Duration.json`) of each variant, for example `TDVRPTW/n=15/Ari-A10-pB-d70-w50.vrp.json` and `TDVRP/n=15/Ari-A10-pB-d70-w50.bks.Duration.json`.
- `sidecars/n=<customers>/` holds the ATF sidecars, stored once and shared by the 2 variants, for example `sidecars/n=15/Ari-A10-pB-d70-w50.atf.json.gz`. Each instance references its sidecar by a path relative to the repository root, with the SHA-256 of the sidecar content.
- `mamut-collection.json` at the root marks the repository as a family collection (`"layout_version": 2`).
- `history/` holds the commit maps of the former repositories (see the history section below).

## Format

- `<Name>.vrp.json`: the instance in the MAMUT shape, with depot 0, mandatory display `coordinates` (deterministic stress-layout embedding of the symmetric raw distance matrix, quality in `metadata.coordinates_method`), `horizon` `[0, depot due date]`, and a `td` block with the sidecar path and the sidecar's storage-independent sha256.
- `<Name>.atf.json.gz`: the canonical ground truth, one arrival-time NDCPWLF per arc, exact IGP consolidation (breakpoints exactly where departure or arrival crosses a speed-zone boundary, each re-evaluated by the exact forward Ichoua loop, the last zone's speed extended beyond the horizon). All sidecars are gzipped in this repository.
- `<Name>.bks.Duration.json`: best known solution under the `Duration` objective. Costs are always the canonical checker's output.

Population pipeline: `populate_td_ari_vu` v1 (curation tooling maintained outside this repository, links will be published with the benchmark paper).

## Library compatibility

This layout is read by `mamut-routing-lib` 0.13 and later, which is in development and not released yet. `mamut-routing-lib` 0.12 and earlier read the former layout, which stays available read-only in the archived former repositories [MAMUT-routing-TDVRP-Ari2018](https://github.com/ANR-MAMUT/MAMUT-routing-TDVRP-Ari2018) and [MAMUT-routing-TDVRPTW-Ari2018](https://github.com/ANR-MAMUT/MAMUT-routing-TDVRPTW-Ari2018).

## Provenance and licensing

Underlying raw data: the Arigliano et al. TD-TSPTW benchmark generator distribution. MAMUT-authored curation artifacts are distributed under the MIT license (see [LICENSE](LICENSE)). This notice does not relicense the underlying third-party benchmark definitions.

## Citation and benchmark authorship

The authors, in citation order, are Adrien Pichon, Florian Rascoussier, Romain Billot, Lina Fahed, Alexandru-Liviu Olteanu, Marc Sevaux and Christine Solnon. The 2 lead authors are ordered alphabetically by surname, followed by the scientific supervisors in alphabetical order. Their ORCID identifiers are recorded in [CITATION.cff](CITATION.cff).

The listed authors contribute the TDVRPTW and TDVRP adaptations of the Arigliano et al. TD-TSPTW distribution: generated demands, capacities, fleet sizes and service times, deterministic selection of 160 instances across the source parameters, and construction and validation of the published arrival-time-function representation. The TDVRPTW variant retains the original time windows. The TDVRP variant removes them to support comparison with the corresponding TDVRPTW instances. Cite this repository for these benchmark-design, generation and selection contributions, and cite the Arigliano et al. TD-TSPTW distribution for the original travel-time data and generator. The pinned legacy attribute files document the generated values used in the adaptation. Individual solution-contributor credits are recorded with the solutions. Record the exact Git commit used in experiments.

## History of this repository

Until November 2026 the 2 variants lived in 2 repositories, `MAMUT-routing-TDVRPTW-Ari2018` and `MAMUT-routing-TDVRP-Ari2018`. Their histories are imported here. Each history was rewritten under the directory of its problem type (`TDVRPTW/` or `TDVRP/`), and a merge commit joins the 2. The rewrite gave every former commit a new identifier. `history/` holds 1 map per former repository, `history/commit-map-MAMUT-routing-TDVRPTW-Ari2018.tsv` and `history/commit-map-MAMUT-routing-TDVRP-Ari2018.tsv`. Each map is tab-separated, with the former commit identifier and the new one on each line, after a header line. See [CHANGELOG.md](CHANGELOG.md) for the reorganisation.
