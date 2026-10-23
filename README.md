# Coverage Tracker

A phone companion for 360° capture. While you walk around with a 360 camera on a stick (for Gaussian splats or photogrammetry), the phone tracks **where you have been**, so you can see what's covered and what's missing. A second page, the **viewer**, shows a finished session on a map, a floor plan or a grid.

It runs in Chrome on Android (WebXR + ARCore), installs as an app and works offline.

**Open the app:** https://SuspenseAndSurprise.github.io/coverage/  
**Viewer:** https://SuspenseAndSurprise.github.io/coverage/viewer.html

> The phone tracks you, not the camera. Positions are where the phone was, about half a metre from the lens.

## Tracker (`index.html`)

**While you walk**
- **Minimap** with your path and coloured coverage cells (0.5 m). Tap it to enlarge. Heading-up or fixed.
- **Floor tiles** drawn on the real floor in the camera view.
- **Speed bar** with a limit line (0.5 / 0.8 / 1.0 / 1.2 m/s). It turns red when you're too fast and shows how much to slow down. The phone also buzzes.
- **Stick height tag** (Low / Mid / High): tap it whenever you change the stick height. The map can show passes or heights.
- **Drift check:** set a mark (tape on the floor), come back later, see the drift in metres and fix it. The trail is also tied to AR anchors, so ARCore's own corrections move the map.
- **Area:** walk the outer edge, finish, and get a coverage percentage, the holes outlined, and the distance to the nearest one.
- **Haptics:** a short tick on new ground, a double buzz when too fast, a long buzz when tracking is lost.
- **Sync:** a chirp (or a silent screen flash) that the 360 camera records, so you can line up the video and the trail later.
- **Eco mode** (slower map updates, lower AR resolution) for battery.

**Settings (☰):** map colours, map rotation, floor tiles, sync signal, speed limit, GPS placement, Eco, Reset, Exit.

**Export:** after you exit AR, an *Export* button saves a `coverage_….json` to Downloads. Export before closing the app; an unexported session only lives in memory.

### Tips
- Walk slower in low light or close to objects (about 0.5 m/s). Outdoors in daylight 1 m/s is usually fine.
- Hold the phone slightly tilted at the scene, not straight down at the floor. ARCore needs texture.
- Do a drift check with a mark on the floor, especially on long walks.

## Viewer (`viewer.html`)

Open a session JSON and see it again:
- **Map:** placed on OpenStreetMap from the GPS fixes and compass at the start. Fine-tune by dragging and rotating.
- **Floor plan:** load a plan image, set its width in metres, drag and rotate the trail onto the walls (good for indoors, where GPS is useless).
- **Grid:** true-scale 1 m grid.
- **Save aligned** stores your placement in a copy of the JSON.

GPS only runs at the start of a session (until you have walked about 25 m, or after 2 minutes), then switches itself off. Indoors it gives up after 30 s.

## Session file

```
samples      [{ t (ms epoch), x, y, z, pass: low|mid|high, brk? }]   x/z horizontal, metres
syncs        [ms epoch, ...]                                          chirp / flash times
driftChecks  [{ t, drift, fixed }]
area         [[x, z], ...] | null
geo          [{ t, lat, lon, acc, x, z }] | null                      GPS fixes paired with AR position
compass      { deg, consistency } | null
```

## Install

**GitHub Pages:** upload these files to a public repo, turn on Settings → Pages (branch `main`, root), open the address in Chrome and choose *Install app*. Open it once online so it stores itself.

**Updating:** upload the new files and raise `VERSION` in `sw.js` (for example `v6` → `v7`), otherwise phones keep the old offline copy. The start screen shows the version and where it runs from (`v6 · GitHub`).

## Requirements
- Android phone with ARCore (Google Play Services for AR) and Chrome.
- Internet for the first start and for map tiles in the viewer.

## Files
| File | What |
|---|---|
| `index.html` | the tracker |
| `viewer.html` | the session viewer |
| `manifest.webmanifest`, `icon-*.png` | install as an app |
| `sw.js` | offline cache |

Map data © [OpenStreetMap](https://www.openstreetmap.org/copyright) contributors. Built with [three.js](https://threejs.org) and [Leaflet](https://leafletjs.com).
