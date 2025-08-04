# Project Conventions

[![Static Badge](https://img.shields.io/badge/Python-3.11-356d9f)](https://www.python.org/downloads/release/python-3110/)[![Static Badge](https://img.shields.io/badge/Sphinx-v7.4.7-094167)](https://www.sphinx-doc.org/en/master/changes/7.4.html)[![Static Badge](https://img.shields.io/badge/sphinx_rtd_theme-v2.0.0-e3e8eb)](https://pypi.org/project/sphinx-rtd-theme/)

## Description 📖

This documentation contains project development guidelines, tool tutorials, and notes. The content is still under construction, so if there are any mistakes, corrections are welcome! Let’s learn and improve together! 😄🥳🎉

- Read the Docs URL: 🌱
    - [master](https://projectconventions.readthedocs.io/en/master/)：A new version will be released whenever a chapter is completed. 🥸
    - [develop](https://projectconventions.readthedocs.io/en/develop/)：Get a sneak peek of small updates as they happen! 🤩


## Run Development 🧰

### 1. Installation Environment

Install Python `uv`, a fast and modern Python package manager.

On macOS and Linux:
```
curl -LsSf https://astral.sh/uv/install.sh | sh
```

On Windows:
```
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

Or pip
```
pip install uv
```

> You can refer to the [uv GitHub repository](https://github.com/astral-sh/uv) for more documentation.
This tool is really fast and useful — highly recommended for Python projects!

### 2. Initialize the environment

Create a Python virtual environment through uv tool.

```
uv venv
```

### 3. Build Sphinx Documentation

Build the Sphinx HTML docs.

```
uv run make html
```

You can then open the generated documentation:

On MacOs and Linux:
```
open build/html/index.html  # macOS/Linux
```

On Windows:
```
start build\html\index.html
```