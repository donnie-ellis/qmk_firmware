# Donnie's Q10 ANSI keymap

Four layers: `MAC_BASE`/`MAC_FN` and `WIN_BASE`/`WIN_FN`. The physical OS switch on
the back of the board picks Mac vs. Windows at boot by setting the default layer
(`dip_switch_update_user`/`dip_switch_update_kb` in [q10.c](../../q10.c)); `MO(MAC_FN)` /
`MO(WIN_FN)` (bottom row) hold into the Fn layer for the active side.

## RGB color scheme

The whole board lights up as a solid color that tells you which layer/mode is active
(`rgb_matrix_indicators_user`):

| Context | Color |
|---|---|
| Windows base | Blue |
| Windows Fn | Orange (per-key, see below) |
| Mac base, profile 1 | Purple |
| Mac base, profile 2 | White (red while the [keep-awake toggle](#keep-awake-auto_shift) is active) |
| Mac Fn, profile 1 | Yellow (per-key) |
| Mac Fn, profile 2 | Pink (per-key) |

On both Fn layers, only keys with a real (non-transparent) binding on that layer light
up in the Fn color — everything else goes dark, so lit keys are exactly the ones that
do something while Fn is held.

## Mac profiles

`MAC_PROF2` (Fn + 3 on the Mac layer) toggles a second Mac "profile," persisted across
reboots via `eeconfig_read_user`/`eeconfig_update_user`. It doesn't change any keys —
just the base RGB color (purple vs. white) and which address `MY_EMAIL` sends, so you
can visually and functionally distinguish two Mac contexts (e.g. two jobs) on one keymap.

## Custom keys

| Key | Layer | Does |
|---|---|---|
| `MY_EMAIL` | Fn + 2 (Mac), Fn + F2 (Windows) | Types an email address. Windows always sends `donnie@dmellis.com`; on Mac it depends on the active profile — profile 1 sends `donnie@dmellis.com`, profile 2 sends `donnie.ellis@mckesson.com`. |
| `MAC_PROF2` | Fn + 3 (Mac) | Toggles between the two Mac profiles (see above). |
| `KC_LOCK` | Fn + top-right key (Mac) | Locks the screen (⌃⌘Q). |
| `KC_MCOPY` / `KC_MPASTE` | Leftmost column, rows 2/3 (Mac base) | One-key ⌘C / ⌘V. |
| `SS_WIN` | Leftmost column, row 4 (Mac base) | Window screenshot — sends ⌘⇧4 then Space to enter window-capture mode; click the window to finish. |
| `KC_SSFULL` | Leftmost column, row 5 (Mac base) | Full-screen screenshot (⌘⇧3), saved straight to the desktop. |
| `AUTO_SHIFT` | Bottom-left (Mac base) | See [Keep-awake](#keep-awake-auto_shift) below. |
| `WIN_FHOLD` | Fn + F (Windows) | Toggle-hold `KC_F`: first press registers `F` as held down (useful for hold-to-interact games) and lights that key red; second press releases it. |
| `KC_TASK` / `KC_FLXP` | Fn + F3/F4 (Windows) | Task View (⊞Tab) / File Explorer (⊞E). |
| `KC_COPY` / `KC_SINS` | Bottom-left / row 4 (Windows base) | Ctrl+C / Shift+Insert (paste), for apps that don't take Ctrl+V. |

### Keep-awake (`AUTO_SHIFT`)

Bottom-left key on the Mac base layer. Toggles a background loop
(`matrix_scan_user`) that taps Shift every 45 seconds to keep the machine from
going idle/locking while you're away. Only really meaningful (and colored) under
Mac profile 2 — see the RGB table above.

## Guides and code used

[Setting a different background per layer](https://www.reddit.com/r/Keychron/comments/128ifs3/qmk_help_can_i_set_a_different_backlight_colour/)
[Keycodes](https://pmortensen.eu/world2/2023/09/18/raw-qmk-keycodes-not-symbolic/)

## Compile this keymap

`qmk compile -kb keychron/q10/ansi_encoder -km donnie`
