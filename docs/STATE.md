# Lindens Planner — state

_Last updated: 17 Nov 2026 · app version v44_

## Current state
- Single-file app: `index.html` (~2,320 lines). Plus `.nojekyll`. No build step.
- Latest release **v44**: (1) W10 no longer has built-in WC/basin fixtures (they couldn't be deleted; add them from the fittings page). (2) New fire **FP5**: 1.0 × 0.5 m three-sided fire inside the living room (x 718–768), glass on the long east side + both ends, south end stops at y 420 (hall door y 420–500; y 390 with F2). Drawn as FX `fire3l` (2D in `fx()`, 3D next to `fire3`). (3) New fit slot **DS** (`DS0` none / `DS1` telescopic 6-leaf slider in the dining–living wall line, middle 4 leaves open, FX `tsd`, opening y 36–384, or 36–310 with FP5). DS1 only with FP0/FP1/FP5; adds a steel if the spine wall is kept. Fit code is now 7 chars (DS last; old 6-char codes still load).
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
| 171–249 | HTML shell; `APP_VERSION` at ~177 |
| 251 | catalogue (options per slot) |
| 376 | rules (clash checks) |
| 443 | geometry (cm; x east from west outer corner, y south from rear face) |
| 704 | fit-out layouts |
| 868 | svg floor plans (`floorSvg`) |
| 985 | page 2: fittings (grid, snapping, codes, legacy layouts) |
| 1267 | state, share codes (`makeCode`, `readCode`) |
| 1362 / 1415 | cost / totals |
| 1458 | ui: `render`, slots, `setPage` (~1656), page-2 controller (~1650), events (~1736) |
| 1825 | page 3: 3D model generator (`gen3d`, walls, slabs, stairs, fittings, roof) |
| 2057 | three.js scene (`w3*` functions) |
| 2202 | shared boards (Supabase: `sbGet`, `sbRpc`, `bd*`) |
| ~2311 | `init` |

## Next steps
- Check FP5 and DS1 in plan and 3D (leaf look, fire facing, clearances); W10 too in the 3D walk-through (corridor, slider, WC) and confirm dimensions/fixtures look right.
- Review v42 loft stair (L4 + LO11) with the family/architect in the 3D walk-through.
- Continue defining and refining the fittings list.
- Keep this file and the section map current after each change.
