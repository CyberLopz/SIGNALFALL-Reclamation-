# LEVEL FIDELITY — STEP 1: THE MAP

Rebuild of Chapter 1 in `CyberLopz/SIGNALFALL-Reclamation-` (branch `level-fidelity`,
base = main @ c7869904) against the ten concept screens in `design/level-refs/`.
This document is the inventory only — no level code has been changed yet.

**How to read it.** Every element of every image is classified as BACKGROUND /
PLATFORM / PROP / HAZARD / ENEMY / EFFECT, and named against the current build:
REPLACES (an existing sprite/system it supersedes), EXTENDS (existing system, new
art or a new variant), or NEW. Anything that changes gameplay is marked
**⚠ APPROVAL** and collected again in §4 — none of those get built until Ryan
signs off on them individually.

**Current build, for reference.** One horizontal band, WORLD 9200×660, six
sections picked by x-threshold (`sectionIndex()`): AWAKENING BAY / MAINTENANCE
CORRIDOR / THE SHAFT / FOREMAN'S YARD / RELAY CORE APPROACH / WRAITH CHAMBER.
Level data lives in SOLIDS, SPIKES, MOVERS, CHECKPOINTS, PICKUPS_DEF,
ENEMIES_DEF, DECOR_<SECTION>, TRACE_SEGS_<SECTION>, SCORCH_<SECTION>.
Enemy roster: walker, shieldwalker, brute (FOREMAN-9), drone, swooper, turret,
ceilturret, crawler, jumper, mine, mortar. Story scenes: SCENE_OPEN, SCENE_BEACON,
SCENE_RELAY, SCENE_AEGIS, SCENE_CAPSULE, SCENE_WRAITH_PRE, SCENE_POST.
Adaptive music tracks: dark, relay, yard, drone, wraith. 18 atlases load at boot.

---

## 1. THE ROUTE (one journey, Ryan's order)

| # | Scene | Ref | Time/light | Music | Story beat |
|---|-------|-----|-----------|-------|------------|
| 1 | Sunset Rooftops | 10 | sunset, amber horizon | dark | ECHO opening (SCENE_OPEN) |
| 2 | Rooftop Gantry | 01 | dusk, fog | dark → relay | Relay beacon relight = checkpoint 1 (SCENE_BEACON); level brightens |
| 3 | Ruined Stairwell | 02 | night interior | relay | descend into the station |
| 4 | Mine Shaft | 04 | deep dark, cyan from below | relay | RELAY mine warning on entry (SCENE_RELAY) |
| 5 | Conveyor Works | 05 | night interior | relay | — |
| 6 | Flooded Pumps | 09 | night, wet teal | relay | 08 Crystal Vault hidden off this scene |
| 6b | Crystal Vault | 08 | warm gold dark | relay (soft) | Signal Heart |
| 7 | Shield Corridor | 03 | night interior | relay | Aegis drops in (SCENE_AEGIS); **checkpoint 2 (halfway)** |
| 8 | Foreman Arena | 07 | amber industrial | yard | FOREMAN-9 mini-boss, locked camera |
| 9 | The Climb Out | (reuse 02/04 kits, night grade) | night → storm gathering | relay → dark | armor capsule just before the gate (SCENE_CAPSULE) |
| 10 | Rain Bridge | 06 | night storm | dark + rain ambience | — |
| 11 | Boss Gate | (corridor kit) | dark | drone | paired shutters, short corridor |
| 12 | Wraith Chamber | (corridor kit, storm-lit) | dark | wraith | SCENE_WRAITH_PRE → fight → SCENE_POST → chapter clear |

New SECTIONS list (replaces the current six):
SUNSET ROOFTOPS / ROOFTOP GANTRY / RUINED STAIRWELL / MINE SHAFT / CONVEYOR
WORKS / FLOODED PUMPS / SHIELD CORRIDOR / FOREMAN ARENA / THE CLIMB OUT /
RAIN BRIDGE / WRAITH CHAMBER. (The vault and the boss gate report as part of
their host sections.)

