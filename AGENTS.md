# Hermes Relay Repository Contract

## Mission

Hermes Relay is the public Android and companion-tool surface that connects users to upstream Hermes while keeping optional Relay power features additive. Product canon lives in `docs/spec.md` and `docs/decisions.md`; release rules live in `RELEASE.md`; deferred work belongs only in `TODO.md`.

## Architecture Boundaries

- **Vanilla Hermes must remain upstream-only.** Chat, Manage, and standard dashboard voice must work against unmodified upstream Hermes.
- Verify every assumed upstream route in the current hermes-agent source before implementing against it.
- Features requiring custom server behavior belong in an upstream contribution or behind the optional Relay plugin with graceful degradation.
- Dashboard authentication, API-server authentication, and Relay pairing are separate trust domains. Do not proxy privileged dashboard administration through Relay.
- Android uses Jetpack Compose, kotlinx.serialization, OkHttp, secure transport, and the established product flavors.
- The Python plugin uses Python 3.11+, asyncio/aiohttp, type hints, and structured logging.
- The desktop CLI uses strict TypeScript/ES modules and ships compiled output.

## Safety Invariants

- Pairing, device control, terminal access, certificates, and tokens must fail closed.
- Never log or commit credentials, pairing secrets, cookies, raw auth headers, private hostnames, or private infrastructure.
- Capability detection must degrade honestly when an upstream or optional surface is unavailable.
- Release builds and public documentation must not depend on BALLER's private environment.

## Public Repository Hygiene

- No personal names, private infrastructure, internal operator jargon, or assistant self-narration in committed public prose.
- CHANGELOG follows Keep a Changelog; DEVLOG is factual engineering history; TODO.md is the sole forward-work list.
- Preserve the `main`/`dev` release model and repository-native Conventional Commit rules. Do not push or release without explicit authorization.

## Working Method

- Read this file, then the relevant product docs and source-owned contract.
- Preserve dirty worktrees and unrelated changes.
- Prefer focused vertical slices and path-specific CI commands over broad speculative rewrites.
- Update docs only when the public contract or verified architecture changes.

## Validation

Choose the commands matching the changed surface:

```powershell
.\gradlew.bat lint --console=plain
.\gradlew.bat assembleDebug --console=plain
python -m compileall -q plugin relay_server hermes_relay_bootstrap
python -m pytest plugin/tests/test_relay_security.py plugin/tests/test_voice_routes.py plugin/tests/test_session_grants.py plugin/tests/test_native_layout_imports.py
cd desktop
npm run build
```

Also run `git diff --check`. Use the path-specific workflows under `.github/workflows/` as the release gate source of truth.
