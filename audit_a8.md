# Audit A8: Nexel Worlds, HD look after the hole, mark, sides and sky work

Worktree `agent-aa548ef88930b78c2` (main at `09bb9af`). My own server copy on port 7881: world `audit8`, seed 12, `--dev`, saves in temp folders under `tmp/a8`. The owner's save was tested on a copy (`tmp/saves_a8/caves`). No game code was changed, nothing was committed, nothing is left running.
Scripts are in `<worktree>/shots/a8/`: `a8.py <screen> <part>` with `part_NEW` (newcomer), `part_FAR` (mountain, night, sprint north), `part_WATER`, `part_HOLES`, `part_OWN` (owner's save), `part_POP` (teleport and stream). Also `t.py` (a7's targeting script), `crop.py`, `csheet.py`, `strip.py` and `fr.py`. Pictures: `tmp/mockups/a8_*.png`. Every move was a real key press, or a real touch on the phones.
Screens: desktop 1440x900; upright phone 390x844 at DPR 3 with touch; landscape phone 844x390 at DPR 3 with touch.

## 1. Re-test

| Item | Now | Picture | Seen |
|---|---|---|---|
| a7 T1: no box while the way-out guide is up | FIXED | `a8_ts_desk_aims` (cave) | A dotted orange box on the tunnel end, with the guide next to it. |
| a7 T2: X has no mark | PARTLY (by design) | `a8_ts_desk_aims` (meadow X) | There is still no mark under you. The words "X Dig down" sit next to the cat. |
| a7 T3: water box, then nothing visible | PARTLY | `a8_ts_desk_aims` (water) | The box now lies on the water's face and says "Raise the bottom". After the press the water goes from mid to shallow, and the change is hard to see. |
| a7 T4: build marks at a 2-high wall | PARTLY | `a8_ts_desk_aims` (w2) | The brackets on your own cat are gone. A green grass square still shows in the middle of the column under the orange hatch. |
| a7 T5: first aim is empty | FIXED | `a8_ts_desk_aims` (meadow) | The box is there on the first frame. |
| a7 D1: shaft 3 and shaft 6 look the same | NOT | `a8_sheet_desk_holes` (top-left, top-middle), `a8_desk_hole_shaft6_c` | Both are a one-tile brown box with a cat in it. In shaft 3 the cat is whole; in shaft 6 you see only its head. Neither reads as a depth. |
| a7 N2: the hole's floor drawn outside it ("drawer") | PARTLY | `a8_desk_new_07_out_01_c`, `a8_sheet_port_hole` (2nd), `a8_sheet_land_hole` (2nd) | The stair, the pit and the shaft seen from above are clean now. One case is left: stepping onto the first dug step (window open, `win.n` 2) shows a brown or khaki slab sticking out below the rim, on all three screens. |
| a7 N4/T6: me shown on the grass while I am in a room | NOT | `a8_desk_hole_room_c` | In a 5x4 room 4 deep the view stays at pitch 36 (not top-down). The cat is a checkered ghost on the grass south of the room. |
| a7 B2: the title tagline lies over the scene | NOT | `a8_desk_title` | Desktop too: "Night music sold separately" is white text over the fence and grass, hard to read. |
| Owner: "quality of the rendering SUCKS, see the sides" | PARTLY | `a8_desk_far_mtn_noon_wall`, `a8_desk_own_58_47_W_wall`, `a8_desk_own_58_47_W_gap`, `a8_desk_new_03_canyon` | The sides have a texture now (cliff mottle, stone bricks). Three problems remain. (1) The texture comes out as 1-px vertical streaks, finer than the tops' art px, so the sides and the tops have different pixel sizes. (2) The rim where a top meets its wall is a soft, blurred smear. (3) Far faces turn to grey mush. A natural mountain reads as laid brick masonry. |
| Owner: "weird artefacts" (black leaf shapes, patchwork tops) | NOT REPRODUCED / new pop-in | `a8_sheet_pop`, `a8_desk_pop_0_600` | I saw no black leaves in about 40 frames of forest at noon, low sun and night, with the natural shader and with it off. I did find a related fault: for 0.3 to 1 s after arriving, the trees and the cat stand over a flat grey void, because the land has not been painted yet. |
| Owner: "sky bumps up behind a mountain" | FIXED | `a8_desk_far_mtn_runN2`, `a8_desk_far_mtn_back10` | The sky's line is flat. The mountain covers it and no longer pushes it up. |
| Owner: "particles cut at the end when sprinting in water" | FIXED | `a8_desk_wat_stop_strip`, `a8_desk_wat_stop_strip2` | The wake thins out in whole px over about 1 s after the stop. Nothing is cut. |
| Owner: "block selection confusing: which level?" | NOT | `a8_sheet_desk_holes`, `a8_desk_hole_shaft6_c`, `a8_desk_new_11_c` | In a shaft or pit seen from above, the "Dig a step" box is drawn on the grass outside the hole. In build mode the X box covers your own cat. |
| Owner: "can't tell how to get out" | PARTLY | `a8_desk_new_05_dig1_c`, `a8_desk_new_07_out_01_c` | The guide works: out of a 2-deep hole in 3 presses and 3 steps on desktop, 3 and 2 on the upright phone, 2 and 2 landscape, following the marks only. But the box it asks you to press sits away from the hole, and a key cap sometimes shows only "▲" or "◀" with no key letter (`a8_desk_hole_room_c`, `a8_ts_desk_aims` room). |
| Owner: "laggy on phone" | PARTLY | `tmp/a8/a8_low.png`, `tmp/a8/a8_low.json` | `phone_sim --preset low --at 40,40` (CPU x13.3, Mac GPU). Play is fine: stand 60 fps; walk 57; sprint 59; stair 56.9 (p95 gap 33 ms, script max 46 ms); swim 58.9. **The title is the lag:** 22.5 fps, 24 long tasks adding up to 4.5 s (the longest 2.1 s), and 5.9 s from Wake up to the land drawn. |

