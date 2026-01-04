# typed-configparser

A small, modern Python library that provides a typed, validated wrapper around Python's built-in `configparser`.

## What it does

`typed-configparser` lets you load INI configuration files into typed dataclasses or Pydantic models with automatic type conversion and validation. It's thin, explicit, and predictable—no magic, no auto-loading, no framework overhead.

## Example

Given an INI file:

```ini
# app.ini
[server]
port = 8000
debug = true
```

And a dataclass:

```python
from dataclasses import dataclass
from typed_configparser import load_section

@dataclass
class ServerConfig:
    port: int
    debug: bool = False

config = load_section("app.ini", "server", ServerConfig)

assert isinstance(config.port, int)  # True
assert isinstance(config.debug, bool)  # True
```

## Features

- **Typed section loading**: Map INI sections to dataclasses or Pydantic models
- **Type conversion**: Automatic conversion for `int`, `float`, `bool`, and `str`
- **Validation**: Clear errors for missing keys, unknown keys, and type mismatches
- **Optional fields**: Support for `Optional[T]` and default values
- **Python 3.10+**: Full type hints throughout

## Non-goals

This library intentionally does **not** provide:

- Environment variable support
- Multiple file merging
- Remote configuration loading
- CLI tools
- Plugin systems
- Section inheritance
- Logging framework integration

If you need these features, this library is not for you.

