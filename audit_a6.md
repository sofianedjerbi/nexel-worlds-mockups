# Audit A6: Nexel Worlds, HD-2D look, walking in and out of holes

Worktree `agent-a5d1ecf79fb71c436` (main at `e88dc0e`), own server on port 7846, fresh world `audit6` seed 12 with `--dev`, saves under `tmp/a6`. No game code changed, nothing committed, nothing left running (checked with `pgrep`).
Screens: desktop 1440x900 (keys), phone upright 390x844 DPR 3 (touch), phone landscape 844x390 DPR 3 (touch). One headless browser at a time.
Scripts: `<worktree>/shots/a6/` (a5's `s1`-`s4`, `p.py`, `strips.py` repointed; new `n1.py` parts ST PIT SH CAVE TUN BEH YARD NIGHT HILL, `n2.py` parts T X BP BRK TWO; `sheet.py` contact sheets and crops). Burst frames and logs: `tmp/a6/burst_*`.
Pictures: 329 files `tmp/mockups/a6_*.png`. I opened 24: contact sheets (`a6_sheet_*`, four pictures at half size each), strips of 8 frames from real walking (`a6_*_strip_every*`), and 3x crops (`a6_crop_*`) of the worst spots. Every finding names one I opened.

Limits, said plainly:
- Most pictures were judged on half-size sheets; only the crops and three single shots at full size.
- Land was staged with `dev_col` and `dev_stamp`. The worst new fault (N1) was re-made with `dev_stamp` levels, the tests' own way, to rule out my staging.
- Placed walls were put with the game's `place` action from a script (two earth blocks high, four stone blocks high), not through build mode. "Standing ON the wall" was a teleport: not judged.
- Not done: swimming and the gorge on the phones (desktop only), two players on the phones, the house on the phones, a torch placed on a wall (the game refuses: "torch cannot be placed").
- Frame cost is headless Chrome on a Mac GPU at CPU x4. It says nothing about a real phone GPU.
- a5's B8 (checker holes in tree crowns) was not re-staged with trees beside the stair: untested.

## 1. Re-test of a5

| a5 | Now | Picture | What I saw |
|---|---|---|---|
| A1 window shuts 0.85 s on a covered stair | FIXED | `a6_desk_stair_down_strip_every30` | `win.a` is 1 on every frame, down and up, on all three screens. The cut is on you the whole walk. |
| A2 body slides up, back, down | PARTLY | same strip; log `burst_desk_stair_down/r.json` | No snap, no jump over 24 px. The body still drifts 165 px up the screen (y 446 to 281) while you walk DOWN, and the view tilts 36 to 68 in the first three steps (sky gone). One smooth move in place of three. |
| A3 dug stair = crates on the meadow | PARTLY | `a6_desk_stair_step5` (inside: reads as a trench), `a6_crop_desk_shaftA_end` (outside: still crates) | From inside, top-down, it is a trench with a grass rim. Back on the grass in the side view, the stair you just dug is four dark boxes, each with a pale strip under it, with no rim: crates again. |
| A4 in a shaft: hole or rug? view flips | PARTLY | `a6_sheet_desk_shaft`, `a6_crop_desk_shaftA_6` | One view now (68) from the third level down to the surface: the flip is gone. But six deep still looks one deep: a cat in a brown box, half of the box drawn below the hole's rim, the name written on the wall. |
| A5 build: E goes two, "Nothing holds it" | FIXED | `a6_sheet_desk_build`, `a6_sheet_port_build` | A ghost block shows where it goes; Space three times stacks three (phone: 3 taps, `topAt` 3). No red sentence. |
| A6 14 deep looks 1 deep | PARTLY | `a6_sheet_desk_room`, `a6_desk_stair_bottom_3s` | Walls and floor are darker and the floor has a shade ring, so it is clearly a pit. The north wall of a room 14 down is still about one and a half tiles tall on the screen: it reads two or three deep. |
| B1 exit mark = black wedge and stray line | FIXED | `a6_sheet_desk_shaft` | An arrow over a square dot. Clean. |
| B2 title: text over text | PARTLY | `a6_sheet_port_first`, `a6_sheet_titles` | No bubble under the tagline in my three shots. The tagline still lies on the house, the garden and the fence (upright and landscape: three lines over the scene). |
| B3 the cat sideways is an egg | NOT | `a6_sheet_port_first` (third picture), `a6_crop_desk_st3_bottom` | Same pale oval with one dot. |
| B4 grey band after the tilt back | not seen again | `a6_sheet_desk_shaft` | Not in the same frames this time. One run only. |
| B5 two views one step apart | PARTLY | `a6_sheet_desk_shaft` (shaft: fixed), `a6_sheet_desk_house_gorge` (gorge: not) | Shaft and its way out: one view. The gorge is still side view on its floor and top-down the moment you dig into its wall. New cases of the same fault: N3 below. |
| B6 earth = plank bench, stone = dark slab | NOT | `a6_sheet_desk_behind_walls`, `a6_sheet_desk_build` | Earth blocks are a beige slab with no grass. Stone is a dark box. |
| B7 "Dig a step" twice, HUD over the room | FIXED | `a6_sheet_land_holes`, `a6_sheet_port_holes` | One button, no second cap. Menu row moved to the top. (But see N9: the hot bar.) |
| B8 checker holes in tree crowns | untested | | |
| B9 inside the house = bus shelter | NOT | `a6_sheet_desk_house_gorge` | Ground-floor walls vanish, the upper storey hangs on posts. |
| B10 block to break is brown in grey stone | NOT | `a6_sheet_desk_house_gorge` (third picture) | In the gorge's stone wall the marked block is still a brown checker box. Judged at half size. |
| B11 bare hands X says "Needs a hoe" | FIXED | `a6_desk_bare_x` (not opened; log) | No toast; the prompt is "Dig down". |

## 2. New faults

### A: blocks understanding or play

| # | Picture | Repro | What a newcomer thinks |
|---|---|---|---|
| N1 | `a6_sheet_desk_hill2` (made with `dev_stamp` levels), `a6_sheet_desk_behind_hill`, `a6_crop_desk_tun_hill_on` | Desktop, meadow. A small hill 4 levels high, 4x3 tiles, three steps up its west side. Walk up the steps onto the top (`myAt.z` 4, `topAt` 4: I am ON it). Or stand one or two tiles north of it. | "The hill fell apart under me." Standing on the top, the x-ray window is open (`win.n` 12, `win.a` 1): the grass top is replaced by a pale plate with a thin outline, lifted off the brown front; I float over a hollow box. Behind the hill the same plate. On the 6-high stone hill a stepped green pit opens in the grey top under my feet. On the big natural terrace (30,-12) the window stays shut, so it is small hills. This is "looks like I can stand there but what is it?" on the surface. |
| N2 | `a6_crop_desk_st3_bottom`, `a6_crop_desk_pit_in`, `a6_sheet_port_holes` (first picture) | Desktop or upright phone. Open stair 3 wide, 6 steps down east to a 3x3 floor; or a 3x3 pit 4 deep: walk in from the west. Side view (pitch stays 36). | "A brown drawer pulled out of the hole onto the grass." The floor and the lower steps are drawn BELOW the hole's grass rim, over the meadow south of it, with a pale strip along the bottom edge and no rim round them. The top half looks like a hole, the bottom half like a slab lying on the meadow. Same shape in the shaft (`a6_crop_desk_shaftA_6`). The other stair in the same picture, the one I am NOT in, is a clean hole with a rim all round (`a6_sheet_desk_st1`): the hole you stand in is the one that looks wrong. This is the old "crater / weird corners" in a new place. |
| N3 | `a6_desk_st3_down_strip_every15` beside `a6_desk_st1_down_strip_every15`; `a6_sheet_desk_st1` (pit) | Same meadow, two open stairs 6 deep, seven tiles apart: one 3 wide, one 1 wide. Walk down each. Then the pit: fall in, press the marked key once. | "Why did the camera change?" The 3-wide stair and the pit keep the side view with sky (36). The 1-wide stair tips to top-down (68), sky gone. In the pit the first marked dig tips it too. Three holes of the same depth, two views, and the rule is not visible. "View inconsistent". |
| N4 | `a6_sheet_desk_behind_walls` (second picture) | Desktop. Three earth blocks in a row, two blocks high, placed on the meadow. Stand one tile north of the middle one. | "I am standing on the wall." The whole cat is drawn, feet on the wall's top face. Nothing hides the legs. I cannot tell behind from on top. |

### B: ugly or weird

| # | Picture | Repro | What a newcomer thinks |
|---|---|---|---|
| N5 | `a6_sheet_desk_tun` (first picture), `a6_desk_tun_through_strip_every22` | Stone hill 6 high, 9x7, a tunnel one wide and 2 high through it east-west. Stand on the grass four tiles WEST of the hill, outside. | "The hill has an orange shelf cut through its front, with a green tile stuck at the end." The tunnel is opened through the hill's south face before I am near it, and its far end is a bright green rectangle. In the walk strip a thin dark line sticks out of the hill's side at tunnel height (frames 3 and 7). |
| N6 | `a6_sheet_desk_behind_walls` (third picture) | Stone blocks four high, three in a row. Stand one tile north. | "A cat is painted on the wall." The cat is drawn in full colour on the wall's dark front, its name on the wall's top. Seen, yes; behind, no. The house does this well with a dithered ring (`a6_sheet_desk_house_gorge`); the wall does not. |
| N7 | `a6_sheet_desk_room` (third picture, night) | Room 14 deep with a campfire, night. | A yellow ring of light lies on the MEADOW round the hole, fourteen levels above the fire. |
| N8 | `a6_sheet_desk_night` | Room 8 deep, night (`dev_time` .9). First nothing in hand, then a torch in hand. | The two pictures are the same: a warm ring round me with or without the torch. Under ground at night reads well, but the torch does nothing I can see, and it "cannot be placed". |
| N9 | `a6_sheet_land_cave` (second and third pictures), `a6_sheet_desk_water_house` (gorge) | Landscape phone: walk north into a cave in a hill's south face. Desktop: gorge floor. | The way out runs down the screen, under the hot bar. On the landscape phone the cave's mouth and the last dots are behind the hot bar's slots; the off-screen arrow sits on the hearts. |
| N10 | `a6_sheet_desk_two` (second picture) | Two players. Ava in a room 8 deep; Bo stands on the grass over the room. Ava's screen. | "A ghost cat is in my room." Bo is drawn hatched inside the room's picture, near its north wall. Nothing says Bo is eight levels above. (In the same room both read well. From above, Bo sees plain meadow: fine.) |
| N11 | `a6_sheet_desk_cave` (third picture), `a6_sheet_desk_tun` (second) | Walk three tiles into a cave or the tunnel and stand 2 s. | Dots, a running light and a key cap show the way out of a straight corridor whose mouth is on the screen. Noise where nothing is hidden. |
| N12 | `a6_desk_cave_in_strip_every17` | Walk north into the cave mouth. | The hill doubles in size on the screen and its roof becomes a grey slab as the view tips; the trees jump to the top edge. Smooth, but the whole world changes for a room three tiles deep. |
| N13 | `a6_sheet_port_build` | Upright phone, build mode. | Up, Down, Done are three big buttons down the right middle of the play area: the hut I was roofing stood half under them. With a roof in hand the A label over the earth stack says "Needs a wall under it", in words, beside a red box. |
| N14 | `a6_sheet_desk_yard` | A 5x5 yard of wood walls with a door, no roof. | The walls are knee-high planks; the door is taller than the wall it is in. Inside and behind it read well. |

### C: nits

- `a6_crop_desk_shaftA_6`, `a6_crop_desk_pit_in`: my name is written on the hole's wall.
- `a6_sheet_port_first`: the upright phone's hot bar has no numbers; the hearts row touches it.
- `a6_sheet_desk_house_gorge`: behind the house the aim bracket is drawn on the roof (a5's nit, still there).
- `a6_crop_desk_st3_bottom`: the stair's floor is a cold grey-violet while the pit's floor (`a6_crop_desk_pit_in`) is warm with a lit patch. Two floors of the same earth, two colours.
- Phone build, log only: with a roof in hand and standing six tiles away, B on the earth stack left a red box and "1 plans wait"; nothing was broken in 20 s. Standing beside the stack with earth in hand, B broke one block in 1.3 s, twice.

## Build mode on the phone (counted)

| Task | Taps | Stick flicks | Result |
|---|---|---|---|
| Into build mode, stack 3 earth | 4 (Build, A, A, A) | 5 to reach the spot | Built, 3 high |
| Roof a 3x3 hut (walls already there) | 10 (roof slot, 9 A) | 14 | Roofed, with a ridge |
| Break one block of the stack, standing beside it | 1 (B) | 0 | Gone in 1.3 s |

## Way out, counted

| Screen | Hole | Down | Out |
|---|---|---|---|
| Desktop | shaft 6 | 6 X | 10 R + 5 steps |
| Upright phone | shaft 6 | 6 B | 9 Dig taps + 5 steps |
| Desktop | pit 4 deep, fell in | walked in | 7 presses + 5 steps |

Doing only what the screen marks got me out every time.

## Frame cost (`?perf=1`, CPU x4, `HdLook.ms`)

| Walk | Upright: fps, script median / worst, HdLook.ms median / p95 / max | Landscape |
|---|---|---|
| Stair down | 60, 5.5 / 9.3 ms, 3.9 / 7.9 / 9.9 | 60, 6.2 / 13.3 ms, 5.3 / 7.1 / 9.0 |
| Stair up | 60, 1.5 / 6.3 ms, 1.3 / 5.4 / 7.9 | 60, 1.5 / 6.3 ms, 1.4 / 6.5 / 8.1 |
| Meadow sprint | 60, 4.5 / 10.9 ms, 2.8 / 5.6 / 7.6 | 60, 4.6 / 10.3 ms, 3.1 / 5.1 / 6.0 |
| Standing | 60, 0.3 / 1.4 ms, 0.1 / 1.2 / 2.1 | 60, 0.4 / 4.8 ms, 0.4 / 3.8 / 4.7 |

No frame over 33 ms in any walk or burst. Same as a5 within noise. The landscape stair's worst script frame (13.3 ms) is the closest to the 16.7 ms budget.

## 3. Good now

- The window never shuts on a stair any more, and the shaft keeps one view to the surface. a5's two worst faults are gone.
- The way out: an arrow and a dot at the exit, one key, one button on the phones. It worked on every hole I tried.
- Build mode: the ghost block, stacking by repeating one key, the roof laid over a hut in ten taps.
- From inside and top-down, a stair is a trench with a rim and steps that darken with depth (`a6_desk_stair_step5`).
- The cave in a hillside and the tunnel, once inside: a clear slot, floor and walls plain (`a6_sheet_desk_cave`).
- Pond and swimming, the upstairs of the house with its roof lifted, the house at night (`a6_sheet_desk_water_house`).
- The roofless yard from inside and behind; two players in one room.
- No page error in any run. 60 fps in every burst.

## Top 10

1. N1 standing on (or behind) a small hill opens the x-ray on it: the top turns into a pale floating plate (`a6_sheet_desk_hill2`).
2. N2 the floor of the hole you stand in is drawn outside the hole, a drawer on the grass (`a6_crop_desk_st3_bottom`, `a6_crop_desk_pit_in`).
3. N3 same depth, two views: 3-wide stair and pit stay side-on, 1-wide stair tips top-down (`a6_desk_st3_down_strip_every15`, `a6_desk_st1_down_strip_every15`).
4. a5 A3, half fixed: from the grass, the stair you dug is still crates (`a6_crop_desk_shaftA_end`).
5. a5 A4/A6, half fixed: 6 deep and 14 deep still look 1 to 3 deep (`a6_crop_desk_shaftA_6`, `a6_sheet_desk_room`).
6. N4 behind a two-high earth wall you look as if you stand on it (`a6_sheet_desk_behind_walls`).
7. N5 a tunnel is cut open through its hill before you are near, with a green tile at its end (`a6_sheet_desk_tun`).
8. N6 behind a four-high stone wall the cat is painted on the wall (`a6_sheet_desk_behind_walls`).
9. N9 the way out runs under the hot bar, worst on the landscape phone (`a6_sheet_land_cave`).
10. a5 B3, B6, B9 untouched: the egg cat, the beige earth slab, the bus-shelter house (`a6_sheet_port_first`, `a6_sheet_desk_house_gorge`).

## Verdict

Walking in and out, the owner would now follow a narrow stair or a shaft and always find the way out, but would still not understand a wide stair or a pit from the side (the floor spills out of the hole), nor how deep anything is, and would be thrown by a hill that opens up when stood on.
