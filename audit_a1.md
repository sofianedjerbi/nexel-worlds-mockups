# Audit A1: Nexel Worlds, HD-2D look, played as a player

Copy of `~/megastructure` (said to be commit abb55b7) in `tmp/fin_A1`, own server on port 7804.
Two worlds: a copy of the owner's `saves/caves`, and a fresh seed-7 world.
Three screens: desktop 1440x900 dpr 2 (keys), phone portrait 390x844 dpr 3 (touch), phone landscape 844x390 dpr 3 (touch).
Input was real: held keys, a held thumb on the stick, taps on the A / B / Menu buttons. Teleports only to reach a start point; items given by the dev tool; the house and the pond were stamped.
No game code was changed. Scripts: `tmp/fin_A1/shots/a1/` (`lib.py`, `s1..s4.py`, `p.py`, `t1.py`).

Pictures: 222 files `tmp/mockups/a1_<screen>_<scene>.png` (`desk`, `port`, `land`). I opened about 75 of them at full size; every finding below names a picture I opened. The others are the in-between frames of the same walks.

Grades: A = ugly / broken / confusing at a glance. B = wrong once noticed. C = polish.

## Verdict

Not finished. Not consistent.

- On the surface, on flat meadow, it is good looking: sky, light, dusk, night, pond, sprites. (`a1_desk_s7_spawn`, `a1_desk_time_dusk`, `a1_desk_time_night`, `a1_desk_house_far`)
- The moment land is over you (cave, tunnel, a 3-deep shaft) the game shows another picture: a black-brown void with a few floating slabs and flat yellow squares. I could not tell floor from wall, and I got stuck on the way out of the owner's cave. The owner's line "every underground view is ununderstandable" is still true on all three screens.
- Going in is a hard pop: the whole meadow is swapped for a brown wall in one frame.
- The title screen is still the old flat look.
- On a phone held upright, Items and Craft open and shut on the same tap.

## The 5 worst things

1. **Under ground is unreadable** (A). `a1_desk_cave_in_04`, `_05`, `_07`, `_09`, `_12`, `a1_desk_cave_room_S`, `a1_port_cave_in_04`, `a1_port_cave_in_09`, `a1_land_cave_in_05`, `a1_land_cave_in_09`, `a1_desk_tunnel_2`, `a1_desk_tunnel_4`, `a1_port_tunnel`.
2. **I could not walk back out of the owner's cave** (A). Walking east from the room I stopped at (-26,-3) for 13 frames and the picture gave no reason. `a1_desk_cave_out_04` (same on portrait and landscape: `cave_out_02..04` end at (-26,-3)).
3. **Entering the cave pops** (A). Meadow to full-screen brown in one 16 ms frame, after a few frames where a brown block eats the left of the meadow. `a1_desk_burst_cave_pop`, `a1_desk_burst_cave_strip`.
4. **Phone portrait: Items and Craft close at once** (A). The panel's "x" opens under the finger and takes the same tap's click. Event trace in `t1.py`: `pointerdown tBag -> bag block -> click bagclose -> bag none`; the same with `craftclose`. Landscape is fine.
5. **Build mode labels sit on top of each other** (A). "Q Down" is drawn over "Space Planned" / "Space Something is in the way" in every build frame. `a1_desk_build_plan_walls`, `a1_desk_build_floor_done`, `a1_desk_build_cursor_up`.

## All findings

### Under ground, caves, digging

| # | Grade | Picture | Where | What a harsh player says | Guess at the cause |
|---|---|---|---|---|---|
| U1 | A | `a1_desk_cave_in_04`, `_05` | owner's stair, (-16,-3) z -3 | "Two steps down and the world is gone. A brown slab fills the screen, the sky is still bright above it." | `underground.js` / `vox_under.js` take over the whole picture once land is over your tile; the surface is drawn as one flat dark slice |
| U2 | A | `a1_desk_cave_in_07`, `_09`, `_12`, `a1_desk_cave_room_S` | stair and room, z -9 to -15 | "Floating brown blocks in black. Which are walls? Which is floor? Where is the room?" | only a ring round the lantern is drawn; cut faces and walls share one dark tone; no outline of the walkable floor; lens blur on top |
| U3 | A | same, and `a1_desk_tunnel_4` | lit floor tiles | "Flat bright yellow rectangles. That is not the tileset." | lantern light multiplied on tops clips to one flat colour |
| U4 | A | `a1_desk_cave_out_04` | (-26,-3) z -13 | "I am stuck and I cannot see why. How do I get out?" | going down I dropped off ledges (z -11, -12, -15); the way up is not straight and nothing shows ledges or the stair |
| U5 | A | `a1_desk_tunnel_2`, `_4`, `a1_port_tunnel` | tunnel I dug 3 under the meadow | "One tile into my tunnel the whole meadow turns black. The meadow is a green line with four flowers." | same full-screen switch for any roof, even one layer of earth |
| U6 | A/B | `a1_desk_shaft_6` | shaft 3 deep, 1 tile wide | "My one-tile hole is drawn as a brown box seven tiles wide. I look like I stand on a wall." | `xray` opens "the whole place" for a dug hole and `vox_section` paints the cut as one big face |
| U7 | B | `a1_desk_tunnel_2`, `a1_port_tunnel` | in the tunnel | "It says Dig into the hill. There is no hill. Then it offers Pick flower, three levels under the flower." | prompt text; reach ignores height for flowers |
| U8 | B | run log of `s3.py C`, `p.py 4` | digging down | "Six presses of X, three levels. Every second press does nothing I can see." | maybe two hits a level; no progress shown, and the "X Dig down" row is gone on the odd presses |
| U9 | B | run log `s3.py C` | bottom of the shaft, ladder in hand | "Needs something taller behind. I am in a pit, everything is taller." | ladder rule checked against the faced tile only |

