---
name: nova-skill-creator
description: "Kich hoat khi user muon viet skill moi, tao skill, lam skill cho AI, nang cap skill, chinh sua skill, sua skill, review skill, kiem tra skill cho Claude hoac Grok hoac LLM khac. BAT CU KHI NAO user de cap den skill, SKILL.md, SOP cho AI, nhan vien ao, skill engineer. Vi du: viet skill cho viec luu link vao Notion, tao skill phan loai email, nang cap skill nay len version moi, review skill nay co dat chuan khong, sua loi trigger cho skill, lam SKILL.md cho workflow nay. Khong kich hoat khi user chi hoi kien thuc chung skill la gi, hoi ve khai niem AI agent ma khong gan voi thiet ke skill cu the, hoac yeu cau tao noi dung khac khong phai skill file."
---

# Nova Skill Creator — Skill Engineer Protocol v2.2

> **Version 2.3.1** · Pattern: Sequential Workflow · Type: Code Scaffolding & Templates

## Mục đích

Đây là meta-skill biến mọi yêu cầu liên quan đến skill (viết mới / nâng cấp / chỉnh sửa / review) thành một quy trình kỹ thuật chuẩn, cho ra một skill chuyên nghiệp deploy được ngay. Mỗi skill đầu ra là một "nhân viên ảo" tự đứng vững, có thể là **1 file SKILL.md** hoặc **1 folder đầy đủ** tuỳ độ phức tạp và môi trường đích.

Khi skill này kích hoạt, mở đầu phản hồi bằng đúng câu:
**"Đã kích hoạt Skill Engineer Protocol v2.2. Bắt đầu quy trình chuẩn..."**

---

## ⚠️ Bảo toàn nội dung khi rewrite (QUY TẮC TỐI THƯỢNG)

"Rewrite full" nghĩa là **tái cấu trúc (restructure)**, KHÔNG phải **nén/tóm tắt (compress)**. Khi áp dụng skill này cho LLM khác (đặc biệt model yếu hơn), lỗi phổ biến nhất là mô tả kỹ thuật bị nén, bảng bị cắt cột, schema/giá trị cụ thể bị lược bỏ — output trông gọn hơn nhưng đã mất nội dung nguyên bản.

**Phân biệt bắt buộc:**

| Restructure (ĐÚNG) | Compress (SAI) |
|---|---|
| Đổi bố cục, gộp section trùng, sửa câu chữ | Bỏ bớt bảng, dòng, cột, ví dụ |
| Giữ nguyên mọi giá trị/schema/ID/quy tắc | Diễn giải lại schema thành "đại khái" |
| Số mục đầu ra >= số mục đầu vào | Rút gọn 10 gotchas còn 4 |
| Tăng độ rõ ràng | Giảm độ chi tiết để "cho ngắn" |

**Quy trình 3 bước chống nén — BẮT BUỘC trước/sau mọi lần rewrite hoặc nâng cấp:**

1. **Kiểm kê trước (Content Inventory):** liệt kê toàn bộ phần tử nguyên bản KHÔNG được mất — mỗi bảng (kèm số dòng/cột), mỗi quy tắc schema, mỗi giá trị cụ thể (ID, version, option), mỗi gotcha, mỗi test case. Ghi thành danh sách đối chiếu.
2. **Rewrite có ràng buộc:** khi viết lại, mọi phần tử trong danh sách phải xuất hiện đầy đủ ở output. Được phép cải thiện cách trình bày, KHÔNG được phép giảm số phần tử hay độ chi tiết của từng phần tử.
3. **Đối chiếu sau (Diff Check):** so output với danh sách kiểm kê. Nếu thiếu bất kỳ phần tử nào hoặc một bảng ít dòng/cột hơn bản gốc → CHƯA ĐẠT, bổ sung lại trước khi trả lời.

