# xcanv-models

Model files that the xcanv desktop app downloads **only when the user chooses to** (Settings → AI). Nothing here is bundled in the installer.

## judge-pack-v1: Laya typed-decisions, ONNX

The on-device "judgement" model: typed questions (a choice, a 0-1 score, or yes/no) answered locally, so no text leaves the user's machine.

| File | What |
|---|---|
| `judge-w8.onnx` (600 MB) | weight-only 8-bit (MatMulNBits), opset 18: **the default, highest fidelity** |
| `judge-w4.onnx` (416 MB) | weight-only 4-bit: smaller and faster |
| `tokenizer.json`, `tokenizer_config.json` | ModernBERT tokenizer |
| `rl_agent_config.json`, `encoder-config.json` | head layout, temperatures, encoder config |
| `SHA256SUMS` | `sha256sum -c SHA256SUMS`; the app refuses any file whose digest differs |

Parity with the PyTorch reference (1,200 public items: 600 AG News, 600 SMS spam, CPU):

| | AG News accuracy | SMS spam AUC | median s/call |
|---|---|---|---|
| PyTorch reference (laya 0.3.7) | 93.3 % | 0.996 | 1.07 |
| judge-w8.onnx | 93.5 % | 0.996 | 1.85 |
| judge-w4.onnx | 93.5 % | 0.994 | 0.57 |

## Licence and attribution

Apache License 2.0 (see `LICENSE`). Derived from:

- the Laya typed-decisions checkpoint, `convaiinnovations/laya` (Apache-2.0);
- the ModernBERT-large encoder, `answerdotai/ModernBERT-large` (Apache-2.0).

Changes: exported to ONNX (opset 18) and quantized weight-only to 8 and 4 bits. No retraining.
