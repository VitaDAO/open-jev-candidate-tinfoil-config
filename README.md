# JEV candidate enclave

Separate confidential deployment for the learned r8 parser pair and verifier v4. Production open-jev and Vita remain unchanged. Do not switch Vita or production without the owner's explicit command.

The attestation config repository is independent of VitaDAO/open-jev-tinfoil so candidate releases cannot change production clients that resolve the original repository's Latest release. Auto-updates and debug/SSH access are disabled. Application outbound network is closed; request logs and data persistence are disabled.

## Resource allocation and model integrity

The candidate is configured for **4 CPUs, 8 GiB RAM and no GPU**. The learned model files are on a separate dm-verity verified read-only model disk. The runtime image retains the original /opt/learned paths through a mount symlink and verifies the original SHA256 manifest and every listed model file before startup. File bytes, thresholds and inference code are unchanged. No retraining or additional quantization was performed.

The Docker image's compressed-plus-unpacked storage estimate is 3.02 GiB, below the CVM's 4 GiB private image disk limit. This replaces the initial oversized 64 GiB deployment, which was stopped. Release v0.1.5 is ready on 8 GiB. Live pinned attestation and 300/300 synthetic endpoint parity checks passed, including 90 acute safety handoffs; authentication, schema rejection and bounded concurrency checks passed on 2026-10-01 local.

Validated runtime source: `40ed2611bc8f477cce0a3e64628c2a0e4d719a5f` on `codex/jev-tinfoil-candidate-20260930` in VitaDAO/open-jev-tinfoil. Image: `ghcr.io/vitadao/open-jev-tinfoil@sha256:3541c5ca189f227eed456f7f8a9872ab8f7b2369a82b76b8a4b0345f7133ea18`.

Linux checks: https://github.com/VitaDAO/open-jev-tinfoil/actions/runs/36751136808 — 2940 passed, 8 skipped; 300 serving parity/safety cases passed under an 8 GiB container limit. The generic /decide and /route model and contract are retained.

Model source: `alexdobrin/open-jev-r8-v4-candidate-20260930@0f3a747adc8b497ea46b503f968ba1aef1a8e6bc`. It contains only the 25 exact model/config/tokenizer files, their original manifest, source license and provenance. These same model files were already published as the model-only GitHub build asset. No training datasets, sealed exams, private requests, health records or credentials were published. ModernBERT base provenance: answerdotai/ModernBERT-base, Apache-2.0. Verified model-pack root: `b3dd8bf34a654341c6cce57ae9cec79e64d05a9e0c049e092a7862c9057f87bd` (schema 1).

Selector SHA256: `56f4a035c6742951ac86eddbd5f66214b6942bb99c85ecb5227b6b382ee39c78`. Learned weights/threshold SHA256: `185a18e02c4b8a3490d263358d12adeaf92fce446bdb249ef57525dbdc61cad4`. Original model manifest SHA256: `da75040b34439f604e4c812690ae91f1ae0453ef902c54f51a98c73d70ecb97d`.

The v2 acquisition contract remains unchanged. Unsupported requests retain native fallback. Endpoint parity checks are synthetic serving checks, not a fresh blind accuracy estimate or Vita final-answer acceptance.

## Admission queue

One running inference and up to 16 FIFO waiters, with a 500 ms queue deadline. Overflow or timeout returns 429 with Retry-After: 1. Queued disconnects/cancellations are removed; cancellation of active work retains the slot until its thread finishes. The HTTP connection ceiling is 64. Live pinned checks: 300/300 serving parity; 12/12 correct at each of 1, 2, 4 and 8 offered requests/sec in a short synthetic probe. An 18-request simultaneous burst returned 5 correct and 13 busy responses. This is not a sustained capacity guarantee. Vita and production remain unchanged.