**Chỉ thị tự ràng buộc cho LLM thực thi:** "Bảo toàn nguyên văn mọi bảng, schema, giá trị cụ thể và gotcha. Không tóm tắt, không lược bỏ chi tiết kỹ thuật. Nếu cần ngắn hơn thì TÁCH sang references/, tuyệt đối không XOÁ nội dung."

---

## Skill là folder — chọn cấu trúc theo độ phức tạp

Claude, Claude Code và các agent hỗ trợ skill dạng **folder** với SKILL.md + tài nguyên đi kèm, có cơ chế **Progressive Disclosure** (load tài nguyên theo nhu cầu), chạy được script offline, và load `references/` động. Không bắt buộc nhồi mọi thứ vào 1 file.

| Mức độ | Cấu trúc khuyến nghị |
|---|---|
| Skill đơn giản, ngắn | Chỉ 1 file `SKILL.md` (vẫn hợp lệ) |
| Skill có logic lặp/deterministic | Folder + `scripts/` |
| Skill nhiều domain / tài liệu dài | Folder + `references/` tách theo variant |
| Skill xuất file mẫu (template/icon/font) | Folder + `assets/` |

**Nguyên tắc:** chọn cấu trúc nhỏ nhất đủ dùng. Đơn giản thì 1 file; phức tạp thì tách folder để giữ SKILL.md gọn. **Khi nội dung quá dài → TÁCH sang references/, KHÔNG nén bỏ.**

---

## Anatomy của một Skill

```
skill-name-kebab-case/
├── SKILL.md          (bắt buộc)
│   ├── YAML frontmatter (5 trường cho account/LLM tự do · CHỈ name + description cho đích Agent Skills chuẩn — xem 'Biến thể theo MÔI TRƯỜNG ĐÍCH')
│   └── Markdown instructions
└── Tài nguyên đi kèm (tuỳ chọn)
    ├── scripts/      - Code thực thi cho task deterministic/lặp lại
    ├── references/   - Tài liệu load vào context khi cần
    └── assets/       - File dùng trong output (template, icon, font)
```

### Progressive Disclosure (3 tầng load)

| Tầng | Nội dung | Khi nào vào context |
|---|---|---|
| 1 | Metadata (`name` + `description`) | Luôn có (~100 từ) |
| 2 | Body SKILL.md | Khi skill kích hoạt (lý tưởng dưới 500 dòng) |
| 3 | Tài nguyên bundled | Khi cần (`references/` load động; `scripts/` chạy không cần load nội dung vào context) |

**Quy tắc:** giữ body SKILL.md dưới 500 dòng. Khi sắp chạm giới hạn → **tách** sang `references/` và để pointer rõ ràng trong SKILL.md chỉ chỗ cần đọc. Reference dài (>300 dòng) nên có mục lục. Tách KHÁC với nén — tách giữ nguyên nội dung ở file khác, nén làm mất nội dung.

---

## Quy trình VIẾT SKILL MỚI (8 bước)

| # | Bước | Mô tả |
|---|---|---|
| 1 | Hỏi rõ thông tin thiếu | Mục tiêu, gotchas, ngôn ngữ, output, môi trường đích. Nếu hội thoại đã có sẵn workflow → trích từ lịch sử trước, chỉ hỏi phần còn thiếu |
| 2 | Chọn 1 Pattern + 1 Loại Skill | Đúng 1 của mỗi loại (xem danh sách bên dưới) |
| 3 | Viết Description TRƯỚC | Bước quan trọng nhất — tuân thủ Protocol Description ở dưới |
| 4 | Chọn cấu trúc + thiết kế nội dung | Single-file hay folder? Phần nào inline, phần nào tách `references/`, logic nào thành `scripts/` |
| 5 | Viết SKILL.md | YAML + Workflow + Rules/Schema + Gotchas; body dưới 500 dòng, offload phần dài sang references |
| 6 | Viết script/reference (nếu cần) | Logic deterministic/lặp lại → `scripts/`; tài liệu dài → `references/` có pointer |
| 7 | Test kích hoạt | 3 positive + 3 negative cases |
| 8 | Trả lời theo Output Format | Xem mục Output Format bên dưới |

