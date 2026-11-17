[![Version](https://img.shields.io/badge/version-2.4.0-blue)](#version-history)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Last Commit](https://img.shields.io/github/last-commit/chuong1224/nova-skill-creator)](https://github.com/chuong1224/nova-skill-creator/commits)
[![Stars](https://img.shields.io/github/stars/chuong1224/nova-skill-creator?style=social)](https://github.com/chuong1224/nova-skill-creator/stargazers)
[![Agent Skill](https://img.shields.io/badge/Agent%20Skill-Claude%20%7C%20Codex%20%7C%20Grok-8A2BE2)](skills/nova-skill-creator/SKILL.md)

<img src="assets/icon.png" alt="Nova Skill Creator icon" width="96">

# Nova Skill Creator

> A meta-skill that turns any "write / upgrade / edit / review a skill" request into a standard engineering workflow and produces a deploy-ready SKILL.md.

---

## English

### Overview

Nova Skill Creator is a **Skill Engineer Protocol** for LLM agents. When you ask an agent to create, upgrade, fix or review a skill, this skill makes it follow a fixed, step-by-step process instead of improvising. The output is a self-contained "virtual employee": either a single `SKILL.md` file or a full skill folder, depending on complexity and the target environment.

The skill is written in English and answers in the language you write in (for example English or Vietnamese).

### Features

- **Includes SKILL.md** (`skills/nova-skill-creator/SKILL.md`) — packaged as a Claude plugin, and also usable as a single file in any skills folder.
- Sequential workflow for 4 request types: new skill · upgrade · edit · review.
- Frontmatter variants per target environment:
  - 5 fields (`name`, `description`, `version`, `pattern`, `type`) for claude.ai accounts, Grok and other free-form LLMs.
  - 2 fields only (`name` + one-line `description`) for the standard Agent Skills format (Claude Code `.claude/skills`, Codex `.agents/skills`).
- 5 skill patterns to choose from: Sequential Workflow · Multi-MCP Coordination · Iterative Refinement · Context-Aware Tool Selection · Domain-Specific Intelligence.
- "Preserve content on rewrite" rule: a 3-step process that stops the model from silently compressing or dropping content.
- Quality checklist (including a diff check) and a gotchas table from real failures.

### Installation

- **Claude plugin:** this repository is a Claude plugin (`.claude-plugin/plugin.json`). In Claude Code, test it locally with `claude --plugin-dir ./nova-skill-creator`.
- **Claude Code (single skill):** copy `skills/nova-skill-creator/SKILL.md` to `.claude/skills/nova-skill-creator/SKILL.md` (project) or `~/.claude/skills/nova-skill-creator/SKILL.md` (global).
- **Codex:** copy `skills/nova-skill-creator/SKILL.md` to `.agents/skills/nova-skill-creator/SKILL.md`.
- **claude.ai:** zip the `skills/nova-skill-creator` folder and upload it in *Settings → Capabilities → Skills*.
- **Grok / other LLMs:** paste the content of `skills/nova-skill-creator/SKILL.md` into the custom instructions or project knowledge.

### Usage

Ask in natural language, for example:

- "Write a skill that saves links into Notion."
- "Upgrade this skill to a new version."
- "Review this skill — does it meet the standard?"
- "Fix the trigger of this skill."

The agent answers with the opening line *"Skill Engineer Protocol v2.2 activated. Starting the standard process..."*, then continues in your language and runs the workflow.

### Data & privacy

This plugin contains **one skill made of Markdown instructions only**.

- It runs no code: no scripts, no hooks, no MCP servers, no package launchers.
- It makes no network requests and sends no data to any server, including the author's.
- It reads no credentials and stores nothing.
- The skill only guides Claude while Claude writes or reviews a skill inside your own conversation. Whatever Claude does with files in that conversation follows your normal Claude settings and permissions.

Because the plugin talks to no remote server, it has no privacy policy URL.

### License

MIT — see [LICENSE](LICENSE).

---

## Tiếng Việt

### Tổng quan

Nova Skill Creator là một meta-skill. Khi bạn nhờ agent viết mới, nâng cấp, sửa hoặc review một skill, skill này buộc agent đi theo một quy trình chuẩn từng bước. Kết quả là một skill tự đứng vững: một file `SKILL.md` hoặc một folder đầy đủ. Skill viết bằng tiếng Anh nhưng trả lời bằng ngôn ngữ bạn dùng, nên hỏi bằng tiếng Việt thì nhận câu trả lời tiếng Việt.

### Tính năng

- Có sẵn `SKILL.md`, dùng được ngay.
- Quy trình cho 4 loại yêu cầu: viết mới · nâng cấp · chỉnh sửa · review.
- Frontmatter theo môi trường đích: 5 trường cho claude.ai/Grok, 2 trường cho Agent Skills chuẩn (Claude Code, Codex).
- 5 pattern skill để chọn.
- Quy tắc bảo toàn nội dung khi viết lại, checklist chất lượng và bảng gotcha.

### Cài đặt

Repo này là một Claude plugin (`.claude-plugin/plugin.json`). Muốn dùng lẻ, chép `skills/nova-skill-creator/SKILL.md` vào thư mục skill của agent (`.claude/skills/nova-skill-creator/` hoặc `.agents/skills/nova-skill-creator/`). Với claude.ai, nén folder `skills/nova-skill-creator` rồi upload trong phần Skills.

### Hướng dẫn sử dụng

Nói bằng câu tự nhiên, ví dụ: "viết skill phân loại email", "nâng cấp skill này lên version mới", "review skill này có đạt chuẩn không".

### Dữ liệu & quyền riêng tư

Plugin chỉ có một skill bằng chữ hướng dẫn. Plugin không chạy code, không gọi mạng, không gửi dữ liệu đi đâu, không đọc mật khẩu hay token, không lưu gì.

### Giấy phép

MIT — xem [LICENSE](LICENSE).

---

## Version History

| Version | Date | Changes |
|---|---|---|
| 2.4.0 | 2026-11-17 | English edition: the whole skill is translated to English and the description is in English. Vietnamese-specific rules are generalized (plain ASCII instead of "no Vietnamese diacritics", trigger phrases in every language your users write in). After the activation sentence, the skill now answers in the user's language. |
| 2.3.1 | 2026-11-17 | Template now wraps the one-line `description` in double quotes and explains which bare values break YAML (parse error vs. silent change of meaning); new gotcha #13. |
| 2.3.0 | 2026-09-12 | Add "target environment variants" section (2-field frontmatter for Agent Skills; stricter registry checks) + gotcha #12. First public release on GitHub: 2026-11-17. Packaged as a Claude plugin on 2026-11-17 (`.claude-plugin/plugin.json`, icon, `skills/` layout, Data & privacy section); SKILL.md `description` wrapped in double quotes so its YAML frontmatter parses (the unquoted `Vi du: ...` broke the parser); instructions unchanged. |
| 2.2.1 | 2026-09-12 | Packaging change only. |
| 2.2.0 | — | Add "preserve content on rewrite" rule, checklist #14 Diff Check, gotcha #1 on LLM compression. |
