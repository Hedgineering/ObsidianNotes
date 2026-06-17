
### General Academic Topic Notes Draft Prompt
```
Create notes in a single plain text code block so I can copy and paste directly into Obsidian.

Formatting requirements:

- Use Obsidian-compatible Markdown.
- Use block LaTeX with double dollar signs:

$$
...
$$

- Use inline LaTeX with single dollar signs, like $X$.
- Do not put Markdown headers directly before LaTeX blocks in a way that makes the math render as a heading.
- Never write things like:

# $$
...
$$

- Display equations clearly with explicit equals signs where appropriate.
- Prefer multi-line equations in clean block LaTeX.
- Escape special LaTeX characters when necessary.
- Use Obsidian-style internal links where helpful, such as [[Law of Total Probability]] or [[Law of the Unconscious Statistician (LOTUS)]].
- Keep notes structured with headings, short explanations, formulas, intuition, examples, worked solutions, and key takeaways when appropriate.
- At the top of larger concept notes, include a quick reference table summarizing major ideas and formulas.
- For math/statistics topics, include intuition first, then formal definitions, formulas, examples, and common mistakes.
- When examples are requested, include fully worked solutions with clear step-by-step calculations.
- Avoid unnecessary prose outside the code block.
- Keep everything copy-paste friendly for Obsidian.
- Include python code snippets where relevant if talking about machine learning models or common stats implementations used for numerical solutions pertaining to a given notes topic
  
for these rows, where you use absolute value or pipe symbol in latex, use latex alternatives like \vert or something because the pipe breaks the table rendering on obsidian 

| Ridge (L2) | $$\lambda \sum_j \beta_j^2$$ | Shrinks coefficients toward zero | Multicollinearity, many correlated features | 
| Lasso (L1) | $$\lambda \sum_j \|\beta_j\|$$ | Shrinks coefficients, can set some exactly to zero | Feature selection |

```

### Prompt 2