### Popping and motion (5 s at 60 fps, desktop)

Frame times were steady: 314 and 313 frames in 5.2 s, none over 33 ms. This is a Mac GPU; I could not test phone lag here.

| # | Grade | Picture | Where | What | Guess at the cause |
|---|---|---|---|---|---|
| M1 | A | `a1_desk_burst_cave_pop` (frames 97-100), `a1_desk_burst_cave_strip` | walking into the cave stair | frames 97-98: a brown block has replaced the left half of the meadow with a hard edge; frame 99: the whole screen is brown | the underground picture "grows in from the screen's edges" and then replaces the land in one frame |
| M2 | B | `a1_desk_burst_terrace_pop` (frames 244-247) | sprinting off the gorge rim | the view jumps most of a screen in one frame; the name "Ava" is left about 200 px above the swimmer for a frame | camera snaps to the new height on a fall; the name tag uses the old place |
| M3 | B | same sheet, frames 244-245 | sprinting uphill | the player has drifted from the middle to the top third of the screen | camera lags the climb |

### Surface land

| # | Grade | Picture | Where | What | Guess at the cause |
|---|---|---|---|---|---|
| S1 | B | `a1_desk_spawn`, `a1_desk_mtn_top_east`, `a1_desk_zoom_in_max`, `a1_desk_fight_1` | top and bottom thirds of every surface picture | "The land is blurry. Pixel art should not be blurred." Flowers and furniture six tiles away are smears | the lens blur in `hd.js` / `hd_view.js` (`blur: 1`, "far rows soft") |
| S2 | B | `a1_desk_fight_1`, `a1_desk_fight_wyrm` | furniture six tiles north of me | "The chest and the bed at the horizon are bigger than me." The world ends about seven tiles north | sprites keep their size while the land rows shrink toward the curve's horizon |
| S3 | B | `a1_desk_mtn_canyon_rim`, `a1_desk_mtn_canyon_swimS`, `a1_port_mtn_canyon`, `a1_land_mtn_canyon`, `a1_desk_mtn_downS_2` | gorge, beside the water | "A flat dark slab next to the river. A hole in the map?" | the gorge cut (`xrayGorge`): the cut face has one dark tone and no texture |
| S4 | B | `a1_port_mtn_canyon`, `a1_land_mtn_canyon`, `a1_desk_mtn_downS_2` | gorge walls and terrace fronts | "Checkerboard patches on the rock." | the 2x2 checker fade of the cut is left showing |
| S5 | B | `a1_desk_mtn_canyon_rim`, `a1_desk_mtn_canyon_swimN` | river in the gorge | "The river is squares of three blues, like a staircase of water." | depth tint in whole steps a tile, no blend between tiles |
| S6 | B | `a1_desk_mtn_north_top` | plateau, gorge to the south | "I walked off a nine-level drop. It looked like steps." Lost three hearts | nothing marks a deadly drop |
| S7 | B | `a1_desk_mtn_foot_south`, `a1_desk_cave_in_02` | terraces east and west of you | "The terraces at the side look like long stripes running down the screen." | strong perspective plus the curve on north-south step edges |
| S8 | C | `a1_desk_mtn_top_east`, `a1_port_spawn` | every stone step | a busy zigzag fringe on each rim; small pale caps on single tiles | `voxpaint` rim and `cap` |
| S9 | B | `a1_desk_house_upstairs`, `a1_desk_house_stairs` | two or three levels up | "A grey band appears under the far hills when I climb a stair." | horizon haze tied to your height |
| S10 | C | `a1_desk_spawn`, `a1_port_spawn` | owner's world | "You wake up in a meadow" while standing on a grey stone mountain | spawn point choice |

