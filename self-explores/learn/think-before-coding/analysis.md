---
date: 2026-03-26
type: context
task: karpathy-skills-3ld
tags: [deep-dive, think-before-coding, anthropic-standards]
---

# Deep Dive: Think Before Coding

## 1. Logic Core

### Tagline
> "Don't assume. Don't hide confusion. Surface tradeoffs."

### 4 Rules

| # | Rule | Trigger Condition | Expected Behavior |
|---|------|-------------------|-------------------|
| 1 | State assumptions explicitly | Bat ky luc nao uncertain | Liet ke ro assumptions, hoi user confirm |
| 2 | Present multiple interpretations | Khi yeu cau mo ho, >1 cach hieu | Trinh bay cac cach hieu, HOI user chon |
| 3 | Push back when simpler exists | Khi co approach don gian hon | Noi ro: "co cach don gian hon..." |
| 4 | Stop when confused | Khong hieu 1 phan nao do | DUNG LAI. Goi ten cai khong hieu. HOI. |

### Core Philosophy
- **Proactive communication** — LLM phai chu dong surface van de, khong doi user phat hien
- **Explicit > implicit** — Moi assumption phai duoc noi ra, khong duoc giau
- **Ask before act** — Hoi truoc khi lam, khong phai lam roi hoi

### Nguon goc — Karpathy Quote
> "The models make wrong assumptions on your behalf and just run along with them without checking. They don't manage their confusion, don't seek clarifications, don't surface inconsistencies, don't present tradeoffs, don't push back when they should."

**Phan tich quote:**
- 6 van de duoc liet ke: wrong assumptions, unmanaged confusion, no clarification, no inconsistency surfacing, no tradeoffs, no pushback
- Nguyen tac nay GOI GON thanh 4 rules bao phu ca 6 van de
- Mapping: assumptions (rule 1,2) + confusion (rule 4) + clarification (rule 1,4) + inconsistencies (rule 2) + tradeoffs (rule 2,3) + pushback (rule 3)

## 2. Example Analysis

### Example 1: Hidden Assumptions (EXAMPLES.md dong 9-55)

**Request:** "Add a feature to export user data"

**LLM lam sai:** Viet ngay ham `export_users()` voi:
- Gia dinh export ALL users (khong hoi pagination, privacy)
- Gia dinh file location (hardcode 'users.json')
- Gia dinh format (JSON + CSV)
- Gia dinh fields (id, email, name)

**Nen lam:** Hoi 4 cau truoc:
1. Scope: All hay filtered? (privacy concern)
2. Format: Download, background job, hay API?
3. Fields: Nhung field nao? (sensitive data?)
4. Volume: Bao nhieu users? (anh huong approach)

**Insight:** 1 yeu cau ngan (7 tu) chua **it nhat 4 assumptions an**. Neu khong surface → code 30 dong bi sai direction → phai rewrite.

### Example 2: Multiple Interpretations (EXAMPLES.md dong 57-93)

**Request:** "Make the search faster"

**LLM lam sai:** Pick "response time" va implement caching + async + indexes (200 dong)

**Nen lam:** Trinh bay 3 cach hieu:
1. Faster response time (latency) → indexes, cache
2. More concurrent searches (throughput) → async, pooling
3. Faster perceived speed (UX) → partial results, progressive loading

**Insight:** Moi cach hieu dan den approach HOAN TOAN KHAC NHAU. Chon sai → lang phi ca ngay coding.

## 3. Danh gia theo Anthropic Standards

### Completeness: 7/10

**Manh:**
- Bao quat 4 loai van de chinh: assumptions, interpretations, simplicity pushback, confusion
- Mapping tot voi Karpathy's 6 complaints

**Thieu:**
- Khong noi ro **KHI NAO** nen hoi vs khi nao nen tu quyet dinh (threshold khong ro)
- Khong co huong dan **FORMAT** hoi nhu the nao (bullet list? matrix? prose?)
- Khong co huong dan cho truong hop user noi "just do it, don't ask" (override mechanism)
- Khong address truong hop user cung khong biet cau tra loi → nen propose default + confirm

**Goi y cai thien:**
- Them "default proposal" pattern: "Neu user khong specify → propose default, ask to confirm"
- Them threshold: "Hoi khi impact > 1 hour of work hoac > 50 lines of code"

### Usability: 9/10

**Manh:**
- 4 rules cuc ky de nho: assumptions, interpretations, pushback, confusion
- Tagline "Don't assume. Don't hide confusion. Surface tradeoffs." = perfect mnemonic
- Examples trong EXAMPLES.md rat cu the va relatable
- Ap dung duoc ngay, khong can config hay setup

