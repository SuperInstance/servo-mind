# servo-mind

Encoder feedback + MetaConstraint + TransferFunction — the adaptive constraint learner

## Dependencies

tile-lifecycle

## Usage

```python
from core.servo_mind import ...
```

## Shell Loading

This tool can be loaded into any PLATO shell environment:

```python
# Neo loads this tool from the weapon rack
from plato_shell_bridge import PlatoShell
shell = PlatoShell("agent-shell")
shell.load_tool("servo-mind")
```

## Tests

```bash
python3 -m pytest tests/test_servo_mind.py -v
```

## License

MIT — Part of the Cocapn Fleet Intelligence System
