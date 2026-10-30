# DASE7506 MP2 - Group-conditional Router Checkpoint

**Protocol:** `dase7506-mp2-open-hard-verifier-v5`

## 1. Overview

This repository contains the frozen inference assets for the
Group-conditional Router developed for DASE7506 MP2.

The matching source code is provided separately in the
DASE7506-MP2-Code repository.

The checkpoint supports inference without retraining.

## 2. Repository Contents

| File | Description |
|---|---|
| `embedding-model.npz` | Embedding model parameters |
| `pairwise-model.npz` | Pairwise model parameters |
| `listwise-model.npz` | Listwise model parameters |
| `text-enhanced-meta.npz` | Text-enhanced stacking meta-model |
| `router_config.json` | Frozen routing policy and parameters |
| `checkpoint_manifest.json` | Asset sizes and SHA256 hashes |

## 3. Frozen Routing Policy

The Group-conditional Router uses the following policy:

- Vote groups 1 and 2: Text-enhanced Stacking with parseability fallback.
- Vote groups 3 and 4: Rule D with parseability fallback.

Rule D parameters:

- Vote weight: `1.5`
- Listwise weight: `0.15`
- Margin threshold: `0.391068`

The fallback selects the highest-scoring parseable candidate
from the same decision branch if the original selection is unparseable.

If no candidate is parseable, the original selection is preserved.

## 4. Evaluation Results

| Evaluation | Correct | Total | Accuracy |
|---|---:|---:|---:|
| Nested OOF | 4346 | 4755 | 91.399% |
| Validation | 745 | 798 | 93.358% |
| Test | 380 | 467 | 81.370% |

The independent official scorer determines the reproduced result.

## 5. Checkpoint Size

Total uncompressed inference assets:

- 80,256 bytes
- 0.076538 MiB

Course limit: 16 MiB.

Individual file sizes and SHA256 hashes are provided in
`checkpoint_manifest.json`.

## 6. How to Use

1. Download or clone this checkpoint repository.
2. Download or clone the matching Code repository.
3. Install the dependencies listed in the Code repository's
   `requirements.txt`.
4. Pass this checkpoint directory to the official prediction runner.

Run the following command from the Code repository's `code/` directory.
Replace `PATH_TO_CHECKPOINT_FOLDER` with the extracted directory path.

    python predict.py --method hybrid_group_router.py --checkpoint "PATH_TO_CHECKPOINT_FOLDER" --split test --output results/test-router-predictions.json

Then run the independent official scorer:

    python score_predictions.py --split test --predictions results/test-router-predictions.json --output results/test-router-score.json

No retraining is required for inference.

## 7. Compatibility

Use this checkpoint with the matching `hybrid_group_router.py`
implementation from the Code repository.

Keep all four NPZ files together. Do not rename or modify them.

## 8. Attribution and AI Assistance

AI coding assistance was used in experiment planning, code drafting,
debugging and result interpretation.

The implementation and results were tested using the supplied
official evaluation interface.
