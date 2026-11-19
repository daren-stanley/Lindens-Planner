# Lindens Planner — state

_Last updated: 19 Nov 2026 · app version v47_

## Current state
- Single-file app: `index.html` (~2,490 lines). Plus `.nojekyll`. No build step.
- Latest release **v47**: **sun simulator** on page 3 (`☀ Sun` button in the 3D bar; panel under the view). Time-of-day slider (04:00–22:00, GMT/BST by date) + date presets 21 Dec / 21 Mar / 21 Jun; real sun position for Woking (`sunPos`, NOAA-style, no library); a shadow-casting `DirectionalLight` (4096 map, ±45 m around the house) lights the model; glass doesn't cast, so sun enters through windows. Sun mode adds a north compass (rotates with the camera), a sun disc, sky dimming at night, and trees and neighbouring buildings (checkbox "Trees & neighbours"). With sun off the scene is unchanged. Site orientation comes from the survey drawing E1000: `SITE.bx = 333.2` = true bearing of the model's +x axis (long axis runs NNW–SSE; model +y, the front, faces ENE towards Lytton Road). `SUN_ENV` holds trees (x, y, canopy radius, height, all in model metres) and neighbour blocks (Jordans house 7.5 m, garage 3 m), scaled off the survey; **tree heights are guesses** (survey is 2D) and trees are drawn in full leaf (worst case for winter shade). Ground plane enlarged to 120 m.
- v46: internal floor area per floor (`floorAreas`, rasterised rooms + stairs, 12 cm wall gaps closed; garage separate; loft excludes `rs` roof stores; 0 for LO0) shown as a label on each plan image, in the PNG header and in a sidebar "Internal floor area" card with total. Sidebar "Stairs" card (`stairInfo`, uses `gen3d(...).stairs` stats from `stairRun`): risers, step height, going (straight / winder at centre line), pitch, min headroom measured to the floor/roof above using the drawn stairwell opening (every stair currently reads 1.55–1.87 m, i.e. under 2.0 m, because the drawn openings start early; worth checking).
- v45: sliding door full length with FP5.
- v44: (1) W10 no longer has built-in WC/basin fixtures (they couldn't be deleted; add them from the fittings page). (2) New fire **FP5**: 1.0 × 0.5 m three-sided fire inside the living room (x 718–768), glass on the long east side + both ends, south end stops at y 420 (hall door y 420–500; y 390 with F2). Drawn as FX `fire3l` (2D in `fx()`, 3D next to `fire3`). (3) New fit slot **DS** (`DS0` none / `DS1` telescopic 6-leaf slider in the dining–living wall line, middle 4 leaves open, FX `tsd`, opening y 36–384 always; with FP5 the fire sits slightly in front of the south leaves, v45). DS1 only with FP0/FP1/FP5; adds a steel if the spine wall is kept. Fit code is now 7 chars (DS last; old 6-char codes still load).
- v43: new ground-floor option **W10** (utility under the stair, 0.8 m corridor west to the garage door, WC 1.7 × 1.5 m off the corridor's south side via a slider). W5 was already taken, so it is W10; it is last in the W list so saved share codes are unaffected. Rules: needs a flipped stair (S4/S5) like W5, must pair with A3 (door at y 410–480), not with H4. Layout is `GEO.W10` (~line 575).
- v42: L4 loft stair over the landing (flipped stairs) with LO11 loft.
- Gotcha: share codes store each option's list position, so append new options to the END of a slot's `opts`.
- Three pages work: structure (1), fittings (2, unlocked once every structure slot is picked), 3D walk-through (3).
- Shared boards live on Supabase "lindens" (`boards`, `board_versions`, RPCs `save_board` / `delete_board`). Publishable key is in the page (fine — RLS/RPC guarded).
- Share codes: structure code + fittings code (base64url per item), both synced to the URL hash.

## Section map of index.html (approx. line numbers — re-grep if edited)
| Lines | Section |
|---|---|
| 1–170 | CSS (two `<style>` blocks) |
| 171–262 | HTML shell; `APP_VERSION` at ~182; page-3 bar + sun panel ~225–245 |
| 264 | catalogue (options per slot) |
| 391 | rules (clash checks) |
| 458 | geometry (cm; x east from west outer corner, y south from rear face) |
| 722 | fit-out layouts |
| 896 | svg floor plans (`floorSvg`) |
| 1056 | page 2: fittings (grid, snapping, codes, legacy layouts) |
| 1338 | state, share codes (`makeCode`, `readCode`) |
| 1433 / 1487 | cost / totals |
| 1530 | ui: `render`, slots, page-2 controller (~1728), `setPage` (~1734), events (~1815) |
| 1920 | page 3: 3D model generator (`gen3d`, walls, slabs, stairs, fittings, roof) |
| 2146 | sun simulator (`SITE`, `SUN_ENV`, `sunPos`, `w3SunRig`, `w3SunApply`, `w3Compass`, `w3SunUI`) |
| 2212 | three.js scene (`w3*` functions) |
| 2363 | shared boards (Supabase: `sbGet`, `sbRpc`, `bd*`) |
| ~2438 | `init` |

## VR feasibility (Quest 3S) — study only, not built
- Page 3 is three.js r149 (CDN), served over https, so WebXR in the Quest Browser should work. Needed: `renderer.xr.enabled=true`, an "Enter VR" button (`navigator.xr.requestSession('immersive-vr')`), swap `requestAnimationFrame` loop for `renderer.setAnimationLoop`, thumbstick walking + snap turn (reuse `w3Blocked` / `w3Ground` collision), optional teleport. The scene is in cm, so scale the rig by 0.01 for true 1:1. Pointer/joystick UI doesn't apply in-headset. Sun shadows (4096 map) may need dropping to 2048 on Quest. Can't be tested without a headset; expect one round of fixes after the first try.

## Next steps
- Check the sun simulator on the phone (slider drag on a touch screen, compass, performance with shadows). Refine tree heights/positions if the shading looks wrong; consider a "bare winter trees" toggle.
- Confirm the grey garage-like block next to the SSE end is a neighbour's (it is modelled as a 3 m neighbour block at x −1…−7 m).
- VR mode (see feasibility above) once a Quest 3S is in hand.
- Decide whether the stairwell openings should be redrawn so headroom reaches 2.0 m.
- Check FP5 and DS1 in plan and 3D (leaf look, fire facing, clearances); W10 too in the 3D walk-through (corridor, slider, WC) and confirm dimensions/fixtures look right.
- Review v42 loft stair (L4 + LO11) with the family/architect in the 3D walk-through.
- Continue defining and refining the fittings list.
- Keep this file and the section map current after each change.
