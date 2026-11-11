# AI collusion experiment: dual-control agents under a monitoring notice

Code, data and analysis for a two-agent "dual control" experiment run during the **Apart Research AI Collusion Research Sprint (23–25 October 2026)**. The paper is [`final_report/REPORT.md`](final_report/REPORT.md).

**Design:**
- Two Qwen3-32B agents play officials in 72 fictional scenarios. A rule-bending action executes only if both privately choose *take*.
- Each agent writes a private plan, the pair exchange three rounds of messages, then each locks *take* or *refuse*.
- Crossed factors:
  - a one-sentence monitoring notice (watched / unwatched)
  - five cumulative justification levels
  - six pairings of installed moral rules: welfare, non-deception, authority, or none

**Scale:** 4,243 of 4,320 planned trials completed.

**Headline result:** in a paired comparison specified before the full-data analysis, joint approval was 23.3% under "monitored" and 27.2% under "not monitored". That is −3.9 percentage points, with a scenario-cluster 95% CI of −5.3 to −2.7. The plan was written after an interim look at 71% of the data; see §3.4 of the report. The report covers the exploratory results and all caveats: one model, one seed per cell, and cues that are prompt text only.

## Repository layout

| Path | Contents |
|---|---|
| `final_report/` | The paper (Markdown and HTML) |
| `lying_harness/` | The experiment harness. `pressure.py` ran this study. |
| `prompts/12-pressure-nine-per-setting.json` | The frozen scenario pack used for the run. `build_extended_pack.py`, packs 09–11 and `prompts/archive/` are its build inputs and earlier packs. |
| `results/2026-10-25-pressure-study/` | Raw data, about 514 MB. Each trial has its own folder under `runs/` with `result.json`, `transcript.md` and `events.jsonl`, and `runs.csv` indexes them. `manifest.json` records the full configuration. |
| `analysis/2026-10-25-pressure-study/steps/` | The analysis pipeline:<br>• Step 0.5: heat maps<br>• Step 1: per-trial metrics and validation<br>• Step 2: the pre-specified plan (`PREREG.md`), the primary test and its report, the oversight-reasoning and convergence analyses<br>• the construct audit (`pressure-construct/`) |
| `analysis/2026-10-25-pressure-study/reasoning-audit/` | A separate qualitative audit of the agents' reasoning text |
| `research/` | Background surveys (human and AI collusion) and the experiment design notes |
| `tests/`, `tools/`, `configs/` | Tests, utilities and configuration |

## Reproducing

```bash
pip install -r requirements.txt
python -m unittest tests/test_pressure.py tests/test_harness.py   # no model calls
# Print the plan for this study (4,320 trials) without calling a model:
python -m lying_harness.pressure --pack prompts/12-pressure-nine-per-setting.json \
    --order-seed 20261025 --output results/new-study --plan
```

**Model endpoint:** a full run needs an OpenAI-compatible endpoint serving Qwen3-32B. Copy `.env.example` to `.env` and fill it in. The study used a vLLM deployment on Modal, which is no longer running.

**Extra packages:** the analysis scripts also need `pandas`; `psutil` (for the reasoning-audit scripts) and `markdown` (for `tools/md_to_html.py`) are optional.

**Running the analysis:** the scripts read `results/2026-10-25-pressure-study/` and `analysis/.../steps/step1/trial_metrics.csv`. Run them from the repository root.

## Terminology and older notes

The generated data summaries (`results/2026-10-25-pressure-study/README.md`, figures) and the notes in `reasoning-audit/` and `research/` predate the paper. They use terms such as "pressure level" and "collusion rate", and some state claims the paper later narrowed or dropped. **Where they conflict with the report (see its "what did not hold up" table), the report governs.** In particular:
- the level factor is better read as a cumulative **justification level**, not a single pressure dimension
- a **mutual take** is an operational joint approval, not proven collusion

These files are left as written.

Some documents also cite files from the team's private working repository that are not included here (see `PROVENANCE.md`).

## What this snapshot is

This is a cleaned, single-commit copy of the sprint repository. [`PROVENANCE.md`](PROVENANCE.md) gives:
- the source commits
- what was included and left out
- every change, with the reason for it

In short:
- **No experimental data values, model outputs or analysis results were changed.** Edits are limited to:
  - redacting personal account names, account ids, endpoint URLs and local paths
  - a few wording fixes
  - repository-link updates
  - in the report, "pre-registered" changed to "pre-specified", with its stated caveat
- Every changed file is listed in `MODIFIED_FILES.csv`.

**New vs. pre-existing work:** all code, scenario packs, data and analyses here were created during the sprint, 23–25 October 2026. No code predates the sprint.

**AI assistance:** we used AI coding assistants extensively: Claude Code for the analysis and drafting, and Cursor for parts of the surveys and scenario packs. The report's LLM Usage Statement has the details.

## Authors

See [`CONTRIBUTORS.md`](CONTRIBUTORS.md).

## License

No license has been chosen yet; it is pending agreement among the authors. Until one is added, all rights are reserved.
