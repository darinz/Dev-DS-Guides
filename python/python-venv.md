# Python Virtual Environment Reference

A comprehensive guide to managing Python virtual environments, dependency management, and best practices for Python development.

## Table of Contents

1. [Understanding Virtual Environments](#understanding-virtual-environments)
2. [Using `venv` (Standard Python)](#using-venv-standard-python)
3. [Using `conda`](#using-conda)
4. [Using `uv`](#using-uv)
5. [Using `pipenv`](#using-pipenv)
6. [Using `poetry`](#using-poetry)
7. [Using `pyenv`](#using-pyenv)
8. [Dependency Management](#dependency-management)
9. [Security Best Practices](#security-best-practices)
10. [Troubleshooting](#troubleshooting)
11. [Deployment Considerations](#deployment-considerations)
12. [Best Practices](#best-practices)

## Understanding Virtual Environments

### What are Virtual Environments?

Virtual environments are isolated Python environments that allow you to install packages and dependencies specific to a project without affecting your system Python installation or other projects.

### Benefits

- **Isolation**: Each project has its own dependencies
- **Reproducibility**: Exact same environment across different machines
- **Clean System**: Keep your system Python installation clean
- **Version Control**: Use different Python versions for different projects
- **Security**: Isolate potentially vulnerable packages

### When to Use Virtual Environments

- **Always** for Python projects
- When working on multiple projects with different dependencies
- When you need specific package versions
- For production deployments
- When collaborating with other developers

## Using `venv` (Standard Python)

### Requirements

* Python 3.3 or higher installed (use `python3 --version` to check)

### Basic Usage

```bash
# Create a virtual environment
python3 -m venv venv-name
# Example: python3 -m venv myproject

# Create with specific Python version
python3.11 -m venv venv-name
# Example: python3.11 -m venv myproject

# Create with specific directory structure
python3 -m venv --system-site-packages venv-name  # Include system packages
python3 -m venv --copies venv-name                 # Copy instead of symlink
```

### Activation

**macOS/Linux:**
```bash
source venv-name/bin/activate
# Example: source myproject/bin/activate
```

**Windows (CMD):**
```cmd
venv-name\Scripts\activate
# Example: myproject\Scripts\activate
```

**Windows (PowerShell):**
```powershell
.\venv-name\Scripts\Activate.ps1
# Example: .\myproject\Scripts\Activate.ps1
```

### Deactivation

```bash
deactivate
```

### Package Management

```bash
# Install packages
pip install package-name
pip install package-name==1.2.3  # Specific version
pip install package-name>=1.2.0  # Minimum version

# Install multiple packages
pip install package1 package2 package3

# Install from requirements file
pip install -r requirements.txt

# Install in development mode
pip install -e .

# Upgrade packages
pip install --upgrade package-name
pip install --upgrade pip

# List installed packages
pip list
pip freeze

# Show package info
pip show package-name
```

### Dependency Management

```bash
# Generate requirements file
pip freeze > requirements.txt

# Generate requirements with specific format
pip freeze --local > requirements.txt  # Only local packages
pip list --format=freeze > requirements.txt

# Install from requirements with constraints
pip install -r requirements.txt --constraint constraints.txt

# Example requirements.txt:
# requests==2.28.1
# numpy>=1.21.0
# pandas~=1.5.0
```

### Advanced venv Usage

```bash
# Create environment with specific Python executable
python3.11 -m venv venv-name --python=python3.11

# Create environment with specific packages pre-installed
python3 -m venv venv-name
source venv-name/bin/activate
pip install --upgrade pip setuptools wheel

# Create environment with specific pip version
python3 -m venv venv-name
source venv-name/bin/activate
pip install --upgrade pip==23.0.1
```

## Using `conda`

### Requirements

* Install [Anaconda](https://www.anaconda.com/products/distribution) or [Miniconda](https://docs.conda.io/en/latest/miniconda.html)

### Basic Usage

```bash
# Create a conda environment
conda create --name env-name python=3.11
# Example: conda create --name myproject python=3.11

# Create with specific packages
conda create --name env-name python=3.11 numpy pandas
# Example: conda create --name data-science python=3.11 numpy pandas matplotlib

# Create from environment file
conda env create -f environment.yml

# List environments
conda env list
conda info --envs

# Activate environment
conda activate env-name
# Example: conda activate myproject

# Deactivate environment
conda deactivate
```

### Package Management

```bash
# Install packages
conda install package-name
conda install package-name=1.2.3  # Specific version
conda install -c conda-forge package-name  # From conda-forge channel

# Install multiple packages
conda install numpy pandas matplotlib

# Install pip packages in conda environment
conda activate env-name
pip install package-name

# Update packages
conda update package-name
conda update --all

# Remove packages
conda remove package-name

# List installed packages
conda list
conda list package-name  # Specific package
```

### Environment Management

```bash
# Export environment
conda env export > environment.yml
conda env export --no-builds > environment.yml  # Without build numbers

# Export with specific format
conda env export --from-history > environment.yml  # Only explicitly installed

# Clone environment
conda create --name new-env --clone existing-env

# Remove environment
conda env remove --name env-name

# Clean unused packages
conda clean --all
```

### Advanced Conda Usage

```bash
# Create environment with specific channel priority
conda create --name env-name python=3.11 -c conda-forge -c defaults

# Install packages with specific channel
conda install -c conda-forge package-name

# Create environment with specific platform
conda create --name env-name python=3.11 --platform linux-64

# Example environment.yml:
# name: myproject
# channels:
#   - conda-forge
#   - defaults
# dependencies:
#   - python=3.11
#   - numpy=1.24
#   - pandas>=1.5.0
#   - pip
#   - pip:
#     - requests==2.28.1
```

## Using `uv`

[`uv`](https://github.com/astral-sh/uv) is a fast Python package installer and virtual environment manager, drop-in compatible with `pip`.

### Installation

```bash
# Using curl
curl -Ls https://astral.sh/uv/install.sh | bash

# Using Homebrew (macOS/Linux)
brew install astral-sh/uv/uv

# Using pip
pip install uv

# Verify installation
uv --version
```

### Basic Usage

```bash
# Create a new environment
uv venv
# Creates .venv directory in current project

# Create with specific Python version
uv venv --python 3.11

# Create with specific name
uv venv --name myproject

# Activate environment
source .venv/bin/activate  # macOS/Linux
.venv\Scripts\activate     # Windows

# Install packages
uv pip install package-name
uv pip install package-name==1.2.3

# Install from requirements
uv pip install -r requirements.txt

# Run scripts in environment
uv run python script.py
uv run python -m pytest
```

### Advanced uv Usage

```bash
# Install with specific index
uv pip install --index-url https://pypi.org/simple/ package-name

# Install with extra dependencies
uv pip install "package-name[extra]"

# Install in development mode
uv pip install -e .

# Generate requirements
uv pip freeze > requirements.txt

# Sync environment from requirements
uv pip sync requirements.txt

# Add package to requirements
uv add package-name
uv add "package-name>=1.2.0"

# Remove package
uv remove package-name

# Update packages
uv pip install --upgrade package-name
```

### uv.toml Configuration

```toml
# uv.toml example
[project]
name = "myproject"
version = "0.1.0"
description = "My Python project"
requires-python = ">=3.8"

[project.dependencies]
requests = "^2.28.0"
numpy = ">=1.21.0"
pandas = "~=1.5.0"

[project.optional-dependencies]
dev = [
    "pytest>=7.0.0",
    "black>=22.0.0",
    "flake8>=5.0.0",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

## Using `pipenv`

[`pipenv`](https://pipenv.pypa.io/) combines pip and virtualenv into a single tool.

### Installation

```bash
# Install pipenv
pip install pipenv

# Verify installation
pipenv --version
```

### Basic Usage

```bash
# Create new project
pipenv install
# Creates Pipfile and Pipfile.lock

# Install package
pipenv install package-name
pipenv install package-name==1.2.3

# Install development dependencies
pipenv install --dev package-name
pipenv install --dev pytest black flake8

# Activate environment
pipenv shell

# Run command in environment
pipenv run python script.py
pipenv run pytest

# Exit environment
exit
```

### Advanced pipenv Usage

```bash
# Install from requirements.txt
pipenv install -r requirements.txt

# Generate requirements.txt
pipenv requirements > requirements.txt

# Lock dependencies
pipenv lock

# Install from lock file
pipenv install --ignore-pipfile

# Update packages
pipenv update
pipenv update package-name

# Remove package
pipenv uninstall package-name

# Show dependency graph
pipenv graph

# Check security vulnerabilities
pipenv check
```

### Pipfile Example

```toml
# Pipfile example
[[source]]
url = "https://pypi.org/simple"
verify_ssl = true
name = "pypi"

[packages]
requests = "==2.28.1"
numpy = ">=1.21.0"
pandas = "~=1.5.0"

[dev-packages]
pytest = "*"
black = "*"
flake8 = "*"

[requires]
python_version = "3.11"
```

## Using `poetry`

[`poetry`](https://python-poetry.org/) is a modern dependency management and packaging tool.

### Installation

```bash
# Install poetry
curl -sSL https://install.python-poetry.org | python3 -

# Verify installation
poetry --version
```

### Basic Usage

```bash
# Create new project
poetry new myproject
cd myproject

# Initialize in existing project
poetry init

# Install dependencies
poetry install

# Add package
poetry add package-name
poetry add package-name@^1.2.0

# Add development dependency
poetry add --group dev package-name
poetry add --group dev pytest black

# Activate environment
poetry shell

# Run command in environment
poetry run python script.py
poetry run pytest

# Exit environment
exit
```

### Advanced poetry Usage

```bash
# Update dependencies
poetry update
poetry update package-name

# Remove package
poetry remove package-name

# Show dependency tree
poetry show --tree

# Export requirements
poetry export -f requirements.txt --output requirements.txt

# Build package
poetry build

# Publish package
poetry publish

# Check for security vulnerabilities
poetry audit
```

### pyproject.toml Example

```toml
# pyproject.toml example
[tool.poetry]
name = "myproject"
version = "0.1.0"
description = "My Python project"
authors = ["Your Name <your.email@example.com>"]
readme = "README.md"
packages = [{include = "myproject"}]

[tool.poetry.dependencies]
python = "^3.8"
requests = "^2.28.0"
numpy = "^1.21.0"
pandas = "^1.5.0"

[tool.poetry.group.dev.dependencies]
pytest = "^7.0.0"
black = "^22.0.0"
flake8 = "^5.0.0"

[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"
```

## Using `pyenv`

[`pyenv`](https://github.com/pyenv/pyenv) manages multiple Python versions.

### Installation

**macOS:**
```bash
brew install pyenv
```

**Linux:**
```bash
curl https://pyenv.run | bash
```

**Windows:**
```bash
# Use pyenv-win
pip install pyenv-win
```

### Basic Usage

```bash
# List available Python versions
pyenv install --list

# Install Python version
pyenv install 3.11.0
pyenv install 3.10.8

# List installed versions
pyenv versions

# Set global Python version
pyenv global 3.11.0

# Set local Python version (per directory)
pyenv local 3.10.8

# Use specific version for command
pyenv exec python --version

# Show current version
pyenv version
```

### Advanced pyenv Usage

```bash
# Install with specific options
PYTHON_CONFIGURE_OPTS="--enable-shared" pyenv install 3.11.0

# Uninstall Python version
pyenv uninstall 3.11.0

# Rehash shims (after installing new version)
pyenv rehash

# Show Python path
pyenv which python

# Install with specific build dependencies
# (Ubuntu/Debian)
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev \
libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm \
libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev

pyenv install 3.11.0
```

### Shell Configuration

Add to your shell profile (`.bashrc`, `.zshrc`, etc.):

```bash
# pyenv configuration
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init --path)"
eval "$(pyenv init -)"
```

## Dependency Management

### Requirements Files

```txt
# requirements.txt example
# Core dependencies
requests==2.28.1
numpy>=1.21.0
pandas~=1.5.0

# Development dependencies
pytest>=7.0.0
black>=22.0.0
flake8>=5.0.0

# Optional dependencies
matplotlib>=3.5.0; python_version >= "3.8"
```

### Constraints Files

```txt
# constraints.txt example
# Pin specific versions for security/reproducibility
requests==2.28.1
urllib3==1.26.12
certifi==2022.12.7
```

### Dependency Resolution

```bash
# Install with dependency resolution
pip install --upgrade pip setuptools wheel
pip install -r requirements.txt

# Use pip-tools for better dependency management
pip install pip-tools
pip-compile requirements.in
pip-sync requirements.txt

# Example requirements.in:
# requests>=2.28.0
# numpy>=1.21.0
# pandas>=1.5.0
```

### Version Specifiers

```bash
# Version specifiers
package==1.2.3      # Exact version
package>=1.2.0      # Minimum version
package<=1.2.9      # Maximum version
package~=1.2.0      # Compatible release (>=1.2.0,<1.3.0)
package!=1.2.3      # Exclude version
package>=1.2.0,<2.0.0  # Version range
```

## Security Best Practices

### Package Security

```bash
# Check for security vulnerabilities
pip-audit
safety check

# Install security updates
pip-audit --fix

# Use trusted package indexes
pip install --index-url https://pypi.org/simple/ package-name

# Verify package signatures
pip install --require-hashes -r requirements.txt
```

### Environment Security

```bash
# Use specific Python versions
python3.11 -m venv venv-name

# Keep pip updated
pip install --upgrade pip

# Use virtual environments for all projects
python3 -m venv .venv
source .venv/bin/activate

# Don't install packages globally
# Always use virtual environments
```

### Dependency Pinning

```bash
# Pin exact versions for production
pip freeze > requirements.txt

# Use constraints for security
pip install -r requirements.txt --constraint constraints.txt

# Regular security updates
pip-audit --fix
pip install --upgrade package-name
```

## Troubleshooting

### Common Issues

```bash
# Permission errors
sudo chown -R $USER:$USER venv-name/
chmod +x venv-name/bin/activate

# Python version conflicts
python3 --version
which python3
pyenv versions

# Package installation issues
pip install --upgrade pip setuptools wheel
pip install --force-reinstall package-name

# Environment activation issues
# Check if virtual environment exists
ls -la venv-name/bin/activate

# Recreate virtual environment
rm -rf venv-name/
python3 -m venv venv-name
```

### Performance Issues

```bash
# Use faster package installers
pip install --upgrade pip
pip install uv  # Much faster than pip

# Use mirrors for faster downloads
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple/ package-name

# Cache pip downloads
pip install --cache-dir ~/.cache/pip package-name

# Use pre-compiled wheels
pip install --only-binary=all package-name
```

### Environment Conflicts

```bash
# Check for conflicting packages
pip check

# Show package dependencies
pip show package-name

# Resolve conflicts manually
pip uninstall conflicting-package
pip install correct-package

# Use conda for complex scientific packages
conda install numpy scipy matplotlib
```

## Deployment Considerations

### Production Environments

```bash
# Use specific Python versions
python3.11 -m venv venv-name

# Pin all dependencies
pip freeze > requirements.txt

# Use constraints for security
pip install -r requirements.txt --constraint constraints.txt

# Minimize environment size
pip install --no-cache-dir -r requirements.txt
```

### Docker Integration

```dockerfile
# Dockerfile example
FROM python:3.11-slim

WORKDIR /app

# Copy requirements first for better caching
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY . .

CMD ["python", "app.py"]
```

### CI/CD Integration

```yaml
# GitHub Actions example
name: Python CI

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: [3.9, 3.10, 3.11]
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up Python ${{ matrix.python-version }}
      uses: actions/setup-python@v4
      with:
        python-version: ${{ matrix.python-version }}
    
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt
    
    - name: Run tests
      run: |
        pytest
```

## Best Practices

### Environment Management

```bash
# Use consistent naming
python3 -m venv .venv  # Hidden directory
python3 -m venv venv   # Visible directory

# Add to .gitignore
echo ".venv/" >> .gitignore
echo "venv/" >> .gitignore
echo "__pycache__/" >> .gitignore
echo "*.pyc" >> .gitignore

# Use environment variables
export PYTHONPATH="${PYTHONPATH}:/path/to/project"
```

### Dependency Management

```bash
# Keep requirements minimal
pip install only-necessary-packages

# Use version specifiers
pip install "package>=1.2.0,<2.0.0"

# Separate dev dependencies
pip install --user package-name  # For development tools

# Regular updates
pip install --upgrade pip
pip install --upgrade package-name
```

### Project Structure

```
myproject/
├── .venv/              # Virtual environment
├── src/                # Source code
│   └── myproject/
├── tests/              # Test files
├── requirements.txt    # Dependencies
├── requirements-dev.txt # Development dependencies
├── setup.py           # Package setup
├── pyproject.toml     # Modern package config
└── README.md
```

### Workflow Tips

```bash
# Always activate virtual environment
source .venv/bin/activate

# Use pip-tools for dependency management
pip install pip-tools
pip-compile requirements.in
pip-sync requirements.txt

# Use pre-commit hooks
pip install pre-commit
pre-commit install

# Regular security checks
pip-audit
safety check
```

**Note**: This guide covers the most commonly used Python virtual environment tools and best practices. Choose the tool that best fits your workflow and project requirements. For more detailed information, refer to the official documentation of each tool.