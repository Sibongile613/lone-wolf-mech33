# Architecture

## Runtime flow

1. `mechfight.run(Bot)` reads newline-delimited JSON protocol messages from stdin.
2. On `hello`, `Bot.__init__` reads match configuration and creates a seeded RNG.
3. On each `obs`, `Bot.act` combines self state, visible enemies, weapons, loot, upgrades, and the zone.
4. `Bot.act` returns an action containing movement, aim, boost, jump, weapon, pickup, and debug fields.
5. `mechfight.run` timestamps and sends the action to stdout.

```text
stdin JSON
    -> mechfight.run
    -> Bot.__init__ / Bot.act
    -> movement + aiming + weapon decisions
    -> stdout JSON action
```

## Decision areas

- **Match state:** health ratio, energy, zone status, surviving enemy count
- **Targeting:** low-health-first score with distance tie-breaking
- **Movement:** zone target, enemy radial distance, seeded strafing, retreat behavior
- **Aiming:** target centre of mass with target cube scaling and projectile lead
- **Weapons:** weapon-specific range and cooldown behavior
- **Loot:** automatic useful secondary-weapon pickup
- **Upgrades:** defensiveness and mobility priority with description fallbacks

## Reliability design

`mechfight.run` catches exceptions in bot initialization and `act()`, logs a traceback to stderr, repeats the last continuous actions after runtime failures, and exits on `end`.

## Future improvements

- Cache hostiles and last-known enemy positions
- Predict projectile trajectories by solving the target–bullet interception equation
- Add line-of-sight and cover routing
- Score threats by damage potential and engagement state
- Record per-match metrics and benchmark placement
