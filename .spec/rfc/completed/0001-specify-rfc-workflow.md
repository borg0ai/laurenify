# RFC 0001: Use Specify to manage Laurenify RFCs

**Status:** Implemented

## Summary

Laurenify adopts Specify as its sole authority for RFC creation, status changes, archiving, and consistency checks. RFC records live in `.spec/ROADMAP.md`, `.spec/TASK_TRACKING.md`, and `.spec/rfc/`. Laurenify routes RFC lifecycle work to the Specify skill and uses `/technical-writing` only to improve RFC prose. Implementation remains blocked until the RFC is validated and the user explicitly approves the implementation plan.

## Problem

Laurenify has no `.spec/` tree or consistent RFC lifecycle. Its router sends RFC work to `/technical-writing`, which covers prose quality but does not create, validate, track, or archive RFCs. This leaves RFC state and implementation approval without one source of truth.

## Goals

- Use Specify's `.spec/` layout and CLI for all RFC lifecycle operations.
- Route RFC lifecycle tasks to the separately installed Specify skill; resolve its CLI relative to the skill's runtime-provided base directory, never a machine-specific path.
- Route RFC prose editing to `/technical-writing` while keeping status and cross-file state under Specify.
- Require RFC validation and explicit user approval of the implementation plan before implementation changes.
- Keep `.spec/ROADMAP.md`, `.spec/TASK_TRACKING.md`, and RFC files synchronized through Specify commands.

## Non-goals

- Vendoring Specify's CLI or skill into Laurenify.
- Replacing Laurenify's existing upstream synchronization, source attribution, or licensing model.
- Changing the content or release lifecycle of existing skills unrelated to RFC work.

## Design

The repository adopts Specify's existing three-file convention. `CONTRIBUTING.md` documents the RFC-first contribution gate and commands using the separately installed Specify skill's CLI. The `poteto-mode` router sends RFC creation, status, archive, and consistency requests to the Specify skill, and sends RFC prose refinement to `/technical-writing`. `poteto-help` exposes the same routing. If Specify is unavailable, the agent stops before editing implementation or RFC state files and reports the missing skill; it does not emulate the CLI or hand-edit synchronized records. RFC body drafting may use normal file editing, followed by Specify validation. RFC index, task board, status, and archive updates must use Specify commands.

## Acceptance

- `.spec/ROADMAP.md`, `.spec/TASK_TRACKING.md`, and `.spec/rfc/` exist and are initialized by Specify.
- Laurenify's router and help distinguish RFC lifecycle management from prose editing and direct lifecycle work to Specify.
- `CONTRIBUTING.md` documents the RFC approval gate, command ownership, and no-hardcoded-install-path rule.
- Specify is not copied into the plugin package, and current upstream attribution and sync files remain unchanged.
- `specify validate` and `specify sync-check` pass for this RFC and the repository state.
