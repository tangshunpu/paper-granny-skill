# Paper Granny Skill

Codex-native arXiv paper interpretation workflow for generating accessible Chinese deep-reading reports directly from arXiv source files.

This skill reads arXiv LaTeX source, builds an evidence map, writes a structured Chinese report, and compiles it with XeLaTeX. It is designed to run inside Codex without invoking the original PaperGranny Python CLI, web server, LangGraph agent, or remote workflow.

## What It Does

- Normalizes arXiv URLs and IDs.
- Downloads and inspects arXiv TeX sources.
- Reads the main paper and important included files before drafting.
- Produces a Chinese report with background, method walkthrough, formula explanations, experiments, limitations, and references.
- Uses the bundled `ModernColorful` LaTeX template when available.
- Compiles the report locally with XeLaTeX.

## Install

Clone this repository into your Codex skills directory:

```bash
mkdir -p "$CODEX_HOME/skills"
git clone https://github.com/tangshunpu/paper-granny-skill.git "$CODEX_HOME/skills/paper-granny"
```

If `CODEX_HOME` is not set, Codex commonly uses `~/.codex`:

```bash
mkdir -p ~/.codex/skills
git clone https://github.com/tangshunpu/paper-granny-skill.git ~/.codex/skills/paper-granny
```

Restart Codex after installation if the skill is not discovered immediately.

## Usage

Ask Codex to use Paper Granny on an arXiv paper:

```text
Use $paper-granny to read https://arxiv.org/pdf/2508.17778 and generate a Chinese interpretation report.
```

The skill writes outputs under:

```text
papers/<arxiv_id>/
```

Typical generated files include:

- `source.tar.gz`
- `source/`
- `report.tex`
- `report.pdf`

## Requirements

- Codex with skill support.
- Network access for downloading arXiv source files.
- Shell tools: `curl`, `tar`, `gunzip`, `find`, `rg`, `sed`, `cp`, `mkdir`.
- XeLaTeX for PDF compilation.

On macOS with MacTeX installed, `xelatex` is usually available at `/Library/TeX/texbin/xelatex`.

## Repository Layout

```text
.
├── SKILL.md
├── agents/
│   └── openai.yaml
├── assets/
│   ├── ModernColorful.cls
│   └── logo.png
└── references/
    └── quality-guide.md
```

## Notes

This skill intentionally tells Codex to do the paper reading and report writing itself. It uses shell tools only for deterministic file operations and XeLaTeX compilation.

## License

MIT
