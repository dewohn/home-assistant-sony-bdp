# 2026-11-07 — hassfest red since 10-04: `requests` in manifest requirements

## Handoff

- leftovers: none
- repos:
  - home-assistant-sony-bdp → already committed and pushed (e51faac, manifest fix); this note: `docs/sessions/2026-11-07-hassfest-requests-requirement.md`, message "Session note: hassfest rejected requests in manifest requirements"
- lessons:
  - target: skill home-assistant-integration-development § Publishing to HACS
    text: |
      - **hassfest rejects any package Home Assistant itself depends on** (`requests`, `aiohttp`, ...)
        in a custom integration's `requirements`, pinned or not: "Requirement requests is a dependency of
        Home Assistant itself and must not be listed in the manifest". Drop it; HA provides it, and a
        library you require that needs it pulls it in anyway. The rule arrived in hassfest `latest` around
        2026-10-04 with no change on our side. Worked example: `kevinclark/home-assistant-sony-bdp` e51faac,
        red for 6 weekly scheduled runs before anyone looked (the second such case after ha-pwrcell).
    evidence: this note; run 42445952309 (fail) vs 42456369051 (pass)
- focus: done session:local_814775b5-236b-47b3-bd72-578ce7e82f03 --why "home-assistant-sony-bdp CI failing: manifest fix e51faac, Validate run 42456369051 green 2026-11-08 06:50Z"
- verified: `git status` clean, `origin/main..main` empty at e51faac; run 42456369051 on e51faac: hassfest success, hacs success.

## What happened

Weekly `Validate` (schedule) failed every run from 2026-10-04 to 2026-11-08 (6 runs) on commit bd7d6c6, which
passed 09-27. Only hassfest failed:
`[REQUIREMENTS] Requirement requests is a dependency of Home Assistant itself and must not be listed in the
manifest of a custom integration.` A newer `ghcr.io/home-assistant/hassfest:latest` introduced the check.

Fix: removed `"requests"` from `custom_components/sony_bdp/manifest.json` `requirements`. No runtime change:
HA ships `requests`, and `sony-bdp-ip` declares `dependencies = ["requests"]`. Version not bumped (no behaviour change).
