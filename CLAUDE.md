# Project-Specific Claude Rules

<!-- 
This file customizes Claude's behavior for this specific project.
Rules here APPEND to, REINFORCE, or REPLACE your global preferences.
Place this file at `.claude/CLAUDE.md` in your project root.
-->

## Project Context

**Project Name:** flox-randoneering

**Description:** Working repo for building and documenting a reproducible Flox environment for data engineering workflows.

**Tech Stack:**
- Flox CLI (environment management)
- Nix/Nixpkgs (package and build base)
- TOML manifests (`.flox/env/manifest.toml` patterns)
- Bash hooks/services
- Markdown skill docs in `.claude/skills/`

**Primary Use Case:**
Build and maintain a Flox environment specifically for data engineering, including repeatable tooling, activation flows, and practical examples for day-to-day data work.

**Current Implementation Focus:**
- Include common Python libraries used in data engineering workflows.
- Support local Airbyte execution for ingesting data from source systems.
- Provide S3-compatible object storage patterns and Apache Iceberg table workflows.
- Include ClickHouse for local analytics and query workloads.

**Implementation Checklist (Data Engineering Environment):**
- [ ] Define base Python toolchain in `.flox/env/manifest.toml` (Python, uv/pip, and common data engineering libraries).
- [ ] Add local Airbyte workflow support (runtime requirements, service commands, and example ingest command path).
- [ ] Add S3-compatible object storage support (service config, bucket setup pattern, and env var wiring).
- [ ] Add Apache Iceberg workflow examples (table create/read flow with reproducible commands).
- [ ] Add ClickHouse local analytics service (host/port vars, service command, and sample query path).
- [ ] Validate critical paths: `flox activate`, `flox build`, and `flox services status`.

**Required Flox Skill Workflow:**
- Start environment creation and dependency setup with `flox-environments` (required first step for new environment work).
- Use `flox-services` when defining or changing local services (Airbyte, object storage, ClickHouse).
- Use `flox-builds` for packaging/build behavior and reproducible build validation.
- Use `flox-sharing` when documenting or implementing team sharing/composition patterns.
- Keep all examples and commands consistent with the relevant `SKILL.md` guidance.

---

## Development Guidelines

### Environment Operations

**MUST:**
- Use Flox environment variables (`$FLOX_ENV`, `$FLOX_ENV_CACHE`, `$FLOX_ENV_PROJECT`) instead of absolute paths.
- Keep hook/service logic idempotent and safe on repeated activation.
- Keep examples runnable and aligned with the corresponding Flox skill guidance.
- Use runtime overrides when helpful (for example `VAR=value flox activate`).

**MUST NOT:**
- Put secrets in manifests or checked-in docs.
- Assume `[hook]` runs during `flox build`.
- Add non-reproducible instructions that depend on machine-specific absolute paths.

### Code Patterns

**Preferred Patterns:**
- Namespace vars/functions/services to avoid collisions in composed environments.
- Use host/port environment variable pairs for services.
- Keep examples small and practical; avoid unnecessary helper wrappers.

**Avoid:**
- Monolithic hooks with mixed responsibilities.
- Duplicate guidance that conflicts with `SKILL.md` sources.
- Hidden assumptions about shell state carrying across services.

---

## Testing Requirements

### Environment Tests

- **Required coverage:** Validate representative environment paths when behavior changes.
- **Test setup:** Use local Flox activation/build/service command checks.
- **Fixtures:** N/A.

### Integration Tests

When behavior guidance changes, validate at least one representative command path (for example `flox activate`, `flox build`, or `flox services status`) in a local test environment.

---

## Deployment & CI/CD

### Pre-deployment Checklist

- [ ] All tests passing
- [ ] Example commands and paths are valid
- [ ] No secrets or tokens introduced in docs/manifests
- [ ] Skill guidance stays consistent across related files

### Migration Strategy

Schema migrations are not part of this repository's core workflow. For environment changes, use small git commits, verify local activation, and share via git and/or FloxHub (`flox push`) when needed.

