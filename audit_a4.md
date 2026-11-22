# Audit A4: Nexel Worlds, HD-2D look, played a fourth time. Main question: can you get out of a hole?

Copy of `~/megastructure` at commit dbf4816 ("The dig direction is a plain list again") in `tmp/fin_A4`, own server on port 7814. No game code changed.
Worlds: a copy of the owner's `saves/caves`, and a fresh seed-7 world.
Screens: desktop 1440x900 dpr 2 (keys), phone portrait 390x844 dpr 3 (touch), phone landscape 844x390 dpr 3 (touch).
Scripts: `tmp/fin_A4/shots/a4/` (`w.py` = the way-out follower, new; audit 3's `s1..s3, n, m, p, s5` re-run). Logs `w1..w3.log`, `b1..b3.log` there.
Pictures: 819 files `tmp/mockups/a4_<screen>_<scene>.png`. I opened 41 at full size. Every finding names one I opened. No page error in any run.

How the main test was done: `w.py` puts me in the hole, takes a picture at 0.3 s and at 3 s, then does only what the screen marks (the key pinned on the block, or one step along the dots), with real keys or real taps (Dig up / A / B buttons, the thumb stick), a picture after each move. It stops when nothing is marked any more.
Limit of the method: the script reads the same state the marks are drawn from. So "it works" below means "the marks are right and lead out". Whether a tired person SEES them is judged from the pictures.

Grades: A = ugly / broken / confusing at a glance. B = wrong once noticed. C = polish.

## Verdict

**Can a tired newcomer get out of every hole by following the screen? With a pickaxe in the hand: yes, in all 11 holes, on all three screens, by day and at night, no heart lost. Without a pickaxe in the hand: no.**

- The owner's cave is solved. In their pocket the screen shows, at once, an orange cracked block, the key R on it, dots up the stair and a glint at the top. Two presses of R, hold right, out (`a4_desk_pocket_00_cold_0s`, `a4_desk_pocket_02`, `a4_desk_pocket_zz_nothing`).
- **Finished? No.** The way out works, the picture around it is still the audit-3 picture.
- **Good looking?** Surface, tunnel at night, the gorge: yes. Under ground: 60 to 80% of the screen is still dull cut rock.
- **Consistent? No.** Still two views (horizon with sky / top-down without).

## 1. Getting out, hole by hole

Presses = key or button presses plus one for each new direction held. "Sees it in 3 s" is my judgement of the 0.3 s and 3 s pictures.

| Hole | Desktop | Portrait | Landscape | Sees it in 3 s? |
|---|---|---|---|---|
| Owner's pocket (-26,-3) z -13 | 3 (R, R, hold right) | 3 | 3 | Yes. Marks appear at 0.3 s |
| Owner's room (-26,-6) z -13 | 4 | - | - | Yes |
| Owner's lower room (-27,-4) z -14 | 5 | - | - | Yes |
| Deepest point (-31,-2) z -15 | 6 | 6 | 6 | Yes (`a4_desk_deep_01_cold_3s`, `a4_land_deep_01_cold_3s`) |
| My shaft, 1 tile, 6 deep (meadow) | 13 (9 R, 4 right) | 13 | 13 | Yes for the first key; the rest of the drawing is a heap (W2) |
| My tunnel, 2 down then 10 sideways | 2 (hold left, R) | 2 | - | Yes, the clearest of all (`a4_desk_tunnel_00_cold_0s`) |
| Natural cave (25,32) | 1 (hold up) | - | - | Only after standing still 2 s (by design). Then yes (`a4_desk_nca_01_cold_3s`) |
| Natural cave (11,-57) | 2 | 2 | - | Same. On portrait the dots leave the screen sideways (`a4_port_ncb_01_cold_3s`) |
| Natural cave (-53,48) | 1 | - | 1 | Same (`a4_desk_ncc_01_cold_3s`) |
| Pit 5 deep, floor half water | 14 (9 digs, 4 holds, 1 turn) | 14 | 14 | Yes: "▲ Space" / "▲ A" on the wall (`a4_desk_water_01_cold_3s`, `a4_port_water_01_cold_3s`) |
| Shaft 5 deep in the stone mountain (22,-30) | 7 | - | - | Key yes; the dots sit far from the boxes (W4) |
| Night: pocket, shaft, cave (11,-57), portrait shaft | same counts | 13 | - | Yes, as readable as by day (`a4_desk_shaft_night_01_cold_3s`) |
| No pickaxe at all: pocket, shaft, deepest (night) | **stuck** | **stuck** | - | Grey boxes and a small pickaxe picture. See W1 |

Not done: my staging of "tunnel sideways then DOWN 3" failed (X did not dig at the tunnel's end; the screen offered no key there), so the tunnel was tested 2 deep only. A way out of the gorge after falling in was not tested.

### Where it misleads, hides, clutters or fails

| # | Grade | Picture | What a tired player says | Guess at the cause |
|---|---|---|---|---|
| W1 | **A** | `a4_desk_pocket_nopick_zz_grey`, `a4_desk_bag_0_plank_in_hand` | "Grey boxes. A tiny dark square over my name. No key works." With the pickaxe in slot 1 but a plank in the hand, the corner says "Lay floor Space / Build mode B". Space and R do nothing. Nothing says "take the pickaxe". With no pickaxe at all you are stuck for good | The only sign is a 16-px icon in a dark box above the name; the hot bar slot holding the pickaxe is not marked; no pickaxe = no move in the search |
| W2 | B | `a4_desk_shaft_00_cold_0s`, `a4_desk_shaft_06`, `a4_land_shaft_06` | "A staircase of dashed boxes floating over the meadow. 'Dig up' is written on top of them." R is pinned on the block AND written again as "R Dig a step" beside it; the label covers the boxes and dots | The far blocks are drawn at their own height, which over flat grass reads as boxes in the air; the aim prompt and the pinned key are both shown |
| W3 | B | `a4_desk_shaft_zz_daylight` (same in all shaft, tunnel, water runs) | "Two below the grass every mark is gone. The screen says 'Space Dig into the wall'." Holding the same direction does walk out (logs: "then held E on -> z 0"), but the only thing shown is a wrong offer | Marks are drawn only under a roof or when trapped; the last two levels are an open walkable trench, so nothing is drawn and the normal dig prompt returns |
| W4 | B | `a4_desk_stone_01_cold_3s` | "Dots on a terrace six tiles away from the boxes. Two brackets, two 'Space'." | Dots are drawn at the path's lower levels; the aim bracket is drawn one level off the pinned key (H3) |
| W5 | B | `a4_desk_pocket_03` | After the two blocks are broken: "Space Dig on / X Dig down" sits on the first three dots of the way | The prompt filter (only the marked key) stops once the next move is a step |
| W6 | B | `a4_desk_bag_6_after_x_3s` | I pressed X by mistake in the pocket: dots now run both west (to a marked block below the west room) and east up the stair, no key shown. I cannot tell which way | After a dig the route is searched again from the lower floor; two legs of it overlap on screen |
| W7 | B | `a4_port_ncb_01_cold_3s`, `a4_land_deep_01_cold_3s` | In a cave I can walk out of, the phone shows three dig buttons ("Dig a step", "Dig on A", "Dig down B") and aim brackets on a wall | Dig offers are not hidden while a walkable way is shown |
| W8 | C | `a4_port_pocket_00_cold_0s`, `a4_land_pocket_00_cold_0s` | "Dig up" label covers my name; on landscape the lit "Dig up" button lies on the end of the stair; on portrait only 4 steps of the stair fit and the glint is off screen | Fixed places for the label and the button |
| W9 | C | `a4_desk_water_01_cold_3s`, `a4_port_water_01_cold_3s` | The blocks to break north of the pit look like a ladder standing on the meadow | Same as W2 |

Nothing of the marks was hidden behind the cat in any picture I opened.

## 2. Audit 3's list

| A3 | What | Now | Picture |
|---|---|---|---|
| Worst 1 / X1 | land turns sky blue far away | **Fixed** (70 tiles from the join point, and at all three natural caves) | `a4_desk_hz_tp_11s`, `a4_desk_nca_01_cold_3s` |
| Worst 2 / X2 | no way out of the owner's cave | **Fixed with a pickaxe**, see section 1. Remains without one (W1) | `a4_desk_pocket_00_cold_0s` |
| Worst 3 / X3 | two views | **Remains** (A/B) | `a4_desk_cave_in_03` (sky) vs `a4_desk_cave_in_06` (top-down, no sky) |
| Worst 4 / X4 | dull striped funnel | **Remains** (B). Portrait 24 deep: about 70% dark brown planks | `a4_desk_deep_01_cold_3s`, `a4_port_deep_room`, `a4_desk_deep_room` |
| U3 | flat cave floors | **Remains** (B). New and worse in the cave at (-53,48): the lit floor is flat lemon yellow | `a4_desk_ncc_01_cold_3s`, `a4_desk_nca_01_cold_3s`, `a4_land_tun_room` |
| Worst 5 / M2, M3 | fall into the gorge jumps | **Fixed**: biggest frame 18% (was 56%), no pop. Still 3 to 4 hearts lost | `a4_desk_burst_terrace_strip`, `a4_desk_mtn_downS_2` |
| S6 | deadly drop looks like steps | **Better**: dark rim and dark face at the gorge | `a4_desk_burst_terrace_strip` (row 4) |
| X7 | brown cut in grey stone | **Fixed**: stone stays stone | `a4_desk_stone_01_cold_3s`, `a4_desk_mtn_downS_2` |
| U6 / X5 | shaft as a crate | **Remains** (B) | `a4_desk_shaft_00_cold_0s`, `a4_port_shaft_01_cold_3s` |
| N6 | sliced tower | **Remains** (B): a low grey U, now with a grass tray round it | `a4_desk_tall_north_2` |
| S2 | far things bigger than me | **Remains** (B) | `a4_desk_zoom_in_max` |
| S7 | terraces as stripes | **Remains** (B) | `a4_desk_cave_in_03`, `a4_desk_burst_terrace_strip`, `a4_desk_stone_01_cold_3s` |
| S5 | river in squares | **Remains** (C) | `a4_desk_mtn_downS_2`, `a4_desk_hz_tp_11s` |
| H3 | aim brackets at two heights | **Remains** (B) | `a4_desk_bm_under_roof`, `a4_desk_house_behind1`, `a4_desk_stone_01_cold_3s` |
| H4, H5 | behind the house | **Fixed**: a checker disc in the roof, me inside it | `a4_desk_house_behind1` |
| H2 | behind a tree | **Still odd** (B): a square checker patch in the brackets, the cat barely seen | `a4_desk_tree_behind` |
| H6 | chest on my head | **Remains** (B) | `a4_desk_furniture_chest_open` |
| T4 | title is the flat old look | **Remains** (B) | `a4_desk_title` |
| T5 | tiny hot bar on portrait | **Remains** (C) | `a4_port_spawn` |
| X8 | HUD over the tunnel | **Remains** (C) | `a4_land_tun_room` |
| N9 | flowers 24 deep ("Pick flower") | **Remains** in staged land | `a4_desk_deep_room`, `a4_port_deep_room` |
| U7 | wrong prompts under ground | Changed form: see W3, W5, W7 | |
| U8, U9, N8 | X one level a press; ladder in a shaft | Stay fixed (log of `s3.py C`: 6 presses 6 levels, ladder placed, out) | not opened |
| X6, X9, S8, S9, S10, N11, N12, death card | | Not re-judged | |

## 3. Fresh look

- **Bursts (desktop, 60 fps, no frame over 33 ms):** into the owner's cave max 13.6% a frame, out 5.6%, staged tunnel 13.0%, house door 4.7%, terraces into the gorge 18.2%. No flash, no pop in either direction or at the tilt.
- **Night under ground:** good. The tunnel at night with the dots and the glint is the best picture of the audit (`a4_desk_tun_night`).
- **New, C:** falling into the gorge, one frame shows a black half disc with a checker edge under the slot (`a4_desk_burst_terrace_strip`, frame 14 of 16). Guess: the see-through disc drawn during the fall.
- **New, C:** portrait Settings: the tab "Shaders" touches both edges of its tab (`a4_port_ui_settings`). The menu itself is in style.
- **Build mode:** the real controls planned and built walls, roof, stairs, ladder again (log of `m.py BM`). Only `a4_desk_bm_under_roof` opened.
- Water, fights, death card, sleeping, furniture row: taken, not opened.
- Phone lag: not testable here (headless Chrome on a Mac GPU).

## The 5 worst things

1. **No pickaxe in the hand = no help** (A, W1). The owner's own words could come back the day they hold a plank or an empty hand in that pocket. A tiny icon over the name is the whole message.
2. **Two views** (A/B). Unchanged since audit 3.
3. **Under ground the screen is mostly dull cut rock, floors are flat colour** (B), and one lit cave floor is lemon yellow.
4. **The marks in an open shaft are a heap, and they vanish two levels below the grass**, leaving "Dig into the wall" (B, W2 + W3).
5. **Old sprite and aim faults still there** (B): chest on the head, brackets at two heights, sliced tower, shaft as a crate, horizon flowers three times my height, flat title.