**Structural note (not a gameplay change, but engine-adjacent):** the current
world is one 660px-tall band with sections chosen by x. This route descends
twice and climbs once, so the world becomes a multi-band map (taller WORLD_H,
stacked bands) and `sectionIndex()` extends from x-thresholds to region boxes.
Camera, collision and all KEEP-listed systems are untouched; only the section
lookup and the world constants change shape. Flagged here so it isn't a surprise
in the graybox step.

**Palette spine** (sampled from the refs; neighbouring scenes share the navy
base so the joins match):

- Shared base navy: #0f1635 → #212744 band; circuit violet #9a6bff family
  (already the game's trace colour); light-strip cyan; lamp amber; enemy-eye red.
- 10 sunset: #392f5a / #8f6754 amber horizon over #14153f.
- 01 gantry: #202147 steel under #596c9c–#939bbb dusk sky.
- 02 stairwell: #1b2049 / #3b4e7b, darkest corners #060f27.
- 04 shaft: #070d2c depths, #316b83 cyan-teal glow rising.
- 05 conveyor: #0f142c / #333346, belt chevron cyan.
- 09 pumps: #1f3442 / #2f6865 wet teal.
- 03 corridor: #212744 / #425d76 machine steel.
- 07 arena: #1c1c29 / #616a68 rivet grey, amber core light.
- 08 vault: #2f293d with the level's only warm gold #7b6250+.
- 06 bridge: #2b3a6b / #4a6297 storm blue.

---

## 2. PER-IMAGE INVENTORY

### 10-sunset-rooftops.png — Scene 1, intro (safe, no enemies)
- BACKGROUND far: dusk gradient sky (indigo→amber), layered tower silhouettes —
  EXTENDS `drawBackground` (new skyline layer set, sunset grade).
- BACKGROUND ambient: bat silhouettes crossing far off — EXTENDS background
  (swooper sprite reused at far parallax, no AI, no collision).
- PLATFORM: rooftop slabs with circuit-trace edge trim — REPLACES Bay floor
  skin in the new rooftop tileset; step blocks = vent/fan housings (SOLIDS).
- PROP: antenna masts, dishes, vent stacks, fan units — EXTENDS props atlases
  (new rooftop kit); fan spin + mast blink = ambient loops, 12 fps, random phase.
- EFFECT: golden shard arc tracing the dash line across the intro gap —
  EXTENDS PICKUPS_DEF (existing shard pickups placed on the taught trajectory).
- GAMEPLAY: none new. The gap is crossed with the existing dash / air-dash.

### 01-rooftop-gantry.png — Scene 2, checkpoint 1
- BACKGROUND far: smoky ruined skyline + drifting fog band — EXTENDS
  `drawBackground` (fog layer: scrolling translucent loop, parallax).
- BACKGROUND mid: steel crane gantries and girders — EXTENDS DECOR (new gantry
  set pieces, `back:true`).
- PLATFORM: stepped purple-circuit blocks with cyan light strips — new gantry
  tileset skin on the existing platform renderer; strips pulse at 12–15 fps.
- PROP: amber beacon lamps (flicker) — EXTENDS existing sconce decor + its
  flicker fx; hanging chains/hooks with sway — EXTENDS DECOR (ambient sway
  loop); smokestack smoke — EXTENDS the particle system (rising smoke emitter).
- ENEMY: spider crawler — REPLACES crawler sprite with the image's design
  (same size, same hitbox, same behaviour).
- STORY/SYSTEM: the relay beacon prop + SCENE_BEACON relocate here;
  CHECKPOINTS[0] at the beacon; the post-beacon brightening already exists in
  the lighting system (`drawLights`) and carries over unchanged.
- GAMEPLAY: none new. Fog is visual only and never covers the playfield line.

### 02-ruined-stairwell.png — Scene 3, descent
- BACKGROUND: interior tower wall panels; the wall breach is a parallax window
  onto the 01 skyline — EXTENDS `drawBackground`/DECOR (breach frame prop +
  skyline layer reused).
- PLATFORM: the diagonal staircase — see ⚠ A1 in §4. Dressed either as stepped
  SOLIDS in a stair skin (no new collision) or true ramps (new). Landing
  platforms and the pit floor as standard SOLIDS.
- HAZARD: the dark pit with hanging rock — pit = existing pit-fall; the hanging
  rock is either DECOR or a crumble platform (the `crumbles` system already
  exists) — crumble use is EXTENDS, not new.
- ENEMY: ceiling turret with red eye — REPLACES ceilturret sprite (same
  behaviour/hitbox); spider crawler on the stairs — REPLACES crawler sprite.
- PROP: amber wall lights — EXTENDS sconces; wall pipes — EXTENDS props.
- EFFECT/SYSTEM: cyan holographic jump pad — the bounce pad already exists in
  the mechanics set; this is a REPLACEMENT skin (holo ring + arrow chevrons,
  idle pulse 15 fps, compress on bounce). Flagged ⚠ A6 only so Ryan confirms
  the counts/placement, not the mechanic.
- CAMERA: vertical scroll — EXTENDS camera clamp to the taller world (see §1
  structural note).

### 04-mine-shaft.png — Scene 4, vertical descent
- BACKGROUND: shaft wall panels, smoking wall breaches — EXTENDS DECOR + smoke
  emitters; cyan glow rising from below — EXTENDS `drawLights` (vertical
  gradient light, animated shimmer).
- PLATFORM: wall-mounted ledges — SOLIDS in the shaft tileset skin.
- HAZARD/ENEMY: spiked red mines, bobbing in the descent line — REPLACES mine
  sprite (same drift behaviour and hitbox); RELAY's warning (SCENE_RELAY
  trigger) relocates to the shaft mouth.
