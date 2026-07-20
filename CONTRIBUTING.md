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

## Commit Convention

Use Conventional Commits so the history clearly identifies the type and scope of each change:

```text
type(scope): short description
```

Common types are `feat`, `fix`, `docs`, `chore`, and `refactor`. Prefer one of these repository scopes when applicable:

| Scope | Use for |
|---|---|
| `skill` | `SKILL.md` behavior or guidance |
| `readme` | Project overview and installation documentation |
| `references` | API indexes and reference documentation |
| `libnx` | Bundled libnx updates |
| `examples` | Bundled switch-examples updates |
| `font` | Button icon font and mapping guidance |
| `release` | Version preparation and release notes |

Examples:

```text
feat(skill): add SDL2 rendering guidance
fix(references): correct framebuffer header path
chore(libnx): update bundled libnx release
docs(release): add v1.1.0 release notes
```

Keep each commit focused on one logical change and write the description in the imperative mood.
