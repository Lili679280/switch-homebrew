# Contributing

Contributions are welcome when they improve correctness, portability, or coverage without replacing authoritative bundled sources with guesses.

Before submitting a change:

1. Keep all skill paths relative; never commit user-specific absolute paths.
2. Verify every libnx API signature against `references/libnx/nx/include/switch/`.
3. Verify usage patterns against `references/switch-examples/`.
4. Keep `api-index.md` and `examples-map.md` as navigation indexes; do not duplicate large API tables.
5. Do not add credentials, private keys, copyrighted SDK files, or unrelated binaries.
6. Explain which files changed and how the result was verified.

When updating bundled dependencies, pin an explicit release or commit and update all version references.
