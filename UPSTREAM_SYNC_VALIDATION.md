# Upstream v1.2.2 sync validation

Validated October 7, 2026 using OpenAI Codex.

## Repository inspection and sync

Authenticated inspection found only main (e5b1443444021969550dfaf5e0d177d642705fa8) and no open pull requests before the sync. main was an ancestor of upstream v1.2.2: 0 fork-only commits, 48 upstream-only commits, no fork-specific tree changes relative to the merge base. No unpublished developer work outside this repository can be assessed.

main was advanced without force or reset, using the expected original head, to upstream v1.2.2 commit 62fd86fe3b62644b65890557ad9bf397abdc7cfb. VERSION now reads 1.2.2. This includes masks, ZIP input, image pipeline/undistortion updates, tile-bin overflow fixes, compressed image caching, macOS MPS default, Metal toolchain documentation, and CPU fallback when the Metal compiler is missing. The single post-release upstream commit (macOS workflow runner update) was inspected but excluded to keep main exactly at the requested release.

## Verification

- PASS: Apple Silicon GitHub Actions build, macos-14 arm64, LibTorch 2.3.1, Release, GPU_RUNTIME=MPS. Logs show Metal framework detection, Metal kernel compilation, default.metallib creation, and successful linking of opensplat and simple_trainer. Run: https://github.com/yeraypabon/OpenSplat/actions/runs/37624780200
- PASS: Linux GCC 13.3 C++20 syntax check of zip_utils.cpp and C syntax check of vendored miniz.
- PASS: independent CMake builds of vendored miniz and zstd static libraries on Linux x86_64.
- BLOCKED: full local CPU configuration/build; CMake configuration fails at missing TorchConfig.cmake. LibTorch and OpenCV development packages are not installed. The subsequent build has no generated Makefile.
- NO TEST SUITE: no enable_testing/add_test declarations outside vendor code; CTest reports no tests. CI builds the executables but does not execute training tests.
- UNVERIFIED: runtime training, masks/ZIP/undistortion correctness, tile-bin overflow behavior, compressed-cache behavior, MPS numerical correctness, automatic macOS MPS default (CI explicitly selects MPS), missing-Metal CPU fallback, and separate Metal toolchain installation on recent Xcode. Relevant configuration and README instructions were inspected.
- Existing upstream warnings: Metal signedness comparisons, duplicate miniz link library, and whitespace reported by git diff --check. No release sources were changed to silence them.
- Other push-triggered CI workflows (Docker CUDA, Ubuntu CUDA, Ubuntu HIP, Windows CUDA) were still running when this record was written; no success claim is made for them. Results: https://github.com/yeraypabon/OpenSplat/actions

This validation record lives on sync/upstream-v1.2.2; main remains the exact upstream release commit. No PR or issue was created, consistent with the imported AGENTS.md restriction.