**CHỈ RELEASE khi đạt đủ 14/14 Checklist.**

---

## Quy trình NÂNG CẤP / CHỈNH SỬA (8 bước)

1. Đọc toàn bộ SKILL.md (và các file đi kèm) hiện tại.
2. **Kiểm kê trước** — liệt kê mọi bảng/schema/giá trị/gotcha/test case KHÔNG được mất (Quy tắc tối thượng, bước 1).
3. Ghi nhận gotcha từ user và từ lần chạy trước.
4. Đánh giá theo Checklist 14 tiêu chí.
5. Xác định loại lỗi: **Under-trigger** / **Over-trigger** / **Execution error** / **Content-loss (nén/cắt xén)**.
6. **Rewrite toàn bộ có ràng buộc bảo toàn** — restructure, KHÔNG compress. TUYỆT ĐỐI KHÔNG patch từng phần.
7. **Đối chiếu sau** — diff output với danh sách kiểm kê, bổ sung nếu thiếu.
8. Test lại 3 positive + 3 negative cases, tăng `version`.

**Nguyên tắc sắt:** Rewrite full = restructure giữ trọn nội dung. Không patch chắp vá. Không nén bỏ chi tiết. Giữ nguyên `name` cũ khi nâng cấp.

---

## YAML Frontmatter (BẮT BUỘC)

```yaml
---
name: skill-name-kebab-case
version: x.x.x
description: >-
  [Mo ta toi uu activation, duoi 1024 ky tu, khong dau tieng Viet, khong dau goc nhon]
pattern: [1 trong 5 Pattern]
type: [1 trong 9 Loai Skill]
---
```

### Biến thể theo MÔI TRƯỜNG ĐÍCH (hỏi trước khi viết frontmatter)

Khối 5 trường ở trên là mẫu cho **account claude.ai / Grok / LLM tự do**. Có đích chỉ nhận đúng 2 trường — nộp bản 5 trường vào đó là **trượt validate ngay**, chưa tới bước review. Bước 1 (thu thập) PHẢI hỏi hoặc suy ra đích; không rõ thì mặc định viết bản 2 trường, vì bản 2 trường chạy được ở mọi đích còn bản 5 trường thì không.

| Đích | Frontmatter | Version / Pattern / Type để đâu |
|---|---|---|
| Account claude.ai, Grok, LLM khác | 5 trường như mẫu trên, `description` folded `>-` | trong YAML |
| **Agent Skills chuẩn** (Claude Code `.claude/skills`, Codex `.agents/skills`) và các skill registry có validate chặt | **CHỈ `name` + `description`, mỗi trường MỘT dòng** — không `>-`, không dòng thụt đầu, không trường thứ ba | một dòng blockquote ngay dưới H1: `> **Version x.y.z** · Pattern: … · Type: …`; version thật sống trong file registry/manifest của hệ thống đích (nếu có) |

Mẫu cho đích Agent Skills:

```yaml
---
name: skill-name-kebab-case
description: "[Mo ta toi uu activation, MOT dong, >= 40 ky tu, khong dau tieng Viet, khong dau goc nhon]"
---
```

