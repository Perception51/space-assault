# Project Guidelines

## Git & GitHub

After completing any meaningful unit of work — a new feature, a bug fix, a refactor, or a content update — commit and push the changes to GitHub.

### Commit message format

Use [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<optional scope>): <short subject>

<body — bullet points describing what changed and why>

Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>
```

**Types:**
- `feat` — new feature or user-visible addition
- `fix` — bug fix
- `refactor` — code restructure with no behavior change
- `style` — visual/CSS-only changes
- `chore` — tooling, config, dependency updates
- `docs` — documentation only

**Rules:**
- Subject line: lowercase, no trailing period, ≤ 72 characters
- Body: explain *what* changed and *why*, not just *how*
- Always include the `Co-Authored-By` trailer

### Versioning

Tag releases using [Semantic Versioning](https://semver.org/):

| Change type | Version bump | Example |
|---|---|---|
| New features | Minor | `v1.0.0` → `v1.1.0` |
| Bug fixes / small patches | Patch | `v1.0.0` → `v1.0.1` |
| Breaking changes | Major | `v1.0.0` → `v2.0.0` |

After tagging, create a GitHub Release via `gh release create` with a brief changelog.
