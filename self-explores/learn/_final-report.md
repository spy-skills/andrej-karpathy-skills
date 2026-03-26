---
date: 2026-03-26
type: context
task: karpathy-skills-8t7
tags: [final-report, summary, recommendations, roadmap]
---

# Bao Cao Nghien Cuu: Andrej Karpathy Skills Plugin

## Tong Quan Du An

**Repo:** forrestchang/andrej-karpathy-skills
**Loai:** Claude Code plugin — documentation-only
**Noi dung:** 4 nguyen tac hanh vi (behavioral guidelines) giam loi LLM coding
**Nguon:** Andrej Karpathy tweet ve LLM coding pitfalls
**License:** MIT

### Cau truc repo

| File | Lines | Vai tro |
|------|-------|---------|
| SKILL.md | 68 | Source of truth — skill definition |
| CLAUDE.md | 98 | Project meta + guidelines copy |
| README.md | 162 | Docs + expanded guidelines |
| EXAMPLES.md | 523 | 7 before/after code examples |
| plugin.json | 12 | Plugin manifest |
| marketplace.json | 30 | Marketplace listing |

**Constraint chinh:** 3-way sync giua CLAUDE.md, SKILL.md, README.md

---

## Xep Hang 4 Nguyen Tac

### Bang tong hop

| Hang | Nguyen tac | Ranking Score | Anthropic Score | Key Strength |
|------|-----------|---------------|-----------------|--------------|
| 1 | **Think Before Coding** | 19/20 | 8.30/10 | Meta-principle, ROI cao nhat |
| 2 | **Simplicity First** | 17/20 | 7.55/10 | Giai quyet #1 LLM weakness |
| 3 | **Surgical Changes** | 16/20 | 8.75/10 | Ro rang nhat, de audit nhat |
| 4 | **Goal-Driven Execution** | 14/20 | 6.75/10 | Leverage cao nhat khi mastered |

**Ghi chu:** Ranking score (ung dung + pho bien + de hoc + leverage) va Anthropic score (completeness + usability + modularity + safety) danh gia 2 khia canh khac nhau:
- Ranking = **nen hoc truoc** (practical priority)
- Anthropic = **chat luong viet** (quality of the guideline itself)

### Nhan xet dac biet

- **Surgical Changes** co Anthropic score CAO NHAT (8.75) du ranking chi #3 — vi nguyen tac nay duoc VIET TOT NHAT (ro rang, concrete, de verify), nhung ung dung hep hon 2 nguyen tac tren
- **Goal-Driven** co Anthropic score THAP NHAT (6.75) du leverage cao — vi KHO AP DUNG, thieu template/bounds, va chi hoat dong khi co test infrastructure

---

## Deep Dive Summary

### 1. Think Before Coding (8.30/10)

**Tagline:** "Don't assume. Don't hide confusion. Surface tradeoffs."

**4 rules:** State assumptions | Present interpretations | Push back | Stop when confused

**Strengths:**
- Meta-principle anh huong toan bo 3 nguyen tac con lai
- 1 cau hoi dung = tiet kiem hang gio rewrite
- Universal — moi language, moi domain

**Gaps:**
- Thieu threshold: khi nao hoi vs tu quyet dinh?
- Thieu "propose default" pattern: khi user cung khong biet → suggest + confirm
- Khong address "just do it" override

**Key example:** "Export user data" (7 tu) → 4 hidden assumptions (scope, format, fields, volume)

---

### 2. Simplicity First (7.55/10)

**Tagline:** "Minimum code that solves the problem. Nothing speculative."

**5 rules:** No extra features | No premature abstraction | No speculative flexibility | No impossible-scenario handling | Rewrite 200→50

**Strengths:**
- Truc tiep chong LLM over-engineering (bloat factor 49x trong example)
- Self-check: "Would a senior engineer say this is overcomplicated?"
- Quantifiable: dong code tru, khong tru

**Gaps:**
- "No error handling for impossible scenarios" — NGUY HIEM neu judgment sai
- Thieu threshold: khi nao abstraction la DUNG?
- Tension voi SOLID/Clean Code truyen thong (intentional)

**Key example:** DiscountCalculator — Strategy+Factory pattern 146 dong → 1 function 3 dong

---

### 3. Surgical Changes (8.75/10)

**Tagline:** "Touch only what you must. Clean up only your own mess."

**6 rules:** 4 rules "editing existing" + 2 rules "own orphans"

**Strengths:**
- **Diem Anthropic cao nhat** — ro rang, de audit, khong mo ho
- Self-check: "Every changed line traces to request"
- Nuanced: phan biet YOUR orphans vs pre-existing dead code