**Luôn bọc `description` một dòng trong ngoặc kép.** Claude Code và Codex đọc frontmatter bằng parser YAML thật. Với giá trị trần, có hai kiểu hỏng. (1) **Lỗi parse** — skill nạp với metadata rỗng mà không báo: có `: ` ở giữa (ví dụ `Vi du: …`), có `:` ở cuối, mở đầu bằng `[` `{` `>` `|` `'` `"` `*` `!` `%` `@` `,` hoặc backtick, hay mở đầu bằng `- ` `? ` `: ` (ký tự kèm khoảng trắng). (2) **Đổi nghĩa âm thầm** — parse được nhưng sai: mở đầu bằng `#` làm description thành rỗng, ` #` ở giữa cắt cụt phần sau, mở đầu bằng `&` thành anchor. Bọc ngoặc kép tránh được cả hai; trong ngoặc kép không dùng `"` hay `\`.

Các registry có validate chặt thường chặn thêm: path tuyệt đối có chữ ổ đĩa Windows (dạng ổ + hai chấm + gạch chéo — viết `<root>/…` thay vì ghi cứng ổ), chuỗi giống credential, link markdown gãy trong package, và mọi script `.py` phải ép stdout/stderr sang UTF-8. Review độc lập thường còn bác cú pháp chỉ chạy trong sandbox claude.ai (`/mnt/skills/…`, `/mnt/user-data/outputs/`, `present_files`, `extract-text`) — viết hướng dẫn trung tính theo host.

### 5 Patterns (chọn đúng 1)

Sequential Workflow | Multi-MCP Coordination | Iterative Refinement | Context-Aware Tool Selection | Domain-Specific Intelligence

### 9 Loại Skill (chọn đúng 1)

Library & API Reference | Product Verification | Code Scaffolding & Templates | Business Process Automation | Data Fetching & Analysis | Code Quality & Review | CI/CD & Deployment | Runbooks | Infrastructure Operations

---

## Protocol viết Description (BẮT BUỘC)

**Mục tiêu:** activation rate 72–90%.

| Yêu cầu | Chi tiết |
|---|---|
| Trigger phrases | Tối thiểu 5 cụm (cả tiếng Việt + tiếng Anh) |
| Văn phong | Kiểu tình huống: "Kich hoat khi user muon...", "BAT CU KHI NAO..." |
| Ví dụ thực tế | Có ít nhất vài prompt mẫu user hay gõ |
| Độ dài | Dưới 1024 ký tự |
| Không dấu tiếng Việt | Bỏ toàn bộ ă â ê ô ơ ư đ và mọi dấu thanh (chỉ trong YAML) |
| Không ký tự góc nhọn | Tuyệt đối không dùng dấu mở/đóng góc nhọn |
| Negative blocker | Kết bằng câu chặn: "Khong kich hoat khi..." |

**Ví dụ Description chuẩn (không dấu):**

```
Kich hoat khi user muon them noi dung vao AI Wiki tren Notion, luu kien thuc AI,
phan loai link bai viet, ghi chu, tool, prompt. BAT CU KHI NAO user paste link,
ghi chu, ten tool, prompt AI can luu lai. Vi du: luu link nay vao wiki, them tool
nay vao notion, ghi lai prompt nay, save bai viet nay. Khong kich hoat khi user
chi hoi kien thuc hoac yeu cau tao noi dung moi khong co y dinh luu.
```

---

## Checklist 14 tiêu chí (GATE — bắt buộc 14/14)

| # | Tiêu chí | Pass khi |
|---|---|---|
| 1 | `name` đúng kebab-case, không chứa "claude"/"anthropic" | ☐ |
| 2 | YAML đủ `name` + `version` + `description` (+ `pattern` + `type`) | ☐ |
| 3 | `description` đạt đầy đủ Protocol Description | ☐ |
| 4 | Không có ký tự góc nhọn trong description | ☐ |
| 5 | Body SKILL.md dưới 500 dòng (phần dài đã offload sang references) | ☐ |
| 6 | Đã chọn đúng 1 Pattern + 1 Loại Skill | ☐ |
| 7 | Không dạy kiến thức cơ bản AI đã biết | ☐ |
| 8 | Có gotchas thực tế (tối thiểu 5) | ☐ |
| 9 | Cấu trúc đúng nhu cầu: tài liệu dài tách `references/` có pointer rõ ràng | ☐ |
| 10 | Logic lặp lại/deterministic đóng gói thành `scripts/` thay vì mô tả thủ công | ☐ |
| 11 | Description tối ưu activation + có negative blocker | ☐ |
| 12 | Đã test 3 positive + 3 negative cases | ☐ |
| 13 | Đã rewrite full (không patch) nếu là nâng cấp | ☐ |
| 14 | **Đã Diff Check: mọi bảng/schema/giá trị/gotcha/test case nguyên bản còn đủ, không bị nén/cắt xén** | ☐ |

**Chỉ trả lời user khi đủ 14/14.**

---

## 6 Nguyên tắc vàng

1. **Không dạy cái AI đã biết** — chỉ tập trung context riêng + gotchas.
2. **Gotchas là tài sản quý nhất** — ghi lại mọi lỗi lặp lại.
3. **Cập nhật liên tục** — mỗi lần sai = gotcha mới → nâng cấp ngay.
4. **Cấu trúc nhỏ nhất đủ dùng** — đơn giản 1 file, phức tạp tách folder và dùng Progressive Disclosure.
5. **Rewrite full = restructure, không compress** — bảo toàn trọn vẹn nội dung kỹ thuật; dài quá thì tách references, không nén bỏ.
6. **Không bao giờ patch chắp vá.**

---

## Output Format khi trả lời user (BẮT BUỘC, đúng thứ tự)

1. **Tóm tắt Skill** — mục đích, pattern, loại, cấu trúc (single-file hay folder).
2. **Nội dung SKILL.md đầy đủ** — trong code block; nếu là folder, liệt kê thêm các file `references/` / `scripts/` / `assets/`.
3. **Hướng dẫn deploy** — các bước cụ thể theo môi trường đích.
4. **Kết quả Test** — 3 positive + 3 negative cases.
5. **Diff Check** — xác nhận không mất nội dung nguyên bản (nếu là nâng cấp/rewrite).
6. **Gotcha đã xử lý** — nếu là nâng cấp.
7. **Phiên bản** — version trong YAML.

---

## Hướng dẫn deploy

**Skill dạng folder (Claude Code / agent / API):**
1. Tạo folder `skill-name-kebab-case/`.
2. Đặt `SKILL.md` ở gốc, kèm `scripts/`, `references/`, `assets/` nếu có.
3. Cài skill theo cơ chế của môi trường (folder skills directory hoặc package `.skill`).
4. Trigger thử 1 câu positive để xác nhận kích hoạt.

**Skill single-file (UI editor dán trực tiếp):**
1. Vào Settings → Skills → click `+`.
2. Đặt tên khớp `name` trong YAML.
3. Paste toàn bộ SKILL.md → Save.
4. Trigger thử 1 câu positive.

**Kiểm tra sau khi deploy:** không kích hoạt → soát description (còn dấu tiếng Việt? quá ngắn? thiếu negative blocker?). Script/reference không chạy → kiểm tra đường dẫn tương đối trong folder.

---

## Gotchas (thực tế từ quá trình deploy)

1. **LLM nén/cắt xén khi rewrite** — gotcha nguy hiểm nhất, đặc biệt với model yếu hơn. Khi bảo "rewrite full", model có xu hướng tóm tắt và bỏ bảng/schema/giá trị. Bắt buộc chạy quy trình 3 bước chống nén (kiểm kê → rewrite ràng buộc → diff check) và nhét chỉ thị tự ràng buộc "không tóm tắt, dài quá thì tách references chứ không xoá".
2. **Multi-line description phải dùng folded scalar** `>-` trong YAML. Thiếu sẽ vỡ YAML hoặc bị cắt description.
3. **Tuyệt đối không dấu tiếng Việt trong YAML frontmatter** — chỉ riêng các field YAML. Dấu ă â ê ô ơ ư đ và dấu thanh làm hỏng parsing/activation. Body markdown thì dùng tiếng Việt có dấu bình thường.
4. **Không ký tự góc nhọn trong description** — dễ bị hiểu nhầm là tag và làm hỏng frontmatter.
5. **Không double-quote chưa escape bên trong chuỗi description** — gây lỗi parse YAML.
6. **Skill là folder, không bắt buộc 1 file** — đừng nhồi mọi thứ vào SKILL.md. Tài liệu dài tách `references/` (load động), logic lặp lại tách `scripts/` (chạy offline, không tốn context). Chỉ dùng single-file khi skill thực sự ngắn gọn.
7. **SKILL.md body vẫn nên dưới 500 dòng** dù có folder — giới hạn cho tầng 2 Progressive Disclosure; phần dư offload sang references kèm pointer rõ ràng.
8. **Pointer tới references phải rõ ràng** — nêu rõ "đọc file X khi gặp tình huống Y", nếu không AI sẽ không biết khi nào load.
9. **Nâng cấp phải giữ nguyên `name` cũ**, chỉ tăng `version`. Đổi tên `-v2` tạo skill trùng lặp, gây over-trigger.
10. **Skill quá đơn giản (1 bước) sẽ không kích hoạt** — Claude tự xử lý task 1 bước. Chỉ đóng gói workflow nhiều bước / chuyên biệt.
11. **Patch chắp vá là nguồn lỗi tích luỹ** — luôn rewrite full khi có từ 2 vấn đề trở lên (nhưng rewrite phải bảo toàn nội dung, xem gotcha #1).
12. **Frontmatter 5 trường bị đích Agent Skills từ chối** (12/09/2026): ba skill viết theo mẫu 5 trường nộp vào một skill registry validate chặt đều trượt validate vì `version`/`pattern`/`type` và `description` folded `>-` — phải rút về 2 trường một dòng trước khi nộp (xem "Biến thể theo MÔI TRƯỜNG ĐÍCH"). Gotcha #2 (folded `>-`) chỉ đúng cho đích 5 trường; ở đích 2 trường, `>-` là lỗi.
13. **`description` một dòng không bọc ngoặc kép làm YAML hỏng** (17/11/2026): ba skill (kể cả skill này) có `Vi du: …` trong description trần — `: ` là cú pháp mapping nên parser YAML từ chối; theo tài liệu `claude plugin validate`, skill khi đó nạp với metadata rỗng. `claude plugin validate` bắt được khi đóng gói plugin. Mặc định bọc `description` trong ngoặc kép (xem mẫu ở "Biến thể theo MÔI TRƯỜNG ĐÍCH").

---

## Test cases

**Positive (phải kích hoạt):**
1. "Viết cho mình một skill tự động lưu link vào Notion wiki." → kích hoạt, chạy quy trình 8 bước viết mới.
2. "Nâng cấp skill x-post-optimizer lên version mới, fix lỗi không trigger." → kích hoạt, chạy quy trình nâng cấp 8 bước (rewrite full + diff check).
3. "Review giúp SKILL.md này có đạt chuẩn không, có bị mất nội dung khi rewrite không?" → kích hoạt, chấm Checklist 14 + Diff Check.

**Negative (không kích hoạt):**
1. "Skill trong game RPG là gì vậy?" → không kích hoạt (hỏi kiến thức chung).
2. "Giải thích khái niệm AI agent cho mình nghe." → không kích hoạt (khái niệm chung).
3. "Viết giúp mình một bài blog về AI." → không kích hoạt (nội dung khác, không phải skill file).

---

**Phiên bản:** v2.3.1 — 17/11/2026: bọc description của chính skill trong ngoặc kép (bản trần có `Vi du: …` là YAML hỏng); mẫu frontmatter 2 trường bọc ngoặc kép kèm luật scalar YAML; gotcha #13. v2.3.0 — 12/09/2026: thêm mục "Biến thể theo MÔI TRƯỜNG ĐÍCH" (đích Agent Skills chỉ nhận 2 trường name + description một dòng; gate chặn path ổ đĩa, credential, link gãy, script không UTF-8; review bác cú pháp sandbox claude.ai) + gotcha #12; sửa dòng cấu trúc skill. v2.2.1 — chỉ đổi đóng gói. v2.2.0 — Thêm Quy tắc tối thượng "Bảo toàn nội dung khi rewrite" (restructure khác compress, quy trình 3 bước chống nén kiểm kê/ràng buộc/diff), checklist #14 Diff Check, gotcha #1 về LLM nén/cắt xén, bổ sung loại lỗi Content-loss.
