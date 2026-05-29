[![DOI](https://img.shields.io/badge/DOI-10.82901%2Fnemar.on004745-blue)](https://doi.org/10.82901/nemar.on004745)

Dataset consists of 6 participants who performed SSVEP tasks. We designed stimulations at 3 different frequencies (2 Hz, 4 Hz, 8 Hz). Each participant attended to 3 trials for each frequency in which they remained static as much as possible to avoid artifacts. They attended to 3 trials for each frequency in which they made voluntary head/neck and eye movements. Please refer to Kumaravel et al., (IEEE EMBC 2021) for further details.

## NEMAR curation changes (2026-05-21, revised 2026-05-27)

The BIDS validator went from 6 errors + 115 warnings to 0 errors + 79 warnings. None of the raw `.set`/`.fdt` files were modified — every change is to a text sidecar.

**Events sidecar (`task-unnamed_events.json`)**
- The `value` column's `Levels` keys were written as `x1`, `x2`, ... `x10`, but every `_events.tsv` lists the matching marker codes as bare numbers (`1`, `2`, ... `10`). The `x` prefix was dropped so the keys line up with the actual data. The text descriptions for each level were left untouched.
- The `value` column also carried `"Units": " "` (a single space). Marker codes don't have a physical unit, so the field was removed rather than left as whitespace.
- The `onset`, `duration`, and `response_time` columns listed units as `"second"`; the BIDS-canonical symbol is `s`, so they were updated.

**Recording task sidecar (`task-unnamed_eeg.json`, new at the dataset root)**
- A shared `_eeg.json` was added at the root so the same information doesn't have to be repeated in every per-subject sidecar (BIDS applies a root sidecar to every matching recording). It records a `TaskDescription` paraphrased from this README (SSVEP at 2/4/8 Hz, three artifact-free trials and three movement trials per frequency).
- It also sets `MISCChannelCount: 0` and `TriggerChannelCount: 0`, which matches every per-subject `channels.tsv` (eight EEG rows, no other channel types).

**Channel tables (`sub-NNN/eeg/sub-NNN_task-unnamed_channels.tsv`, six files)**
- The `type` column was `n/a` for all eight rows in each file. The matching `.set` files were opened in MNE and confirmed to contain eight EEG-typed channels, and each per-recording `_eeg.json` already declares `EEGChannelCount: 8`, so the column was set to `EEG`.
- The `units` column was also `n/a`. The same MNE inspection showed sample values in the 1e-4 V range (roughly 100 µV, standard scalp-EEG scale), so the column was set to `uV`.
- The channel `name` column (`1` through `8`) was left as-is. The device (`BioWolf + IDUN Technologies`, per the existing per-recording sidecars) does not have a documented electrode-name mapping in this dataset, so inventing one without source confirmation is not defensible.

**Dataset description (`dataset_description.json`)**
- Added `DatasetType: "raw"` so the dataset is validated as raw data rather than a derivative.
- Updated `BIDSVersion` from `1.2` to `1.11.1` (the version the current validator checks against).
- `GeneratedBy` was left absent, exactly as the source published it — nothing was added there.

**Remaining warnings (79) — left on purpose**
- These are all "recommended but missing" fields that need information from the study, lab, or equipment that isn't in the dataset (for example: manufacturer, model name, software version, device serial, cap model, head circumference, ground electrode, hardware filters, participant instructions, cognitive-atlas IDs, stimulus-presentation software, and the dataset-level `GeneratedBy` provenance). They were left blank rather than filled with guesses.
