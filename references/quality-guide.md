# Paper Granny Quality Guide

Use this guide when improving a Paper Granny report or editing this skill's local guidance files.

## Reading Contract

Before writing, extract a compact evidence map:

- Title, authors, venue/date if present, arXiv ID
- Problem statement in one sentence
- Why existing approaches are insufficient
- Main contribution claims, each tied to paper sections, figures, equations, or tables
- Method pipeline, with inputs, outputs, training/inference steps, and assumptions
- Key equations with symbol meanings
- Theorems/propositions/lemmas with premise, conclusion, and role in the method
- Experiments: datasets, baselines, metrics, ablations, and failure cases
- Important figures/tables and their paths

Do not write the report from the abstract alone.

## Explanation Standard

For each difficult concept:

1. State the original technical term.
2. Give a plain-language translation.
3. Explain why the paper needs it.
4. Connect it to a concrete equation, algorithm step, figure, or experiment.

For each key equation:

1. Preserve the original LaTeX form.
2. Explain what quantity it computes.
3. Define every non-obvious symbol.
4. Explain the intuition in one or two paragraphs.
5. State what would change if the term were removed or modified.

For each experiment:

1. State the question being tested.
2. Describe the setup and metric.
3. Explain the main observation.
4. Tie the observation back to a claim in the introduction or method.

## Report Shape

Prefer this structure unless the paper demands a different one:

1. Background knowledge
2. The problem and why it matters
3. Core idea in one paragraph
4. Method walkthrough
5. Formula and theorem explanation
6. Experiment interpretation
7. Strengths, limitations, and likely failure modes
8. Final takeaway

Keep the tone patient and concrete. The reader should feel guided through the paper, not handed a compressed abstract.

## Anti-Hallucination Rules

- Do not invent datasets, baselines, numbers, theorem names, or claims.
- Mark uncertainty when source text is ambiguous.
- Prefer "the paper claims" over presenting unverified claims as fact.
- If source files are incomplete or arXiv has no LaTeX source, say so and stop or ask for a PDF workflow.

## LaTeX Safety

- Use only commands/environments exposed by the local `ModernColorful` template unless you have read the class file.
- Avoid raw Unicode arrows and math symbols in body text; use LaTeX commands like `$\rightarrow$`.
- Escape `&`, `%`, `#`, `_` in text.
- Copy figures into the report directory before referencing them.
- Use `get_image_info` before setting figure widths.
