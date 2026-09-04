# Embergard Edge Models

On-device GGUF model binaries for Embergard — the secure AI companion.

**R21/R22: Ember models only. No Ollama. No Gemma.**

## Release Assets

Model binaries are distributed via GitHub Releases. Each release includes:

- GGUF model file (Q4_K_M quantized)
- SHA-256 checksum
- Ed25519 signature (production releases)

## Available Models

| Model | Filename | Size | Quant | Context | Device Class |
|-------|----------|------|-------|---------|--------------|
| Ember Edge | ember-fallback.gguf | ~1.9GB | Q4_K_M | 2048-8192 | All (auto-selected) |

## Download Protocol

Embergard app downloads follow this priority:

1. **Primary CDN**: `https://embergard.app/models/`
2. **Fallback mirror**: GitHub Releases (this repo)

All downloads use HTTPS with SHA-256 integrity verification.

## P1 Compliance

All model data stays on-device. No model content leaves the device. No PII in training data. No cloud inference.
