# KWS Model Checkpoints: TC-ResNet & BC-ResNet

Trained keyword spotting models on [Speech Commands V2](https://arxiv.org/abs/1804.03209) (12 labels).

## Model Sources

The model architectures are from [Google Research kws_streaming](https://github.com/google-research/google-research/tree/master/kws_streaming):

| Model | Paper | Code |
|-------|-------|------|
| TC-ResNet | [Temporal Convolution for Real-time Keyword Spotting on Mobile Devices](https://arxiv.org/abs/1904.03814) | [`models/tc_resnet.py`](models/tc_resnet.py) |
| BC-ResNet | [Broadcasted Residual Learning for Efficient Keyword Spotting](https://arxiv.org/abs/2106.04140) | [`models/bc_resnet.py`](models/bc_resnet.py) |

> **Note:** The model code files in `models/` are copied from the [Google Research](https://github.com/google-research/google-research) repository (Apache 2.0 License).

## Dataset

[Speech Commands V2](https://storage.googleapis.com/download.tensorflow.org/data/speech_commands_v0.02.tar.gz) — 105,000 utterances, 35 words, 1-second clips at 16kHz.

**12-label setup:** `yes, no, up, down, left, right, on, off, stop, go` + `silence` + `unknown`

| Split | Samples |
|-------|---------|
| Training | ~84,000 |
| Validation | ~4,400 |
| Testing | ~4,800 |

## Results

| Model | Params | MACs | Size | Val Acc | Test Acc | Streaming |
|-------|--------|------|------|---------|----------|-----------|
| **BC-ResNet-2** | 36K | 1.4M | 0.5MB | 97.50% | **97.33%** | ✅ |
| **TC-ResNet-14** | 305K | 6.7M | 3.5MB | 97.45% | 97.17% | ❌ |
| TC-ResNet-Paper | 367K | 13.5M | 2.8MB | 97.55% | 97.04% | ❌ |
| TC-ResNet-31k | 32K | 0.7M | 0.4MB | 96.36% | 96.08% | ❌ |
| BC-ResNet-1 | 14K | 1.1M | 0.3MB | 96.32% | 96.02% | ✅ |

## Checkpoint Structure

```
checkpoints/
├── tc_resnet_14/        # TC-ResNet-14 (best overall)
├── tc_resnet_paper/     # TC-ResNet paper config
├── tc_resnet_31k/       # TC-ResNet lightweight (31K params)
├── bc_resnet_1/         # BC-ResNet-1 (14K params, streamable)
└── bc_resnet_2/         # BC-ResNet-2 (36K params, best accuracy)
```

Each checkpoint directory contains:

| File | Description |
|------|-------------|
| `best_weights.*` | Best validation accuracy weights |
| `last_weights.*` | Final training step weights |
| `flags.json` | Full training configuration |
| `accuracy_last.txt` | Final test set accuracy |
| `model_summary.txt` | Model architecture summary |
| `labels.txt` | Label list |
| `non_stream/` | TensorFlow SavedModel for inference |
