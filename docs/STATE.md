# Lindens Planner — state

_Last updated: 24 Nov 2026 · app version v50_

## Current state
- Single-file app: `index.html` (~2,610 lines). Plus `.nojekyll`. No build step.
- Latest release **v50**: **L6** = L5 stair with a taller front dormer giving **2.4 m over the landing** (steps keep ≥2.0 m). `DORM` for L6: x 433–575, y 541–648 (1.42 × 1.07 m plan), ceiling 605, top 625, face ~0.9 m above the roof, window 548–600. `GEO.L6` reuses `GEO.L5`. Cost points 1.7; own planning note.
- v49: new loft stair **L5** = L4 evolved with a **small front dormer** (architect: Woking often approve front dormers that don't overlook a house).
  - Layout (`GEO.L5`): 4 steps west (x 523–613, going 225), flat 80×80 quarter landing (x 443–523, y 556–636, drawn as a 1-step winder `WD(...,1)` = landing), 7 steps north (y 399–556, going 224) into the loft; 13 risers × 195 mm, ~41°. Rules as L4 (S4/S5 only, LO11 only). Cost points 1.4. Planning note: front dormer needs full planning.
  - Dormer (`DORM` in `gen3d`): smallest flat-roofed box giving 2.0 m over the landing and upper west steps. Outside x 433–575 (1.42 m wide), y 589–648 (0.59 m deep in plan, face set back 14 cm from the brick face so the eaves run on in front), ceiling 565, roof top 585 (flush with the slope at the back), face ~0.5 m above the roof with a 1.0 × 0.33 m window. `under()` lifts to 565 inside it; roof slab gets an `open` hole (`slabHoles` skips plug/pane). Without it the landing would have 1.64 m at its front edge.
  - Stair card: landings count as one tread, headroom sampled over the whole landing; L5 reads min headroom 2.03 m (L4: 1.98 m at the winder centre line, ~1.64 m at its outer edge).
  - Fixed: loft stair rails no longer drawn where the roof is under 1.5 m above the loft floor (they were the brown triangles poking through the front roof with L4/L5).
- v48: **Velux + daylight map** on page 3.
  - Roofs: every porch (P1–P4) now has a **gable roof** (ridge front to back, `PORCH_DEG=30`, ridge capped 12 cm under the first-floor front window sills; `porchRoof` in `gen3d`). The rear extension (R7–R10) has a **10° lean-to** (`EXT_DEG`) rising to the house wall (`extRoof`); because it meets the wall at 272/289 cm, the first-floor FULL-height master windows (M2/M9) over it get a raised sill (`extSillOf`, 23 cm for the 1 m ext, 41 cm for the 2 m ext; also noted in the planning notes). Garage roof unchanged (flat).
  - Velux: 2 per porch (one per slope, 78×98), 2 fixed panes in the extension (78×98, or 55×78 on the 1 m ext, centres x 195/505), one 78×98 pane on the loft front slope per office/bedroom ≥1.5 m wide (`vxPane` list, `slabHoles`). Every opening is built as roof pieces round a hole plus a roof-coloured **plug** (layers `pOff/eOff/rOff`) and a glass **pane** (`pOn/eOn/rOn`); the 3D bar buttons `Velux: All | Porch | Extension | Roof rooms` (`W3.vx`, `w3VxUI`, `w3Visibility`) just flip group visibility, so shadows and sun patches update with no rebuild. Default: off. Panes are view-only (not in share codes). Prism builder now supports `ax:'y'` (reversed points keep normals outward).
  - **Daylight map** (`Daylight map` button, panel `#w3dl`, `dlCompute` / `dlEval` / `w3DlRefresh`): CIE-overcast sky component on an 85 cm work plane, 20 cm grid, straight-line sight through the plan wall mask (`gen3d(...).dl`), plus a flat BRE split-flux reflected allowance per room. Windows and each Velux zone are stored as separate arrays so toggling a zone only re-adds them. Heat-map overlay per floor (canvas plane 2 cm above the floor) and a table: room, daylight factor now, Velux gain, share of floor above target (kitchen 2%, living/dining/extension/office 1.5%, bedrooms 1%). No trees or neighbours; absolute figures are estimates, the with/without difference is the useful bit.
  - Test rig used (not in repo): headless Chrome + local three r149; 260 random valid designs build with no errors or NaNs.
  - v47: **sun simulator** on page 3 (`☀ Sun` button in the 3D bar; panel under the view). Time-of-day slider (04:00–22:00, GMT/BST by date) + date presets 21 Dec / 21 Mar / 21 Jun; real sun position for Woking (`sunPos`, NOAA-style, no library); a shadow-casting `DirectionalLight` (4096 map, ±45 m around the house) lights the model; glass doesn't cast, so sun enters through windows. Sun mode adds a north compass (rotates with the camera), a sun disc, sky dimming at night, and trees and neighbouring buildings (checkbox "Trees & neighbours"). With sun off the scene is unchanged. Site orientation comes from the survey drawing E1000: `SITE.bx = 333.2` = true bearing of the model's +x axis (long axis runs NNW–SSE; model +y, the front, faces ENE towards Lytton Road). `SUN_ENV` holds trees (x, y, canopy radius, height, all in model metres) and neighbour blocks (Jordans house 7.5 m, garage 3 m), scaled off the survey; **tree heights are guesses** (survey is 2D) and trees are drawn in full leaf (worst case for winter shade). Ground plane enlarged to 120 m.
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
| 1–180 | CSS (two `<style>` blocks; `#w3sun,#w3dl` panel styles) |
| 181–272 | HTML shell; `APP_VERSION` ~184; page-3 bar (Sun, Daylight map, Velux buttons) + sun and daylight panels ~225–265 |
| 273 | catalogue (`SLOTS`, options per slot) |
| 399 | rules (clash checks) |
| 466 | geometry (cm; x east from west outer corner, y south from rear face) |
| 732 | fit-out layouts |
| 1046 | svg floor plans (`floorSvg`) |
| 1151–1740 | page 2 fittings, share codes, state, cost (`costHtml`, `totals` ~1496), `render` ~1590, page-2 controller ~1736, `setPage` ~1742, events ~1822 |
| 1913 | `W3` state; `EXT_DEG`/`PORCH_DEG`/`extSillOf` ~1919 |
| 1932 | page 3: 3D model generator `gen3d` (walls, slabs, stairs, fittings; Velux roofs ~2019; main roof + loft panes ~2185) |
| 2196 | sun simulator (`SITE`, `SUN_ENV`, `sunPos`, `w3Sun*`, `w3Compass`) |
| 2249 | daylight map (`dlCompute`, `dlEval`, `w3DlRefresh`), then `w3VxUI` ~2318 |
| 2337 | three.js scene (`w3Build`, `w3Visibility`, controls) |
| 2491 | shared boards (Supabase: `sbGet`, `sbRpc`, `bd*`), then `init` |

## VR feasibility (Quest 3S) — study only, not built
- Page 3 is three.js r149 (CDN), served over https, so WebXR in the Quest Browser should work. Needed: `renderer.xr.enabled=true`, an "Enter VR" button (`navigator.xr.requestSession('immersive-vr')`), swap `requestAnimationFrame` loop for `renderer.setAnimationLoop`, thumbstick walking + snap turn (reuse `w3Blocked` / `w3Ground` collision), optional teleport. The scene is in cm, so scale the rig by 0.01 for true 1:1. Pointer/joystick UI doesn't apply in-headset. Sun shadows (4096 map) may need dropping to 2048 on Quest. Can't be tested without a headset; expect one round of fixes after the first try.

## Next steps
- Review L5 + front dormer with the architect: dormer size (min 1.42 × 0.59 m), whether a window in the 0.5 m face is wanted, eaves/gutter detail, and whether the 1.9/1.8 m loft-conversion headroom relaxation would let it shrink further.
- Look at v48 on the phone: Velux toggles (All / Porch / Extension / Roof rooms), Sun with Velux on/off (try 21 Dec and 21 Jun, morning for porch and loft panes, afternoon for the extension), the daylight map table. Decide pane positions/sizes, the 10° extension pitch and 30° porch pitch (single constants), and whether Velux/roof-light cost belongs in the structure costs.
- Loft front-slope panes sit about 2 m up (rooms stop at y=433, ridge at y=331): high-level light, no view. Consider moving the loft rooms/knee wall or using a larger pane if the loft stays dark.
- Daylight map ignores trees/neighbours and the sun disc; could add `SUN_ENV` obstruction and a sunlight-hours view.
- Check the sun simulator on the phone (slider drag on a touch screen, compass, performance with shadows). Refine tree heights/positions if the shading looks wrong; consider a "bare winter trees" toggle.
- Confirm the grey garage-like block next to the SSE end is a neighbour's (it is modelled as a 3 m neighbour block at x −1…−7 m).
- VR mode (see feasibility above) once a Quest 3S is in hand.
- Decide whether the stairwell openings should be redrawn so headroom reaches 2.0 m.
- Check FP5 and DS1 in plan and 3D (leaf look, fire facing, clearances); W10 too in the 3D walk-through (corridor, slider, WC) and confirm dimensions/fixtures look right.
- Review v42 loft stair (L4 + LO11) with the family/architect in the 3D walk-through.
- Continue defining and refining the fittings list.
- Keep this file and the section map current after each change.
