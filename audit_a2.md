# Audit A2: Nexel Worlds, HD-2D look, played again from scratch

Copy of `~/megastructure` (said to be commit 5a2c709) in `tmp/fin_A2`, own server on port 7806. No game code changed.
Two worlds: a copy of the owner's `saves/caves`, and a fresh seed-7 world.
Three screens: desktop 1440x900 dpr 2 (keys), phone portrait 390x844 dpr 3 (touch), phone landscape 844x390 dpr 3 (touch).
Input was real (held keys, a held thumb, taps on A / B / Items / Craft). Teleports only to reach a start point. Staged with the dev tools: the house, the pond, the 4-high terraces, the 24-deep stair and room, the long tunnel with a branch and a room.
Scripts: `tmp/fin_A2/shots/a2/` (`lib.py`, `s1..s4.py`, `p.py`, `t1.py` from audit 1; `n.py` new: parts T, T2, D, L, X, C1, CX, S6, P2, scan). Logs: `batch1..5.log` there.

Pictures: 438 files `tmp/mockups/a2_<screen>_<scene>.png`. I opened about 60 at full size, plus two 4x crops. Every finding names a picture I opened. The rest are the in-between frames of the same walks.

Grades: A = ugly / broken / confusing at a glance. B = wrong once noticed. C = polish.

## Verdict

**Not finished. Much better than audit 1. Still not one consistent picture.**

