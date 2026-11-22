# Audit A5: Nexel Worlds, HD-2D look, played a fifth time as a newcomer

Worktree `agent-a5b8e25952190ed3d` (based on main), own server on port 7835, fresh world seed 12 with `--dev`, own saves dir under `tmp/a5`. No game code changed, nothing committed, nothing left running.
Screens: desktop 1440x900 (keys), phone upright 390x844 DPR 3 (touch stick and buttons), phone landscape 844x390 DPR 3 (touch).
Scripts: `<worktree>/shots/a5/` (`s1.py` title, meadow, trees, hill; `s2.py` house; `s3.py` A shaft, B bare hands, C stair, D room 14 deep, E natural cave; `s4.py` W water and gorge, Z build mode; `p.py` frame cost; `strips.py` strips of 8 frames). Burst frames and per-frame logs: `tmp/a5/burst_*`.
Pictures: 259 files `tmp/mockups/a5_<screen>_<scene>.png`. I opened 45 at full size. Every finding names one I opened, unless it says "log".

Limits, said plainly:
- The dev stamp that clears the meadow did not always clear the trees. Two stair runs had a tree standing in the staged stair 12 deep ("Chop tree" offered under ground). I treat that as my staging, not a game fault.
- No rain exists in the game (no weather code found). Not tested.
- A 2-high wall was not built: build mode refused it (A5 below). Breaking a block in build mode was planned by key X but I did not open its picture.
- Phone lag was measured in headless Chrome on a Mac GPU with CPU x4. It says nothing about a real phone GPU.

Grades: A = blocks understanding or play. B = ugly or weird. C = nit.

## Verdict

**Would the owner understand underground now? Standing still in a room or a cave: yes. While moving in and out, and in any shaft or stair seen from the side view: no.**

The way out is solved: on all three screens, doing only what the screen marks got me out of a 6-deep shaft (15 presses), with bare hands out of a 4-deep shaft (9 presses), and out of the gorge. But walking down a stair the hole closes over you for almost a second, a dug stair looks like a pile of blocks standing on the grass, and 14 deep looks 1 deep.

## A: blocks understanding or play

| # | Picture | Repro | What a newcomer thinks |
|---|---|---|---|
| A1 | `a5_desk_stair_down_strip_every9` (8 frames, 0.15 s apart), `a5_desk_stair_down_strip_160` (8 consecutive) | Desktop, cleared meadow at (-2,4). Staged stair east: 6 open steps down, 8 more steps under the land, a flat passage 14 deep. Hold Right from the top. | "I walked down my stair and it vanished. I am walking on plain grass." Log: from step 11 to step 15 (0.85 s, 50 frames) the window is shut (`win.n` 0, `win.a` 0) while I am 11 to 15 levels under the land. It faded OUT at step 10 (6 frames), then came back with 13 tiles at step 15 (6 frames). Same on both phones (`win.a` goes 1, 0, 1 in the logs). This is "flashes" and "opens late". |
| A2 | same strips; log of `burst_desk_stair_down/r.json` | Same walk. | "The cat slides around the screen." Going down the open steps my body climbs UP the screen from y 446 to 331, snaps back to 446 over 0.4 s when the roof starts, then drops to 275 in 0.17 s when the tilt starts (pitch 36 to 68, only after the window was already missing). Three camera moves in one 3 s walk. |
| A3 | `a5_desk_shaftA_end`, `a5_desk_stair_step5`, `a5_land_stair_step5`, `a5_desk_shaftA_out_12` | Desktop: pickaxe in hand, X six times, then follow the marks out (R, Right, ...). Or stand on the staged stair's step 5. | "A pile of chocolate crates stacked ON the meadow." The dug stair reads as raised blocks, not a trench: the wall faces are tall and dark, the treads are thin light bars, and the far end sits higher on the screen than the near end. On landscape the 14-deep end of the stair is drawn above the feet of the trees beside it. This is "looks like I can climb but I can't", inverted. |
| A4 | `a5_desk_shaftA_6`, `a5_port_shaftA_6`, `a5_desk_shaftA_out_01`, `a5_desk_shaftA_out_06`, `a5_land_shaftA_out_06` | Same shaft, 6 deep, then one step into the dug way out. | "Am I in the hole or standing on the grass below a brown rug?" In the shaft (side view, pitch 36) six levels are a brown strip two tiles long lying on the meadow with me at its lower end. One step east the view becomes top-down (pitch 68, sky gone) and I am a cat in a small dashed box beside a dark square. Nothing says "deep". Two views one step apart. |
| A5 | `a5_desk_build_lift` | Desktop: dirt in hand, Z, plan three earth blocks north (they are built), press E to go up one and add the second layer. | "E jumped to +2 and says in red: 'Nothing holds it: put it against a block'." One press of E goes two levels, the earth block is one level, so the second layer cannot be planned this way. I could not build a 2-high earth wall with the keys shown. The answer is a sentence, not a picture. |
| A6 | `a5_desk_room_furnished`, `a5_desk_room_3s`, `a5_land_room_out_03`, `a5_desk_stair_bottom_3s`, `a5_port_stair_bottom_3s` | Room 7x5, 14 deep under the meadow, furniture on its north wall. Passage at the stair's foot, 14 deep. | "A sand pit one step deep." Where to go is clear (dots, cracked block, key). How deep is not: the room 14 down has the same thin dark rim as a hole 1 down, bright floor, bright meadow all round. On the upright phone the hillside terraces beside the 14-deep passage look lower than the passage. Graded A because depth is the thing the owner keeps asking for. |

