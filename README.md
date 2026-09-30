# JEV candidate enclave

Separate confidential candidate deployment for the learned r8 parser pair and verifier v4. This repository is independent of VitaDAO/open-jev-tinfoil so candidate attestation releases cannot change production clients that resolve the latest release.

Production open-jev and Vita remain unchanged. No automatic updates, debug access, outbound network, request logs or data persistence. Only synthetic acceptance requests are used for deployment verification.

Source: VitaDAO/open-jev-tinfoil, branch codex/jev-tinfoil-candidate-20260930. Runtime model code is the locally tested candidate; the entrypoint maps a dedicated candidate secret into the existing API contract. Models and file manifest are pinned.

Selector SHA256: 56f4a035c6742951ac86eddbd5f66214b6942bb99c85ecb5227b6b382ee39c78. Learned model/threshold SHA256: 185a18e02c4b8a3490d263358d12adeaf92fce446bdb249ef57525dbdc61cad4. v2 wire contract, acquisition only; unsupported requests preserve the native fallback.

Vita must not be switched to this candidate until explicitly instructed by the owner.

Candidate source commit: `c481b9448cd7d610b840cf25eef63b04b1fb5e82`.

Learned models derive from answerdotai/ModernBERT-base (Apache-2.0). Generic /decide and /route preserve the pinned com-kotobalabs/open-jev-deberta-v3-large backbone and its recorded FP16 storage derivation. No model retraining, quantization or cutoff change is part of this deployment.

Linux portability fixes register the serving UID and restore Mac-tagged head tensors explicitly on CPU. Exact statuses and acquisition queries match the original local build on all 300 readable parity/safety cases. No weights or acceptance thresholds changed.

Resource allocation: **4 CPUs and 8 GiB CVM memory**, reduced at owner request. Model files are split into separate Docker layers to reduce transient unpacking pressure; their pinned bytes, thresholds and selector identity are unchanged. The original 64 GiB candidate was stopped. Platform startup and live attested acceptance must pass before claiming this allocation is ready.

Linux build and acceptance: https://github.com/VitaDAO/open-jev-tinfoil/actions/runs/36731266178 (2928 passed, 8 skipped; 300 serving parity/safety checks passed under an 8 GiB container limit).
