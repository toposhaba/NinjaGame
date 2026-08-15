# Bunny Ninja: Obstacle Course — Build Plan

## Story / Theme
A cute cartoon little girl ninja in pastel pink overalls with white bunny ears trains at a ninja obstacle course. Level 1 has two challenges: a bouncy beam over deadly red sticky goo, and a swing bar with a timing meter you must release at the perfect moment.

## Tech (keep it simple)
- ONE file: `index.html` with inline CSS + JS. No frameworks, no build step, no image assets.
- HTML5 `<canvas>` 960x540, `requestAnimationFrame` loop, fixed timestep is not needed (use delta time capped at 50ms).
- Draw everything with canvas shapes (rects, circles, arcs). Pastel palette: background `#ffeef8`, platforms `#c9b6e4`, goo `#ff4d6d`, girl skin `#ffe0cc`, overalls `#ffb3d1`, ears white with pink inner.

## Character
- Player = simple stick-together shapes: round head with a dark pink **ninja mask** covering the lower face (eyes visible), a **ninja headband** with two small trailing ribbon rects, two tall white ear rectangles with pink inner rects poking through the headband, body rect in pink overalls with a small katana rect strapped on her back, small legs. About 30px wide, 50px tall. Hitbox = plain rectangle 24x48.
- Physics: gravity 2000 px/s², move speed 220 px/s, jump velocity -650 px/s. Left/Right arrows (or A/D) to move, Space/Up to jump.

## Level 1 Layout (left to right, world is ~2400px wide, camera follows player)
1. **Start platform** (x 0–300, floor at y 450).
2. **Bounce beam** (x 340–1000): a narrow horizontal beam at y 430, only 14px thick. It "bounces": the beam's y oscillates with a sine wave, amplitude 25px, period 2s. Player stands on it and moves with it. Below the beam (y 500 to bottom) is **red sticky goo**: animated wavy red rectangle. Touching goo = instant death (respawn at start, deaths counter +1).
3. **Rest platform** (x 1040–1240, y 450).
4. **Swing bar section** (x 1300–2100):
   - A horizontal **bar** hangs at (x 1400, y 200). When player jumps and their hitbox touches the bar, they auto-grab: state becomes `SWINGING`.
   - While swinging, the player is a pendulum: angle = `sin(time * 2.5) * 75°` around the bar, rope length 130px. Goo covers the ground under this whole section.
   - **Timing meter UI** (drawn top-center of screen while swinging): a horizontal track with a **green circle** at a fixed spot and a **gray arrow** that sweeps back and forth across the track (triangle-wave, full sweep every 1.6s).
   - Press Space to release. If the gray arrow is inside the green circle (within ~8% of track width) at release, launch the player with a fixed winning velocity (vx 420, vy -520) that lands them on the goal platform. If the timing is wrong, launch weakly (vx 150, vy -200) so they fall into the goo and die.
5. **Goal platform** (x 2150–2400, y 450) with a small dojo gate (two posts + roof rect) and a flag. Touching the flag = WIN screen.

## Game states (simple switch)
`MENU` (title + "Press Space") → `PLAYING` → `SWINGING` (sub-state of playing) → `DEAD` (flash red 0.6s, then respawn) → `WIN` (confetti rects + "You Win! Press R").

## Build order for the implementing LLM (do in this order, test after each)
1. Canvas + game loop + input handling.
2. Player rect with gravity, ground collision on static platforms, left/right/jump.
3. Draw the cute character shapes over the hitbox; simple 2-frame walk wobble (tilt ±5°).
4. Bounce beam (sine y-offset, player rides it) + goo kill zone + respawn/death counter.
5. Swing bar: grab detection, pendulum motion, timing meter UI, release logic (perfect vs. failed launch).
6. Goal flag + dojo gate, WIN state, MENU state (title: "Bunny Ninja"), HUD (deaths count), pastel polish (clouds, hearts, cherry-blossom petals drifting down).

## Acceptance checklist
- [ ] Falling in red goo always kills and respawns at start.
- [ ] Beam visibly bobs up/down and carries the player.
- [ ] Grabbing the bar always works from a jump; meter appears only while swinging.
- [ ] Perfect-timing release always reaches the goal platform; bad timing never does.
- [ ] Win screen reachable; R restarts.
