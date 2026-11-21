# Lone Wolf Mech 33

A Python mech-combat bot for the Builders Mech Royale (`mechfight/1`) competition.

## Features

- Seeded, deterministic movement decisions
- Weak-target and distance-based target selection
- Projectile lead using live weapon projectile speed
- Zone-aware navigation
- Weapon-specific range, reload, and firing decisions
- Laser charging state management
- Debug overlays for aim, zone, health, and bot state

## Repository layout

- `lone_wolf_bot.py` — competition bot implementation
- `mechfight.py` — generated protocol types and JSON-lines runtime wrapper
- `Dockerfile` — sandboxed bot container
- `tests/` — unit tests
- `.github/workflows/ci.yml` — syntax and test CI

## Run locally

```bash
python3 lone_wolf_bot.py
```

The bot communicates through JSON-lines on stdin/stdout. The competition runner starts it inside a container, so it is not intended to be a standalone interactive game.

## Build and run the container

```bash
docker build -t lone-wolf-mech33 .
docker run --rm lone-wolf-mech33
```

The Match runner supplies protocol messages on stdin and receives actions on stdout.

## Test

```bash
python3 -m compileall -q lone_wolf_bot.py mechfight.py
python3 -m unittest discover -s tests -v
```

No third-party Python packages are required.

## Strategy summary

The bot uses the shrinking zone as its primary movement objective, while balancing enemy pressure and match state. It selects a low-health visible mech, leads moving targets with live projectile speed, strafes unpredictably, gathers useful secondary weapons, and prioritizes survivability upgrades.

## Submission evidence

Include the following in the final competition entry:

- [ ] Latest Docker image and repository URL
- [ ] Match replay or recording
- [ ] 20–100 seeded match benchmark results
- [ ] Architecture explanation
- [ ] Strategy description
- [ ] Known limitations and future work
