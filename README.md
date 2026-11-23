# Embergard Pluggable Models

Official on-device GGUF model mirrors for **Embergard** — the private, on-device AI companion.

This repository distributes quantized model binaries via **GitHub Releases**. The Embergard Android app downloads them directly, verifies integrity with SHA3-256, and runs inference entirely on-device.

**R21 / R22 compliance**

- No Ollama. These are native GGUF files consumed by the bundled `ember-streaming` Rust engine.
- No forced Gemma. Models are pluggable and user-selectable; the bundled tokenizer currently matches the BPE family best (Qwen).

## Release status

Release `v1.0.0-pluggable` has been created. Asset uploads for `qwen2.5-1.5b-q4.gguf` and `tinyllama-1.1b-q4.gguf` are pending because:

1. Direct uploads from the local build host are blocked by the OpenClaw egress proxy (`uploads.github.com` returns `403` through the CONNECT tunnel).
2. A GitHub Actions mirror workflow exists (`.github/workflows/mirror-models.yml`) but the `Aethel-Guard` org currently cannot run Actions due to a billing/spending-limit flag.

To complete the mirror from an unblocked network, download the two files from HuggingFace and upload them to the release:

```bash
# From a machine without the OpenClaw proxy:
curl -L -o qwen2.5-1.5b-q4.gguf \
  "https://huggingface.co/Qwen/Qwen2.5-1.5B-Instruct-GGUF/resolve/main/qwen2.5-1.5b-instruct-q4_k_m.gguf"
curl -L -o tinyllama-1.1b-q4.gguf \
  "https://huggingface.co/TinyLlama/TinyLlama-1.1B-Chat-v1.0-GGUF/resolve/main/tinyllama-1.1b-chat-v1.0.Q4_K_M.gguf"

# Upload to the existing release
gh release upload v1.0.0-pluggable qwen2.5-1.5b-q4.gguf tinyllama-1.1b-q4.gguf \
  --clobber --repo Aethel-Guard/embergard-models
```

Verify the SHA3-256 hashes above after download.

## Available release assets

| Model | Filename | Size | Quant | Tokenizer family | License | Commercial use |
|---|---|---|---|---|---|---|
| Qwen 2.5 1.5B Instruct | `qwen2.5-1.5b-q4.gguf` | ~1.1 GB | Q4_K_M | BPE ✅ engine-matched | Apache-2.0 | Yes, with attribution |
| TinyLlama 1.1B Chat | `tinyllama-1.1b-q4.gguf` | ~638 MB | Q4_K_M | SentencePiece | Apache-2.0 | Yes, with attribution |
| Gemma 2 2B Instruct | `gemma-2-2b-q4.gguf` | ~1.2 GB | Q4_K_M | SentencePiece | Gemma Terms of Use | Yes, subject to Google terms |
| Llama 3.2 3B Instruct | `llama-3.2-3b-q4.gguf` | ~2.0 GB | Q4_K_M | SentencePiece | Llama 3.2 Community License | Yes if < 700M MAU |
| Phi 3.5 Mini | `phi-3.5-mini-q4.gguf` | ~2.0 GB | Q4_K_M | SentencePiece | MIT | Yes |
| Nemotron 3 Nano | `nemotron-3-nano-q4.gguf` | ~1.3 GB | Q4_K_M | SentencePiece | TBD | TBD |

> **Engine compatibility note:** The bundled `ember-streaming` engine currently ships a Qwen/BPE tokenizer. SentencePiece-family models (TinyLlama, Gemma, Llama, Phi, Nemotron) will load but may decode into gibberish until per-model tokenizer support lands. Qwen is the recommended first download.

## Release checksums

Integrity is verified with SHA3-256. Checksums are published here and pinned in the app.

| Filename | SHA3-256 |
|---|---|
| `qwen2.5-1.5b-q4.gguf` | `sha3-256:4984ab13737302f76ed665ac7cab9f42875c80eec4519eebc814c6535e875e17` |
| `tinyllama-1.1b-q4.gguf` | `sha3-256:27dd93103d21600b967d9f8fac8e87f31ff30ed2c613a6fee1207ef3bce6b69f` |

## Download protocol

Embergard uses this priority:

1. **Primary mirror**: GitHub Releases (`https://github.com/Aethel-Guard/embergard-models/releases/download/...`)
2. **Fallback**: upstream HuggingFace / original model source

All downloads are HTTPS-only and include:

- SHA3-256 integrity check
- GGUF magic-byte validation
- Minimum-size check

## License notices

### Apache-2.0 models (Qwen 2.5, TinyLlama)

Licensed under the Apache License, Version 2.0. You may obtain a copy at https://www.apache.org/licenses/LICENSE-2.0

### Gemma 2

Subject to the Google Gemma Terms of Use. See https://ai.google.dev/gemma/terms

### Llama 3.2

Subject to the Meta Llama 3.2 Community License Agreement. Commercial use permitted for products/services with fewer than 700 million monthly active users; larger deployments require a separate license from Meta.

### Phi 3.5

Licensed under the MIT License. Copyright (c) Microsoft Corporation.

## P1 / P7 compliance

- All model data stays on-device.
- No model content leaves the device.
- No PII in training data.
- No cloud inference by default.
- Downloads use HTTPS only.
