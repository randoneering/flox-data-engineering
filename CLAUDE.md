# Project-Specific Claude Rules

## Project Context

- Repo: `flox-randoneering`
- Purpose: maintain a reproducible Flox environment for data engineering workflows.
- Main areas: Python tooling, Airbyte, S3-compatible storage, Apache Iceberg, and ClickHouse.

## Required Skill Workflow

- Use `flox-environments` first for new environment or dependency work.
- Use `flox-services` for service changes.
- Use `flox-builds` for `flox build` behavior or packaging.
- Use `flox-sharing` for layering, composition, FloxHub, or team-sharing patterns.
- Keep guidance consistent with the relevant `SKILL.md` files.

## Flox Working Rules

- Never use absolute paths. Use `$FLOX_ENV`, `$FLOX_ENV_CACHE`, and `$FLOX_ENV_PROJECT`.
- Put persistent local data, logs, caches, and venvs in `$FLOX_ENV_CACHE`.
- Keep hooks fast, idempotent, and quiet. Use `return`, not `exit`.
- Return to `$FLOX_ENV_PROJECT` at the end of hooks when shell state matters.
- Support runtime overrides like `VAR=value flox activate`.
- Never store secrets in manifests or checked-in docs.
- Keep examples runnable and avoid extra helper functions, aliases, comments, or noisy output unless they help.

## Manifest Guidance

- Use clear sections: `[install]`, `[vars]`, `[hook]`, `[profile]`, `[services]`, `[build]`, `[include]`, `[options]`.
- Prefer namespaced variable, function, and service names so composed environments do not collide.
- Use host/port variable pairs for services.
- Remember: `[hook]` runs on every activation, not during `flox build`.
- Put user-invokable shell helpers in `[profile]`, not `[hook]`.
- Service commands start fresh; do not assume hook state carries over.

## Services And Builds

- Log service output to `$FLOX_ENV_CACHE/logs`.
- Test service commands with `flox activate -- <command>` before wiring them into `[services]`.
- When debugging a service, run the exact manifest command manually first.
- `flox build` only sees packages in the default `toplevel` group.
- Use reproducible build steps and remember pure builds cannot rely on undeclared host state.

## Validation

- When behavior changes, check the relevant path with `flox activate`, `flox build`, and `flox services status` as needed.
- Keep examples and command paths valid.
- Review changes for reproducibility, cross-platform clarity where relevant, and safety.

## Do Not Modify

- Git metadata or generated lock/cache artifacts unless the user asks.
