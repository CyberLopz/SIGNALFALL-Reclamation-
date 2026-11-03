# SIGNALFALL Reclamation

**Chapter 1 — First Signal.** A Mega Man X-style action platformer: Sable, first of the Rewritten, fights through the Sector 7 relay station to relight its beacon and face the Static Wraith.

## Play

Serve this folder (for example `python3 -m http.server`) and open `index.html`, or just open `index.html` in a browser. The `assets/` folder must sit next to `index.html` — it holds the sprite atlases and the stage background. Sound and music are synthesized in-page (WebAudio); there are no audio files.

## Controls

- **WASD / arrows** — move
- **Space** — jump (variable height; wall-jump off a wall slide)
- **J** — dash (ground + one air dash; **↓+J** ground slide)
- **K** — breaker sword: 3-hit combo chain, **↑+K** launcher, **↓+K** crouch sweep / air dive, spin slash while rising; a full-charge swing fires a blade beam
- **L** — pendant blaster: tap to fire, hold to charge (two levels, full charge pierces). Aim with **↑/↓** — in the air, **↓ + direction shoots diagonally down**, ↓ alone shoots straight down
- **P** — pause · **M** — music on/off · **G** — graphics (Full / Performance)
- Legacy alternates: Z jump · X sword · C dash · V blaster · Shift dash
- **Gamepad** (standard layout): stick / d-pad move · **A** jump · **B / RB / RT** dash · **X** blaster · **Y / LB / LT** sword · **Start** pause · **Select** music
- Touch controls appear automatically on phones.

## Chapter 1 structure

Awakening Bay → Maintenance Corridor (relight the relay beacon) → The Shaft → Foreman's Yard (Aegis drops in; Foreman-9 mini-boss holds the beacon's power cell) → Relay Core Approach → the Static Wraith in its own arena: teleport dashes, hex shield, ground slam, enrage phase, destructible orb.

Collect data shards and 3 hidden data chips; find the Signal Heart (+2 max HP) and the pre-boss armor capsule.

## Audio & presentation

- **Adaptive score**: a synthesized step sequencer that follows the chapter: a dark ambient cue until the beacon relights, the driving *Relay* theme after it, full intensity while Foreman-9 is engaged, a drone in the arena, then the Static Wraith's own faster harmonic-minor theme, which adds layers in phase 2. Changes land on bar lines; dialogue ducks it and pause muffles it.
- **Mix**: SFX and music run through a compressor with shared reverb and a tempo-synced delay; impacts are panned to where they happen.
- **Feel**: hitstop on kills and on taking damage, trauma-based camera shake, a look-ahead camera that ignores small jumps, buffered dash input, enemy hit-shake, HP damage "ghost" bars and an MMX-style boss bar fill-up.
- **Graphics (G)**: Full adds bloom, light pools and a foreground parallax layer; Performance drops them. Slow machines switch to Performance automatically once.

## Repository status

First commit carries the game code (`index.html`) and this README. The binary sprite atlases under `assets/` (16 PNG/JPG files, ~13 MB) are added with a direct `git push` — the connector upload channel used for this first commit accepts text files only.
