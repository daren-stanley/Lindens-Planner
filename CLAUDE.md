# Lindens Planner

House-renovation option picker for the Lindens house, used by Daren, his wife and family.
Hosted on GitHub Pages: https://daren-stanley.github.io/Lindens-Planner/ (deploys from `main`).

- **Page 1 – structure**: pick an option for every slot (layout, stairs, loft…); rules flag clashes; costs and totals.
- **Page 2 – fittings**: drag-and-drop furniture, bathroom and kitchen items onto the plan; own share code.
- **Page 3 – 3D walk-through**: three.js model generated from the picks and placed fittings.
- **Shared boards**: Supabase project "lindens" (tables `boards`, `board_versions`; RPCs `save_board`, `delete_board`; family passcode to save).

## Rules
- Read `CLAUDE.md` and `docs/STATE.md` first; open other files only when the task needs them.
- Never read large files whole: search (grep), then read only the needed lines.
  Large files: **`index.html`** (~2,300 lines, ~218 KB — the whole app). Use `docs/STATE.md`'s section map to jump to line ranges.
- Don't re-read files that haven't changed since you last read them.
- Supabase: select only the needed columns, always with `LIMIT`.
- Keep replies brief: say what changed and where; don't echo code back.
- Bump `APP_VERSION` (and the version/date line beside it) on every release.
- At the end of each piece of work, update `docs/STATE.md` (current state and next steps, under ~100 lines) so a fresh chat can pick up from it.
