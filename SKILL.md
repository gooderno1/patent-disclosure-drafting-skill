---
name: patent-disclosure-drafting
description: Use when drafting, restructuring, reviewing, illustrating, searching, or exporting Chinese patent disclosure documents (`技术交底书`, `专利交底书`). Covers splitting invention points, building the markdown disclosure structure, writing method steps with embedded mathematical expressions, keeping all variables/functions/thresholds/version items in one definition table, generating figures with `gpt-image-2`, performing strict prior-art and patentability review with citations, and syncing a visually verified `.docx`.
---

# Patent Disclosure Drafting

## Use This Skill When

- The user asks to write, rewrite, expand, or review a Chinese patent disclosure document.
- The user has raw technical ideas but not yet a stable disclosure structure.
- The user asks for `查重`, `检索`, `授权可能性`, `现有技术对比`, or final `docx` export with formulas.

## Core Rules

1. Treat `md` as the source of truth and edit it directly.
2. Do not regenerate disclosure正文 with repo-local Python pipelines. If helper tooling is unavoidable for export or QA, keep it outside the patent workspace or use an existing external skill.
3. Generate figures with `gpt-image-2` directly. Do not use `draw.io`, Mermaid, or Python chart pipelines for the patent package.
4. Mathematics exists to explain the method. Do not separate a large formula block from the actual procedural steps.
5. Define every scalar, vector, matrix, set, function, threshold, binding item, and version item exactly once in `2.3.1`.
6. Keep titles, terminology, symbols, captions, file names, and examples consistent across `md`, images, search reports, and `docx`.

## Workflow

### 1. Intake and Split Inventions

- Distill each invention into four parts: object, core mechanism, target effect, and novelty anchor.
- If multiple inventive kernels exist, split them into separate disclosures before drafting.
- Decide a provisional title and filing priority for each disclosure.

Read [references/workflow.md](references/workflow.md) for the full intake and decomposition method.

### 2. Establish Workspace Conventions

- If the repo lacks local rules, create or update `agent.md` from [assets/agent-template.md](assets/agent-template.md).
- Create one disclosure `md` per invention plus a `配图/` folder.
- Keep an independent prompt file for image generation; do not keep prompt prose inside the disclosure正文.

### 3. Draft the Markdown Disclosure

- Start from [assets/disclosure-template.md](assets/disclosure-template.md).
- Keep the standard structure:
  - `发明名称`
  - `一、简要概括希望保护的内容`
  - `二、技术方案`
  - `三、技术效果`
  - `四、技术背景或研发背景`
  - `是否有替代方案`
  - `附图说明`
- In `2.3.1`, merge variables, functions, thresholds, binding items, and version items into the same definition table.
- In `2.5` and `2.6`, embed equations inside each method step. Each equation must directly explain a decision, score, state transition, or output in that step.
- In `2.10`, provide a concrete example that can be followed from input data to output conclusion.
- Expand non-technical sections so they are not thin; these sections must support later claim drafting and attorney handoff.

### 4. Produce Figures

- Usually prepare three figures per disclosure:
  - method flow
  - system composition
  - key relation, conflict, or version-update logic
- Generate the final bitmap figures with `gpt-image-2`.
- Insert the images into the markdown and keep the prompt text only in the independent prompt file.

### 5. Review and Final Polish

- Use [references/review-checklist.md](references/review-checklist.md).
- Resolve contradictions, undefined symbols, missing decision branches, placeholder text, and inconsistent numbering.
- Prefer `step -> equation -> result` phrasing over detached mathematical exposition.

### 6. Prior-Art Search and Patentability Assessment

- When the user asks for `查重`, `检索`, `授权可能性`, or close prior art, browse the web and cite sources.
- Follow [references/patentability-search.md](references/patentability-search.md).
- Produce per-disclosure findings:
  - closest prior art
  - novelty risk
  - inventive-step risk
  - likely examiner combination attack
  - key features to defend
  - filing priority recommendation

### 7. Export and Verify DOCX

- Use the `documents` skill for `.docx` work and follow [references/docx-export.md](references/docx-export.md).
- After every markdown structural change that affects formulas or tables, re-sync the `docx`.
- Shipping gate: render and inspect pages that contain formulas, large tables, figures, and section transitions.

## Output Expectations

- Deliverables normally include:
  - one finalized disclosure `md` per invention
  - generated figures placed in `配图/`
  - one independent image prompt file if figures were generated
  - one patentability/search report if search was requested
  - one verified `docx` per disclosure if export was requested
- If the user asks for only one stage, execute only the relevant slice of the workflow and avoid unnecessary artifacts.

## Reference Files

- [references/workflow.md](references/workflow.md): full end-to-end process distilled from this project
- [references/review-checklist.md](references/review-checklist.md): final audit checklist
- [references/patentability-search.md](references/patentability-search.md): strict search and patentability workflow
- [references/docx-export.md](references/docx-export.md): markdown-to-docx sync and formula QA
