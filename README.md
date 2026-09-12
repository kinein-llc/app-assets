# Kinein app-assets

Public, CDN-served assets loaded **only inside Kinein mobile apps** (the AppMySite
WebView). Served by GitHub Pages at:

    https://kinein-llc.github.io/app-assets/

## Why this repo exists

Web Ninja rejects a stylesheet save with an empty HTTP 500 once the compiled
`main.css` passes a compiler ceiling. On `shop.sin360.us` the client's own CSS is
~240 KB compiled, which left roughly 1 KB of headroom for the app-mode skin —
every new rule had to be paid for with an equal prune, and several finished pieces
of work were stuck in a queue behind it.

Serving the skin from here removes the ceiling entirely. The site's CSS editor
keeps only the client's original stylesheet; the app fetches the full, unpruned
skin at runtime.

## Contents

| Path | Site | Notes |
|---|---|---|
| `sin360/app-skin.css` | shop.sin360.us | Full unpruned app-mode skin, compiled flat and compressed |

## Safety properties

- **Every rule is gated on `html.app-mode`.** That class is set only by the
  activator, only when the UA is the app (or an explicit `?previewmode=`). A raw
  web visitor who somehow loaded this file would see no change.
- **No `@view-transition`.** The navigation trigger is an un-gateable at-rule that
  once killed every tap in the Android app. The build script hard-fails if it
  reappears.
- **No credentials.** These files are public. Nothing licence-bearing goes here —
  the Strich key lives only in the site's own JS editor.
- **Generated, not authored.** Source of truth is the private repo
  `kinein-llc/mobile-apps`, at `clients/<host>/`. Rebuild with
  `clients/shop.sin360.us/build-app-skin.sh`, which runs a leak gate before it
  writes and restores the deployed mirror afterwards. Never hand-edit a file here.

## Cache behaviour

GitHub Pages serves with a short max-age, so a push reaches app users within
roughly ten minutes without any change to the site's JS. If a change must land
immediately, bump the `v=` parameter in the site's CSS bridge.
