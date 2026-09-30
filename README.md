# JEV candidate enclave

Separate confidential candidate deployment for the learned r8 parser pair and verifier v4. This repository is independent of VitaDAO/open-jev-tinfoil so candidate attestation releases cannot change production clients that resolve the latest release.

Production open-jev and Vita remain unchanged. No automatic updates, debug access, outbound network, request logs or data persistence. Only synthetic acceptance requests are used for deployment verification.

Source: VitaDAO/open-jev-tinfoil, branch codex/jev-tinfoil-candidate-20260930. Runtime model code is the locally tested candidate; the entrypoint maps a dedicated candidate secret into the existing API contract. Models and file manifest are pinned.

Selector SHA256: 56f4a035c6742951ac86eddbd5f66214b6942bb99c85ecb5227b6b382ee39c78. Learned model/threshold SHA256: 185a18e02c4b8a3490d263358d12adeaf92fce446bdb249ef57525dbdc61cad4. v2 wire contract, acquisition only; unsupported requests preserve the native fallback.

Vita must not be switched to this candidate until explicitly instructed by the owner.

Candidate source commit: `e895c80913a94aa53ac2e4936d17af5e3527f473`.

Learned models derive from answerdotai/ModernBERT-base (Apache-2.0). Generic /decide and /route preserve the pinned com-kotobalabs/open-jev-deberta-v3-large backbone and its recorded FP16 storage derivation. No model retraining, quantization or cutoff change is part of this deployment.

Linux portability fixes register the serving UID and restore Mac-tagged head tensors explicitly on CPU. Exact statuses and acquisition queries match the original local build on all 300 readable parity/safety cases. No weights or acceptance thresholds changed.

Resource allocation: 4 CPUs and 64 GiB CVM memory. Linux process acceptance runs under an 8 GiB limit, with measured 2.7 GiB peak. Compressed plus unpacked image storage is about 5.1 GiB, exceeding the fixed 4 GiB private image disk used by CVM 0.14.7 below its 32 GiB detected-RAM threshold. The larger allocation addresses image storage without changing model weights.

Linux build and acceptance: https://github.com/VitaDAO/open-jev-tinfoil/actions/runs/36724090406 (2928 passed, 8 skipped; 300 serving parity/safety checks passed).

Tinfoil control-plane sizes are discrete. A 40 GiB configuration was rejected before any container was created. 64 GiB is the supported size safely above the CVM 32 GiB detected-RAM threshold; a nominal 32 GiB guest can lose usable RAM to boot reservations and retain the 4 GiB fallback disk.
