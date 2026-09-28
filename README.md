# appium-server

Appium 3.x WebDriver server as an OpenCharly candy — with the uiautomator2
Android driver and a pinned WebView chromedriver.

This candy installs the Appium 3.x WebDriver server (`/usr/bin/appium`) plus the
uiautomator2 Android driver (per-user under `~/.appium/node_modules`) and a
chromedriver pinned to the API-36 `google_apis_playstore` System WebView
(Chrome 133) at `/opt/chromedriver/133`, so WebView context switching works
offline. The `appium` service runs on port `4723` at base path `/wd/hub`.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `appium-server` |
| Binary | `/usr/bin/appium` (Arch AUR `appium`) |
| Driver | `appium-uiautomator2-driver` (per-user) |
| chromedriver | `/opt/chromedriver/133/chromedriver` |
| Service / port | `appium` (40) on `4723`, base path `/wd/hub` |
| Requires | `layer-nodejs`, `layer-java-openjdk`, `layer-android-sdk`, `layer-supervisord` |

The chromedriver version vars track the System WebView's Chrome version; the
session cap `appium:chromedriverDisableBuildCheck` tolerates the small patch
skew. Every claim is a file/binary on disk or an HTTP response from the running
server.

## How to use it

Compose the candy into a box that provides the Android toolchain — the
`android-emulator-layer` candy already requires it:

```yaml
my-android:
  candy:
    base: cachyos.cachyos
    candy:
      - '@github.com/opencharly/pod-appium-server:<tag>'
```

Then build and deploy with the charly CLI:

```bash
charly box build my-android
charly start my-android
```

Drive it with the `appium:` check verb. See the owning skill for the WebDriver
session model, element operations, and the mobile capabilities.

## Layout

- `charly.yml` — the `appium-server:` candy entity: the four requires, the
  version vars, the `appium` service, the port, and the plan that installs the
  driver and chromedriver and asserts the server.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `CHANGELOG/` — per-CalVer history.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-check:appium` — the `appium:` check verb and the
  WebDriver session surface. This candy has no `skill:` entity of its own; the
  family skill covers the verb surface, and the missing-owning-skill gap is
  recorded against [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).
- Sibling verb: `/charly-check:adb`.
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
