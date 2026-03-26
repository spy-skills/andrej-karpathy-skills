---
date: 2026-03-26
type: context
task: karpathy-skills-ztk
tags: [ranking, priority, principles, analysis]
---

# Xep hang uu tien 4 nguyen tac Karpathy

## Tieu chi danh gia

| Tieu chi | Mo ta | Thang diem |
|----------|-------|------------|
| Ung dung thuc te | Dung duoc ngay, moi ngay, trong moi project | 1-5 |
| Do pho bien | Bao nhieu % LLM coding sessions gap van de nay | 1-5 |
| Do phuc tap hoc | De internalize va ap dung (5 = rat de) | 1-5 |
| Leverage value | Hieu qua dai han: hoc 1 lan → cai thien vinh vien | 1-5 |

## Danh gia chi tiet

### Nguyen tac 1: Think Before Coding

> "Don't assume. Don't hide confusion. Surface tradeoffs."

| Tieu chi | Diem | Ly do |
|----------|------|-------|
| Ung dung thuc te | 5 | Moi task deu can hieu requirements truoc khi code. Ap dung 100% thoi gian. |
| Do pho bien | 5 | Day la van de #1 cua LLMs — Karpathy noi ro: "make wrong assumptions and just run along." Hau nhu moi session deu gap. |
| Do phuc tap hoc | 4 | Kha de hieu concept, nhung can ky luat de thuc hanh (phai "dung lai" thay vi "lam luon"). |
| Leverage | 5 | Mot lan hoi dung → tranh hang gio rewrite. ROI cuc cao. |
| **TONG** | **19/20** | |

**Phan tich sau:**
- Day la nguyen tac "meta" — no anh huong den tat ca cac nguyen tac khac
- Neu hoi truoc → biet scope ro → tu dong ap dung Simplicity First + Surgical Changes
- Karpathy quote: LLMs "don't manage their confusion, don't seek clarifications"
- EXAMPLES.md cho 2 vi du: Hidden Assumptions (export user data) va Multiple Interpretations (make search faster)
- **Failure mode phổ biến nhất:** LLM giả định sai → code 200 dòng → user nói "không phải ý tôi" → phải viết lại từ đầu

### Nguyen tac 2: Simplicity First

> "Minimum code that solves the problem. Nothing speculative."

| Tieu chi | Diem | Ly do |
|----------|------|-------|
| Ung dung thuc te | 5 | Moi dong code deu co the don gian hon. Ap dung 100%. |
| Do pho bien | 5 | Karpathy: "bloat abstractions... 1000 lines when 100 would do." Rất phổ biến — LLMs yeu thich over-engineering. |
| Do phuc tap hoc | 3 | Kho vi phai "resist" urge to add more. Can kinh nghiem de biet "du" la o dau. Junior devs thuong thich code nhieu. |
| Leverage | 4 | Code don gian → it bug, de maintain, de test. Nhung context-dependent (doi khi can abstraction). |
| **TONG** | **17/20** | |

**Phan tich sau:**
- LLMs co xu huong sinh ra Strategy Pattern, Factory, Builder cho 1 function don gian
- EXAMPLES.md cho 2 vi du tuyet voi: DiscountCalculator (146 dong → 3 dong) va PreferenceManager (40 dong → 5 dong)
- **Lien ket voi Think Before Coding:** Neu hieu ro scope → tu dong don gian hon
- **De nham lan:** "Simplicity" khong phai "lazy" — van can correct logic, chi la minimal code
- "Would a senior engineer say this is overcomplicated?" — cau hoi tu kiem tra tot

### Nguyen tac 3: Surgical Changes

> "Touch only what you must. Clean up only your own mess."

| Tieu chi | Diem | Ly do |
|----------|------|-------|
| Ung dung thuc te | 4 | Rat quan trong khi edit existing code (majority of work), nhung khong ap dung khi viet greenfield. |
| Do pho bien | 4 | Karpathy: "change/remove comments and code they don't sufficiently understand as side effects." Pho bien nhung khong phai moi session. |
| Do phuc tap hoc | 5 | Rat de hieu va ap dung: "chi sua dung cho can sua." Rule ro rang. |
| Leverage | 3 | Giam noise trong diffs, nhung impact nho hon 2 nguyen tac tren. Chu yeu cai thien review experience. |
| **TONG** | **16/20** | |

**Phan tich sau:**
- Day la nguyen tac "do no harm" — defensive, khong tao gia tri moi
- EXAMPLES.md cho 2 vi du: Drive-by Refactoring (fix bug nhung refactor ca function) va Style Drift (them logging nhung doi quote style)
- **Dac biet quan trong cho:** PR reviews, legacy codebases, team projects
- **Test don gian:** "Moi dong thay doi co trace truc tiep den yeu cau user khong?"
- Nguyen tac nay de nhat de tu dong hoa bang linting/diffing tools

