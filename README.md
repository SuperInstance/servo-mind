# servo-mind ⚒️🧠

**Self-learning constraint system for PLATO tiles.**

The encoder feedback processor that closes the loop that was always open.

## What It Does

PLATO tiles collect win/loss outcomes (encoder ticks) but nobody reads them back into the constraint parameters. Servo-mind is the wiring between encoder output and motor driver input.

Three new components:

- **FeedbackProcessor** — reads tile outcomes, computes parameter adjustments (signal processor)
- **MetaConstraint** — constraints that evolve from their own enforcement history (adaptive PID)
- **TransferFunctionModel** — learns the actual query→outcome mapping (Bode plot)
- **ServoMind** — orchestrates the cycle (the controller)

## The Metaphor

A servo with an encoder doesn't just move — it knows where it IS. The encoder measures the GAP between commanded and actual. That gap IS the system's self-knowledge.

The original code was good enough for the job (fixed-gain PID). This code learns new jobs by learning its own system better.

## Architecture

```
SERVO COMPONENT          →  CODE COMPONENT
─────────────────────────────────────────────
Motor (actuator)         →  constraint-theory-core
Encoder (sensor)         →  Tile win/loss, outcome_log, cancer detector
Signal processor         →  FeedbackProcessor (NEW)
Adaptive controller      →  MetaConstraint (NEW)
Transfer function model  →  TransferFunctionModel (NEW)
Orchestrator             →  ServoMind (NEW)
```

## Quick Start

```python
from core.tile_lifecycle import TileStore, Tile
from core.servo_mind import ServoMind

store = TileStore()
mind = ServoMind(store)

# Record outcomes as tiles are used
mind.record_and_learn(tile.id, succeeded=True, constraint_type="confidence", constraint_strength=tile.confidence)

# Run a self-learning cycle
result = mind.cycle()
print(result)  # adjustments, sweep results, updated params

# Check status
mind.status()
```

## Tests

```bash
python3 tests/test_servo_mind.py
```

7/7 passing. Proves:
- Win rate detection works
- Mortality adapts in the right direction (high WR → lower, low WR → raise)
- MetaConstraints learn optimal thresholds
- Transfer function accumulates knowledge
- Full cycle integrates cleanly
- Cancer response triggers aggressive adaptation
- Parameters converge over time

## The Five Dimensions

- X, Y, Z — space (where in the lattice)
- T — time (Lamport clock ordering)
- S — scale (room ↔ tile folding)

This module operates on **S** — it adjusts the system's own parameters, which changes how rooms fold/unfold, how tiles get named, and how constraints evolve.

## License

MIT
