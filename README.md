# LaTeX Project Template

## Table of Contents
- [Dependencies](#dependencies)
  - [1. A LaTeX Distribution](#1-a-latex-distribution)
  - [2. Perl](#2-perl)
  - [3. Python + Pygments](#3-python--pygments)
  - [4. SumatraPDF (optional)](#4-sumatrapdff-optional)
  - [5. Neovim + vimtex (optional)](#5-neovim--vimtex-optional)
- [LaTeX Packages](#latex-packages)
- [Running the Template](#running-the-template)
- [Customizing .latexmkrc](#customizing-latexmkrc)

---

## Dependencies

### 1. A LaTeX Distribution

This provides the compilers (`xelatex`, `pdflatex`), the package manager, and `latexmk`.

#### Windows — MiKTeX (recommended)
MiKTeX is the most convenient option on Windows because it installs missing LaTeX packages automatically on first compile.

**Via winget:**
```
winget install MiKTeX.MiKTeX
```
**Via Chocolatey:**
```
choco install miktex
```
**Or download the installer** from [miktex.org](https://miktex.org/download)

> After installing, open the **MiKTeX Console** and run **Check for updates** once to make sure everything is current.

#### Linux — TeX Live
```
sudo apt install texlive-full        # Debian / Ubuntu
sudo pacman -S texlive-most          # Arch
sudo dnf install texlive-scheme-full # Fedora
```

#### macOS — MacTeX
```
brew install --cask mactex
```
Or download from [tug.org/mactex](https://tug.org/mactex/)

---

### 2. Perl

`latexmk` is a Perl script, so Perl must be installed and available in your PATH.

#### Windows
MiKTeX bundles its own Perl internally, so `latexmk` should work out of the box. If it doesn't:

**Via winget:**
```
winget install StrawberryPerl.StrawberryPerl
```
**Via Chocolatey:**
```
choco install strawberryperl
```
**Or download** from [strawberryperl.com](https://strawberryperl.com)

#### Linux / macOS
Perl is almost certainly already installed. Verify with:
```
perl --version
```

---

### 3. Python + Pygments

Required by the `minted` package, which handles syntax-highlighted code blocks. `minted` calls Python's Pygments library externally during compilation.

#### Windows
**Via winget:**
```
winget install Python.Python.3
```
**Via Chocolatey:**
```
choco install python
```
Then install Pygments via pip:
```
pip install Pygments
```

#### Linux
```
sudo apt install python3 python3-pip   # Debian / Ubuntu
pip3 install Pygments
```

#### macOS
```
brew install python
pip3 install Pygments
```

> If you never use `minted` for code blocks in your documents, you can remove `\usepackage{minted}` from `packages.sty` and skip this dependency entirely.

---

### 4. SumatraPDF — optional, for SyncTeX on Windows

SumatraPDF is a lightweight PDF viewer that supports SyncTeX, which lets you jump between a line in the PDF and the corresponding source line in your editor (and vice versa).

**Via winget:**
```
winget install SumatraPDF.SumatraPDF
```
**Via Chocolatey:**
```
choco install sumatrapdf
```
**Or download** from [sumatrapdfreader.org](https://www.sumatrapdfreader.org)

> On Linux, use **Evince** (`sudo apt install evince`) or **Okular** (`sudo apt install okular`) instead.

---

### 5. Neovim + vimtex — optional, for editor integration

If you use Neovim, the **vimtex** plugin provides SyncTeX support, compilation management, and LaTeX-aware editing features. Install it with your plugin manager (example using lazy.nvim):

```lua
{
    'lervag/vimtex',
    config = function()
        vim.g.vimtex_view_method = 'general'      -- SumatraPDF on Windows
        vim.g.vimtex_compiler_method = 'latexmk'  -- reads your .latexmkrc
    end
}
```

After installing vimtex, configure SumatraPDF for reverse sync (PDF → editor) by going to **Settings → Options → Set inverse search command** and entering:
```
nvim --headless -c "VimtexInverseSearch %l '%f'"
```
Or if VSCode is your prefered editor use:
```
"C:\Users\User\AppData\Local\Programs\Microsoft VS Code\Code.exe" --goto %f:%l
```

Forward sync (editor → PDF) is then triggered in Neovim with `\lv`.

---

## LaTeX Packages

All LaTeX packages used by this template are listed in `packages.sty`. They are installed automatically by MiKTeX on first compile. On TeX Live (Linux/macOS) with a full installation (`texlive-full`) they are all included by default.

For reference, the packages used are:

| Package | Purpose |
|---|---|
| `biblatex` + `biber` | Bibliography and references |
| `appendix` | Appendix formatting |
| `caption`, `subcaption` | Figure and sub-figure captions |
| `tocbibind` | Add bibliography to table of contents |
| `minted` | Syntax-highlighted code blocks |
| `xcolor` | Colors |
| `tikz` | Diagrams and flowcharts |
| `import` | Import sections from separate `.tex` files |
| `csquotes` | Internationalised quotation marks |
| `hyperref` | Clickable links and PDF metadata |
| `comment` | Block comments |
| `pdfpages` | Insert external PDF pages |
| `graphicx`, `wrapfig`, `float` | Images and figure placement |
| `siunitx` | SI units |
| `babel` (catalan) | Language and hyphenation rules |
| `amsmath`, `amssymb`, `amsthm`, `bm` | Mathematics |
| `gensymb`, `textcomp` | Extra symbols |
| `booktabs` | Professional tables |
| `geometry` | Page margins and layout |
| `fancyhdr` | Custom headers and footers |
| `multirow`, `multicol` | Advanced table and column layouts |

---

## Running the Template

### Project structure
```
your-project/
├── .latexmkrc          ← compiler configuration
├── main.tex            ← entry point
├── packages.sty        ← all package imports
├── portada.tex         ← title page
├── referencies.bib     ← this file lives inside Text_files/
└── Text_files/
    ├── referencies.bib
    ├── INTRODUCCIÓ.tex
    ├── OBJECTIUS.tex
    ├── RESULTATS I DISCUSSIÓ.tex
    ├── CONCLUSIONS.tex
    ├── ANNEX.tex
    └── BIBLIOGRAFIA.tex
```

### Compile once
Produces `build/main.pdf` and exits:
```
latexmk main.tex
```

### Compile and watch for changes
Recompiles automatically every time you save a `.tex` file, and opens the PDF in your viewer:
```
latexmk -pvc main.tex
```

### Clean auxiliary files (keep PDF)
```
latexmk -c
```

### Clean everything including the PDF
```
latexmk -C
```

> All output files go into the `build/` directory as configured in `.latexmkrc`. Your PDF will be at `build/main.pdf`.

---

## Customizing `.latexmkrc`

The `.latexmkrc` file controls how latexmk compiles your project. Place it in the project root next to `main.tex`.

### Switching compiler

Change `$pdf_mode` and add the corresponding command variable:

```perl
# pdflatex
$pdf_mode = 1;
$pdflatex = 'pdflatex -interaction=nonstopmode -synctex=1 -shell-escape %O %S';

# xelatex (default in this template — best for UTF-8 and Catalan)
$pdf_mode = 5;
$xelatex = 'xelatex -interaction=nonstopmode -synctex=1 -shell-escape %O %S';

# lualatex
$pdf_mode = 4;
$lualatex = 'lualatex -interaction=nonstopmode -synctex=1 -shell-escape %O %S';
```

### Changing the PDF viewer

```perl
$pdf_previewer = 'start';    # Windows — opens with default viewer
$pdf_previewer = 'evince';   # Linux
$pdf_previewer = 'open';     # macOS
$pdf_previewer = 'zathura';  # Linux alternative
```

### Disabling SyncTeX

If you don't use editor/viewer sync, remove `-synctex=1` from the compiler command and remove `synctex.gz` from `@generated_exts`.

### Disabling shell-escape

If you remove `minted` from `packages.sty` and don't need external tool calls during compilation, remove `-shell-escape` from the compiler command. Leaving it in when not needed is harmless but slightly less secure.

### Changing the output directory

```perl
$out_dir = 'build';   # default in this template
$out_dir = 'out';     # any name works
$out_dir = '';        # empty string = project root (no subdirectory)
```

### Removing the output directory

If you prefer all files in the project root, delete or comment out the `$out_dir` line:
```perl
# $out_dir = 'build';
```