---

## Common Tasks

### Adding a New Environment

```toml
# 1. Create or update `.flox/env/manifest.toml`

# 2. Define install targets
[install]
python.pkg-path = "python311Full"
uv.pkg-path = "uv"

# 3. Add vars/hooks with idempotent logic
[vars]
APP_PORT = "8000"

# 4. If needed, define services with configurable host/port
[services.app]
command = '''exec python -m http.server "$APP_PORT"'''

# 5. Validate with:
# flox activate
# flox activate -s
# flox services status
```

### Troubleshooting Flox Activation Issues

1. Check manifest syntax and package names.
2. Confirm no absolute paths in hooks/services.
3. Verify needed tools are in `[install]` and in the `toplevel` group for builds.
4. Run activation/build commands with verbose output and inspect logs.

---

## Project-Specific Constraints

### Performance Requirements

- Keep activation time reasonable for docs/examples (target under 10 seconds for simple examples).
- Keep hook output minimal and focused.
- Prefer deterministic, repeatable commands over machine-specific shortcuts.

### Data Retention

- Production data: N/A.
- Logs: Store transient service logs in `$FLOX_ENV_CACHE/logs`.
- Backups: N/A for repository-managed data.

---

## Team Conventions

### Naming Conventions

**Tables:** N/A
**Columns:** N/A
**Indexes:** N/A
**Constraints:** N/A

### Code Review Requirements

Review for correctness against Flox skill guidance, reproducibility, cross-platform clarity (when relevant), and safety (no secrets, no destructive defaults).

---

## External Resources

### Documentation

- Internal docs: `.claude/skills/README.md`
- API docs: Flox CLI docs and each `SKILL.md` under `.claude/skills/`
- Architecture diagrams: N/A

### Monitoring & Alerts

- Dashboard: N/A
- Alert channels: N/A
- On-call: N/A

---

## Notes for Claude

<!-- Any specific instructions about how Claude should work on this project -->

**When making changes:**
- Always check related skill docs when updating behavior guidance.
- Consider impact on activation, services, builds, and sharing workflows.
- Keep examples consistent with Flox environment variable conventions.
- Follow the Required Flox Skill Workflow above; do not skip `flox-environments` for new environment setup tasks.

**Testing priority:**
- Critical path: `flox activate`, `flox build`, and service command examples.
- Integration points: Flox CLI behavior and cross-skill consistency.

**Do not modify:**
- Git metadata or generated lock/cache artifacts unless explicitly requested.


## Writing Style & AI Phrase Avoidance

### Avoid AI Buzzwords
Replace these overused AI phrases with clearer alternatives:
- `leverage` → use, apply
- `synergy` → teamwork, working together
- `cutting-edge` → new, advanced, latest
- `robust` → strong, solid
- `seamlessly` → works well, easy to use
- `utilize` → use
- `revolutionary/transformative/game-changing` → describe specific impacts
- `scalable solution` → expandable, easily adjustable
- `innovative` → new, creative (or describe what's actually new)
- `comprehensive` → complete, detailed, thorough (or be specific)

### Avoid AI Filler Phrases
These add no value and signal AI-generated content:
- `moreover/furthermore` → also, or split into two sentences
- `in conclusion` → so, to wrap up
- `it is important to note` → state the point directly
- `in today's society/fast-paced world` → be specific about time or setting
- `harnessing the power of` → use, tap into
- `this essay/document will discuss` → start with the argument directly

### Natural Writing Guidelines
- Use conversational tone (as if explaining to a colleague)
- Prefer concrete details over vague claims
- Avoid overly formal or academic phrasing
- Read documentation aloud - if it doesn't sound like something you'd say, rewrite it
- Show don't tell: instead of "innovative solution", describe what's actually new

### In Code Comments & Documentation
- Write comments as you would explain to a teammate
- Be direct and specific
- Avoid marketing-speak in technical docs
- Focus on "what" and "why", not impressive-sounding adjectives