## B: ugly or weird

| # | Picture | Repro | What a newcomer thinks |
|---|---|---|---|
| B1 | `a5_desk_shaftA_6`, `a5_port_shaftA_6`, `a5_desk_hill_top_N`, `a5_desk_gorge_out_18` | Any hole with a dig-out route: where the route reaches the surface. | "A black L-shaped hole with a stray thin line on the grass." A black wedge, a 1-px vertical line beside it and small squares mark the exit. It reads as a rendering glitch, not as a goal. On landscape a lone thin vertical line stands on the grass above the block (`a5_land_shaftA_out_06`). |
| B2 | `a5_port_title`, `a5_desk_title` | Open the page, wait 2.5 s. | Upright phone: the tagline "Step aside. No, further." is written across a speech bubble ("Did you wate", cut off) and across a character; a scrap of another bubble sticks out under the logo. Desktop: the bubble "Do fish sleep?" is cut by the logo, a stray "1" beside it, the tagline lies over the campfire scene. First thing seen, and it is text on text. |
| B3 | `a5_port_trees_front`, `a5_desk_trees_front`, `a5_desk_hill_mid` | Walk left or right, stop. | "My cat turned into an egg." Sideways the body is a pale oval with a purple outline and one dot. Facing down it is a clear cat (`a5_desk_spawn`). At phone size the egg is the hero for half the play time. |
| B4 | `a5_desk_shaftA_out_12` | Desktop, digging out of the shaft: the frame after the view tilts back from top-down to the side view. | A wide pale grey band lies between the meadow and the mountains where the far land should be. Gone a moment later (`a5_desk_shaftA_end`). One still, not a strip: seen once. |
| B5 | `a5_desk_shaftA_out_01` vs `a5_desk_shaftA_6`; `a5_desk_gorge_in_3s` vs `a5_desk_gorge_out_18` | Shaft: step from the open shaft under its first roof tile. Gorge: dig the first step into its wall. | Still two views: side view with sky, top-down without. The switch happens for one tile of roof. A3's finding "two views" from audits 3 and 4 remains. |
| B6 | `a5_desk_build_result`, `a5_port_build_plan_stone` | Build mode: three earth blocks, three stone blocks on the meadow. | Earth blocks are a beige slab with no grass: "a plank bench". Stone blocks are a dark slab. My name is drawn on the stone slab. On the phone the cat stands on the stone with aim brackets round its feet and "Planned" floating over button A. |
| B7 | `a5_land_shaftA_out_06`, `a5_port_shaftA_6`, `a5_land_room_out_03` | Phones, any dig-out step. | "Dig a step" is written twice: a big cap beside the block and a button on the right. The cap covers the dots. On landscape the Items/Craft/Talk/Menu row covers the room's north wall and the top of the dots. |
| B8 | `a5_land_stair_step5` | Landscape, step 5 of the stair, trees nearby. | Square checker holes punched into two tree crowns that are not in front of me; a light blue sliver and a green bar beside my name. |
| B9 | `a5_desk_house_inside`, `a5_desk_house_far` | House 5x5, two storeys: walk in by the door. | Inside, the ground-floor walls vanish and the upper storey hangs on two posts like a bus shelter; a loose shadow patch lies to the left. Readable, but odd. Upstairs and behind the house read well. |
| B10 | `a5_desk_hill_top_N`, `a5_desk_shaftA_2` | Gorge in the stone mountain; shaft on the meadow. | The block to break is a brown or orange cracked earth block even in grey stone. |
| B11 | log of `s3.py desk B` (toast "Needs a hoe") | Bare hands on the meadow, press X. | "Needs a hoe"? No prompt is shown before, and the answer talks about a hoe when I wanted to dig. A sentence instead of a picture. |

