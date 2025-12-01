# Python Setup Guide for Cursor

This guide will help you set up Python for use in Cursor (which is based on VSCode).

## Prerequisites

### 1. Install Python

If Python is not installed on your computer, install it first:

- **macOS**:
  - Recommended: Use [Homebrew](https://brew.sh/): `brew install python` or `brew install python@3.11`
  - Alternative: Download from [python.org](https://www.python.org/downloads/)
- **Windows**: Download from [python.org](https://www.python.org/downloads/) (make sure to check "Add Python to PATH" during installation)
- **Linux**: Usually pre-installed, or use your distribution's package manager:
  - Ubuntu/Debian: `sudo apt-get install python3 python3-pip`
  - Fedora: `sudo dnf install python3 python3-pip`

After installation, verify Python is working by opening a terminal and running:

```bash
python3 --version
# or
python --version
```

You should see something like `Python 3.11.5` or similar.

### 2. Verify pip Installation

`pip` (Python package installer) should come with Python. Verify it's installed:

```bash
pip3 --version
# or
pip --version
```

If `pip` is not installed:

- **macOS/Linux**: `python3 -m ensurepip --upgrade`
- **Windows**: Usually comes with Python installer, or run: `python -m ensurepip --upgrade`

## Virtual Environments

Virtual environments allow you to create isolated Python environments for different projects, preventing package conflicts.

### Option A: Using venv (Built-in)

`venv` comes with Python 3.3+ and is the recommended approach for most projects.

#### Creating a Virtual Environment

1. Navigate to your project directory in the terminal:

```bash
cd /path/to/your/project
```

2. Create a virtual environment:

```bash
python3 -m venv .venv
# or
python -m venv .venv
```

This creates a `.venv` folder in your project directory.

#### Activating the Virtual Environment

- **macOS/Linux**:

  ```bash
  source .venv/bin/activate
  ```

- **Windows (Command Prompt)**:

  ```bash
  .venv\Scripts\activate
  ```

- **Windows (PowerShell)**:
  ```bash
  .venv\Scripts\Activate.ps1
  ```

When activated, you'll see `(.venv)` at the beginning of your terminal prompt.

#### Deactivating the Virtual Environment

```bash
deactivate
```

#### Installing Packages in venv

Once activated, install packages normally:

```bash
pip install package-name
```

For example, to install common data science packages:

```bash
pip install pandas numpy matplotlib jupyter
```

#### Creating requirements.txt

To save your project's dependencies:

```bash
pip freeze > requirements.txt
```

To install from requirements.txt:

```bash
pip install -r requirements.txt
```

### Option B: Using Conda

Conda is a package and environment manager that can handle both Python packages and system dependencies.

#### Installing Conda

1. **Miniconda** (recommended, lighter): Download from [conda.io](https://docs.conda.io/en/latest/miniconda.html)
2. **Anaconda** (full distribution): Download from [anaconda.com](https://www.anaconda.com/products/distribution)

After installation, verify conda is installed:

```bash
conda --version
```

#### Creating a Conda Environment

1. Navigate to your project directory:

```bash
cd /path/to/your/project
```

2. Create a conda environment:

```bash
# Create environment with specific Python version
conda create -n myenv python=3.11

# Or create environment in project directory (.conda folder)
conda create --prefix .conda python=3.11
```

#### Activating the Conda Environment

```bash
# If named environment
conda activate myenv

# If using .conda folder
conda activate .conda
```

#### Installing Packages in Conda

```bash
# Using conda (preferred for conda packages)
conda install package-name

# Using pip (for packages not available in conda)
pip install package-name
```

#### Deactivating the Conda Environment

```bash
conda deactivate
```

#### Exporting Conda Environment

To save your environment:

```bash
# Export to environment.yml
conda env export > environment.yml

# Or for cross-platform compatibility
conda env export --from-history > environment.yml
```

To recreate from environment.yml:

```bash
conda env create -f environment.yml
```

## Cursor Setup

### 3. Install Python Extension

1. Open Cursor
2. Go to Extensions (click the Extensions icon in the left sidebar, or press `Cmd+Shift+X` on macOS / `Ctrl+Shift+X` on Windows/Linux)
3. Search for "Python" (by Microsoft)
4. Click "Install"

The Python extension provides:

- IntelliSense (code completion)
- Linting
- Debugging
- Jupyter notebook support
- Code formatting

### 4. Select Python Interpreter

After installing the Python extension:

1. Open a Python file (`.py`) in your project
2. Press `Cmd+Shift+P` (macOS) or `Ctrl+Shift+P` (Windows/Linux) to open the command palette
3. Type "Python: Select Interpreter"
4. Choose your virtual environment interpreter:
   - For `.venv`: Select `.venv/bin/python` (macOS/Linux) or `.venv\Scripts\python.exe` (Windows)
   - For `.conda`: Select `.conda/bin/python` (macOS/Linux) or `.conda\Scripts\python.exe` (Windows)
   - Or select your system Python if not using a virtual environment

You can also click on the Python version in the bottom status bar to change the interpreter.

### 5. Recommended Settings

Open Settings (JSON) by pressing `Cmd+,` (macOS) or `Ctrl+,` (Windows/Linux) and clicking "Open Settings (JSON)". Add:

```json
{
  // Auto-activate virtual environment when opening terminal
  "python.terminal.activateEnvironment": true,

  // Auto-detect virtual environments
  "python.venvPath": "${workspaceFolder}",

  // Enable linting
  "python.linting.enabled": true,
  "python.linting.pylintEnabled": false,
  "python.linting.flake8Enabled": true,

  // Format on save
  "editor.formatOnSave": true,
  "python.formatting.provider": "black",

  // Auto-save
  "files.autoSave": "afterDelay",
  "files.autoSaveDelay": 1000
}
```

### 6. Installing Additional Extensions (Optional)

- **Pylance**: Enhanced language server (usually comes with Python extension)
- **Jupyter**: For notebook support (if using Jupyter notebooks)
- **Python Docstring Generator**: For generating docstrings
- **autopep8** or **Black Formatter**: Code formatters

## Using Python in Cursor

### Running Python Code

1. **Run entire file**:

   - Right-click in the editor and select "Run Python File in Terminal"
   - Or press `F5` and select "Python File"
   - Or use the play button in the top right

2. **Run selected code**:

   - Select code and press `Shift+Enter`
   - Or right-click and select "Run Selection/Line in Python Terminal"

3. **Run in interactive mode**:
   - Use the Python REPL: `Cmd+Shift+P` → "Python: Start REPL"
   - Or open terminal and type `python` (make sure virtual environment is activated)

### Debugging

1. Set breakpoints by clicking in the gutter (left of line numbers)
2. Press `F5` to start debugging
3. Select "Python File" configuration
4. Use the debug toolbar to step through code

### Jupyter Notebooks

1. Create a `.ipynb` file
2. Select a kernel (Python interpreter) from the top right
3. Run cells with `Shift+Enter`
4. Install Jupyter if needed: `pip install jupyter`

## Project Structure Best Practices

A typical Python project structure:

```
my-project/
├── .venv/              # Virtual environment (or .conda/)
├── src/                 # Source code
│   └── main.py
├── tests/               # Test files
│   └── test_main.py
├── requirements.txt     # Dependencies (for venv)
├── environment.yml      # Conda environment (if using conda)
├── .gitignore          # Git ignore file
└── README.md
```

### .gitignore for Python

Create a `.gitignore` file in your project root:

```
# Virtual environments
.venv/
.conda/
venv/
env/
ENV/

# Python
__pycache__/
*.py[cod]
*$py.class
*.so
.Python

# Jupyter
.ipynb_checkpoints

# IDE
.vscode/
.idea/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db
```

## Troubleshooting

### Python Not Detected

- Verify Python is installed: Run `python3 --version` or `python --version` in terminal
- Check PATH: Make sure Python is in your system PATH
- Restart Cursor after installing Python
- Manually set Python path in Cursor settings:
  ```json
  {
    "python.defaultInterpreterPath": "/usr/local/bin/python3"
  }
  ```

### Virtual Environment Not Activating

- **venv issues**:

  - Make sure you're in the project directory
  - Check that `.venv` folder exists
  - Try recreating: `rm -rf .venv && python3 -m venv .venv`
  - On Windows PowerShell, you may need to run: `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser`

- **conda issues**:
  - Verify conda is installed: `conda --version`
  - Initialize conda for your shell: `conda init zsh` (or `bash`, `fish`, etc.)
  - Restart terminal after conda init

### Packages Not Installing

- Make sure virtual environment is activated (check for `(.venv)` or `(.conda)` in prompt)
- Upgrade pip: `pip install --upgrade pip`
- Check internet connection
- Try using `--user` flag: `pip install --user package-name`
- For conda: Try `conda update conda` first

### Wrong Python Interpreter Selected

- Check bottom status bar for current interpreter
- Click on it to change, or use `Cmd+Shift+P` → "Python: Select Interpreter"
- Make sure you're selecting the interpreter from your virtual environment

### Import Errors

- Verify package is installed: `pip list` or `conda list`
- Check that correct virtual environment is activated
- Verify Python path in Cursor matches the virtual environment
- Try restarting Cursor

### Linting/Formatting Not Working

- Install the linter/formatter:
  ```bash
  pip install flake8 black autopep8
  ```
- Check extension is enabled in Cursor
- Verify settings are correct in Settings (JSON)
- Reload Cursor window: `Cmd+Shift+P` → "Developer: Reload Window"

### Terminal Not Using Virtual Environment

- Enable auto-activation: Set `"python.terminal.activateEnvironment": true` in settings
- Manually activate in terminal: `source .venv/bin/activate` (macOS/Linux) or `.venv\Scripts\activate` (Windows)
- Check that `.venv` is in your project root

### Conda Environment Issues

- Update conda: `conda update conda`
- Clean conda cache: `conda clean --all`
- Recreate environment if corrupted
- Check conda channels: `conda config --show channels`

## Additional Resources

- [Python Official Documentation](https://docs.python.org/3/)
- [venv Documentation](https://docs.python.org/3/library/venv.html)
- [Conda Documentation](https://docs.conda.io/)
- [Python Extension for VSCode](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
