# Upstream sources and attribution

This project distributes selected modifications as source patches, rather than
redistributing complete runtimes or model assets.

- NInfer: https://github.com/iamwavecut/ninfer-all, revision
  `33163f05fb54459c19e2574607f5d252f4be2ba4`.
  Its [Apache license](backends/ninfer/LICENSE) and the
  [mkyt MIT adaptation notice](backends/ninfer/THIRD_PARTY_NOTICES.md) are retained.
- llama.cpp / ggml: https://github.com/ggml-org/llama.cpp, revision
  `6050b467c07a95c7b87debd4ddac2f8ba8d9b807`.
  The [complete MIT license](backends/llama-cpp/LICENSE) is retained.
- WanGP: https://github.com/deepbeepmeep/WanGP, revision
  `2c67e50c8ed2c7074b4060bf6d6372af593a55ec`.
  The [complete Community License](backends/wangp/LICENSE.txt) and
  [MMGP notice](backends/wangp/mmgp-LICENSE.md) are retained.

Modifications in this release are dated 2026-10-30. Each backend manifest records
its exact base revision, source target allowlist and patch SHA256. Names are used
for attribution and compatibility identification, without implying endorsement.
