---
name: nova-skill-creator
description: "Use when the user wants to write a new skill, create a skill, build a skill for an AI agent, upgrade a skill, edit or fix a skill, or review and audit a skill for Claude, Grok or another LLM. WHENEVER the user mentions a skill, SKILL.md, an SOP for AI, a virtual employee or a skill engineer. Examples - write a skill that saves links to Notion, create a skill that sorts emails, upgrade this skill to a new version, review whether this skill meets the standard, fix the trigger of this skill, make a SKILL.md for this workflow. Do not use when the user only asks what a skill is in general, asks about AI agent concepts without designing a specific skill, or asks for other content that is not a skill file."
---

# Nova Skill Creator — Skill Engineer Protocol v2.2

> **Version 2.4.1** · Pattern: Sequential Workflow · Type: Code Scaffolding & Templates
>
> "Skill Engineer Protocol v2.2" is the name of the process this skill follows, not the release number.

## Purpose

This meta-skill turns every skill-related request (write new / upgrade / edit / review) into a standard engineering process and produces a professional skill that can be deployed immediately. Each output skill is a self-standing "virtual employee": either **1 SKILL.md file** or **1 full folder**, depending on complexity and the target environment.

When this skill activates, start the response with exactly this sentence:
**"Skill Engineer Protocol v2.2 activated. Starting the standard process..."**

After that sentence, respond in the language the user writes in.

---

## ⚠️ Preserve content when rewriting (SUPREME RULE)

"Rewrite full" means **restructure**, NOT **compress/summarize**. When this skill is applied with another LLM (especially a weaker model), the most common failure is that technical descriptions get compressed, table columns get cut, and concrete schemas/values get dropped — the output looks tidier but has lost original content.

**Mandatory distinction:**

| Restructure (CORRECT) | Compress (WRONG) |
|---|---|
| Change layout, merge duplicate sections, fix wording | Drop tables, rows, columns, examples |
| Keep every value/schema/ID/rule exactly | Paraphrase a schema into "roughly" |
| Number of output items >= number of input items | Shrink 10 gotchas down to 4 |
| Increase clarity | Reduce detail "to make it shorter" |

**3-step anti-compression process — MANDATORY before/after every rewrite or upgrade:**

1. **Inventory first (Content Inventory):** list every original element that must NOT be lost — each table (with its row/column count), each schema rule, each concrete value (ID, version, option), each gotcha, each test case. Write it down as a checklist.
2. **Constrained rewrite:** while rewriting, every element in the list must appear in full in the output. You may improve the presentation; you may NOT reduce the number of elements or the detail of any element.
3. **Check afterwards (Diff Check):** compare the output with the inventory. If any element is missing, or a table has fewer rows/columns than the original → NOT DONE; restore it before answering.

**Self-binding instruction for the executing LLM:** "Preserve verbatim every table, schema, concrete value and gotcha. Do not summarize, do not drop technical detail. If it must be shorter, SPLIT it into references/; never DELETE content."

---

## A skill is a folder — choose the structure by complexity

Claude, Claude Code and other agents support skills as a **folder** with SKILL.md + bundled resources, with **Progressive Disclosure** (resources load on demand), offline-runnable scripts, and dynamically loaded `references/`. You do not have to cram everything into 1 file.

| Complexity | Recommended structure |
|---|---|
| Simple, short skill | Just 1 `SKILL.md` file (still valid) |
| Skill with repetitive/deterministic logic | Folder + `scripts/` |
| Multi-domain skill / long documentation | Folder + `references/` split by variant |
| Skill that outputs bundled files (templates/icons/fonts) | Folder + `assets/` |

**Principle:** choose the smallest structure that is enough. Simple → 1 file; complex → split into a folder to keep SKILL.md lean. **When content is too long → SPLIT it into references/, do NOT compress it away.**

---

## Anatomy of a Skill

```
skill-name-kebab-case/
├── SKILL.md          (required)
│   ├── YAML frontmatter (5 fields for accounts/free-form LLMs · ONLY name + description for standard Agent Skills targets — see 'Variants by TARGET ENVIRONMENT')
│   └── Markdown instructions
└── Bundled resources (optional)
    ├── scripts/      - Executable code for deterministic/repetitive tasks
    ├── references/   - Documents loaded into context when needed
    └── assets/       - Files used in the output (template, icon, font)
```

