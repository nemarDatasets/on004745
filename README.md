[![DOI](https://img.shields.io/badge/DOI-10.82901%2Fnemar.on004745-blue)](https://doi.org/10.82901/nemar.on004745)

Dataset consists of 6 participants who performed SSVEP tasks. We designed stimulations at 3 different frequencies (2 Hz, 4 Hz, 8 Hz). Each participant attended to 3 trials for each frequency in which they remained static as much as possible to avoid artifacts. They attended to 3 trials for each frequency in which they made voluntary head/neck and eye movements. Please refer to Kumaravel et al., (IEEE EMBC 2021) for further details.

## NEMAR curation changes (2026-05-21)

BIDS validator: 6 errors + 115 warnings → 0 errors + 78 warnings. Raw `.set`/`.fdt` binary payloads unchanged.

### `dataset_description.json`
- Added `DatasetType: "raw"`.
- Added `GeneratedBy: [{Name: "nemar-cli", Version: "0.8.8", CodeURL: "https://github.com/nemar-org/nemar-cli"}]`.

### `task-unnamed_events.json`
- `onset`, `duration`, `response_time` columns: `"Units": "second"` → `"Units": "s"` (BIDS-canonical SI symbol).
- `value` column `Levels` keys: dropped the `x` prefix (`"x1" → "1"`, ..., `"x10" → "10"`) so they match the bare numeric strings in every `_events.tsv`'s `value` column. Closes the 6× `TSV_VALUE_INCORRECT_TYPE` errors. Descriptions unchanged.
- `value` column: removed the invalid `"Units": " "` (whitespace-only). Marker codes don't have a unit.

### `task-unnamed_eeg.json` (new, inheriting root sidecar)
- `TaskDescription`: paraphrased verbatim from this README (SSVEP at 2/4/8 Hz, 3 artifact-free + 3 artifact trials per frequency).
- `MISCChannelCount: 0`, `TriggerChannelCount: 0` (mechanical — every per-subject `channels.tsv` has 8 EEG rows and 0 other types).

### `sub-NNN/eeg/sub-NNN_task-unnamed_channels.tsv` (6 files)
- `type` column: `n/a` → `EEG` for all 8 rows in each file. Verified by inspecting `sub-001_task-unnamed_eeg.set` with MNE — 8 channels, all `eeg` type. Per-recording `_eeg.json` already declares `EEGChannelCount: 8`.
- `units` column: `n/a` → `uV` for all 8 rows. Verified via signal-range inspection on the same `.set` file (samples in the 1e-4 V range = ~100 µV, standard scalp EEG scale). Closes the 6× `EEG_CHANNEL_COUNT_MISMATCH` warnings.
- Channel `name` column (`1`..`8`) left as-is. The device (`BioWolf + IDUN Technologies`, per existing per-recording sidecars) does not have a documented electrode-name mapping in this dataset, and inventing one without source confirmation is not defensible.