**Gaps:**
- Security tension: "match existing style" vs "existing style insecure"
- Formatting tools (prettier/black) auto-change ca file → conflict

**Key example:** Fix empty email bug — LLM sua 20+ dong (them docstring, username validation) → chi can 6 dong

---

### 4. Goal-Driven Execution (6.75/10)

**Tagline:** "Define success criteria. Loop until verified."

**2 mechanisms:** Transform imperative→declarative | Multi-step with verification

**Strengths:**
- Unlock autonomous LLM execution (Karpathy's core insight)
- Test-first = bang chung concrete
- Incremental delivery = less risk

**Gaps:**
- **Kho ap dung nhat** — can skill decompose + write AC + design tests
- Test-centric bias — khong address non-testable work
- Missing loop bounds va escalation rules

**Key insight:** "Don't tell it what to do, give it success criteria and watch it go" — Karpathy

---

## Relationships Giua 4 Nguyen Tac

```
Think Before Coding     ←── Foundation: WHAT (hieu yeu cau)
         ↓
Surgical Changes        ←── Discipline: WHERE (pham vi thay doi)
         ↓
Simplicity First        ←── Taste: HOW MUCH (vua du, khong thua)
         ↓
Goal-Driven Execution   ←── Mastery: VERIFY (loop until done)
```

**4 nguyen tac KHONG doc lap** — chung tao thanh pipeline:
1. Think → biet CAN LAM GI
2. Surgical → biet LAM O DAU
3. Simplicity → biet LAM BAO NHIEU
4. Goal-Driven → biet KHI NAO XONG

---

## Recommendations

### A. Cho nguoi dung plugin

**Lo trinh hoc tap:**

| Phase | Nguyen tac | Thoi gian | Focus |
|-------|-----------|-----------|-------|
| Week 1 | Think Before Coding | 3-5 ngay | Hoi truoc moi task. Tap list assumptions. |
| Week 2 | Surgical Changes | 3-5 ngay | Review moi diff: "dong nay trace den request?" |
| Week 3 | Simplicity First | 5-7 ngay | Viet xong → review: "co the ngan hon?" |
| Week 4+ | Goal-Driven Execution | Ongoing | Viet AC truoc code. Tap TDD workflow. |

**Quick wins (ap dung ngay):**
1. Them CLAUDE.md vao project → Think + Surgical + Simplicity tu dong apply
2. Review diff truoc khi commit → Surgical Changes check
3. Hoi "can bao nhieu dong?" truoc khi bat dau → Simplicity check

### B. Cho tac gia plugin (improvement suggestions)

**Priority 1 — Safety fix:**
- Simplicity First rule "No error handling for impossible scenarios" → them disclaimer: "Except security-critical code (auth, input validation, financial)"

**Priority 2 — Usability:**
- Think Before Coding → them "propose default" pattern: "When user can't answer → suggest default, ask to confirm"
- Goal-Driven → them template format cho success criteria (Given/When/Then)
- Goal-Driven → them loop bounds: "Max 3 iterations before asking user"

**Priority 3 — Completeness:**
- Them EXAMPLES.md vao sync constraint (hien chi noi 3 file)
- Them "when NOT to apply" section cho moi nguyen tac
- Them "override mechanism" khi user explicitly muon skip guideline

### C. Cho Anthropic ecosystem

**Tuong thich voi:**
- Claude Code system prompt patterns
- Anthropic's "ask for clarification" design
- Chain-of-Thought + Step-by-Step reasoning
- Claude's existing safety alignment

**Bo sung cho:**
- Cursor / Copilot coding workflows
- PR review automation
- Code generation quality metrics

---

## Metrics Tong Hop

| Metric | Gia tri |
|--------|---------|
| Tong so files phan tich | 7 |
| Tong so dong code | ~900 |
| So nguyen tac | 4 |
| So examples (EXAMPLES.md) | 7 |
| Average Anthropic Score | 7.84/10 |
| Highest scored principle | Surgical Changes (8.75) |
| Most practical principle | Think Before Coding (19/20 ranking) |
| Most advanced principle | Goal-Driven Execution |
| Key constraint | 3-way sync (CLAUDE.md ↔ SKILL.md ↔ README.md) |

---

## Ket Luan

Plugin nay giai quyet **dung van de** ma Karpathy chi ra: LLMs over-engineer, khong hoi, khong verify. 4 nguyen tac tao thanh **pipeline hoan chinh** tu "hieu yeu cau" den "verify xong."

**Diem manh lon nhat:** Practical, immediately applicable, well-exemplified.
**Diem yeu lon nhat:** Usability cua Goal-Driven (kho master), safety concern cua Simplicity (skip error handling).

**One-line summary:** "Hoi truoc, lam vua du, chi sua dung cho, loop cho den khi pass."
