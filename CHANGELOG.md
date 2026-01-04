# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- Comprehensive publishing guide (`PUBLISHING.md`) with step-by-step instructions
- Development tooling configuration (mypy, ruff) in `pyproject.toml`
- Separate `test` and `dev` optional dependency groups
- Python 3.13 support in classifiers
- Additional metadata fields (maintainers, expanded keywords)

### Changed
- Updated package name from `typed-configparser` to `configparser-typed` for PyPI publication
- Updated `pyproject.toml` to use modern SPDX license format (`license = "MIT"`)
- Improved project URLs structure for better readability
- Enhanced pytest configuration with strict markers and coverage options
- Expanded keywords for better package discoverability

### Fixed
- Removed deprecated license table format to eliminate build warnings
- Removed deprecated license classifier in favor of SPDX expression

## [0.1.0] - 2026-01-04

### Added
- Initial release of typed-configparser
- Core `ConfigLoader` class for loading and validating INI configuration files
- Type casting support for common Python types (str, int, float, bool, list, dict, Path)
- Automatic type validation using Python type hints
- Support for dataclass-based configuration schemas
- Custom error types (`ConfigError`, `ValidationError`, `MissingKeyError`)
- Comprehensive test suite
- Type hints and `py.typed` marker for full typing support
- Support for Python 3.10, 3.11, and 3.12

### Features
- Seamless integration with Python's built-in `configparser`
- Automatic type conversion based on dataclass field annotations
- Clear error messages for validation failures
- Support for optional fields with default values
- Path handling with automatic `pathlib.Path` conversion

[Unreleased]: https://github.com/PierrunoYT/typed-configparser/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/PierrunoYT/typed-configparser/releases/tag/v0.1.0