### Progressive Disclosure (3 loading levels)

| Level | Content | When it enters context |
|---|---|---|
| 1 | Metadata (`name` + `description`) | Always (~100 words) |
| 2 | SKILL.md body | When the skill activates (ideally under 500 lines) |
| 3 | Bundled resources | When needed (`references/` load dynamically; `scripts/` run without loading their content into context) |

**Rule:** keep the SKILL.md body under 500 lines. When you approach the limit → **split** into `references/` and leave a clear pointer in SKILL.md saying where to read. A long reference (>300 lines) should have a table of contents. Splitting is DIFFERENT from compressing — splitting keeps the content intact in another file, compressing loses content.

---

## WRITE A NEW SKILL process (8 steps)

| # | Step | Description |
|---|---|---|
| 1 | Ask for missing information | Goal, gotchas, language, output, target environment. If the conversation already contains the workflow → extract it from the history first, ask only for what is missing |
| 2 | Choose 1 Pattern + 1 Skill Type | Exactly 1 of each (see the lists below) |
| 3 | Write the Description FIRST | The most important step — follow the Description Protocol below |
| 4 | Choose the structure + design the content | Single file or folder? What stays inline, what goes to `references/`, which logic becomes `scripts/` |
| 5 | Write SKILL.md | YAML + Workflow + Rules/Schema + Gotchas; body under 500 lines, offload long parts to references |
| 6 | Write scripts/references (if needed) | Deterministic/repetitive logic → `scripts/`; long documentation → `references/` with pointers |
| 7 | Test activation | 3 positive + 3 negative cases |
| 8 | Answer using the Output Format | See the Output Format section below |

**RELEASE ONLY when all 14/14 Checklist items pass.**

---

## UPGRADE / EDIT process (8 steps)

1. Read the whole current SKILL.md (and its bundled files).
2. **Inventory first** — list every table/schema/value/gotcha/test case that must NOT be lost (Supreme rule, step 1).
3. Record gotchas from the user and from previous runs.
4. Evaluate against the 14-item Checklist.
5. Identify the failure type: **Under-trigger** / **Over-trigger** / **Execution error** / **Content-loss (compression/truncation)**.
6. **Rewrite everything under the preservation constraint** — restructure, do NOT compress. NEVER patch piece by piece.
7. **Check afterwards** — diff the output against the inventory, restore anything missing.
8. Re-test 3 positive + 3 negative cases, bump `version`.

**Iron rule:** Rewrite full = restructure that keeps all content. No patchwork. No compressing away detail. Keep the old `name` when upgrading.

---

## YAML Frontmatter (MANDATORY)

```yaml
---
name: skill-name-kebab-case
version: x.x.x
description: >-
  [Description optimized for activation, under 1024 characters, plain ASCII with no diacritics, no angle brackets]
pattern: [1 of the 5 Patterns]
type: [1 of the 9 Skill Types]
---
```

### Variants by TARGET ENVIRONMENT (ask before writing the frontmatter)

The 5-field block above is the template for **claude.ai accounts / Grok / free-form LLMs**. Some targets accept exactly 2 fields — submitting the 5-field version there **fails validation immediately**, before review even starts. Step 1 (gathering) MUST ask for or infer the target; if unclear, default to the 2-field version, because the 2-field version works on every target while the 5-field version does not.

| Target | Frontmatter | Where Version / Pattern / Type go |
|---|---|---|
| claude.ai account, Grok, other LLMs | 5 fields as in the template above, `description` folded `>-` | in the YAML |
| **Standard Agent Skills** (Claude Code `.claude/skills`, Codex `.agents/skills`) and skill registries with strict validation | **ONLY `name` + `description`, each on ONE line** — no `>-`, no indented lines, no third field | one blockquote line right under the H1: `> **Version x.y.z** · Pattern: … · Type: …`; the real version lives in the target system's registry/manifest file (if any) |

