# R Setup Guide for Cursor

This guide will help you set up R for use in Cursor (which is based on VSCode).

## Prerequisites

### 1. Install R

If R is not installed on your computer, install it first:

- **macOS**: Download from [CRAN](https://cran.r-project.org/bin/macosx/)
- **Windows**: Download from [CRAN](https://cran.r-project.org/bin/windows/)
- **Linux**: Use your distribution's package manager or download from [CRAN](https://cran.r-project.org/bin/linux/)

After installation, verify R is working by opening a terminal and running:

```bash
R --version
```

### 2. Install Required R Packages

Open R (either from terminal with `R` command or using R.app/RStudio) and install the following packages:

```r
# R Language Server (for syntax highlighting and code completion)
install.packages("languageserver")

# Better plot outputs for Cursor/VSCode
install.packages("httpgd")
```

## Cursor Setup

### 3. Install R Extension

1. Open Cursor
2. Go to Extensions (click the Extensions icon in the left sidebar, or press `Cmd+Shift+X` on macOS / `Ctrl+Shift+X` on Windows/Linux)
3. Search for "R Extension for Visual Studio Code" (by REditorSupport)
4. Click "Install"

After installation, you should see an R icon appear in the left sidebar.

### 4. Configure R Path

The R path might be detected automatically. If not, you need to configure it:

#### Option A: Using Settings UI

1. Click the Extensions icon in the left sidebar
2. Find and select "R Extension for Visual Studio Code"
3. Click the gear icon next to it
4. Select "Extension Settings"
5. Configure the following:
   - **R > Rpath**: Set the path to your R executable
     - macOS/Linux: Usually `/usr/local/bin/R` or `/opt/homebrew/bin/R`
     - Windows: Usually `C:/Program Files/R/R-4.x.x/bin/R.exe`
   - **R > RTerm**: Set the path to your R terminal executable (same as Rpath)
   - **R > Lib Paths**: Set the path to your R library directory
     - macOS/Linux: Usually `~/Library/R/x86_64/4.x/library` or `/usr/local/lib/R/library`
     - Windows: Usually `C:/Program Files/R/R-4.x.x/library`

#### Option B: Using Settings JSON

1. Press `Cmd+,` (macOS) or `Ctrl+,` (Windows/Linux) to open Settings
2. Click the "Open Settings (JSON)" icon in the top right
3. Add the following (adjust paths for your system):

```json
{
  "r.rpath.mac": "/usr/local/bin/R",
  "r.rterm.mac": "/usr/local/bin/R",
  "r.rpath.linux": "/usr/bin/R",
  "r.rterm.linux": "/usr/bin/R",
  "r.rpath.windows": "C:/Program Files/R/R-4.3.0/bin/R.exe",
  "r.rterm.windows": "C:/Program Files/R/R-4.3.0/bin/R.exe"
}
```

### 5. Attach R Terminal

1. After configuring the R path, restart Cursor
2. Look at the bottom status bar for "Not Attached" or an R version indicator
3. Click on it to attach the R terminal
4. You should now see your R version displayed in the status bar

## Recommended Settings

You can modify R extension settings to improve usability. Open Settings (JSON) and add:

```json
{
  // Show detailed variable information in Environment viewer
  "r.session.levelOfObjectDetail": "Detailed",

  // Limit data view to 1000 rows (improves performance)
  "r.session.data.rowLimit": 1000,

  // Enable httpgd for plot output
  "r.plot.useHttpgd": true
}
```

## Keyboard Shortcuts (Optional)

You can customize keyboard shortcuts for R. Go to:

- **macOS**: `Code > Settings > Keyboard Shortcuts`
- **Windows/Linux**: `File > Preferences > Keyboard Shortcuts`

Click the "Open Keyboard Shortcuts (JSON)" icon and add:

````json
[
  {
    "key": "ctrl+shift+m",
    "command": "editor.action.insertSnippet",
    "when": "editorTextFocus",
    "args": {
      "snippet": " |> "
    }
  },
  {
    "key": "ctrl+shift+m",
    "command": "workbench.action.terminal.sendSequence",
    "when": "terminalFocus",
    "args": {
      "text": " |> "
    }
  },
  {
    "key": "ctrl+shift+r",
    "command": "editor.action.insertSnippet",
    "when": "editorTextFocus",
    "args": {
      "snippet": "# section -----------------------------------------------------------\n"
    }
  },
  {
    "key": "ctrl+alt+i",
    "command": "editor.action.insertSnippet",
    "when": "editorTextFocus",
    "args": {
      "snippet": "```{r}\n$0\n```"
    }
  }
]
````

## Code Styling

The R Extension uses `lintr` for code styling (following the [tidyverse style guide](https://style.tidyverse.org/index.html)).

You can customize lintr settings by creating a `.lintr` file in your project directory:

```r
linters: linters_with_defaults(
    line_length_linter(80),
    commented_code_linter = NULL,
    trailing_whitespace_linter = NULL,
    trailing_blank_lines_linter = NULL
  )
```

## Using R in Cursor

### Running R Code

1. **Create an R file**: Create a new file with `.R` extension
2. **Run code**:
   - Select code and press `Cmd+Enter` (macOS) or `Ctrl+Enter` (Windows/Linux)
   - Or use the "Run" button that appears above code blocks
3. **View output**: Results appear in the R terminal at the bottom

### Viewing Plots

- Plots will appear in a dedicated plot viewer panel (usually on the right side)
- You can zoom in/out using the zoom icons
- To split the plot viewer: Right-click on the plot viewer tab > "Split Down"

### Environment Viewer

- Click the R icon in the left sidebar
- View "Global Environment" to see all variables, including data frames
- With `levelOfObjectDetail` set to "Detailed", you'll see variable names and types

### Useful Cursor Shortcuts

- `F1` - Open command palette (search for any command)
- `Cmd+Shift+C` (macOS) / `Ctrl+Shift+C` (Windows/Linux) - Open terminal
- `Cmd+F` (macOS) / `Ctrl+F` (Windows/Linux) - Find in file
- `Cmd+S` (macOS) / `Ctrl+S` (Windows/Linux) - Save file
- `F11` - Toggle full screen
- `Cmd+K T` (macOS) / `Ctrl+K T` (Windows/Linux) - Select color theme

## Quarto Support (Optional)

If you want to use Quarto for creating documents:

1. **Install Quarto**: Download from [quarto.org](https://quarto.org/docs/get-started/)
2. **Install Quarto Extension**: Search for "Quarto" in Extensions and install it
3. **Create `.qmd` files**: You'll get a "Render" button to preview documents
4. **Configure output**: Add `embed-resources: true` in the YAML header to get a single HTML file

## Troubleshooting

### R Not Detected

- Verify R is installed: Run `R --version` in terminal
- Check the R path in settings matches your actual R installation
- Restart Cursor after changing R path settings

### Packages Not Loading

- Make sure you've installed `languageserver` and `httpgd` packages
- Check that your R library path is correctly configured

### Plots Not Showing

- Ensure `httpgd` package is installed
- Check that `r.plot.useHttpgd` is set to `true` in settings

## Reference

This guide is adapted from: [Using R + VSCode](https://rolkra.github.io/R-VSCode/)
