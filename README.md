# SIGNALFALL Reclamation

**Chapter 1 — First Signal.** A Mega Man X-style action platformer: Sable, first of the Rewritten, fights through the Sector 7 relay station to relight its beacon and face the Static Wraith.

## Play

Serve this folder (for example `python3 -m http.server`) and open `index.html`, or just open `index.html` in a browser. The `assets/` folder must sit next to `index.html` — it holds the sprite atlases and the stage background. Sound is synthesized in-page (WebAudio); there are no audio files.

## Controls

- **WASD / arrows** — move
- **Space** — jump (variable height; wall-jump off a wall slide)
- **J** — dash (ground + one air dash; **↓+J** ground slide)
- **K** — breaker sword: 3-hit combo chain, **↑+K** launcher, **↓+K** crouch sweep / air dive, spin slash while rising; a full-charge swing fires a blade beam
- **L** — pendant blaster: tap to fire, hold to charge (two levels, full charge pierces). Aim with **↑/↓** — in the air, **↓ + direction shoots diagonally down**, ↓ alone shoots straight down
- **P** — pause
- Legacy alternates: Z jump · X sword · C dash · V blaster · Shift dash
- Touch controls appear automatically on phones.

## Chapter 1 structure

Awakening Bay → Maintenance Corridor (relight the relay beacon) → The Shaft → Foreman's Yard (Aegis drops in; Foreman-9 mini-boss holds the beacon's power cell) → Relay Core Approach → the Static Wraith in its own arena: teleport dashes, hex shield, ground slam, enrage phase, destructible orb.

Collect data shards and 3 hidden data chips; find the Signal Heart (+2 max HP) and the pre-boss armor capsule.

## Repository status

First commit carries the game code (`index.html`) and this README. The binary sprite atlases under `assets/` (16 PNG/JPG files, ~13 MB) are added with a direct `git push` — the connector upload channel used for this first commit accepts text files only.
