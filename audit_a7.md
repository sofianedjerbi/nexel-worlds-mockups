# Audit A7: Nexel Worlds, targeting and walking in and out of holes

Worktree `agent-a0a150547cc28d568` (main at `73f50ba`, target.js). Own server on port 7873, world `audit7`, seed 12, `--dev`, saves in a temp folder under `tmp/a7`. No game code was changed, nothing was committed, nothing is left running.
Scripts are in `<worktree>/shots/a7/`: `t.py` (targeting: aim, press, result), `strip4.py`, plus a6's `n1.py`, `p.py`, `strips.py`, `sheet.py` and `lib.py`, repointed to this worktree. Pictures: `tmp/mockups/a7_*.png`.
What I did not do: the landscape phone targeting cases, the 4-deep vs 14-deep room, the upstairs of the house, the house on the phones. The staged land was made with `dev_stamp` and `dev_col`. Every key was a real key press, and on the phones a real touch on A, B, Build or Up.

## 1. Re-test of a6

| a6 | Now | Picture | Seen |
|---|---|---|---|
| N1 a small hill's top becomes a floating plate | FIXED | `a7_sheet_desk_surf` (1st) | You stand on the grass top and the window stays shut (`win.n` 0). |
| N2 the floor of the hole you are in is drawn outside it | NOT | `a7_sheet_desk_holes` (all 4), `a7_c_desk_room_0_1aim` | 3-wide stair, 1-wide stair, pit, room 4 deep: the floor and lower steps hang below the grass rim as a pale slab. |
| N3 same depth, two views | PARTLY | burst logs, `a7_desk_st3_down_strip_every15` | Both stairs now tip from 36 to 68, so they match. The pit you fall into stays at 36. |
| A3 a dug stair from the grass looks like crates | PARTLY | `a7_sheet_desk_room` (2nd) | A small hole, and a lit slab below it on the meadow. |
| A4/A6 6 deep looks 1 deep | NOT | `a7_sheet_desk_room` (1st, shaft 3) vs `a7_sheet_desk_holes` (3rd, shaft 6), `a7_sheet_phone_holes` | Shaft 3 and shaft 6 are the same picture: a cat in a brown box one tile wide. |
| N4 behind a 2-high earth wall you seem to stand on it | NOT | `a7_sheet_desk_surf` (2nd) | The cat is drawn on the wall's top, hatched. |
| N5 a tunnel cut open before you are near | FIXED | `a7_desk_tun_through_strip_every26` | The hill stays whole until you enter. Then a clean slot. |
| N6 a cat painted on the stone wall | PARTLY | `a7_sheet_desk_surf` (3rd) | Now a hatched ghost over the dark face: reads as behind, faintly. |
| B2 the title tagline lies over the scene | NOT | `a7_sheet_titles` (middle) | Upright: the tagline sits on the house roof and the crops. |
| Others (B9, B10, N7 to N14) | untested | | |

## 2. Targeting score

Strips: aim, then press (key held 140 ms), then result, cropped round you: `a7_ts_<screen>_<case>_<i>.png`. "Said" is the picture before the press; "changed" is the cell diff (relative to you; z is the level).

| Case | Key | Desk: said / changed / clear? | Upright phone |
|---|---|---|---|
| Meadow, earth | Space | no box on the first aim after arriving; it appears only after / put at (1,0,1) / NO (`a7_ts_desk_meadow_0`) | same |
| Meadow | X | nothing on your own tile / dug (0,0,0) under you / NO | same |
| Meadow, build | Space | cream box (1,0,1) / (1,0,1) / YES | YES (`a7_ts_port_meadow_2`) |
| 1-high block | Space | cream box on the block's top (1,0,2) / (1,0,2) / YES (`a7_ts_desk_b1_0`) | - |
| 1-high block | X | nothing / dug under YOU, not the block / NO | - |
| 2-high wall | Space | first aim empty, then the box (1,0,3) / (1,0,3) / PARTLY | YES |
| 2-high wall, build | Space, X | cream z3 over an orange hatch at z2 / both right / PARTLY: ghost, hatch, a green grass square in the middle of the column, and brackets round YOUR cat (`a7_ts_desk_w2_1`, `a7_ts_port_w2_1`) | same |
| Air (E E) | Space | red box, dotted drop line, checker shadow / nothing / YES (`a7_ts_desk_air_0`) | YES |
| Cave wall, pickaxe | Space | nothing: only the way-out arrow and dots / dug (1,0,1) at your body / NO (`a7_ts_desk_cave_0`, `a7_c_desk_cave_0_1aim`) | way-out guide only |
| Cave, pickaxe | R | nothing; a dotted orange outline shows only during the press / dug (1,0,2), the head cell / NO (`a7_ts_desk_cave_1`) | "Dig a step" tap: no change |
| Ledge 2 high | Space | cream box at the ledge's foot, its top level with your feet / (1,0,1) on the low ground / PARTLY (`a7_ts_desk_ledge_0`) | - |
| Room 4 deep | Space | no box; the way-out guide and a dashed box with your name on the north wall / (1,0,-3) / NO (`a7_c_desk_room_0_1aim`) | - |
| Room, pickaxe | X | nothing / dug under you / NO | - |
| Stair | Space | cream box on the lower step (1,0,-2) / same / YES (`a7_ts_desk_stair_0`) | - |
| Mid water | Space | a cream block standing on the water / no block: water mid to shallow, nothing visible / NO (`a7_ts_desk_water_0`) | - |
| Tree, earth | Space | red box and red brackets / nothing, toast / YES (`a7_ts_desk_tree_0`) | - |
| Tree, axe | Space | the tree's brackets / stump / YES | - |