Template for Agent Skills targets:

```yaml
---
name: skill-name-kebab-case
description: "[Description optimized for activation, ONE line, >= 40 characters, plain ASCII with no diacritics, no angle brackets]"
---
```

**Always wrap a one-line `description` in double quotes.** Claude Code and Codex read the frontmatter with a real YAML parser. With a bare value there are two kinds of breakage. (1) **Parse error** — the skill loads with empty metadata and no warning: a `: ` in the middle (for example `Vi du: …` or `Example: …`), a `:` at the end, starting with `[` `{` `>` `|` `'` `"` `*` `!` `%` `@` `,` or a backtick, or starting with `- ` `? ` `: ` (the character followed by a space). (2) **Silent change of meaning** — it parses but is wrong: starting with `#` makes the description empty, ` #` in the middle cuts off the rest, starting with `&` turns it into an anchor. Double quotes avoid both; inside the double quotes do not use `"` or `\`.

Registries with strict validation often also block: absolute paths containing a Windows drive letter (drive + colon + slash — write `<root>/…` instead of hard-coding a drive), strings that look like credentials, broken markdown links inside the package, and any `.py` script that does not force stdout/stderr to UTF-8. Independent review also often rejects syntax that only works in the claude.ai sandbox (`/mnt/skills/…`, `/mnt/user-data/outputs/`, `present_files`, `extract-text`) — write host-neutral instructions.

### 5 Patterns (choose exactly 1)

Sequential Workflow | Multi-MCP Coordination | Iterative Refinement | Context-Aware Tool Selection | Domain-Specific Intelligence

### 9 Skill Types (choose exactly 1)

Library & API Reference | Product Verification | Code Scaffolding & Templates | Business Process Automation | Data Fetching & Analysis | Code Quality & Review | CI/CD & Deployment | Runbooks | Infrastructure Operations

---

## Description writing Protocol (MANDATORY)

**Goal:** activation rate 72–90%.

| Requirement | Detail |
|---|---|
| Trigger phrases | At least 5 phrases (in every language your users write in, romanized if needed, e.g. English + Vietnamese) |
| Style | Situational: "Use when the user wants...", "WHENEVER..." |
| Real examples | At least a few sample prompts users actually type |
| Length | Under 1024 characters |
| No diacritics | YAML fields only: plain ASCII — strip every accented letter and tone mark (e.g. Vietnamese ă â ê ô ơ ư đ). For languages not written in Latin script, use romanized or English trigger phrases |
| No angle brackets | Never use opening/closing angle brackets |
| Negative blocker | End with a blocking sentence: "Do not use when..." |

**Example of a standard Description (plain ASCII):**

```
Use when the user wants to add content to an AI Wiki on Notion, save AI knowledge,
classify article links, notes, tools, prompts. WHENEVER the user pastes a link,
a note, a tool name or an AI prompt to keep. Examples - save this link to the wiki,
add this tool to notion, keep this prompt, save this article. Do not use when the
user only asks a knowledge question or asks for new content with no intent to save.
```

---

## 14-item Checklist (GATE — 14/14 required)

| # | Criterion | Pass when |
|---|---|---|
| 1 | `name` is correct kebab-case and does not contain "claude"/"anthropic" | ☐ |
| 2 | YAML has `name` + `version` + `description` (+ `pattern` + `type`) | ☐ |
| 3 | `description` fully meets the Description Protocol | ☐ |
| 4 | No angle brackets in the description | ☐ |
| 5 | SKILL.md body under 500 lines (long parts offloaded to references) | ☐ |
| 6 | Exactly 1 Pattern + 1 Skill Type chosen | ☐ |
| 7 | Does not teach basics the AI already knows | ☐ |
| 8 | Has real gotchas (at least 5) | ☐ |
| 9 | Structure fits the need: long documentation split into `references/` with clear pointers | ☐ |
| 10 | Repetitive/deterministic logic packaged as `scripts/` instead of described by hand | ☐ |
| 11 | Description optimized for activation + has a negative blocker | ☐ |
| 12 | Tested 3 positive + 3 negative cases | ☐ |
| 13 | Rewrote in full (no patching) if this is an upgrade | ☐ |
| 14 | **Diff Check done: every original table/schema/value/gotcha/test case is still complete, nothing compressed or truncated** | ☐ |

**Answer the user only when all 14/14 pass.**

---

## 6 Golden principles

1. **Do not teach what the AI already knows** — focus only on the specific context + gotchas.
2. **Gotchas are the most valuable asset** — record every repeated failure.
3. **Keep updating** — every mistake = a new gotcha → upgrade right away.
4. **Smallest structure that is enough** — simple = 1 file, complex = split into a folder and use Progressive Disclosure.
5. **Rewrite full = restructure, not compress** — preserve technical content completely; if it is too long, split into references, do not compress it away.
6. **Never patch piecemeal.**

---

## Output Format when answering the user (MANDATORY, in this order)

1. **Skill summary** — purpose, pattern, type, structure (single file or folder).
2. **Full SKILL.md content** — in a code block; if it is a folder, also list the `references/` / `scripts/` / `assets/` files.
3. **Deployment guide** — concrete steps for the target environment.
4. **Test results** — 3 positive + 3 negative cases.
5. **Diff Check** — confirm no original content was lost (if this is an upgrade/rewrite).
6. **Gotchas handled** — if this is an upgrade.
7. **Version** — the version in the YAML.

---

## Deployment guide

**Folder skill (Claude Code / agent / API):**
1. Create the folder `skill-name-kebab-case/`.
2. Put `SKILL.md` at the root, plus `scripts/`, `references/`, `assets/` if any.
3. Install the skill the way the environment expects (skills directory folder or a `.skill` package).
4. Trigger 1 positive prompt to confirm activation.

**Single-file skill (UI editor, paste directly):**
1. Go to Settings → Skills → click `+`.
2. Set the name to match `name` in the YAML.
3. Paste the whole SKILL.md → Save.
4. Trigger 1 positive prompt.

**Check after deploying:** does not activate → inspect the description (too short? missing the negative blocker? frontmatter fails to parse, file has a BOM or the wrong encoding?). Script/reference does not run → check the relative paths inside the folder.

---

## Gotchas (real ones from deployments)

1. **LLMs compress/truncate when rewriting** — the most dangerous gotcha, especially with weaker models. When told to "rewrite full", a model tends to summarize and drop tables/schemas/values. Always run the 3-step anti-compression process (inventory → constrained rewrite → diff check) and include the self-binding instruction "do not summarize; if too long, split into references instead of deleting".
2. **A multi-line description must use the folded scalar** `>-` in YAML. Without it the YAML breaks or the description gets cut.
3. **Write YAML frontmatter in plain ASCII, and save the file as UTF-8 without a BOM** — this applies only to YAML fields; the markdown body can use any language with full diacritics as normal. Accented letters and tone marks (for example Vietnamese ă â ê ô ơ ư đ) do NOT break YAML by themselves: a UTF-8 file with diacritics parses correctly and Claude Code reads the full description. Keep YAML in ASCII because the real risk is the file encoding: (a) a file saved in the wrong code page (for example Windows-1258) makes a UTF-8 parser such as PyYAML fail, while `claude plugin validate` does not report it, so do not rely on it to catch this; ASCII text is byte-identical in all common encodings (UTF-8, Windows-125x, VISCII), so it is not affected; (b) accented text in decomposed NFD form (common when copied from macOS) looks identical to the NFC form but is a different, longer string, so string matching and character-limit counts drift. ASCII does not protect against a BOM: with a BOM at the start of the file, `claude plugin validate` reports that no frontmatter was found, but only as a warning and the validation still passes — so validate does not block this either; always save as UTF-8 without a BOM.
4. **No angle brackets in the description** — they are easily mistaken for tags and break the frontmatter.
5. **No unescaped double quotes inside the description string** — they cause YAML parse errors.
6. **A skill is a folder, not necessarily 1 file** — do not cram everything into SKILL.md. Split long documentation into `references/` (loaded dynamically) and repetitive logic into `scripts/` (runs offline, costs no context). Use a single file only when the skill is truly short.
7. **The SKILL.md body should still stay under 500 lines** even with a folder — that is the limit for Progressive Disclosure level 2; offload the excess to references with clear pointers.
8. **Pointers to references must be explicit** — say clearly "read file X when situation Y happens", otherwise the AI will not know when to load it.
9. **An upgrade must keep the old `name`**, only bump `version`. Renaming to `-v2` creates a duplicate skill and causes over-triggering.
10. **A skill that is too simple (1 step) will not activate** — Claude handles 1-step tasks on its own. Package only multi-step / specialized workflows.
11. **Patchwork is a source of accumulated errors** — always rewrite in full when there are 2 or more problems (but the rewrite must preserve content, see gotcha #1).
12. **5-field frontmatter is rejected by Agent Skills targets** (12/09/2026): three skills written with the 5-field template all failed validation when submitted to a strictly validating skill registry because of `version`/`pattern`/`type` and the folded `>-` `description` — reduce them to 2 one-line fields before submitting (see "Variants by TARGET ENVIRONMENT"). Gotcha #2 (folded `>-`) only applies to 5-field targets; on 2-field targets, `>-` is an error.
13. **A one-line `description` without double quotes breaks YAML** (17/11/2026): three skills (including this one) had `Vi du: …` (Vietnamese for "Example: …") in a bare description — `: ` is mapping syntax, so the YAML parser rejects it; according to the `claude plugin validate` documentation, the skill then loads with empty metadata. `claude plugin validate` catches it when packaging a plugin. Wrap `description` in double quotes by default (see the template under "Variants by TARGET ENVIRONMENT").

---

## Test cases

**Positive (must activate):**
1. "Write me a skill that automatically saves links to my Notion wiki." → activates, runs the 8-step new-skill process.
2. "Upgrade the x-post-optimizer skill to a new version and fix it so it triggers properly." → activates, runs the 8-step upgrade process (rewrite full + diff check).
3. "Review whether this SKILL.md meets the standard and whether it lost content during the rewrite." → activates, scores the 14-item Checklist + Diff Check.

**Negative (must not activate):**
1. "What is a skill in an RPG game?" → does not activate (general knowledge question).
2. "Explain the concept of an AI agent to me." → does not activate (general concept).
3. "Write me a blog post about AI." → does not activate (other content, not a skill file).

---

**Version history:** v2.4.1 — 17/11/2026: English edition of v2.3.2 — gotcha #3 corrected: diacritics do not break UTF-8 YAML; the real reasons to keep YAML in ASCII are wrong code pages and NFD text; new warning that a BOM hides the frontmatter and validate only warns; the post-deploy check no longer asks "still has diacritics?" and asks about parse errors, BOM and encoding instead. v2.4.0 — 17/11/2026: English edition of v2.3.1 — full translation of the skill; the description is now in English; Vietnamese-specific rules generalized for international users (plain ASCII instead of "no Vietnamese diacritics", trigger phrases in every language users write in, English example description and test cases); added "respond in the language the user writes in" after the activation sentence; added `Example: …` next to `Vi du: …` as an English sample of the `: ` parse error; non-Latin-script languages use romanized or English trigger phrases; clarified that "Protocol v2.2" is the process name, not the release number. v2.3.1 — 17/11/2026: wrapped this skill's own description in double quotes (the bare version containing `Vi du: …` was broken YAML); the 2-field frontmatter template now uses double quotes, with the YAML scalar rules; gotcha #13. v2.3.0 — 12/09/2026: added the "Variants by TARGET ENVIRONMENT" section (Agent Skills targets accept only the 2 fields name + description on one line; gates block drive paths, credentials, broken links, non-UTF-8 scripts; review rejects claude.ai sandbox syntax) + gotcha #12; fixed the skill-structure line. v2.2.1 — packaging change only. v2.2.0 — Added the Supreme rule "Preserve content when rewriting" (restructure is not compress, 3-step anti-compression process inventory/constraint/diff), checklist #14 Diff Check, gotcha #1 about LLM compression/truncation, added the Content-loss failure type.
