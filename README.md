[![Version](https://img.shields.io/badge/version-2.3.0-blue)](#version-history)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Last Commit](https://img.shields.io/github/last-commit/chuong1224/nova-skill-creator)](https://github.com/chuong1224/nova-skill-creator/commits)
[![Stars](https://img.shields.io/github/stars/chuong1224/nova-skill-creator?style=social)](https://github.com/chuong1224/nova-skill-creator/stargazers)
[![Agent Skill](https://img.shields.io/badge/Agent%20Skill-Claude%20%7C%20Codex%20%7C%20Grok-8A2BE2)](SKILL.md)

# Nova Skill Creator

> A meta-skill that turns any "write / upgrade / edit / review a skill" request into a standard engineering workflow and produces a deploy-ready SKILL.md.

---

## English

### Overview

Nova Skill Creator is a **Skill Engineer Protocol** for LLM agents. When you ask an agent to create, upgrade, fix or review a skill, this skill makes it follow a fixed, step-by-step process instead of improvising. The output is a self-contained "virtual employee": either a single `SKILL.md` file or a full skill folder, depending on complexity and the target environment.

The skill text is written in Vietnamese.

### Features

- **Includes SKILL.md** — one file, ready to drop into a skills folder.
- Sequential workflow for 4 request types: new skill · upgrade · edit · review.
- Frontmatter variants per target environment:
  - 5 fields (`name`, `description`, `version`, `pattern`, `type`) for claude.ai accounts, Grok and other free-form LLMs.
  - 2 fields only (`name` + one-line `description`) for the standard Agent Skills format (Claude Code `.claude/skills`, Codex `.agents/skills`).
- 5 skill patterns to choose from: Sequential Workflow · Multi-MCP Coordination · Iterative Refinement · Context-Aware Tool Selection · Domain-Specific Intelligence.
- "Preserve content on rewrite" rule: a 3-step process that stops the model from silently compressing or dropping content.
- Quality checklist (including a diff check) and a gotchas table from real failures.

### Installation

- **Claude Code:** copy `SKILL.md` to `.claude/skills/nova-skill-creator/SKILL.md` (project) or `~/.claude/skills/nova-skill-creator/SKILL.md` (global).
- **Codex:** copy `SKILL.md` to `.agents/skills/nova-skill-creator/SKILL.md`.
- **claude.ai:** zip the `nova-skill-creator` folder and upload it in *Settings → Capabilities → Skills*.
- **Grok / other LLMs:** paste the content of `SKILL.md` into the custom instructions or project knowledge.

### Usage

Ask in natural language, for example:

- "Write a skill that saves links into Notion."
- "Upgrade this skill to a new version."
- "Review this skill — does it meet the standard?"
- "Fix the trigger of this skill."

The agent answers with the opening line *"Đã kích hoạt Skill Engineer Protocol v2.2. Bắt đầu quy trình chuẩn..."* and then runs the workflow.

### License

MIT — see [LICENSE](LICENSE).

---

## Tiếng Việt

### Tổng quan

Nova Skill Creator là một meta-skill. Khi anh nhờ agent viết mới, nâng cấp, sửa hoặc review một skill, skill này buộc agent đi theo một quy trình chuẩn từng bước. Kết quả là một skill tự đứng vững: một file `SKILL.md` hoặc một folder đầy đủ.

### Tính năng

- Có sẵn `SKILL.md`, dùng được ngay.
- Quy trình cho 4 loại yêu cầu: viết mới · nâng cấp · chỉnh sửa · review.
- Frontmatter theo môi trường đích: 5 trường cho claude.ai/Grok, 2 trường cho Agent Skills chuẩn (Claude Code, Codex).
- 5 pattern skill để chọn.
- Quy tắc bảo toàn nội dung khi viết lại, checklist chất lượng và bảng gotcha.

### Cài đặt

Chép `SKILL.md` vào thư mục skill của agent (`.claude/skills/nova-skill-creator/` hoặc `.agents/skills/nova-skill-creator/`). Với claude.ai, nén folder rồi upload trong phần Skills.

### Hướng dẫn sử dụng

Nói bằng câu tự nhiên, ví dụ: "viết skill phân loại email", "nâng cấp skill này lên version mới", "review skill này có đạt chuẩn không".

### Giấy phép

MIT — xem [LICENSE](LICENSE).

---

## Version History

| Version | Date | Changes |
|---|---|---|
| 2.3.0 | 2026-09-12 | Add "target environment variants" section (2-field frontmatter for Agent Skills; stricter registry checks) + gotcha #12. First public release on GitHub: 2026-11-17. |
| 2.2.1 | 2026-09-12 | Packaging change only. |
| 2.2.0 | — | Add "preserve content on rewrite" rule, checklist #14 Diff Check, gotcha #1 on LLM compression. |
