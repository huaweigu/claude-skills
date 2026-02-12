---
name: publish-report
description: Generate a styled HTML report from a Jupyter notebook or markdown + PNGs and publish it to GitHub Pages
---

# Publish Report

Takes a Jupyter notebook, markdown file, or HTML file with associated PNG charts and generates a self-contained styled HTML report with embedded images, then deploys it to GitHub Pages as a public URL.

## Usage

| Command | Description |
|---------|-------------|
| `/publish-report` | Auto-detect source files in current directory and generate + publish report |
| `/publish-report html` | Generate self-contained HTML report only (no deploy) |
| `/publish-report deploy` | Deploy an existing HTML file to GitHub Pages |
| `/publish-report deploy <repo-name>` | Deploy with a custom GitHub repo name |

## Prerequisites

- `gh` CLI authenticated (`gh auth status`)
- Python 3 available
- For notebook sources: `jupyter` installed in a venv

## Execution Steps

### Command: `/publish-report` (default — full pipeline)

1. **Detect source files** in the current working directory:
   - Look for `.ipynb` files (Jupyter notebooks)
   - Look for `.md` files (markdown reports)
   - Look for `.html` files (pre-built reports)
   - Look for `.png` files (charts/images to embed)
   - If multiple source types found, prefer: `.ipynb` > `.md` > `.html`
   - If no source files found, ask the user what to convert

2. **If source is a Jupyter notebook (`.ipynb`)**:
   - Check if notebook has been executed (has outputs). If not, execute it:
     ```bash
     jupyter nbconvert --to notebook --execute --output <same_file> <notebook>
     ```
   - Extract all PNG outputs saved by the notebook
   - Parse notebook cells to extract markdown text, tables, and data
   - Use the notebook content to build the HTML report

3. **If source is markdown (`.md`)**:
   - Parse the markdown content for headings, tables, text, callouts
   - Find associated PNG files in the same directory

4. **Generate self-contained HTML report**:
   - Read all PNG files and base64-encode them
   - Embed images inline as `data:image/png;base64,...` in `<img>` src attributes
   - Apply styled HTML template with:
     - Clean Arial font, max-width 900px centered layout
     - Styled tables: dark header row (`#2C3E50`), alternating row shading, hover effects
     - Color coding: green (`.positive`) for favorable values, red (`.negative`) for adverse
     - Callout boxes: blue border for info, orange border for warnings
     - Metric summary boxes in a grid layout for key numbers
   - The HTML must be fully self-contained (no external CSS, JS, or image references)
   - Save as `<report-name>.html`

5. **Deploy to GitHub Pages**:
   - Verify `gh auth status` works
   - Create a temp directory, copy HTML as `index.html`
   - Initialize git repo, commit (use `--no-gpg-sign` to avoid signing issues)
   - Use `gh repo create <repo-name> --public --source=. --push` to create and push
   - Enable GitHub Pages via API:
     ```bash
     gh api repos/<owner>/<repo>/pages -X POST \
       -f "build_type=legacy" \
       -f "source[branch]=master" \
       -f "source[path]=/"
     ```
   - Wait a few seconds, then verify the page returns HTTP 200
   - Return the public URL: `https://<owner>.github.io/<repo-name>/`

6. **Output the public URL** to the user

### Command: `/publish-report html`

Run steps 1-4 only (generate HTML, skip deploy). Output the file path.

### Command: `/publish-report deploy`

Skip generation steps. Look for an existing HTML file in the current directory:
- If exactly one `.html` file, use it
- If multiple, ask the user which one
- Run step 5 to deploy it

### Command: `/publish-report deploy <repo-name>`

Same as `/publish-report deploy` but use the provided repo name instead of auto-generating one.

## HTML Template Style Reference

Use this CSS for the generated HTML reports:

```css
body {
  font-family: Arial, sans-serif;
  max-width: 900px;
  margin: 40px auto;
  padding: 0 20px;
  color: #333;
  line-height: 1.5;
}
h1 { color: #1a1a1a; border-bottom: 2px solid #333; padding-bottom: 8px; }
h2 { color: #2c3e50; margin-top: 36px; border-bottom: 1px solid #ccc; padding-bottom: 4px; }
table { border-collapse: collapse; width: 100%; margin: 16px 0; font-size: 13px; }
th { background: #2c3e50; color: white; padding: 8px 12px; text-align: left; }
td { padding: 6px 12px; border-bottom: 1px solid #ddd; }
tr:nth-child(even) { background: #f8f9fa; }
tr:hover { background: #e8f4f8; }
.positive { color: #27ae60; font-weight: bold; }
.negative { color: #e74c3c; font-weight: bold; }
.highlight-row { background: #e8f6e8 !important; }
img { max-width: 100%; margin: 16px 0; border: 1px solid #ddd; border-radius: 4px; }
.callout { background: #f0f7ff; border-left: 4px solid #3498db; padding: 12px 16px; margin: 16px 0; }
.callout-warn { background: #fff8f0; border-left: 4px solid #e67e22; padding: 12px 16px; margin: 16px 0; }
.metric-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 12px; margin: 16px 0; }
.metric-box { background: #f8f9fa; border: 1px solid #ddd; border-radius: 8px; padding: 16px; text-align: center; }
.metric-box .value { font-size: 28px; font-weight: bold; color: #2c3e50; }
.metric-box .label { font-size: 12px; color: #666; margin-top: 4px; }
```

## Image Embedding

All images must be embedded as base64 to make the HTML self-contained:

```python
import base64

def img_to_base64(path):
    with open(path, 'rb') as f:
        return base64.b64encode(f.read()).decode()

# Use in img tag:
# <img src="data:image/png;base64,{base64_string}" alt="Chart description">
```

## Repo Naming

When auto-generating a repo name:
- Derive from the source file name or directory name
- Convert to kebab-case
- Append `-report` if not already present
- Example: `usdc_usdt_analysis.ipynb` → `usdc-usdt-analysis-report`

## Output Format

### Full Pipeline Success
```
Source detected: analysis.ipynb (+ 5 PNG charts)

Generating HTML report...
  - Embedded 5 charts as base64
  - Report saved: Analysis_Report.html (850 KB)

Deploying to GitHub Pages...
  - Created repo: username/analysis-report
  - GitHub Pages enabled

Report published: https://username.github.io/analysis-report/
```

### HTML Only
```
Source detected: analysis.ipynb (+ 5 PNG charts)

Generating HTML report...
  - Embedded 5 charts as base64
  - Report saved: Analysis_Report.html (850 KB)
```

### Deploy Only
```
Deploying Analysis_Report.html to GitHub Pages...
  - Created repo: username/analysis-report
  - GitHub Pages enabled

Report published: https://username.github.io/analysis-report/
```

## Notes

1. **Self-contained HTML** — All images are base64-embedded. No external file dependencies.
2. **Public repos** — GitHub Pages requires public repos on free accounts. The deployed report will be publicly accessible.
3. **GPG signing** — Use `--no-gpg-sign` for commits to avoid signing issues in non-interactive environments.
4. **Existing repos** — If a repo with the same name already exists, ask the user whether to overwrite or choose a different name.
5. **Large reports** — Base64 encoding increases file size ~33%. Reports with many high-resolution charts may be several MB.
6. **Propagation delay** — GitHub Pages may take 1-2 minutes to go live after deployment. Verify with a curl check before returning the URL.
