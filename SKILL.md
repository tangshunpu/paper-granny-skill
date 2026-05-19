---
name: paper-granny
description: Codex-native arXiv paper interpretation workflow that produces Paper Granny style Chinese reading reports without running the PaperGranny Python agent, web server, or remote workflow. Use when the user asks for Paper Granny, 论文奶奶, Scholar Granny, arXiv 论文解读, paper reading reports, Chinese paper explainers, or converting an arXiv paper/source into an accessible report generated directly by Codex.
---

# Paper Granny

## Core Rule

Act as Paper Granny directly in Codex. Do not run the PaperGranny Python CLI, LangGraph agent, FastAPI web server, or wrapper scripts. Do not call `python` for this workflow.

Use ordinary shell tools only when needed for deterministic file work:

- `curl` to fetch arXiv source if the paper source is not already local
- `tar`, `gunzip`, `find`, `rg`, `sed`, `cp`, `mkdir` for source inspection and file handling
- `xelatex` for compiling the final LaTeX report

The reasoning, paper reading, explanation, and report writing must be done by Codex.

## Workflow

1. Normalize the input arXiv URL or ID.

Supported forms:

- `https://arxiv.org/abs/2603.03251`
- `https://arxiv.org/pdf/2603.03251`
- `2603.03251`
- `arXiv:2603.03251`

2. Create a local work directory:

```bash
mkdir -p papers/<arxiv_id>/source
```

3. Fetch source only if needed:

```bash
curl -L "https://arxiv.org/e-print/<arxiv_id>" -o "papers/<arxiv_id>/source.tar.gz"
tar -xzf "papers/<arxiv_id>/source.tar.gz" -C "papers/<arxiv_id>/source"
```

If `tar` fails because arXiv returned a single gzip file:

```bash
gunzip -c "papers/<arxiv_id>/source.tar.gz" > "papers/<arxiv_id>/source/main.tex"
```

If the user already provided local source files, skip download.

4. Inspect the source with Codex:

```bash
find "papers/<arxiv_id>/source" -maxdepth 3 -type f
rg -n "\\\\documentclass|\\\\title|\\\\author|\\\\begin\\{abstract\\}|\\\\input|\\\\include|\\\\includegraphics|\\\\begin\\{theorem\\}|\\\\begin\\{proposition\\}|\\\\begin\\{lemma\\}|\\\\begin\\{equation" "papers/<arxiv_id>/source"
```

Read the main `.tex` file and every important `\input{}` / `\include{}` file. Do not write the report from the abstract alone.

5. Build an evidence map before drafting:

- title, authors, arXiv ID
- research problem and motivation
- claimed contributions, tied to specific sections, figures, equations, or tables
- method pipeline, including inputs, outputs, assumptions, and training/inference steps
- key equations with symbol meanings
- theorems/propositions/lemmas with premise, conclusion, and role
- experiments: datasets, baselines, metrics, results, ablations, failure cases
- important figures/tables and their file paths

6. Write the report directly as `papers/<arxiv_id>/report.tex`.

Use the bundled template from `assets/ModernColorful.cls` when available. Resolve `assets/` relative to this skill directory, then copy the class and logo into the report directory:

```bash
cp "<skill_dir>/assets/ModernColorful.cls" "papers/<arxiv_id>/"
cp "<skill_dir>/assets/logo.png" "papers/<arxiv_id>/" 2>/dev/null || true
```

Report structure:

```latex
\section{背景知识}
\section{核心问题与创新思路}
\section{方法详解}
\section{公式与定理拆解}
\section{实验结果}
\section{优势、局限与适用场景}
\section{总结}
\section{参考资料}
```

7. Compile with XeLaTeX from the report directory:

```bash
cd "papers/<arxiv_id>" && xelatex -interaction=nonstopmode -halt-on-error -file-line-error report.tex
cd "papers/<arxiv_id>" && xelatex -interaction=nonstopmode -halt-on-error -file-line-error report.tex
```

Fix LaTeX errors locally by editing `report.tex`, then recompile. Do not switch to the Python compiler.

8. Final response:

- paper title
- generated PDF path
- any source/compile limitations

## Explanation Standard

The output is not a summary or translation. It must be an accessible deep reading:

- Explain jargon in plain Chinese.
- Add background when a weak reader would be lost.
- Preserve key equations and explain every non-obvious symbol.
- Explain what each important figure/table is trying to prove.
- Interpret experiments as question, setup, metric, observation, and conclusion.
- Preserve technical claims faithfully and mark uncertainty when the paper is ambiguous.

Read `references/quality-guide.md` for the full quality checklist when writing or improving a report.

## LaTeX Rules

- Use `\documentclass{ModernColorful}` if the template is available.
- Use only template-defined boxes and commands unless you have read the class file.
- Escape text special characters: `&`, `%`, `#`, `_`.
- Avoid raw Unicode arrows/math symbols; prefer `$\rightarrow$`, `$\leftarrow$`, `$\Rightarrow$`.
- Copy referenced figures into the report directory or use correct relative paths.
- Keep figure widths conservative: `0.7\textwidth`, `0.85\textwidth`, or `\textwidth` depending on shape.