## 2. New faults (ranked)

### A: blocks understanding or play
| # | Picture | Repro | A newcomer thinks |
|---|---|---|---|
| A1 | `a8_desk_new_11_c`, `a8_sheet_port_hole` (4th) | Dig 2 down, follow the guide out, then Z (Build) on desktop or Build on the phone, on seed 12 at (37,7). | "Where did I go?" The orange hatched "Break" box covers the cat completely. The name label stays a tile higher, over a rock. |
| A2 | `a8_sheet_desk_holes` (shaft 3, shaft 6, pit), `a8_desk_hole_shaft6_c` | `part_HOLES`: in a shaft 3 or 6 deep, or a 3x3 pit 4 deep (top-down view). | "R digs that grass?" The "Dig a step" box is drawn faded on the grass beside the hole, not on the hole's wall where the step is. This is the owner's "which level?". |
| A3 | `a8_desk_hole_room_c` | `part_HOLES`: dev_col a 5x4 room at -4, teleport into it at z -4. | "I'm outside, on the grass?" The view stays tilted at 36. The cat is a checker ghost south of the room's rim. The floor is a thin strip under a huge north wall. |
| A4 | `a8_desk_own_58_47_morning` | Owner's save copy, teleport to (-58,47). | You stand under the owner's grey stone slab, which has no edge or thickness, so it reads as flat ground. The cat is a green checker on top of it. Nothing is opened. "Am I on it or under it?" |