**Thieu:**
- Khong co "bad example of asking" (hoi qua nhieu, hoi cai hien nhien cung lam giam UX)
- Khong noi ro do granular cua questions (hoi tung cai vs hoi batch)

### Modularity: 8/10

**Manh:**
- Doc lap hoan toan — co the ap dung rieng ma khong can 3 nguyen tac kia
- Khong depend on bat ky tool, framework, hay language nao
- Hoat dong cho moi loai task (code, design, planning, debugging)

**Thieu:**
- Lien ket chat voi #4 Goal-Driven (dinh nghia success criteria cung la 1 dang "think before code")
- Co the overlap voi #2 Simplicity (pushback khi co approach don gian hon)

### Safety: 9/10

**Manh:**
- Nguyen tac nay BAN CHAT la safety mechanism — prevent wrong direction
- Giup detect privacy concerns (VD: export user data → "which fields? sensitive data?")
- Giup detect security risks (VD: "who can access this export?")
- Giam risk cua "silent failures" — van de duoc surface som

**Thieu:**
- Khong explicitly mention security/privacy review nhu 1 dang "assumption to surface"
- Khong co checklist cho sensitive domains (finance, health, auth)

### Tong hop diem

| Tieu chi | Diem | Weight | Weighted |
|----------|------|--------|----------|
| Completeness | 7/10 | 25% | 1.75 |
| Usability | 9/10 | 30% | 2.70 |
| Modularity | 8/10 | 20% | 1.60 |
| Safety | 9/10 | 25% | 2.25 |
| **TONG (weighted)** | | | **8.3/10** |

## 4. Uu diem

1. **Meta-principle:** Anh huong tich cuc den tat ca 3 nguyen tac con lai
2. **ROI cao nhat:** 1 cau hoi dung luc = tiet kiem hang gio rewrite
3. **Universal:** Ap dung cho moi language, framework, domain
4. **Easy to audit:** De kiem tra xem LLM co "think" truoc khi code khong (xem co hoi khong)
5. **Tagline memorable:** "Don't assume. Don't hide confusion. Surface tradeoffs."

## 5. Nhuoc diem

1. **Khong co threshold:** Khi nao hoi vs tu quyet dinh? Hoi qua nhieu → user frustrated
2. **Missing "propose default" pattern:** Khi user khong biet → nen propose, khong chi hoi
3. **Khong address power dynamics:** User noi "just do it" → guideline khong noi cach xu ly
4. **Khong co format guidance:** List questions nhu the nao? Prioritize cau hoi nao truoc?
5. **Tinh trang overuse:** Neu follow 100% → moi task deu bat dau bang 4 cau hoi → slow down trivial tasks

## 6. Tich hop tiem nang

### Voi Claude Code
- **System prompt injection:** Them vao CLAUDE.md = tu dong ap dung moi session
- **Pre-implementation hook:** Truoc khi goi Edit/Write tool → check assumptions
- **Confidence scoring:** Tu danh gia "confidence level" truoc khi code → low confidence = hoi

### Voi cac nguyen tac khac
- **Think + Simplicity:** "Hieu ro yeu cau" → "lam vua du"
- **Think + Surgical:** "Hieu ro scope" → "chi sua dung pham vi"
- **Think + Goal-Driven:** "Hieu ro muc tieu" → "dinh nghia AC" → "loop until pass"

### Voi Prompt Engineering
- Tuong thich voi: Chain-of-Thought, Step-by-Step Reasoning
- Tuong thich voi: Anthropic's "ask for clarification" pattern
- Bo sung cho: "Think step by step" bang "Think about WHAT to do before HOW to do it"

## 7. Technical Notes

- Nguyen tac nay **khong phai** "always ask" — no la "surface what you don't know"
- Effective khi combine voi confidence threshold: high confidence → do it, low → ask
- Best practice: Batch questions (hoi tat ca cung luc, khong hoi 1 cai roi doi)
- Anti-pattern: Hoi cai hien nhien (VD: "Ban muon function return value khong?" — obvious la co)

## 8. Takeaways

1. **#1 principle** theo ranking — foundation cho moi thu khac
2. **8.3/10 weighted score** — high usability/safety, moderate completeness
3. **Gap lon nhat:** threshold + "propose default" pattern → co the cai thien
4. **Strength lon nhat:** Universal applicability, easy to adopt, high ROI
5. **Lien ket quan trong nhat:** Voi Goal-Driven Execution — "think" de "define goals"
