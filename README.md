[![DOI](https://img.shields.io/badge/DOI-10.82901%2Fnemar.on004854-blue)](https://doi.org/10.82901/nemar.on004854)

TX18 dataset

This is a placeholder dataset.

## NEMAR curation changes (2026-05-21, revised 2026-05-27)

The BIDS validator went from 1 error + 47 warnings to 0 errors + 37 warnings. None of the raw `.set`/`.fdt` signal payloads were modified; every change is to a text sidecar.

**Dataset description (`dataset_description.json`)**
- Added `DatasetType: "raw"` so the dataset is validated as raw data rather than a derivative.
- Updated `BIDSVersion` from an older value to `1.11.1` (the version the current validator checks against).
- `ReferencesAndLinks` was `[""]`; the empty-string element was removed (an empty string is not a valid reference, and an empty list is the correct way to say "no references").
- `GeneratedBy` was left absent, exactly as the source published it; nothing was added there.

**Channel table (`sub-001/eeg/sub-001_task-nback_channels.tsv`)**
- All 64 rows had `type=n/a` and `units=n/a`. These are EEG channels recorded in microvolts, so the type was set to `EEG` and the units to `uV`. Channel names were not changed.

**Recording sidecar (`sub-001/eeg/sub-001_task-nback_eeg.json`)**
- Added `MISCChannelCount: 0` and `TriggerChannelCount: 0` so the channel-count fields are explicit rather than missing.
- Added `EEGPlacementScheme: "10-10"` because the channel names are standard 10-10 labels.
- No other keys were touched.

**Events dictionary (`task-nback_events.json`)**
- Added a top-level definition for the `sample` column, which was present in the event tables but undocumented.
- Dropped the `value.HED` entries for event codes `"1"` and `"307"` (105 of the original 107 entries are preserved). These two codes only ever appear in `events.tsv` at the same onset as another code that is already tagged `"Task"`, and the BIDS-HED validator merges HED strings across rows that share an onset. With both codes tagged `"Task"`, the merged string became `"Task,Task"` and the validator legitimately flagged a duplicate tag. Dropping just these two HED entries removes the duplicate-tag collisions without affecting any standalone event. `value.Levels` was left fully intact (all 107 entries preserved), so the human-readable labels are unchanged.
- This is the shared STRONG-cohort fix: on004849, on004850, on004852, on004853, on004854, and on004855 all carry the byte-identical defect (197 colliding-onset pairs per `sub-001_task-nback_events.tsv`), and the same two-entry HED drop clears it across the cohort.

**Remaining warnings (37) — left on purpose**
- These are all "recommended but missing" fields that would require information from the study, lab, or equipment that isn't in the dataset, such as manufacturer, manufacturer's model name, software versions, device serial number, task description, instructions, cognitive-atlas IDs, institution name and address, cap manufacturer and model, ground electrode, head circumference, hardware filters, subject-artefact description, and stimulus-presentation details. One HED sidecar-key-missing warning for value `[1]` is also left in place, since it is the validator-visible side of the deliberate HED entry drop described above. `GeneratedBy` at the dataset level is also among these recommended-but-missing fields and was intentionally left absent to match what the source published. All of these were left blank rather than filled with guesses.