### B: ugly or weird
| # | Picture | Repro | A newcomer thinks |
|---|---|---|---|
| B1 | `a8_desk_far_mtn_noon_wall`, `a8_desk_own_58_47_W_wall`, `a8_desk_new_03_canyon` | Seed 12 at (26,-22) looking at the canyon, or the owner's gorge at (-58,47). | The sides are vertical 1-px streaks at a finer pixel size than the land, the rims are blurry, and the far faces are smeared. "Cheap 3D, not pixel art." This is the owner's main complaint. |
| B2 | `a8_desk_own_41_69_N_walls` | Owner's save at (-41,69), walk 4 tiles north and look at the far walls. | The ends of the owner's straight stone walls bend into round loaves (the curve). Between them there is a hazy wall under a checker band. "Melted." |
| B3 | `a8_desk_pop_0_600`, `a8_sheet_pop` | Teleport to (150,120) and look at 250 to 600 ms. | Trees and the cat float on a flat grey plane, with a sky-blue band, before the land comes in. The same path probably serves a respawn. |
| B4 | `a8_desk_own_41_69_night_holes` | Owner's save at (-41,69) at night (phase .9). | Hole floors are pale lavender, brighter than the night grass: "the holes glow". By day they are also cream, lighter than the grass. |
| B5 | `a8_desk_mtn_runN3_end_c`, `a8_desk_far_mtn_runN2_c` | Seed 12, from (26,-10) sprint north about 7 s, into the slot at the foot of the canyon cliff (26,-36). The cat also loses hearts. | The cat stays a checkerboard with a cyan fringe, permanently: "glitched sprite". |
| B6 | `a8_desk_new_01_c`, `a8_port_new_01_wake3s` | Wake up on seed 12 (23,6), desktop and phone. | A berry bush and its bracket sit on the cat's head, and the name label is printed over the bracket: a jumble. Upright the name covers the bush. |
| B7 | `a8_desk_new_05_dig1_c`, `a8_sheet_land_hole` (1st) | Dig down into stone. | The "Dig a step" box is a tan earth block on grey stone, and the dug steps are brown earth inside a stone hill. The material doesn't match. |
| B8 | `a8_desk_title` | Title, desktop. | The tagline is unreadable over the scene (a7 B2, now on desktop as well). |

### C: nits
- Name labels float about a tile above the head, on all screens. Over water the name stays at standing height (`a8_desk_wat_stop_deep`).
- "You wake up in a meadow" while you wake on a stone mountain (seed 12).
- F3 says "Tile 37,7 grass" while you stand on stone (`a8_desk_new_12_f3_c`).
- Stair step tops are smooth gradients, not stepped tones (`a8_sheet_desk_holes`, bottom row).
- Build-mode prompts on the phones still name "1-9", "E" and "Q" keys.
- With the curve on "far", the screen edges bend hard on the upright phone (`a8_port_new_01_wake3s`).

## 3. What is good
- The sky is fixed to the camera and stays put behind mountains (`a8_desk_far_mtn_runN2`).
- The sprint wake on water fades properly (`a8_desk_wat_stop_strip2`). The see-through pool reads well (`a8_desk_wat_stop_deep`).
- Night is calm and readable, with a soft glow round you (`a8_desk_far_mtn_night`).
- From above, the stairs and the pit are clean, and the way-out dots climb the steps (`a8_sheet_desk_holes`, bottom row). Every way out was followed by the marks alone, on all three screens.
- The F3 overlay is clean and pixel-styled (`a8_desk_new_12_f3_c`).
- The low-sun forest with long shadows in the natural shader is lovely (`a8_desk_pop_sprint_120`).
- Play is 57 to 60 fps on the simulated low phone. No page errors in any run.

## 4. Verdict
No. The HD look is not yet "good enough, no weird things, renders properly". The three things that most stand in the way:
1. **The sides** (B1): they need the land's own pixel size and whole art px, with no streaks, a crisp rim and no smeared far faces. Today the tops are pixel art and the walls are not.
2. **Marks and the body in and around holes** (A1 to A3, plus the N2 drawer on the first step): every box must sit on the cell at the level it acts on, never on the grass outside and never over your own cat, and one view must hold in every dug space, rooms included.
3. **Things floating on a void, or glowing** (B3, B4, B5, A4): no frame should show the land unpainted, and hole floors and roofs over you must take the hour's light and a visible edge.
