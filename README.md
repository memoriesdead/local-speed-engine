---
license: other
license_name: backend-specific-licenses
license_link: LICENSES.md
tags:
- local-inference
- inference-engine
- cuda
- optimization
- source-code
---

# Local Speed Engine

Weight-preserving acceleration patches and local batch tools for compatible LLM
and video backends. The engine code changes execution, caching and data movement;
it does not train, edit, convert, replace or redistribute model weights.

This is a source release. Run your own compatible models on your own hardware.
No prompts or media are sent to hosted inference. Model downloads and backend
installation are separate setup steps controlled by the user.

## Backend support

| Backend | Included optimization | Applicability |
| --- | --- | --- |
| NInfer | Vectorized, bit-preserving BF16 embedding gather | Dense width 5120, aligned buffers, batches 32–128; existing fallback for other shapes |
| llama.cpp | Experimental grouped-query CPU attention reuse | One-token q4_0-cache decode, Q=24 / KV=4 / D=256; existing fallback otherwise |
| WanGP | Reference-latent caching, guarded H3 VSA graphs, finite-batch retention | Compatible pinned WanGP backend; graph/retention optimizations have explicit H3 admission rules |

The project is designed around backend adapters. **It does not currently support
every model, architecture, device or model format.** Each upstream runtime decides
which models it can load; each optimization has a documented compatibility gate.
There is no universal TPS, latency or unchanged-output guarantee across runtimes.

## Get started

Requirements for the patch installer: Python 3.10+ and Git. The installer itself
uses only Python's standard library. Backend build/runtime requirements differ.

```shell
python tools/engine.py list
```

Use a fresh checkout at the exact revision in the backend's `backend.json`.
For example, for NInfer:

```shell
git clone https://github.com/iamwavecut/ninfer-all.git ninfer
git -C ninfer checkout 33163f05fb54459c19e2574607f5d252f4be2ba4
python tools/engine.py check ninfer --checkout ./ninfer
python tools/engine.py apply ninfer --checkout ./ninfer
```

`check` performs a read-only patch preflight. `apply` requires the pinned revision,
clean tracked source, exact patch hashes and the source-file allowlist. Existing
edits are preserved by refusing to apply to a dirty tracked tree. No automatic
stash, reset, model conversion, download, upload or inference occurs.

Build and run the patched runtime using the backend instructions:

- [NInfer build and compatible shapes](backends/ninfer/README.md)
- [llama.cpp experimental CPU patch](backends/llama-cpp/README.md)
- [WanGP local video integration](backends/wangp/README.md)

Keep inference endpoints bound to loopback unless you intentionally configure
network access. Check available GPU/CPU/RAM before generation, and serialize heavy
jobs when hardware is shared.

## Measurements and limits

Historical RTX 5090 observations from the development stack:

| Measurement | Observation | Scope |
| --- | ---: | --- |
| LLM code, direct API | 371.80 generated tokens/s | One short-prompt code request, request wall clock |
| LLM code through Claude Code | 310.84 generated tokens/s | SDK API portion; full invocation was 146.52 tokens/s |
| Warm video task | 9.4979 s median | Four later tasks, 640×384, 107 frames, 24 FPS, audio, eight steps |
| First video task | 25.1131 s median | Two first tasks; includes loading/shape preparation within the task |

These are **whole-stack reference observations**, not an attributable speedup from
this patch bundle and not a guarantee for another model. The video observations
were recovered from completed saved tasks; the surrounding four-phase coordinator
was stopped by a resource identity guard. The saved media passed a separate full
decode and same-profile repeatability audit. This does not convert the interrupted
execution into a passing run or establish cross-model quality equivalence.

Source-specific measurements and limitations are in the backend READMEs and
[aggregate benchmark metadata](benchmarks/reference.json). The native gather
microbenchmarks and CPU attention oracle cover narrow operations. Validate output
correctness and end-to-end speed on your own workload before enabling an option.

**1,000 single-request tokens/s, usable million-token coding context and one-second
fresh finished video have not been achieved by this release.** Reference context
was 98,304 configured tokens, with a separate actual 90,000-token retrieval check;
the short-prompt TPS figures are not million-token-context measurements.

B32 draft replacement, alternate text encoders, projection weights, LoRA files,
quantization/conversion tools and unselected numerical experiments are excluded.

## Verify the release

```shell
python -B -m unittest discover -s tests
python tools/check_release.py --check
```

Backend-specific tests may require their documented dependencies. CUDA benchmarks
are optional, require compatible hardware and are never launched by the installer.
The release hash manifest covers the exact public payload; local checkpoints,
prompts, logs, credentials, generated media and workspace history are not included.

## Licenses

This is a mixed-license distribution, **not an entirely Apache/MIT package**.
Original top-level installer/release tooling is Apache-2.0. NInfer, llama.cpp and
WanGP-derived files keep their respective licenses and attribution. In particular,
WanGP Community License 2.0 has its own distribution and commercial-use conditions.
Read [LICENSES.md](LICENSES.md) and [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)
before redistributing backend code. No upstream project endorses this release.
