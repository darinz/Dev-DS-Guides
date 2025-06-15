# Python Virtual Environment Reference

A guide to managing Python virtual environments using both `venv` and `conda`.


## Table of Contents

1. [Using `venv` (Standard Python)](#using-venv-standard-python)
2. [Using `conda`](#using-conda)
3. [General Tips](#general-tips)
4. [Why Use Virtual Environments?](#why-use-virtual-environments)

---

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


## General Tips

* Use virtual environments per project to isolate dependencies.
* Avoid installing global packages unless absolutely necessary.
* Use `.gitignore` to exclude virtual environment folders (`venv/`, `env/`, `.venv/`).
* Common virtual environment directory names: `venv`, `.venv`, `env`

```bash
echo "venv/" >> .gitignore
```


## Why Use Virtual Environments?

* Prevents conflicts between project dependencies.
* Enables reproducible builds and easier collaboration.
* Keeps your system Python clean and stable.
* Works well with tools like Docker, CI/CD, and deployment platforms.