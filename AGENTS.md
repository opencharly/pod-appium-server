# AGENTS.md — pod-appium-server

Standalone candy repo for the `appium-server` candy — the Appium 3.x WebDriver
server (uiautomator2 driver + a pinned WebView chromedriver) that the
`android-emulator-layer` candy requires. The entire candy lives in `charly.yml`
at the repo root: the `appium-server:` entity with its requires, version vars,
service, port, and a `plan:` that installs the driver and chromedriver and
asserts the running server. There is no source tree.

Canonical files:

- `charly.yml` — the `appium-server:` candy entity (description, `require`,
  `var`, distro package, `service`, port, `plan`).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `CHANGELOG/` — per-CalVer history.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-check:appium` — the family skill for the `appium:` check verb
  (WebDriver sessions, element operations, mobile capabilities).
- `/charly-check:adb` — the sibling `adb:` check verb served by `plugin-adb`.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, package sections, service declarations).
  Load before editing any entity field or plan step.
- `/charly-internals:git-workflow` — before any git/PR action.

The candy carries no `skill:` entity of its own; the family skill
`/charly-check:appium` covers the surface. The gap is routed to the named
skill-authoring batch
[opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291);
when that lands, add the owning skill here.

## Build / validate / test

- `charly box validate` at the repo root — the structural check: the manifest
  must parse and validate at the installed charly.
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no**
  per-repo candy gate. Its only workflow file is
  `.github/workflows/tag-on-merge.yml`.
- The live R10 witness is the candy's own `plan:` runtime checks (the Appium
  status endpoint, the `appium: status` verb) plus the consuming box's check
  bed. These require a host with the rootless podman socket.

## Modify this repo

- Edit the `appium-server:` candy entity in `charly.yml`; the inline checks are
  part of that entity's `plan:`.
- Bump `WEBVIEW_CHROMEDRIVER_VERSION` and `WEBVIEW_CHROMEDRIVER_MAJOR` in
  lockstep with the system image's WebView version, the literal
  `/opt/chromedriver/133` path, and the emulator-layer cap.
- Keep the `layer-nodejs` / `layer-java-openjdk` / `layer-android-sdk` /
  `layer-supervisord` pins and the `version:` schema stamp in step.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
