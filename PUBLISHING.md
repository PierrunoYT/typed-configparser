# Publishing Guide for typed-configparser

This guide walks you through the process of building and publishing the `typed-configparser` package to PyPI.

## Prerequisites

### 1. Install Required Tools

```powershell
pip install build twine
```

### 2. Create PyPI Account

- Production PyPI: https://pypi.org/account/register/
- Test PyPI (for testing): https://test.pypi.org/account/register/

### 3. Generate API Tokens

For security, use API tokens instead of passwords:

1. Go to https://pypi.org/manage/account/token/
2. Click "Add API token"
3. Give it a name (e.g., "typed-configparser-upload")
4. Set scope to "Entire account" or specific to this project
5. Copy the token (starts with `pypi-`)

Repeat for Test PyPI: https://test.pypi.org/manage/account/token/

### 4. Configure Authentication

Create or edit `~/.pypirc` (on Windows: `%USERPROFILE%\.pypirc`):

```ini
[distutils]
index-servers =
    pypi
    testpypi

[pypi]
username = __token__
password = pypi-YourActualProductionTokenHere

[testpypi]
repository = https://test.pypi.org/legacy/
username = __token__
password = pypi-YourActualTestTokenHere
```

**Important:** Keep this file secure and never commit it to version control!

## Pre-Publishing Checklist

Before publishing, ensure:

- [ ] All tests pass: `pytest`
- [ ] Code is properly typed: `mypy typed_configparser/`
- [ ] Code is linted: `ruff check .`
- [ ] Version number is updated in `pyproject.toml`
- [ ] `CHANGELOG.md` is updated with changes
- [ ] `README.md` is up to date
- [ ] All changes are committed to git
- [ ] You're on the main/master branch

## Building the Package

### Step 1: Clean Previous Builds

```powershell
# Remove old build artifacts
Remove-Item -Recurse -Force dist, build, *.egg-info -ErrorAction SilentlyContinue
```

### Step 2: Build Distribution Files

```powershell
python -m build
```

This creates two files in the `dist/` directory:
- `typed_configparser-X.Y.Z-py3-none-any.whl` (wheel distribution)
- `typed_configparser-X.Y.Z.tar.gz` (source distribution)

### Step 3: Verify the Build

```powershell
twine check dist/*
```

This checks for common issues with your package metadata.

## Publishing to Test PyPI (Recommended First)

Always test on Test PyPI before publishing to production:

```powershell
twine upload --repository testpypi dist/*
```

### Test Installation from Test PyPI

```powershell
# Create a test environment
python -m venv test_env
.\test_env\Scripts\Activate.ps1

# Install from Test PyPI
pip install --index-url https://test.pypi.org/simple/ typed-configparser

# Test that it works
python -c "from typed_configparser import ConfigLoader; print('Success!')"

# Clean up
deactivate
Remove-Item -Recurse -Force test_env
```

## Publishing to Production PyPI

Once you've verified everything works on Test PyPI:

```powershell
twine upload dist/*
```

Your package will be available at: https://pypi.org/project/typed-configparser/

### Verify Production Installation

```powershell
pip install typed-configparser
```

## Post-Publishing Steps

1. **Create a Git Tag**
   ```powershell
   git tag -a v0.1.0 -m "Release version 0.1.0"
   git push origin v0.1.0
   ```

2. **Create GitHub Release**
   - Go to: https://github.com/PierrunoYT/typed-configparser/releases/new
   - Select the tag you just created
   - Add release notes from `CHANGELOG.md`
   - Attach the distribution files from `dist/`

3. **Announce the Release**
   - Update project documentation
   - Post on social media, forums, etc.

## Version Bumping for Future Releases

When preparing a new release:

1. **Update Version Number** in `pyproject.toml`:
   ```toml
   version = "0.2.0"  # Follow semantic versioning
   ```

2. **Update CHANGELOG.md**:
   - Move items from `[Unreleased]` to new version section
   - Add release date
   - Create new empty `[Unreleased]` section

3. **Commit Changes**:
   ```powershell
   git add pyproject.toml CHANGELOG.md
   git commit -m "Bump version to 0.2.0"
   git push
   ```

4. **Follow the publishing steps above**

## Semantic Versioning Guidelines

Follow [Semantic Versioning](https://semver.org/):

- **MAJOR** (1.0.0): Breaking changes
- **MINOR** (0.1.0): New features, backwards compatible
- **PATCH** (0.0.1): Bug fixes, backwards compatible

Examples:
- `0.1.0` → `0.1.1`: Bug fix
- `0.1.0` → `0.2.0`: New feature
- `0.9.0` → `1.0.0`: First stable release or breaking change

## Troubleshooting

### "File already exists" Error

PyPI doesn't allow re-uploading the same version. Solutions:
- Bump the version number
- Delete the release on PyPI (only for mistakes)

### Authentication Failed

- Verify your API token is correct in `~/.pypirc`
- Ensure username is `__token__` (with double underscores)
- Check token hasn't expired

### Import Errors After Installation

- Verify `typed_configparser/__init__.py` exports the right symbols
- Check `tool.setuptools.packages` in `pyproject.toml`
- Ensure `py.typed` file exists for type hints

### Build Warnings

- Update deprecated configurations in `pyproject.toml`
- Run `twine check dist/*` to identify issues

## Automated Publishing with GitHub Actions

For automated releases, create `.github/workflows/publish.yml`:

```yaml
name: Publish to PyPI

on:
  release:
    types: [published]

permissions:
  contents: read

jobs:
  publish:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Set up Python
      uses: actions/setup-python@v5
      with:
        python-version: '3.10'
    
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install build twine
    
    - name: Build package
      run: python -m build
    
    - name: Check package
      run: twine check dist/*
    
    - name: Publish to PyPI
      env:
        TWINE_USERNAME: __token__
        TWINE_PASSWORD: ${{ secrets.PYPI_API_TOKEN }}
      run: twine upload dist/*
```

Add your PyPI API token as a GitHub secret named `PYPI_API_TOKEN`.

## Quick Reference Commands

```powershell
# Complete publishing workflow
Remove-Item -Recurse -Force dist, build, *.egg-info -ErrorAction SilentlyContinue
python -m build
twine check dist/*
twine upload --repository testpypi dist/*  # Test first
twine upload dist/*                        # Production
git tag -a v0.1.0 -m "Release v0.1.0"
git push origin v0.1.0
```

## Support

If you encounter issues:
- Check PyPI status: https://status.python.org/
- PyPI help: https://pypi.org/help/
- Packaging guide: https://packaging.python.org/