## C: nits

- `a5_port_trees_front`: the hot bar on the upright phone has no numbers and tiny slots (old T5).
- `a5_port_stair_bottom_3s`: 1-px dark vertical lines on the terraces at the lower right, and one through the hot bar.
- `a5_desk_gorge_in_3s`: a white tick between hot-bar slots 8 and 9.
- `a5_desk_build_lift`: aim brackets drawn round my NAME ("[Ava]").
- `a5_desk_shaftA_6`: the welcome text fades as ghost letters over the land.
- `a5_desk_house_behind1`: the aim bracket for the wall is drawn on the roof.
- `a5_desk_shaftA_1`: one level down, "Space Dig into the wall" with the bracket on the grass beside the hole.

## Seen once, no picture kept

Standing on step 5 of the stair after walking down and back up, the WHOLE meadow to the horizon was drawn as brown earth with the trees standing on it. My next run overwrote the file and it did not come back in three more tries. Worth a hunt: it would be the worst picture of the audit.

## Frame cost (`?perf=1`, `HdLook.ms`), upright phone page, CPU x4

| Walk | fps | Frame gap median / max | Readout: script median / worst | HdLook.ms median / p95 / max |
|---|---|---|---|---|
| Stair down | 60 | 16.7 / 16.8 ms | 7.0 / 14.9 ms (land 5.8) | 3.0 / 7.9 / 9.4 |
| Stair up | 60 | 16.7 / 16.8 ms | 2.8 / 9.7 ms (land 5.7) | 1.4 / 7.7 / 9.4 |
| Meadow sprint (two runs) | 60 | 16.7 / 16.8 ms | 0.6 to 3.6 / 1.7 to 5.8 ms | 0.6 to 2.7 / 1.5 to 5.1 / 7.1 |
| Standing | 60 | 16.7 / 16.8 ms | 0.2 / 1.6 ms | 0.2 / 0.8 / 1.5 |

Landscape, CPU x4, meadow sprint: script 3.3 / 9.6 ms, 60 fps. No frame over 33 ms in any burst on any screen. The stair's worst script frame (14.9 ms) is close to the 16.7 ms budget at x4. The phone page draws the land at 195x422 x2. "Laggy on phone" did not show here; this machine cannot rule it out on a real phone GPU.

## Good now

- Getting out works everywhere I tried, with a pickaxe and with bare hands, by keys and by taps. Audit 4's worst fault (no pickaxe, no help) is gone.
- Natural cave and its passage: clear plan, dots, arrow cap (`a5_desk_cave_room_3s`, `a5_desk_cave_passage`).
- The room 14 deep: floor, walls, furniture and the way are clear at a glance, apart from depth (`a5_desk_room_3s`).
- Night under ground reads as well as day (`a5_desk_stair_step12_night`).
- Pond and swimming: pretty and clear (`a5_desk_pond_swim_mid`). Gorge and its fall: readable, guide leads out (`a5_desk_gorge_in_3s`).
- Surface: meadow, terraces, stone mountain, house from outside, upstairs, behind the house (`a5_desk_spawn`, `a5_desk_hill_mid`, `a5_desk_house_upstairs`, `a5_desk_house_behind1`).
- No page error in any run. 60 fps in every burst.

## Top 10

1. A1 the window shuts for 0.85 s while you walk down a covered stair (`a5_desk_stair_down_strip_every9`).
2. A3 a dug stair looks like crates stacked on the meadow (`a5_desk_shaftA_end`, `a5_land_stair_step5`).
3. A4 in a shaft you cannot tell you are in a hole; one step on, the view flips (`a5_desk_shaftA_6`, `a5_desk_shaftA_out_01`).
4. A2 the body slides up, back and down the screen on one stair walk (`a5_desk_stair_down_strip_160`).
5. A6 14 deep looks 1 deep (`a5_desk_room_3s`, `a5_port_stair_bottom_3s`).
6. A5 build mode: E goes up two, "Nothing holds it", no 2-high wall (`a5_desk_build_lift`).
7. B1 the exit mark is a black wedge and a stray line (`a5_desk_shaftA_6`).
8. B2 title: text over text, worst on the upright phone (`a5_port_title`).
9. B3 the cat seen from the side is an egg (`a5_port_trees_front`).
10. B4 a grey band where far land should be, for a moment after the tilt back (`a5_desk_shaftA_out_12`).