### Houses, trees, hiding

| # | Grade | Picture | Where | What | Guess at the cause |
|---|---|---|---|---|---|
| H1 | B | `a1_desk_house_inside`, `a1_port_house_inside` | one step inside the door | "My two-storey house is now a low wooden tray. Roof, upper floor and walls are gone." | everything over your head is left out, the walls cut to a stub |
| H2 | B | `a1_desk_tree_behind` | one tile north of a tree | "The tree is a checker ghost and I cannot see myself either." Only the name and a shadow show | the tree is dithered but the player is still drawn under it |
| H3 | B | `a1_desk_house_front`, `a1_desk_house_behind1` | aim mark near a house | "The aim brackets are split: half at the door, half up on the roof." | the 3D aim box (`vox_mark`) drawn at two heights |
| H4 | C | `a1_desk_house_far` | the house | the roof is a flat shingle slab, the house reads as a brown box | roof blocks are flat cells |
| H5 | C | `a1_desk_house_behind1` | behind the house | the ghost of the player through the roof is faint | dither strength |
| H6 | B | `a1_desk_furniture_chest_open` | facing a chest | "The chest sits on my head." | draw order of a thing one tile north of you |

### Screen text, menus, phone

| # | Grade | Picture | Where | What | Guess at the cause |
|---|---|---|---|---|---|
| T1 | A | `a1_desk_build_plan_walls`, `a1_desk_build_floor_done`, `a1_desk_build_cursor_up` | build mode | the "Q Down" cap is drawn over the "Space ..." row; two texts collide | E / Q labels placed without regard to the Space / X rows |
| T2 | A | trace in `t1.py`; `a1_port_ui_bag` shows where the x is | phone portrait | "Items opens and shuts at once. Same for Craft." | the panel's close button appears under the Items / Craft button; `tapButton` acts on pointerdown, the click that follows lands on `#bagclose` / `#craftclose` |
| T3 | B | `a1_desk_zoom_in_max`, `a1_desk_zoom_out_max` | zoomed in or out | the "Space Pick flower" label covers the player | label anchored on the player's tile |
| T4 | B | `a1_desk_title`, `a1_port_title` | title screen | "The title shows a flat top-down game. The game is not that." | title scene still the classic painter |
| T5 | C | `a1_port_spawn`, `a1_port_tunnel` | phone portrait | hot bar slots are tiny, no numbers, counts hard to read | bar scaled to the width |
| T6 | C | `a1_port_pond_swim`, `a1_land_spawn` | name tag | "Ava" overlaps the aim mark or a tree | no collision check for the name |
| T7 | C | `a1_desk_house_front` | at a door | Space offers "Take down door (slow)" as the main action | prompt order |

### Things that were fine

- Meadow by day, dusk, night, dawn: `a1_desk_s7_spawn`, `a1_desk_time_dusk`, `a1_desk_time_night`, `a1_desk_time_dawn`.
- Pond and swimming: `a1_desk_house_far`, `a1_port_pond_swim`.
- Menus, crafting, inventory, settings in the panel's own pixel style: `a1_desk_ui_menu`, `a1_desk_ui_craft`, `a1_land_ui_bag`, `a1_land_ui_craft`, `a1_port_ui_settings`, `a1_port_ui_menu`.
- Fighting, death and waking again: `a1_desk_fight_1`, `a1_desk_fight_wyrm`, `a1_desk_death_0`, `a1_desk_respawn`.
- Zoom steps and the zoom sign: `a1_desk_zoom_in_max`, `a1_desk_zoom_out_max`.
- Walking up terraces from the east: `a1_desk_mtn_upE_1`.

## Not covered or not proven

- Phone lag: not testable on this machine. Headless Chrome on the Mac GPU held 60 fps in all three sizes.
- "Long time to trigger x-ray": not seen on desktop; the cut came within a few frames of going under cover. Not timed on a phone.
- Build mode: my cursor moves were blind, so roof, stairs and ladder plans were mostly refused ("Needs a wall under it", "Stairs go up to something higher"). Walls, floor and one ladder were built. Roof and stairs by build mode are not judged.
- Sleeping in a bed and Esc closing a chest: my script was still at the chest; not judged.
- Climbing out of the shaft: I did not find a way with a ladder (U9); I did not try digging steps.
- Phone landscape and portrait: the house, night and build pictures were taken but only the portrait ones named above were opened.