- EFFECT: Sable diving and shooting straight down uses the existing air
  down-aim pose from the p9 aim set — no new pose, no new mechanic. Descent is
  ordinary gravity; ledges are placed so a blind drop is never lethal (mines
  are dodgeable from every ledge).
- GAMEPLAY: none new.

### 05-conveyor-works.png — Scene 5
- PLATFORM/SYSTEM: two chevron conveyor belts running opposite directions —
  EXTENDS the existing conveyor mechanic; chevrons scroll as a seamless loop
  texture on the belt surface; belt direction readable at a glance.
- PLATFORM: crumbling bridge between the belts — EXTENDS `crumbles`; falling
  rock debris while it breaks — EXTENDS particles.
- ENEMY: mortar turret on the upper deck — REPLACES mortar sprite; its arcing
  shell gains a smoke-trail — EXTENDS the existing projectile (particle trail
  only, same arc/damage).
- HAZARD: ⚠ the crusher claw chewing scrap at the belt's end — NEW, see §4 A2.
- PROP: scrap piles, gears, crates, hanging hooks — EXTENDS props/DECOR.
- EFFECT: dash splash where Sable's dash meets the belt end — EXTENDS dash FX
  + particles (visual only).

### 09-flooded-pumps.png — Scene 6
- ⚠ WATER — the pool is this level's biggest open question, see §4 A3. Visual
  side regardless of the physics call: surface line with ripple loop,
  reflections of lamps/strips (mirrored draw, masked), bubble particles,
  drips from ceiling pipes with landing splashes — EXTENDS particles/lighting.
- PLATFORM: pipe runs and pump housings above the waterline — SOLIDS, pumps
  tileset skin; submerged amber lights — EXTENDS sconces (underwater variant).
- PROP: pouring pipes — animated water columns (seamless loop + splash
  emitter where they land), valves, pump machinery — EXTENDS props.
- ENEMY: ⚠ frog robot (the Diver from the Chapter 2 design, brought forward)
  — NEW enemy, see §4 A4. The lurking one reads as two glowing eyes at the
  surface until it leaps.
- SIDE ROOM: the vault entrance is hidden in this scene's wall — see 08.