### Nguyen tac 4: Goal-Driven Execution

> "Define success criteria. Loop until verified."

| Tieu chi | Diem | Ly do |
|----------|------|-------|
| Ung dung thuc te | 4 | Cuc ky huu ich cho complex tasks, nhung overkill cho simple changes. |
| Do pho bien | 3 | Khong phai van de pho bien nhat — nhieu LLM sessions la simple Q&A hoac small edits. |
| Do phuc tap hoc | 2 | Kho nhat de master. Can ky nang decompose task, viet AC, va set up verification loops. Doi hoi kinh nghiem. |
| Leverage | 5 | Khi master duoc → autonomous execution power. Karpathy: "give it success criteria and watch it go." Day la nguyen tac unlock self-correction loop. |
| **TONG** | **14/20** | |

**Phan tich sau:**
- Day la nguyen tac "advanced" — tao gia tri lon nhat nhung kho ap dung nhat
- EXAMPLES.md cho 3 vi du: Vague vs Verifiable (fix auth), Multi-Step with Verification (rate limiting), Test-First (sorting bug)
- **Key insight cua Karpathy:** "LLMs are exceptionally good at looping until they meet specific goals"
- Transform: "Fix the bug" → "Write test that reproduces it, make it pass"
- **Lien ket nguoc voi Think Before Coding:** Can "think" de dinh nghia success criteria truoc
- Nguyen tac nay la "force multiplier" — khi co good criteria, LLM tu loop va fix

## Bang tong hop xep hang

| Hang | Nguyen tac | Diem | Key Strength |
|------|-----------|------|-------------- |
| 🥇 1 | **Think Before Coding** | 19/20 | Meta-principle: anh huong tat ca nguyen tac khac. ROI cao nhat. |
| 🥈 2 | **Simplicity First** | 17/20 | Giai quyet van de #1 cua LLM code: over-engineering. |
| 🥉 3 | **Surgical Changes** | 16/20 | De hoc nhat. Giam noise ngay lap tuc trong moi PR. |
| 4 | **Goal-Driven Execution** | 14/20 | Leverage cao nhat nhung kho master. Advanced skill. |

## Recommendation: Thu tu hoc

### Phase 1: Foundation (hoc ngay, ap dung ngay)
1. **Think Before Coding** — Hoc TRUOC TIEN vi no la "gate" cho moi thu khac. Neu khong hoi truoc → 3 nguyen tac con lai deu vo ich vi ban dang giai bai toan sai.
2. **Surgical Changes** — Hoc THU HAI vi no don gian nhat: "chi sua dung cho can sua." Ap dung duoc ngay khong can suy nghi nhieu.

### Phase 2: Mastery (can luyen tap)
3. **Simplicity First** — Hoc SAU khi da co thoi quen Think + Surgical. Can kinh nghiem de biet "bao nhieu la du." Lien quan den taste va judgement.
4. **Goal-Driven Execution** — Hoc CUOI CUNG vi no la advanced skill. Can tat ca 3 nguyen tac tren lam nen tang. Nhung khi master → unlock autonomous coding power.

### Tai sao thu tu nay?

```
Think Before Coding  ←── Foundation: Hieu dung bai toan
         ↓
Surgical Changes     ←── Discipline: Lam dung pham vi
         ↓
Simplicity First     ←── Taste: Lam vua du, khong thua
         ↓
Goal-Driven Execution ←── Mastery: Tu loop, tu verify, tu complete
```

**Analogy:** Giong nhu hoc lai xe:
1. Think = Hieu luat giao thong (prerequisites)
2. Surgical = Giu lan duong (rule-based, de hoc)
3. Simplicity = Chon duong ngan nhat (can kinh nghiem)
4. Goal-Driven = Tu lai autopilot (advanced, can tat ca tren)

## Takeaways

1. **Think Before Coding la #1 khong ban cai** — meta-principle, ROI cao nhat, pho bien nhat
2. **Goal-Driven co leverage cao nhat** nhung can foundation tu 3 nguyen tac kia
3. **Surgical Changes la "quick win"** — de hoc, thay hieu qua ngay trong code review
4. **4 nguyen tac co relationship hierarchy**, khong doc lap — hoc theo thu tu moi hieu qua
5. **Karpathy's key insight:** "Don't tell it what to do, give it success criteria" — day la nguyen tac #4 nhung chi hoat dong khi #1-3 da vung
