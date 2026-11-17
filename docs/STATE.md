# Lindens Planner — state

_Last updated: 17 Nov 2026 · app version v42_

## Current state
- Single-file app: `index.html` (~2,320 lines). Plus `.nojekyll`. No build step.
- Latest release **v42**: L4 loft stair over the landing (flipped stairs) with LO11 loft — added to check the architect's loft-stair idea in 3D.
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
- Review v42 loft stair (L4 + LO11) with the family/architect in the 3D walk-through.
- Continue defining and refining the fittings list.
- Keep this file and the section map current after each change.
