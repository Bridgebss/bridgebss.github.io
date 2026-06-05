# bridgebss.github.io

GitHub Pages **org site** for `Bridgebss`. Serves the root domain `https://bridgebss.github.io/`
and hosts a test harness for Android **App Links** and iOS **Universal Links**.

## Why this repo must be named `bridgebss.github.io`

GitHub Pages resolves URLs by repo name:

| Repo name              | Served at                              |
| ---------------------- | -------------------------------------- |
| `bridgebss.github.io`  | `https://bridgebss.github.io/` (root)  |
| any other repo `foo`   | `https://bridgebss.github.io/foo/`     |

Deep-link targets like `/splash` must live at the **root domain**, so they must be served from a
repo named exactly `bridgebss.github.io`.

## Two-repo layout

| Path on `bridgebss.github.io` | Served by repo |
| ----------------------------- | -------------- |
| `/`, `/splash`                | `bridgebss.github.io` (this repo) |
| `/.well-known/*`              | [`Bridgebss/.well-known`](https://github.com/Bridgebss/.well-known) |

The `.well-known` repo is literally named `.well-known`, so its root files are served at
`https://bridgebss.github.io/.well-known/...` — exactly where Android/iOS look. This repo owns
the web pages; that repo owns the verification files.

## Contents

```
index.html    Test dashboard (link buttons + live verification-file checks)
splash.html   Deep-link landing page  -> /splash
.nojekyll     Skip Jekyll; serve files as-is
```

## Verification requirements (in the .well-known repo)

- **Android** `assetlinks.json` — `target.namespace` must be the literal `"android_app"`
  (not a product name).
- **iOS** `apple-app-site-association` — must be reachable **without** a `.json` extension at
  `https://bridgebss.github.io/.well-known/apple-app-site-association`, over HTTPS, no redirects.
- The iOS `appIDs` value `S8QB4VV633.com.example.deeplinkCookbook` is a sample placeholder.
  Replace it with `<TeamID>.<your.bundle.id>` for your real app.

## Test

- Open `https://bridgebss.github.io/` and confirm both verification files show **200 · json**.
- Android: `adb shell am start -a android.intent.action.VIEW -d "https://bridgebss.github.io/splash" com.bfr.sp`
- iOS: paste `https://bridgebss.github.io/splash` into Notes/Messages on a device and tap it.
- Google verifier: `https://digitalassetlinks.googleapis.com/v1/statements:list?source.web.site=https://bridgebss.github.io&relation=delegate_permission/common.handle_all_urls`
- Apple validator: `https://app-site-association.cdn-apple.com/a/v1/bridgebss.github.io`
