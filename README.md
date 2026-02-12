# Claude Code Skills

A collection of custom skills for [Claude Code](https://claude.com/claude-code).

## Skills

| Skill | Description |
|-------|-------------|
| [publish-report](./publish-report/) | Generate a styled HTML report from a Jupyter notebook or markdown + PNGs and publish it to GitHub Pages |
| [pdd-review](./pdd-review/) | Run a 9-agent review team on a Product Design Document (PDD) with per-agent verdicts and actionable synthesis |

## Installation

Symlink a skill into your Claude Code skills directory:

```bash
git clone https://github.com/huaweigu/claude-skills.git
ln -s /path/to/claude-skills/publish-report ~/.claude/skills/publish-report
```

## Usage

Once installed, invoke skills with slash commands in Claude Code:

```
/publish-report          # Auto-detect source, generate HTML, deploy to GitHub Pages
/publish-report html     # Generate HTML report only
/publish-report deploy   # Deploy existing HTML to GitHub Pages
/pdd-review path/to/pdd.md  # Run 9-agent review on a Product Design Document
```