### 08-crystal-vault.png — Scene 6b, hidden side room
- ⚠ BREAKABLE WALL — the cracked wall hiding the entrance, see §4 A5.
- BACKGROUND/PROP: stone altar (the level's only stonework — reads as older
  than the station, deliberate), dormant machinery at the edges — NEW decor
  set, small.
- EFFECT: golden crystal with orbiting shards (ambient orbit loop), light rays
  through the breach, floating dust — EXTENDS `drawLights` + particles; the
  crystal itself is a NEW prop sprite (static + glow, no gameplay role).
- PICKUP: the Signal Heart — existing heart pickup (PICKUPS_DEF), placed here
  per the brief (reachable only via the pumps + the wall).
- GAMEPLAY: apart from the wall, none. No enemies, no hazards — a held breath.

### 03-shield-corridor.png — Scene 7, halfway checkpoint
- ENEMY: heavy mech behind an octagonal energy shield — REPLACES shieldwalker
  sprite with the image's heavier design (same size, hitbox, frontal-block
  behaviour). The shield's crack overlay as it absorbs hits is a visual state
  layered on the existing shield — EXTENDS shield rendering; behaviour
  (blocks frontal fire, vulnerable from behind/above) is unchanged.
- ENEMY: spider crawler — REPLACES crawler sprite (shared with 01/02).
- PROP: sealed door with amber light rows — EXTENDS the existing shutter/gate
  art (this is the corridor's exit door, opens on clear); ceiling pipes and
  circuit walls — EXTENDS props/DECOR corridor kit.
- SYSTEM: CHECKPOINTS[1] (halfway) at the corridor entrance; SCENE_AEGIS
  relocates here — Aegis drops in before Foreman, per the brief.
- GAMEPLAY: none new.

### 07-foreman-arena.png — Scene 8, mini-boss
- BOSS: FOREMAN-9 — REPLACES the brute's sprite with the image's riveted mech:
  cannon arm, hook arm, amber core eye. Same state machine, same hitbox, same
  attacks reskinned (cannon = its existing shot; the hook arm is the visual
  for its existing melee-range slam). ⚠ A7 only if Ryan wants the hook to gain
  a genuinely new attack (grab/throw) — not assumed.
- ARENA: locked camera — EXTENDS the existing yard arena bounds/camera lock;
  hazard-stripe floor edges — EXTENDS DECOR; hanging chains and hooks (sway) —
  shared with 01's kit; sparks and debris during the fight — EXTENDS the
  existing boss-hit particles.
- MUSIC: yard (existing track, trigger relocates with the arena).

### The Climb Out — Scene 9 (no ref; connective tissue)
- Built from the 02/04 kits run in reverse (ladders, ledges, wall-jumps —
  all existing), night grade deepening; storm light begins at the top:
  distant rain haze layer fades in — EXTENDS background (pre-echo of 06).
- The armor capsule (existing capsule + SCENE_CAPSULE) sits just before the
  boss gate, per the brief — off the direct line, reached by wall-jump.
- Data chips (existing shard pickups) seed the climb as the skill reward line.

### 06-rain-bridge.png — Scene 10
- ⚠ WEATHER: slanted rain — NEW presentation system, see §4 A8 (visual +
  ambience audio layer; no wind force assumed).
- BACKGROUND: fogged ruined city in storm blue (#2b3a6b/#4a6297), lamp glow
  halos in the wet air — EXTENDS `drawBackground` + `drawLights`.
- PLATFORM: bridge deck with the broken gap — SOLIDS, bridge skin; the gap is
  a taught dash-jump (shard arc optional here too).
- PROP: street lamps — EXTENDS sconce art (tall variant), rain glitter under
  each lamp (particles).
- ENEMY: three bat-winged flyers — REPLACES swooper sprite with the image's
  bat-wing design (same swoop behaviour/hitbox).

### Boss Gate + Wraith Chamber — Scenes 11–12 (no ref; existing kits)
- GATE: paired shutter doors with a short corridor between — EXTENDS the
  existing gate/shutter system (two gates, corridor = drone music pocket).
- CHAMBER: Wraith boss — existing boss, state machine and atlas (wraith p2)
  unchanged; chamber dressed in the corridor kit with storm light through
  ceiling breaches (rain shafts, shared with 06) so the finale rhymes with
  the bridge. Camera locks (existing boss-room behaviour). SCENE_WRAITH_PRE /
  SCENE_POST relocate here; chapter-clear flow unchanged.

---

## 3. SHARED KITS (tilesets, not paintings)

Five skins over the one platform renderer, all sharing the palette spine:
ROOFTOP (10/01), STATION (02/03 + gate/chamber), SHAFT (04 + climb out),
WORKS (05/09), plus one-off sets for the ARENA trim and the VAULT stone.
Prop kits: gantry steel (cranes, girders, chains, hooks), rooftop clutter
(antennas, dishes, fans), pipework (shared 02/04/09), machinery (05/07),
lamps (one sconce family, three heights, dry + submerged variants).
Enemy redraws, all at current size/hitbox: crawler, ceilturret, shieldwalker,
mortar, mine, swooper. Boss reskin: FOREMAN-9 (brute). Unchanged art: Sable
(all existing atlases incl. p9 aim, p10 ladder, p11 run-charge), Aegis,
Wraith, pickups, portraits.

## 4. ⚠ GAMEPLAY APPROVALS — needed before the graybox

- **A1. Staircase collision (02).** (a) Stepped solids dressed as stairs —
  zero new collision, my recommendation; or (b) true slope ramps — new
  collision code touching movement. Pick one.
- **A2. Crusher claw (05).** New timed hazard. Needs: damage or instant
  death? (Recommendation: heavy damage + knockback, telegraphed cycle, safe
  window readable — never a blind kill.) Plus its timing period.
- **A3. Water (09).** (a) Shallow wade: Sable stays grounded, move speed
  reduced in the pool, splash/ripple FX, no swim — small, my recommendation;
  (b) full swim physics — a new movement mode, much bigger. Pick one.
- **A4. Diver / frog robot (09).** New enemy: lurks at the surface (eyes
  visible), leaps in an arc at Sable, splashes back down. HP/ damage in line
  with the jumper. Approve bringing it forward from the Chapter 2 design?
- **A5. Breakable wall (08 entrance).** New: a visibly cracked wall section
  that breaks under sword/blaster hits, revealing the vault. (Alternative
  with zero new code: a gap hidden behind foreground decor — less honest.
  Recommendation: the breakable.)
- **A6. Jump pad (02).** Existing bounce-pad mechanic, new holo skin.
  Confirm pads are wanted in the new layout (count/placement set in graybox).
- **A7. Foreman's hook arm (07).** Reskin only (recommended) — or approve a
  new hook grab/throw attack for the mini-boss.
- **A8. Rain (06, gathering in 09).** Visual system + storm ambience audio
  layer only; NO wind force on Sable. Confirm no-wind.
- **A9. Multi-band world (structural).** Section lookup extends to region
  boxes; WORLD_H grows. No player-facing change; flagged for transparency.

Everything not listed here is art, placement, or relocation of systems that
already exist — that work proceeds under the brief without separate approval.

## 5. WHAT HAPPENS NEXT (per the work order)

- (b) On MAP approval (incl. A1–A8 calls): full graybox — plain blocks, all
  placements, story triggers and music zones wired; a paced playthrough
  report (per-scene times, hard spots, empty spots, total vs the 4–6 min
  target). Wait for approval.
- (c) Art pilot: dress 01-rooftop-gantry; side-by-side concept vs in-game at
  a matched camera, plus captures of every new animation loop and the
  frame-count/fps table for that scene. Wait for approval.
- (d) Remaining scenes one at a time, same check each; PR when all ten land.

Animation method (brief §3) is adopted as written — frame counts derived from
motion duration × rate (24 fps actions, up to 30 for dashes/slashes/
explosions, 12–15 ambient), elapsed-time playback, per-prop phase offsets,
atlases ≤2048², per-section load with next-section preload. The per-animation
table ships with each scene's dressing step, starting with the 01 pilot.
