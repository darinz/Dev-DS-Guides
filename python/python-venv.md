# Python Virtual Environment Reference

A guide to managing Python virtual environments using `venv`, `conda`, and `uv`.

## Table of Contents

1. [Using `venv` (Standard Python)](#using-venv-standard-python)
2. [Using `conda`](#using-conda)
3. [Using `uv`](#using-uv)
4. [General Tips](#general-tips)
5. [Why Use Virtual Environments?](#why-use-virtual-environments)

## Using `venv` (Standard Python)

### Requirements

* Python 3.3 or higher installed (use `python3 --version` to check)

### Create a Virtual Environment

```bash
python3 -m venv venv-name
```

* `venv-name` is the directory where the environment will be stored.

### Activate the Environment

* **macOS/Linux**:

  ```bash
  source venv-name/bin/activate
  ```

* **Windows** (CMD):

  ```cmd
  venv-name\Scripts\activate
  ```

* **Windows** (PowerShell):

  ```powershell
  .\venv-name\Scripts\Activate.ps1
  ```

### Deactivate the Environment

```bash
deactivate
```

### Install Packages

```bash
pip install <package-name>
```

### Freeze and Export Dependencies

```bash
pip freeze > requirements.txt
```

### Install from `requirements.txt`

```bash
pip install -r requirements.txt
```

---

## Using `conda`

### Requirements

* Install [Anaconda](https://www.anaconda.com/products/distribution) or [Miniconda](https://docs.conda.io/en/latest/miniconda.html)

### Create a Conda Environment

```bash
conda create --name env-name python=3.11
```

### Activate the Environment

```bash
conda activate env-name
```

### Deactivate the Environment

```bash
conda deactivate
```

### Install Packages

```bash
conda install numpy
```

### Export Environment

```bash
conda env export > environment.yml
```

### Create Environment from File

```bash
conda env create -f environment.yml
```

---

## Using `uv`

[`uv`](https://github.com/astral-sh/uv) is a fast Python package installer and virtual environment manager, drop-in compatible with `pip`.

### Requirements

* Install `uv` using `curl` or `brew`:

```bash
# Using curl
curl -Ls https://astral.sh/uv/install.sh | bash

# Or using Homebrew (macOS/Linux)
brew install astral-sh/uv/uv
```

### Create a New Environment and Install Packages

```bash
uv venv
uv pip install requests
```

* `uv venv` sets up a `.venv` directory in your project.
* `uv pip` works like standard pip but is significantly faster.

### Activate the Environment

* **macOS/Linux**:

  ```bash
  source .venv/bin/activate
  ```

* **Windows**:

  ```cmd
  .venv\Scripts\activate
  ```

### Export Dependencies

```bash
uv pip freeze > requirements.txt
```

### Install from Requirements

```bash
uv pip install -r requirements.txt
```

### Run Scripts in the Virtual Environment

You can run Python scripts with:

```bash
uv pip run python your_script.py
```

---

## General Tips

* Use virtual environments per project to isolate dependencies.
* Avoid installing global packages unless absolutely necessary.
* Add virtual environment directories to `.gitignore`:

```bash
echo ".venv/" >> .gitignore
```

* Common virtual environment folders: `venv`, `.venv`, `env`

## Why Use Virtual Environments?

* Prevents dependency conflicts between projects.
* Enables reproducible development and testing environments.
* Keeps your system Python clean.
* Works seamlessly with Docker, CI/CD tools, and cloud platforms.