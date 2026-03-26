---
date: 2026-03-26
type: context
task: karpathy-skills-10n
tags: [overview, repo-analysis, skill-structure]
---

# Tong quan repo andrej-karpathy-skills

## 1. Repo Summary

**Loai project:** Claude Code plugin — documentation-only, khong co build/test/lint.
**Tac gia:** forrestchang
**License:** MIT
**Nguon cam hung:** Andrej Karpathy tweet ve LLM coding pitfalls (x.com/karpathy/status/2015883857489522876)
**Muc dich:** Cung cap 4 nguyen tac hanh vi (behavioral guidelines) giup LLM viet code tot hon.

## 2. File Map — Toan bo components

| # | File | Lines | Vai tro | Domain |
|---|------|-------|---------|--------|
| 1 | `CLAUDE.md` | 98 | Guidelines + project instructions | Core Skill + Meta |
| 2 | `skills/karpathy-guidelines/SKILL.md` | 68 | Skill definition (YAML frontmatter + guidelines) | Core Skill |
| 3 | `README.md` | 162 | Docs: gioi thieu, install, chi tiet 4 nguyen tac | Documentation |
| 4 | `EXAMPLES.md` | 523 | Before/after code examples cho 4 nguyen tac | Documentation |
| 5 | `.claude-plugin/plugin.json` | 12 | Plugin manifest — declares skill path | Distribution |
| 6 | `.claude-plugin/marketplace.json` | 30 | Marketplace listing metadata | Distribution |
| 7 | `.gitignore` | 3 | Git ignore (.dolt/, *.db) | Config |

**Tong cong:** 7 files chính (khong tinh .git/, .beads/, self-explores/)

## 3. Phan nhom theo Domain

### Domain A: Core Skill (Noi dung san pham)

**SKILL.md** — Nguon su that chinh (source of truth) cho skill content:
- YAML frontmatter: `name: karpathy-guidelines`, `license: MIT`
- Description: "Behavioral guidelines to reduce common LLM coding mistakes..."
- Trigger condition: "Use when writing, reviewing, or refactoring code"
- Content: 4 nguyen tac (Think Before Coding, Simplicity First, Surgical Changes, Goal-Driven Execution)
- 68 dong, clean, khong co project-specific metadata

**CLAUDE.md** — Dual-purpose file:
- **Phan 1 (dong 1-29):** Project metadata — repo structure, key constraint, plugin system
- **Phan 2 (dong 31-97):** Guidelines — DUPLICATE cua SKILL.md content (can SYNC)
- 98 dong tong

### Domain B: Documentation

**README.md** — Expanded version:
- Karpathy quotes lam motivation
- Table mapping 4 nguyen tac → van de giai quyet
- Chi tiet moi nguyen tac (voi bullet points mo rong)
- 2 phuong phap cai dat: Plugin (recommended) + curl CLAUDE.md
- Customization guide
- Tradeoff note
- 162 dong

**EXAMPLES.md** — Minh hoa thuc te:
- 7 vi du before/after (2 cho Think, 2 cho Simplicity, 2 cho Surgical, 3 cho Goal-Driven)
- Python code diffs
- Anti-Patterns Summary table
- Key Insight section ve timing of complexity
- 523 dong — file lon nhat trong repo

### Domain C: Distribution (Plugin system)

**plugin.json:**
```json
{
  "name": "andrej-karpathy-skills",
  "skills": ["./skills/karpathy-guidelines"]
}
```
- Path mapping: `skills` array → `./skills/karpathy-guidelines` → `SKILL.md`
- Keywords: guidelines, best-practices, coding, karpathy

**marketplace.json:**
```json
{
  "name": "karpathy-skills",
  "id": "karpathy-skills",
  "plugins": [{ "name": "andrej-karpathy-skills", "source": "./" }]
}
```
- Install flow: `/plugin marketplace add forrestchang/andrej-karpathy-skills`
- Then: `/plugin install andrej-karpathy-skills@karpathy-skills`

## 4. Sync Constraints (QUAN TRONG)

### 3-way sync: CLAUDE.md ↔ SKILL.md ↔ README.md

Ba file chua **cung noi dung 4 nguyen tac** nhung o format khac nhau:

