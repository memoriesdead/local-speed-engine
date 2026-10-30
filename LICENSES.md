# License scope

Licenses apply per component; the top-level license does not replace third-party terms.

| Files | Terms |
| --- | --- |
| Original top-level `tools/` and `tests/` code and original documentation | Apache License 2.0, in [LICENSE](LICENSE) |
| `backends/ninfer/` source patch and derived fixtures | NInfer Apache-2.0 plus the preserved MIT adaptation notice in its [notices](backends/ninfer/THIRD_PARTY_NOTICES.md) |
| `backends/llama-cpp/` patch and CPU fixture | [ggml/llama.cpp MIT license](backends/llama-cpp/LICENSE) |
| `backends/wangp/` patch, adapter and derived tests | [WanGP Community License 2.0](backends/wangp/LICENSE.txt); [MMGP notice](backends/wangp/mmgp-LICENSE.md) |

Modified backend code is identified in each backend README and patch, dated
2026-10-30. Attribution and license texts are distributed alongside the patches.
WanGP terms include conditions on redistribution and commercial services; do not
infer Apache/MIT rights for the video component from the top-level tooling license.

Upstream runtime dependencies installed separately retain their own licenses.
No model weights, CUDA runtime binaries, FFmpeg binaries or vendored runtime
dependency trees are distributed in this source package.
