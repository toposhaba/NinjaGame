# Bunny Ninja 3D — Build Plan

## Story / Theme
A cute cartoon little girl **ninja** in pastel pink overalls with white bunny ears trains on a 3D obstacle course. Level 1: bounce beam over deadly red goo, then a swing bar with a timing meter.

## Tech
- ONE file: `index.html`
- **Three.js** from CDN (ES module importmap). No build step, no bundler, no image assets.
- Simple custom physics (no physics engine). `requestAnimationFrame` + delta time capped at 50ms.
- Pastel look: sky `#ffd6ec`, platforms `#c9b6e4`, goo `#ff4d6d`, overalls `#ffb3d1`, ground fog soft pink.

## Character (low-poly mesh group)
- Round head + dark pink ninja mask + headband ribbons + white bunny ears (pink inner)
- Pink overalls body, small legs, grey katana on back
- Capsule-ish hitbox ~0.6 wide × 1.4 tall
- Move: WASD / arrows. Jump: Space. Camera: third-person behind player.

## Level 1 (forward = +Z)
1. **Start platform** — box from z=0..10
2. **Bounce beam** — thin long box z=11..30, y oscillates with sine (amp 0.45, period 2.2s). Player rides it. Red goo plane underneath; touching goo = death/respawn.
3. **Rest platform** — z=31..40
4. **Swing bar** — bar at z≈46, y≈3.2. Jump into it to grab. Pendulum swing. HUD meter: gray arrow sweeps, green circle target. Space to release. Perfect timing → strong launch to goal. Bad timing → weak launch into goo.
5. **Goal** — platform z=58..72 with torii/dojo gate + flag. Touch flag = WIN.

## States
`MENU` → `PLAYING` → `SWINGING` → `DEAD` → `WIN`  
Demo autopilot: `?demo=1`

## Build order
1. Three.js scene, lights, ground, camera follow
2. Player mesh + move/jump on static platforms
3. Bounce beam + goo kill
4. Swing grab + pendulum + timing HUD + launch
5. Goal / menu / win / demo mode / polish (petals as sprites or small meshes)
