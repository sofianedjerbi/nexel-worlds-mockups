# Audit A3: Nexel Worlds, HD-2D look, played a third time from scratch

Copy of `~/megastructure` at commit 754c349 in `tmp/fin_A3`, own server on port 7808. No game code changed.
Two worlds: a copy of the owner's `saves/caves`, and a fresh seed-7 world.
Three screens: desktop 1440x900 dpr 2 (keys), phone portrait 390x844 dpr 3 (touch), phone landscape 844x390 dpr 3 (touch).
Input was real (held keys, a held thumb, taps on A / B / Items / Craft). Teleports only to reach a start point. Staged with the dev tools: the house, the pond, the 4-high terraces, the 24-deep stair and room, the tunnel with a branch and a room, the 3-high ledge for build mode.
Scripts: `tmp/fin_A3/shots/a3/` (audit 2's `lib.py`, `s1..s3.py`, `p.py`, `t1.py`, `n.py`; new `m.py` parts NM, SL, BM, OP, OD, NC, DG, HZ; `s5.py` + `s5an.py` for the bursts). Logs `b1..b5.log` there.

Pictures: 511 files `tmp/mockups/a3_<screen>_<scene>.png`. I opened about 65 at full size. Every finding names a picture I opened. The rest are in-between frames of the same walks.

Grades: A = ugly / broken / confusing at a glance. B = wrong once noticed. C = polish.

## Verdict

**Not finished. The best of the three audits under ground. Still two pictures, not one.**

- **Good looking?** Meadow, mountain top, gorge (now real cliffs), a tunnel under the meadow by day and night, the 24-deep room, two players and a boar in one room, the house upstairs: yes.
- **Consistent?** No. On the surface the eye looks at a horizon with a sky. Under a cave roof it swings to almost straight down, the sky goes, sprites look squashed from above. In a shaft I dug myself (open to the sky) it does not swing. In a house it does not swing. So the game has two views and which one I get depends on the kind of hole. The owner asked for "the same consistent hd-2d view everywhere".
- **Understandable under ground?** Where am I: yes now. Where are the walls: mostly yes. How do I get out: **no** in the owner's cave. The stair I came down turns grey ("not for you"), a ladder is refused, the pickaxe only digs level. I never got out, on any screen.
- **New and bad:** about 60 tiles from where I woke up the land turns flat sky blue. I walked there with the keys; it is not a teleport artefact.
- Phone lag: not testable here (headless Chrome on a Mac GPU: 60 fps on all three sizes, no frame over 33 ms).

## The 5 worst things

1. **The land turns sky blue far from where I woke** (A, new). `a3_desk_hz_walk_7` (sprinted there by keys: the east third of the screen fades to pale blue), `a3_desk_hz_tp_11s` (70 tiles further: the whole meadow is flat light blue, still so after 11 s and after walking on), `a3_desk_mtn_foot_east` (the owner's world at (55,-30): blue meadow, fogged mountain), `a3_desk_nc_c_01`, `a3_desk_nc_c_03` (natural cave at (-53,48): the whole screen is light blue with lilac slabs: no floor, no wall, no cave), `a3_desk_nc_b_01`, `_03`, `_05` (cave at (11,-57): stone washed pale blue). Same with shader off and with the curve off (`a3_desk_dg_c_curve_off`). The phone runs woke nearer and show the same cave without it (`a3_port_nc_b_03`). Guess: the horizon haze of the engine is measured from where you joined (or the map's middle), not from you.
2. **Still no way out of the owner's cave, and now the picture says "you can"** (A). At (-26,-3) z -13 the stair I walked down a minute ago is drawn one tile east of me, grey-blue. With a ladder the aim turns red: "Needs something taller behind" (`a3_desk_op_2_ladder_1`, same on portrait and landscape: logs of `m.py OP`). With a pickaxe the game offers only "Dig on" and "Dig down": I dug 8 tiles east, level, 13 under the grass, and was still in (`a3_desk_od_end`). Cause: the stair's own floor is over my head on that tile (two floors, -13 and -11), so a ladder has no head room; nothing digs upward.
3. **Two views** (A/B). Horizon view: `a3_desk_cave_in_03`, `a3_desk_sl_dig_6`, `a3_desk_house_inside`. Top-down view two steps later: `a3_desk_cave_in_05`, `a3_desk_tunnel_3`, `a3_desk_two_room_creature`. Mid-gorge the same swing (`a3_desk_mtn_canyon_swimS` has a sky, `a3_desk_mtn_downS_2` has none).
4. **Under ground most of the screen is a dull striped funnel** (B). Owner's cave: about 60% of the desktop and 70% of the phone is khaki strata; the room I am in is a small box in the middle (`a3_desk_cave_room_S`, `a3_port_cave_in_09`, `a3_land_cave_out_04`). Staged earth: brown "planks" that read as wooden boards (`a3_desk_deep_room`, `a3_port_deep_room`). Cave floors are still flat colour with no tile art: pale yellow sheet in the natural caves, with soft blurred edges (`a3_desk_nc_a_03`, `a3_port_nc_b_03`), flat brown in the owner's cave.
5. **The fall into the gorge still jumps and still costs hearts with no warning** (B). Burst, frame 241: 56% of the screen changes in one frame, after the player has drifted to the top quarter (`a3_desk_burst_terrace_pop`). Lost 3 to 7 hearts walking off rims that look like steps (`a3_desk_mtn_downS_2`, `a3_desk_terr_gorgeS_4`).

## Compared with audit 2, finding by finding

| A2 | What | Now |
|---|---|---|
| Worst 1 / N1 | land in front sunk to my level, pale sheet / lawn at my feet | **Fixed**. The land toward the camera is a stepped funnel of cut rock (`a3_desk_cave_room_S`, `a3_desk_deep_room`, `a3_port_deep_room`). Replaced by worst thing 4 (big and dull) |
| Worst 2 / N2 | flat blue hole when deep | **Fixed** at 24 deep on all three screens (`a3_desk_deep_room`, `a3_port_deep_room`). But see worst thing 1: a whole blue screen for another reason |
| Worst 3 / N3 | other player and creature drawn on the surface | **Fixed**. Bo and the tusker stand in the room (`a3_desk_two_room_creature`) |
| Worst 4 / N4 | gorge bank ironed flat | **Fixed**. Real cliffs (`a3_desk_mtn_canyon_swimS`, `a3_desk_terr_gorgeS_4`, `a3_desk_terr_gorgeW_3`) |
| Worst 5 / N7 / U4 | stuck in the owner's cave | **Remains**, see worst thing 2. Better: the floor overhead is a checker slab and the lost stair is grey, so "not reachable" is said. Worse: a ladder and a pickaxe do not help either |
| N5 | comb of dark spikes on cuts | **Fixed** (strata, no spikes: `a3_desk_cave_room_S`) |
| N6 | tower sliced to a stub north of it | **Remains** (B) (`a3_desk_tall_north_2`: a 12-high tower is a low grey U, nothing says it is cut) |
| N8 | ladder pokes into the sky | **Fixed** (`a3_desk_sl_ladder_N`, `a3_port_sl_ladder_N`) |
| N9 | flowers 24 down | **Remains** in staged land only (`a3_desk_deep_room`: "Pick flower"); in a tunnel I dug myself it is gone (`a3_desk_tunnel_3`: "Dig on"). Not a player's case |
| N10 | lantern lights the grass above | **Fixed** (`a3_desk_tun_night`) |
| N11 | pond black behind the death card | not re-opened |
| N12 | spawned creatures float on rock | **Remains** as a dev-spawn artefact (`a3_desk_cave_night`: tusker and wisp float on the strata) |
| U3 | flat yellow / orange floors | **Remains** (B) (worst thing 4) |
| U6 | shaft drawn too wide | **Remains** (B): a 1-tile shaft 6 deep is a brown box 3 wide that reads as a crate standing on the lawn (`a3_desk_sl_dig_6`, `a3_port_sl_ladder_N`) |
| U7 | wrong prompts under ground | **Mostly fixed**: "Dig into the wall", "Dig on". Left: "Chop tree (slow)" with the aim on a tunnel wall 4 under the tree (`a3_desk_tun_in_05`); "Needs something taller behind" as the Space prompt on open meadow with a ladder in hand (log of SL) |
| U8 | two presses of X a level | **Fixed**: 6 presses, 6 levels, keys and touch |
| U9 | ladder refused in a pit | **Fixed** in a shaft: placed at the first try, climbed out, keys, portrait and landscape. Not in the owner's pocket |
| M1 | cave entry pops | **Stays fixed**, see bursts |
| M2, M3 | jump on a fall, player drifts up | **Remains** (worst thing 5) |
| S1 | land blurred | **Better** near the player; far terraces and cave floors still smeared (`a3_desk_cave_in_03`, `a3_desk_mtn_canyon_swimS` right side) |
| S2 | far things bigger than me | **Remains** (B) (`a3_desk_zoom_in_max`: a sunflower on the horizon is three times my height) |
| S5 | river in squares | **Remains** (C) (`a3_desk_terr_gorgeW_3`) |
| S6 | deadly drop looks like steps | **Remains** (B) |
| S7 | terraces at the sides are long stripes | **Remains** (B) (`a3_desk_cave_in_03`, `a3_desk_stone_close_cave`) |
| S8, S9, S10 | rim fringe, grey band, "meadow" on stone | S10 remains (`a3_desk_spawn`); others not re-judged |
| H1 | house a low tray inside | **Better / fine** (`a3_desk_house_inside`, `a3_land_house_inside`) |
| H2 | behind a tree: checker ghost | **Changed, still odd** (B): the tree shrinks to a stub at my feet, I seem to stand on a bush (`a3_desk_tree_behind`) |
| H3 | aim brackets at two heights | **Remains** (B) (`a3_desk_house_inside`, `a3_desk_bm_under_roof`, `a3_land_house_inside`) |
| H4, H5 | flat roof, faint ghost | **New form** (B): behind the house I am drawn solid on top of the roof, as if standing on it (`a3_desk_house_behind1`) |
| H6 | chest on my head | **Remains** (B) (`a3_desk_furniture_chest_open`) |
| T1, T2, T3 | build labels, Items / Craft shut at once | **Stay fixed** (trace of `t1.py port`: both stay open) |
| T4 | title is the old flat look | **Remains** (B) (`a3_desk_title`) |
| T5 | tiny hot bar on portrait | **Remains** (C) (`a3_port_spawn`) |

## New findings

| # | Grade | Picture | Where | What a harsh player says | Guess at the cause |
|---|---|---|---|---|---|
| X1 | A | `a3_desk_hz_walk_7`, `a3_desk_hz_tp_11s`, `a3_desk_mtn_foot_east`, `a3_desk_nc_c_03` | about 60 tiles or more from where I woke | "The grass turned into ice. Then everything is light blue." | haze by distance from the join point, not from the player |
| X2 | A | `a3_desk_op_2_ladder_1` | owner's pocket (-26,-3) | "The stairs are right there. Red box. Needs something taller behind?!" | two floors on one tile; ladder needs head room; no dig-up |
| X3 | A/B | `a3_desk_cave_in_03` vs `_05`, `a3_desk_sl_dig_6` vs `a3_desk_tunnel_3` | any roof of land | "The camera flips to a map view. My cat is seen from above. In my own shaft it does not." | the 68° tilt only for roofed land, not for open pits or houses |
| X4 | B | `a3_desk_cave_room_S`, `a3_port_cave_in_09` | owner's cave | "Two thirds of my phone is brown stripes." | the funnel is opened wide and painted in one calm tone |
| X5 | B | `a3_desk_tunnel_3`, `a3_land_tunnel`, `a3_desk_sl_dig_6` | pit dug in flat meadow | "Is that a hole or a crate? There is an L-shaped block hanging under it." These are the weird corners now | the cut's wedge toward the camera is drawn as brown steps below the pit's rim |
| X6 | B | `a3_desk_nc_b_night` | natural room at night | "My cat is cut in two: a square head, legs apart." | sprite clipped by the pit's rim |
| X7 | B | `a3_desk_mtn_downS_2` | gorge, tilted | "Brown earth wall cut into a grey stone mountain." | cut face painted as earth strata whatever the rock |
| X8 | C | `a3_desk_two_room_creature`, `a3_desk_tun_night` | tunnel near the bottom of the screen | the hot bar and hearts sit on the passage I walk | HUD over the pit |
| X9 | C | `a3_desk_nc_a_03` | natural corridor | a grey ramp crosses over the lit corridor and hides its far end | unreachable floor drawn over the reachable one |

## Asked for specifically

- **Natural caves** (they are straight 2-wide corridors and rooms, floor 3 under the top): walked in, round and out at (25,32), (11,-57), (-53,48). Never stuck. On desktop the pictures are spoiled by X1 (`nc_b` washed blue, `nc_c` a blue screen). On the phone the (11,-57) room is clear: grey stone, dark walls, one flat pale-yellow floor (`a3_port_nc_b_03`).
- **Shaft 6 down and out with a ladder**: works on keys, portrait, landscape. 6 presses, ladder at the first try, out. Looks like a crate (X5).
- **2-storey house up and down**: works, looks good (`a3_desk_house_inside`, `a3_desk_house_upstairs`).
- **Build mode with the real controls** (B, arrows, Space held for a row, E / Q): walls, a roof on them and beside them, a 2-step stair up a 3-high ledge, a ladder against it: all planned, all built by the character, no refusals (`a3_desk_bm_result`, `a3_desk_bm_under_roof`).
- **Out of the owner's pocket with a ladder**: no (X2).
- **Two players, a creature under ground**: right place now.
- **Night under ground**: good (`a3_desk_tun_night`, `a3_desk_cave_night`, `a3_desk_nc_b_night`).
- **24 deep**: readable, no hole, no false lawn.

## 60 fps bursts (desktop, screencast frames; share = pixels changed against the frame before, HUD left out)

| Run | Frames | rAF | Share a frame: median / p95 / max | Pop |
|---|---|---|---|---|
| into the owner's cave (the tilt) | 328 in 6.7 s | 16.7 ms, none over 33 | 1.4% / 6.4% / 15.8% | none: 20 frames of 11 to 16% while the eye swings (frames 105 to 125), `a3_desk_burst_cavein_pop` |
| back out of it (stuck at the pocket after 1 s) | 215 | 16.7 ms | 0% / 3.6% / 4.7% | none |
| house door in and out | 129 in 2.3 s | 16.7 ms | 1.0% / 2.2% / 4.0% | none |
| staged tunnel in and out | 301 in 5.1 s | 16.7 ms | 2.1% / 8.1% / 11.2% | none: entry 6 to 8% for 15 frames, exit 9 to 11% for 30 |
| sprint up the terraces into the gorge | 305 in 5.2 s | 16.7 ms | 4.3% / 15.4% / **56.1%** | **yes**: frame 241, then 18 to 27% for 11 frames, `a3_desk_burst_terrace_pop` |

No flash under ground in any run.

## Not covered or not proven

- Phone lag: not testable on this machine.
- The blue land (X1): seen on desktop only, because only there did I go 60 tiles from the join point. Not proven on a phone.
- The natural caves on desktop were only seen through X1.
- Death card, sleeping, S8, S9, N11: taken, not re-opened.
- One slip of mine: I ran the burst analysis with `uv run --with pillow` from `~/megastructure` as the common brief says; uv rebuilt and re-installed the project package in that folder's venv (1 package). Nothing else there was touched.