| Aspect | CLAUDE.md | SKILL.md | README.md |
|--------|-----------|----------|-----------|
| Format | Compact bullets | Compact bullets | Expanded prose + tables |
| Frontmatter | Khong | Co (name, desc, license) | Khong |
| Extra content | Project meta (structure, constraint, plugin) | Khong | Install, Karpathy quotes, customization |
| Guidelines text | **DONG Y CHINH XAC** voi SKILL.md | **SOURCE OF TRUTH** | **EXPANDED VERSION** (giu y nghia, them context) |

### Kiem tra sync hien tai:

**CLAUDE.md vs SKILL.md:** ✅ DONG BO — Noi dung 4 nguyen tac GIONG NHAU tu chu (dong 33-97 CLAUDE.md = dong 8-68 SKILL.md, tru line cuoi "These guidelines are working if...")

**Khac biet nho:**
- SKILL.md bat dau voi "derived from [Andrej Karpathy's observations](...)" — co link
- CLAUDE.md khong co link tren phan guidelines
- CLAUDE.md co them dong cuoi: "These guidelines are working if: fewer unnecessary changes..."
- SKILL.md KHONG co dong cuoi nay

**README.md vs SKILL.md:** ✅ DONG BO VE Y NGHIA nhung EXPANDED:
- Moi nguyen tac duoc viet dai hon, co headings phu
- "The test:" duoc tach thanh bold line rieng
- Table format cho "Transform tasks" thay vi bullet list
- Them context xung quanh moi diem

### Rui ro sync:
- Neu sua 1 file ma quen 2 file con lai → guidelines bi inconsistent
- CLAUDE.md dong cuoi co them "working if" metrics — SKILL.md thieu → user cai plugin se khong thay phan nay
- README co expand text → kho verify 1:1 sync, chi verify y nghia

## 5. Architecture Assessment

### Strengths
1. **Don gian, toan dien:** 1 skill, 4 nguyen tac, tat ca trong < 900 dong tong
2. **Multiple install paths:** Plugin marketplace + curl CLAUDE.md
3. **Practical examples:** EXAMPLES.md minh hoa cu the voi code diffs
4. **Self-documenting:** CLAUDE.md vua la huong dan cho contributors vua la san pham

### Limitations
1. **Single skill architecture:** Chi co 1 skill (`karpathy-guidelines`) — khong modular (VD: khong the cai rieng "Simplicity First")
2. **3-way sync burden:** Moi thay doi phai update 3 files — error-prone
3. **No versioning beyond git:** Khong co CHANGELOG, khong co semver tags
4. **No automated sync check:** Khong co script/test kiem tra 3 file dong bo
5. **EXAMPLES.md khong trong sync constraint:** CLAUDE.md noi "3 places that must stay in sync" nhung EXAMPLES.md cung reference 4 nguyen tac — neu sua ten/noi dung nguyen tac, EXAMPLES.md cung can update

### Dependency Graph

```
marketplace.json
  └→ plugin.json
       └→ ./skills/karpathy-guidelines/SKILL.md  (source of truth)
            ↕ sync
       CLAUDE.md (project meta + guidelines copy)
            ↕ sync (expanded)
       README.md (docs + guidelines expanded)
            ↕ references
       EXAMPLES.md (code examples cho 4 nguyen tac)
```

## 6. 4 Nguyen tac — Quick Reference

| # | Ten | Tagline | Core Message |
|---|-----|---------|-------------- |
| 1 | Think Before Coding | Don't assume. Surface tradeoffs. | Hoi truoc khi lam. Neu mo ho → dung lai, hoi. |
| 2 | Simplicity First | Minimum code. Nothing speculative. | Khong thua. Khong over-engineer. 200 dong → 50 dong. |
| 3 | Surgical Changes | Touch only what you must. | Chi sua dung cho can sua. Khong "improve" code xung quanh. |
| 4 | Goal-Driven Execution | Define success criteria. Loop until verified. | Bien "lam X" → "test X, loop cho den khi pass". |

## 7. Takeaways

1. **Repo nay la documentation-only plugin** — san pham chinh la TEXT (guidelines), khong phai code
2. **3-way sync la constraint lon nhat** — moi thay doi guidelines phai update CLAUDE.md + SKILL.md + README.md
3. **EXAMPLES.md la file lon nhat (523 dong)** va la tai san co gia tri nhat vi minh hoa cu the
4. **Plugin system hoat dong:** marketplace.json → plugin.json → SKILL.md path chain
5. **Cai thien tiem nang:** them sync check script, tach EXAMPLES.md cung vao sync constraint, version tags
