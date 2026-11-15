# Provenance of this snapshot

This repository is a cleaned copy of the team's private sprint repository, built from two source commits:

| Source | Commit | Date (UTC) | Used for |
|---|---|---|---|
| Sprint `main` branch | `fd6faeb6` | 2026-10-25 22:01 | Analysis steps, report, research notes |
| Study branch | `ff1b3743` | 2026-10-25 20:12 | The harness, scenario packs and raw data that ran the study |

**Timing:**
- Both commits are before the sprint deadline (2026-10-25 23:59 AoE = 2026-10-26 11:59 UTC).
- The source history has no commits before 2026-10-23.
- The data snapshot cutoff is 2026-10-25T16:42:07Z (`results/2026-10-25-pressure-study/SNAPSHOT.json`).
- The snapshot itself was assembled after the sprint.

**How it was built:** a deterministic script (kept in the private repository, because it necessarily contains the strings it redacts):
1. extracted the included paths byte-for-byte
2. applied the identity redactions to every text file
3. applied the wording edits to the named files below
4. regenerated the HTML copies of edited Markdown files
5. recorded every changed file

## Included

- **From the study branch:**
  - `lying_harness/` (except `pressure_publish.py`), `tests/`, `tools/` (except `verify_results_layout.py`), `configs/`, `requirements.txt`, `.gitignore`
  - scenario packs 09–12, `prompts/build_extended_pack.py` and `prompts/archive/`
  - `results/2026-10-25-pressure-study/`
  - `analysis/2026-10-25-pressure-study/reasoning-audit/`
- **From `main`:**
  - `analysis/2026-10-25-pressure-study/steps/`, except `step3/`
  - `research/01`, `02`, `04` and `05`
  - `final_report/REPORT.*`

**Added for this snapshot:** the submitted paper, `final_report/what-moves-two-ai-agents-to-bend-the-rules-together.pdf`, downloaded unchanged from the project's [Apart Research page](https://apartresearch.com/sprints/projects/what-moves-two-ai-agents-to-bend-the-rules-together-g64a); `README.md` (replaces the sprint README), `PROVENANCE.md`, `CONTRIBUTORS.md`, `MODIFIED_FILES.csv`, `SHA256SUMS.public.txt`, `.gitattributes` and `.env.example`. The `.env.example` replaces the sprint one, which named the team's endpoint and a deployment-specific model id; it now holds generic placeholders.

## Left out

**From both sources:**
- **Earlier pilot and partial runs** (`results/` on `main`, `results/archive/`): they aren't evidence for the paper, and they used other providers and older harness versions.
- **Superseded scenario packs and inputs:** `prompts/07`, `prompts/08`, `criteria.json`, `scenario.txt`, `stages.json`.
- **Earlier and archived analyses:** `analysis/archive/`, `analysis/qwen-pressure-final-*`, `analysis/README.*` and the study-branch analysis summary.
- **Planning notes:**
  - paper-planning and competition-planning notes: `analysis/.../steps/step3/`, `research/03`, `research/06`–`10`
  - the analysis-pipeline design: `analysis-pipeline/`
- **Team tooling and hand-off material:** the team's prompt library and starter prompts, session hand-off notes (`RESUME_CLAUDE_SESSION.*`, `next-steps/`), and the results-layout notes and verifier (`docs/`, `tools/verify_results_layout.py`). These describe the private repository and can't run here.
- **The publishing helper** (`lying_harness/pressure_publish.py`), which pushes to a private branch.
- **Top-level documents:** the sprint's top-level `README`/`RESULTS` and `results/README`, which describe the private repository.
- **The sprint's report template PDF** (third-party).
- **Git history**, which holds personal account details. Authorship is summarised in `CONTRIBUTORS.md`.

## Changes made to included files

Every changed file is listed in [`MODIFIED_FILES.csv`](MODIFIED_FILES.csv) with its original SHA-256 (the committed blob), its published SHA-256 and the reason. All other files that came from the source commits are byte-identical to them. [`SHA256SUMS.public.txt`](SHA256SUMS.public.txt) covers every published file.

**1. Identity redactions** (applied to every text file, including run outputs):

| Original | Replaced with |
|---|---|
| Modal endpoint URLs containing personal workspace names | `https://modal-workspace-a--…redacted.modal.run` (not a routable address) (the endpoint labelled Modal-A in the analysis) or `modal-workspace-b--…` (Modal-B) |
| Bare workspace names | `modal-workspace-a` / `modal-workspace-b` |
| A Modal account id in error messages | `ac-REDACTED` |
| Absolute local paths | repo-relative paths, or `<local-path>` |

These are the only changes to run outputs (`results/…/runs/`) and to data tables such as `steps/step1/trial_metrics.csv`. Model text, numbers, outcomes and actions are unchanged.

**About the two endpoints:** the run was served by two Modal deployments of the same model, on two Modal workspaces. Collection moved from A to B, then back to A when B reached its spending limit. `endpoint_history.json` logs these switches; the analysis treats the endpoint as a time block.

**2. Wording edits** in named files only:
- `final_report/REPORT.md`:
  - repository links and branch references now point to this repository
  - empty affiliation placeholders removed
  - "pre-registered" changed to "pre-specified", since §3.4 states the plan was written after an interim look at 71% of the data
  - a reference to an excluded file replaced
  - the not-included ablation pack marked as such
  - the LLM Usage Statement now also names Cursor
- `lying_harness/report.py`: an internal task assignment reworded.
- `steps/step2/REPORT.md` and `steps/step2/analyze_full.py`: an audience line that mentioned the competition judges reworded.
- `research/04`: three phrases addressed to the judges reworded.
- `reasoning-audit/README.md` and `reasoning-audit/ANALYSIS_PLAN.md`: a section on positioning for the competition removed.

**Integrity note:** the original checksum and migration files inside `results/2026-10-25-pressure-study/` (`SHA256SUMS.json`, `MIGRATION_SOURCE_MANIFEST.json`, `SNAPSHOT.json`) describe the *pre-redaction* bytes. Files changed by the redaction therefore won't match them. Use `MODIFIED_FILES.csv` and `SHA256SUMS.public.txt` to verify the published bytes.

**References to absent files:** some documents cite files that exist only in the private repository, such as `analysis-pipeline/PROCESS.md`, `steps/step3/` and earlier result folders. Those references are informational.