**Desktop: 7 YES, 3 PARTLY, 8 NO out of 18.** Every key changed the cell the game intended. The picture fails in three ways: X never has a mark; nothing is drawn while the way-out guide is up; the first aim after arriving is empty.

## 3. New faults (ranked)

### A: blocks understanding or play
| # | Picture | Repro | A newcomer thinks |
|---|---|---|---|
| T1 | `a7_c_desk_cave_0_1aim`, `a7_c_desk_room_0_1aim` | Tunnel end or a dug room, tool in hand, way-out guide up. | "What does Space hit?" No box; only the way out is marked. The box shows while the key is held. |
| T2 | `a7_ts_desk_b1_0` | X anywhere. | "X takes the block I face." It digs under you; nothing shows that. |
| T3 | `a7_ts_desk_water_0` | Mid water, earth, Space. | A block box on the water; after the press nothing visible changes (the bed rose). |
| T4 | `a7_ts_desk_w2_1`, `a7_ts_port_w2_1` | Build mode at a 2-high wall. | Four marks for two cells, a grass square inside the column, brackets on my own cat. |
| D1 | `a7_sheet_desk_room` (1st) vs `a7_sheet_desk_holes` (3rd), `a7_sheet_phone_holes` | Shaft 3, then 6. | "How deep am I?" Same picture. |
| N2 | `a7_sheet_desk_holes` | Stairs, pit, room. | "A drawer pulled out onto the grass." |

### B: ugly or weird
| # | Picture | Repro | A newcomer thinks |
|---|---|---|---|
| T5 | `a7_ts_desk_meadow_0` | Arrive, aim without moving. | The box is missing until the first press or step. |
| T6 | `a7_c_desk_room_0_1aim` | Room 4 deep. | A dashed box with my name on the north wall, over a faded cat: me shown twice. |
| T7 | `a7_ts_desk_w2_1` | Build mode, press. | Key labels jump above and below the box after each press. |
| T8 | `a7_ts_desk_ledge_0` | On a 2-high ledge. | The box at the foot looks like it is at my level. |
| T9 | `a7_ts_port_cave_1` | Upright phone, tunnel, "Dig a step". | The tap did nothing. |
| N4 | `a7_sheet_desk_surf` (2nd) | Behind a 2-high earth wall. | "I am on the wall." |
| B2 | `a7_sheet_titles` | Upright title. | Text over the scene. |

### C: nits
- `a7_ts_desk_stair_0`: a light-blue line floats in the stair's north wall.
- `a7_ts_desk_air_0`: two levels up reads as one.
- On the phone the corner prompts still name keys ("Space", "X").

## 4. Frame cost (`?perf=1`, CPU x4)
| Walk | Upright: HdLook.ms median / p95 / max (script p95) | Landscape |
|---|---|---|
| Stair down | 1.9 / 8.3 / 9.5 (7.2) | 5.4 / 7.8 / 11.0 (14.6, closest to the budget) |
| Stair up | 1.4 / 5.3 / 6.6 | 1.4 / 6.2 / 7.9 |
| Sprint | 0.6 / 8.7 / 10.2 | 3.2 / 5.1 / 6.4 |
60 fps, no frame over 33 ms. Mac GPU only.

## 5. Good
- On open ground the box language reads at a glance: cream to put, orange hatch to break, red with a drop shadow for "can't" (meadow, block, stair, air).
- Every key changed the cell intended (18/18).
- Hills no longer open the x-ray. The tunnel is not cut from outside. The cave walk-in is smooth. Both stairs have one view.
- Shaft 6 out in 9 presses and 6 steps, pit out in 7 presses and 5 steps, by the marks only (desktop and upright phone).
- 60 fps on the phones at CPU x4. No page errors.