- Good looking now: meadow at every hour, a tunnel or shaft dug under the meadow (an open trench: I understood it at once), the owner's cave stair on the way down, night under ground, menus, the death card.
- The open pit works for shallow places. It breaks down when the pit is deep or when the land round you is high: the land toward the camera is flattened to your level and fills half the screen as a pale blurred sheet (the owner's cave) or as a lawn 24 levels too low (deep room), and on the phone held sideways a flat blue hole fills a quarter of the screen.
- The gorge has the same fault: the 9 to 12 high bank beside the river is drawn flat at water level with the terraces' rims left as squiggly lines. "Why is this cut?" is still the right question.
- Other players and creatures in the same room under ground are drawn up on the grass, not in the room.
- I still cannot walk out of the owner's cave the way I came, and the picture still does not show why.
- Phone lag: not testable here (headless Chrome on a Mac GPU held 60 fps on all three sizes, zero frames over 33 ms).

## The 5 worst things

1. **The pit lies about the land in front of you** (A). Land south of you is drawn sunk to your own level. Owner's cave at z -15: 60% of the screen is a pale blurred stone sheet (`a2_desk_cave_room_S`, `a2_port_cave_in_09`: the whole lower half of the phone). 24 deep: the meadow and a tree are drawn at my feet (`a2_desk_deep_in_06`, `a2_port_deep_room`). So yes: 20+ levels deep the surface fills half the screen, and it is at the wrong height.
2. **A flat blue hole in the screen when deep** (A). `a2_land_deep_room` (bottom-right quarter is flat blue), `a2_desk_deep_room` (a blue band across the bottom 18%).
3. **Others in my cave are drawn on the surface** (A). Bo stood at (-7,-12) z -4, one tile from me in the same room; he is drawn up on the north wall. The tusker at (-7,-10) z -4, south-east of me, is drawn north-east on the rim. Server heights checked in the log (`batch3.log`, part P2). `a2_desk_two_room_creature`, `a2_desk_tun_room_two`, and from Bo's own screen `a2_desk_tun_two_other` (tusker and chicken on the rim).
4. **The gorge bank is flattened** (A). `a2_desk_mtn_canyon_swimS`, `a2_desk_terr_gorgeW_3`, `a2_desk_terr_gorgeS_4`, `a2_land_mtn_canyon_swim`, `a2_desk_mtn_downS_2`: land that is 9 to 12 levels over the water is a flat pale plain at water level, with ghost outlines of its terraces and rows of dark spikes along the cut.
5. **Still stuck in the owner's cave, and the picture does not say why** (A/B). Walking east from the room I stop at (-26,-3) z -13 on all three screens (`a2_desk_cave_out_04`, `a2_land_cave_out_04`). The truth (part CX, `surfacesAt`): that tile has two floors, the stair I came down at z -11 and a passage under it at z -13; the stair has a 2-level drop at x -29 (z -12 to -14) that cannot be climbed. The picture shows me in a small box with what looks like one ordinary step to the east.

## Compared with audit 1

| A1 | What | Now |
|---|---|---|
| U1, U2 | under ground a black-brown void, floating slabs | **Fixed** as a void: the stair, the walls and the way in are drawn (`a2_desk_cave_in_04`, `_07`, `_11`). Replaced by worst things 1 and 2 |
| U3 | flat bright yellow / orange floor | **Remains** (B). Tunnel floor is a flat pale yellow bar, cave floor flat orange, no tileset texture (`a2_desk_tunnel_4`, `a2_desk_tun_branchN_2`, `a2_desk_cave_room_S`) |
| U4 | stuck on the way out of the owner's cave | **Remains** (worst thing 5) |
| U5 | one tile into a tunnel the meadow turns black | **Fixed**. An open trench in the meadow (`a2_desk_tunnel_4`, `a2_desk_tun_in_04`, `a2_port_tunnel`) |
| U6 | 1-tile shaft drawn 7 wide | **Mostly fixed**. Now about 2 wide with a notch toward the camera (`a2_desk_shaft_6`, `a2_desk_shaft6_bottom`) |
| U7 | "Dig into the hill", "Pick flower" under ground | **Remains** (B). "Dig into the hill" in a shaft in flat meadow; "Pick flower" 24 levels down (`a2_desk_deep_in_03`; logs of s3 C and n D) |
| U8 | two presses of X a level, nothing shown on the odd one | **Remains** (B). 12 presses for 6 levels (log of n S6) |
| U9 | ladder refused in a pit | **Half fixed**. In a 6-deep shaft the ladder is refused facing north, south and west, accepted facing east; I climbed out. The placed ladder is drawn sticking three tiles up into the air over the meadow (`a2_desk_shaft6_ladder`) |
| M1 | entering the cave pops to a full brown screen | **Fixed**. Biggest frame change is small; the pit opens step by step (`a2_desk_burst_cave_pop`, `a2_desk_burst_cave_strip`) |
| M2, M3 | view jumps on a fall, player drifts to the top of the screen | **Remains** (B). Frames 242-245: player in the top quarter, then the view jumps half a screen (`a2_desk_burst_terrace_pop`) |
| S1 | land blurred top and bottom | **Remains** (B). Worst zoomed in and on cut land (`a2_desk_stone_close_1`, `a2_desk_cave_room_S`) |
| S2 | far things bigger than me, world ends 7 tiles north | **Remains** (B) (`a2_desk_fight_wyrm`, `a2_desk_shaft_6`, `a2_desk_house_inside`) |
| S3, S4 | dark slab and checker patches in the gorge | Checker **gone**. The slab is now the flattened bank (worst thing 4) |
| S5 | river in squares of three blues | **Remains** (B/C) (`a2_desk_mtn_canyon_rim`) |
| S6 | deadly drop looks like steps | **Remains** (B). Lost hearts again walking into the gorge (`a2_desk_terr_gorgeS_4`) |
| S7 | terraces at the sides are long stripes | **Remains** (B) (`a2_desk_mtn_canyon_swimS` right side, `a2_desk_cave_in_07`) |
| S8 | zigzag fringe on stone rims | **Remains** (C) (`a2_desk_stone_close_1_crop`) |
| S9 | grey band under the hills when up high | **Remains** (B) (`a2_desk_house_upstairs`, `a2_desk_tall_top_S`) |
| S10 | "You wake up in a meadow" on a stone mountain | **Remains** (C) (`a2_desk_spawn`) |
| H1 | house is a low tray when inside | **Better**. Upper floor band and roof stay behind you (`a2_desk_house_inside`, `a2_port_house_inside`) |
| H2 | behind a tree: checker ghost, no player | **Remains** (B) (`a2_desk_tree_behind`) |
| H3 | aim brackets split in two heights | **Remains** (B) (`a2_desk_house_inside`, `a2_desk_house_behind1`) |
| H4, H5 | flat roof, faint ghost through the roof | **Remain** (C) (`a2_desk_house_behind1`) |
| H6 | chest sits on my head | **Remains** (B) (`a2_desk_furniture_chest_open`; same with a rock in `a2_desk_stone_close_1`) |
| T1 | build labels on top of each other | **Fixed** (`a2_desk_build_plan_walls`) |
| T2 | phone portrait: Items and Craft shut at once | **Fixed**. Both stay open (trace in `batch4.log`, `a2_port_ui_bag`) |
| T3 | label covers the player when zoomed | **Fixed** zoomed out (`a2_desk_zoom_out_max`); zoomed in a rock and its brackets cover the face (`a2_desk_stone_close_1`) |
| T4 | title screen is the old flat look | **Remains** (B) (`a2_desk_title`) |
| T5, T6, T7 | tiny hot bar on portrait, name over the aim mark, "Take down wall" as main action | **Remain** (C) (`a2_port_spawn`, `a2_port_house_inside`) |

## New findings

| # | Grade | Picture | Where | What a harsh player says | Guess at the cause |
|---|---|---|---|---|---|
| N1 | A | `a2_desk_cave_room_S`, `a2_port_cave_in_09`, `a2_desk_deep_in_06`, `a2_port_deep_room` | any pit deeper than about 6 | "The hill in front of me has sunk to my feet. Half my screen is a grey smear / a lawn that is not there." | the cut toward the camera goes down to my level over a wide wedge; what is left is drawn as flat tops with no fronts, under the lens blur |
| N2 | A | `a2_land_deep_room`, `a2_desk_deep_room` | 24 deep | "A blue hole in the game." | the camera sits 24 levels down; the screen's lower part is past the land that is drawn, the clear colour shows |
| N3 | A | `a2_desk_two_room_creature`, `a2_desk_tun_room_two`, `a2_desk_tun_two_other` | a room 4 under the meadow | "My friend and the boar are on the grass, the prompt says Punch tusker." | other bodies are placed at the surface height of their tile, not at their own z, when the tile has a roof |
| N4 | A | `a2_desk_mtn_canyon_swimS`, `a2_desk_terr_gorgeW_3`, `a2_desk_terr_gorgeS_4` | gorge floor | "The mountain beside the river is ironed flat, with scribbles on it." | the gorge cut (`xrayGorge`) slices every tile toward the camera to the floor's level; the tops keep their rim art |
| N5 | B | `a2_desk_cave_room_S_crop`, `a2_desk_cave_room_S`, `a2_desk_deep_room`, `a2_desk_terr_gorgeS_4` | edges of every cut | "Rows of dark spikes, like a comb, on the rock." These are the weird corners now | the cut's side faces drawn as thin slivers between terraces of the wedge |
| N6 | B | `a2_desk_tall_north_2` | 2 tiles north of a 12-high tower | "The tower became a low grey box." No sign it is a cut | cut tops take a plain stone face; nothing marks a slice |
| N7 | B | `a2_desk_cave_out_04`, `a2_land_cave_out_04` | (-26,-3), two floors on one tile | "I am in a little box. Is that a step? Why can't I go up?" | two floors stacked; the upper one reads as beside, not above |
| N8 | B | `a2_desk_shaft6_ladder` | ladder in a 6-deep shaft | "The ladder pokes three tiles into the sky." | ladder drawn full length from the pit floor without the cut |
| N9 | B | `a2_desk_deep_in_03`, `a2_desk_deep_in_06` | staged stair | flowers sit on steps and on the floor 24 down | the surface's flower kept on a tile whose floor is now far below (staged with `dev_col`; may not happen by digging) |
| N10 | C | `a2_desk_tun_night` | lantern in a tunnel | the light ring also lights the grass 4 levels above | light drawn by screen distance |
| N11 | C | `a2_desk_death_0` | death card | the pond turns black behind the card | grey filter on the water pass |
| N12 | C | `a2_desk_cave_creature` | owner's cave | the creatures I put beside me cannot be seen (they went into rock; a faint ghost shows at night, `a2_desk_cave_night`) | dev spawn into solid tiles; not judged further |

## Asked for specifically

- **Tall terraces on the mountain**: walking on the surface past them, nothing is notched wrongly (`a2_desk_terr_E_4`, `a2_desk_mtn_top_east`). On the gorge floor every terrace toward the camera is flattened (N4). North of a staged 12-high tower it is sliced to a stub (N6).
- **20+ levels deep**: yes, the surface fills half the screen, drawn at my level, and a blue hole opens (N1, N2).
- **Two players in the same cave**: the other is drawn on the surface (N3).
- **A creature under ground**: same (N3).
- **Stone up close**: blurred when zoomed in; rims are a wobbly dark fringe; the comb spikes on cuts (S1, S8, N5).
- **60 fps bursts**: 313 frames in 5.2 s, median 16.7 ms, none over 33 ms, both runs. Cave entry no longer pops. The fall into the gorge still jumps.
- **Natural cave from the generator**: seed 7 has none within 120 tiles of spawn. The owner's world has roofed ground near (25,32), (11,-57) and (-53,48); I found these late and did not walk them. Not judged.
- **Stairs, roof and ladder in build mode**: again refused by my blind cursor moves ("Needs a wall under it", "Stairs go up to something higher"). Walls and floor built. Not judged.
- **Phone lag**: not testable on this machine.

## Things that were fine

Meadow by day, dusk, night, dawn (`a2_desk_time_night`). Tunnel and trench views by day and night (`a2_desk_tun_in_04`, `a2_desk_tun_branchN_2`, `a2_desk_tun_night`). Deep room at night with a torch (`a2_desk_deep_room_night_torch`). The stair down into the owner's cave (`a2_desk_cave_in_04`, `_07`). Two-storey house in and up (`a2_desk_house_inside`, `a2_desk_house_upstairs`). Menus, inventory on the phone, death card (`a2_desk_build_result`, `a2_port_ui_bag`, `a2_desk_death_0`). Fight (`a2_desk_fight_wyrm`). Build labels (`a2_desk_build_plan_walls`).
