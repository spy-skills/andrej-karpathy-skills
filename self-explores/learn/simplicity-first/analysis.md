---
date: 2026-03-26
type: context
task: karpathy-skills-899
tags: [deep-dive, simplicity-first, anthropic-standards]
---

# Deep Dive: Simplicity First

## 1. Logic Core

### Tagline
> "Minimum code that solves the problem. Nothing speculative."

### 5 Rules

| # | Rule | Anti-pattern it prevents |
|---|------|------------------------|
| 1 | No features beyond what was asked | Feature creep, gold plating |
| 2 | No abstractions for single-use code | Premature abstraction (Strategy, Factory, Builder for 1 case) |
| 3 | No "flexibility" or "configurability" not requested | YAGNI violation, speculative generality |
| 4 | No error handling for impossible scenarios | Defensive programming overkill |
| 5 | If 200 lines could be 50, rewrite it | Code bloat, verbosity |

### Self-check question
> "Would a senior engineer say this is overcomplicated? If yes, simplify."

### Core Philosophy
- **YAGNI (You Aren't Gonna Need It)** — viet code cho hom nay, khong phai ngay mai
- **Simplicity != laziness** — code don gian van phai CORRECT, chi la MINIMAL
- **Complexity is a cost** — moi dong code them = them bug potential, them maintenance

### Nguon goc — Karpathy Quote
> "They really like to overcomplicate code and APIs, bloat abstractions, don't clean up dead code... implement a bloated construction over 1000 lines when 100 would do."

**Phan tich:**
- "overcomplicate" — khong phai sai, ma la THUA
- "bloat abstractions" — layer khong can thiet
- "1000 lines when 100 would do" — 10x bloat factor la common

## 2. Example Analysis

### Example 1: Over-abstraction (EXAMPLES.md dong 99-161)

**Request:** "Add a function to calculate discount"

**LLM lam sai (146 dong):**
- Abstract class `DiscountStrategy` voi `calculate()` method
- 2 subclasses: `PercentageDiscount`, `FixedDiscount`
- Dataclass `DiscountConfig` voi `min_purchase`, `max_discount`
- Class `DiscountCalculator` voi `apply_discount()` method
- Can 30+ dong setup code de su dung

**Nen lam (3 dong):**
```python
def calculate_discount(amount: float, percent: float) -> float:
    return amount * (percent / 100)
```

**Insight:**
- Bloat factor: 146 dong → 3 dong = **49x**
- LLM ap dung Strategy Pattern — dung trong textbook nhung WRONG TIMING
- Key: "When to add complexity: Only when you actually need multiple discount types"
- Pattern: **Premature abstraction** — add layer truoc khi co nhu cau

### Example 2: Speculative Features (EXAMPLES.md dong 163-221)

**Request:** "Save user preferences to database"

**LLM lam sai (40 dong):**
- Class `PreferenceManager` voi cache, validator, notify
- Method `save()` voi params: merge, validate, notify
- Optional caching layer
- Optional notification system
- Optional validation

**Nen lam (5 dong):**
```python
def save_preferences(db, user_id: int, preferences: dict):
    db.execute("UPDATE users SET preferences = ? WHERE id = ?",
               (json.dumps(preferences), user_id))
```

**Insight:**
- Bloat factor: 40 dong → 5 dong = **8x**
- 3 features khong ai hoi: caching, validation, notifications
- Key: "Add later if needed: Caching (when performance matters), validation (when bad data appears)"
- Pattern: **Speculative generality** — build for hypothetical future

## 3. Danh gia theo Anthropic Standards

### Completeness: 8/10

**Manh:**
- 5 rules bao quat cac dang over-engineering pho bien nhat
- Self-check question ("Would a senior engineer...") la tool tot
- "If 200 lines could be 50" — concrete, quantifiable

**Thieu:**
- Khong address khi nao NEN add complexity (threshold)
- Khong noi ve refactoring path: "sau nay khi can, refactor the nao?"
- Khong phan biet greenfield vs adding to existing complex codebase

### Usability: 8/10

**Manh:**
- 5 rules de nho, moi rule bat dau bang "No..."
- Examples cuc ky illustrative — 146 dong → 3 dong la shocking
- Self-check question la actionable

**Thieu:**
- "Minimum code" la subjective — minimum cho ai? Junior vs Senior co khac nhau
- Khong co metric: bao nhieu dong/classes la "too many"?
- Khong co "code smell" checklist cu the

### Modularity: 7/10

**Manh:**
- Doc lap — co the ap dung rieng
- Khong depend on language hay framework

**Thieu:**
- Overlap voi Think Before Coding rule 3 ("If a simpler approach exists, say so")
- Overlap voi Surgical Changes (khong them code thua)
- Khi combine voi team conventions → co the conflict (team muon abstraction, guideline noi khong)

### Safety: 7/10

**Manh:**
- It code = it bug surface area
- Don gian = de review = de catch security issues
- Khong them unnecessary dependencies = giam supply chain risk

**Thieu:**
- "No error handling for impossible scenarios" — NGUY HIEM neu judgment sai
  - VD: LLM nghi scenario "impossible" nhung thuc te khong phai
  - Ai quyet dinh scenario nao "impossible"? Can context
- Khong noi ve security-related code (auth, input validation) — day KHONG NEN simplified
- Risk: LLM interpret "simplicity" nhu "skip validation" → security hole

### Tong hop diem

| Tieu chi | Diem | Weight | Weighted |
|----------|------|--------|----------|
| Completeness | 8/10 | 25% | 2.00 |
| Usability | 8/10 | 30% | 2.40 |
| Modularity | 7/10 | 20% | 1.40 |
| Safety | 7/10 | 25% | 1.75 |
| **TONG (weighted)** | | | **7.55/10** |

## 4. Uu diem

1. **Truc tiep giai quyet #1 LLM weakness:** Over-engineering la van de phổ biến nhất trong LLM code
2. **Quantifiable:** "200 lines → 50 lines" cho phep do luong
3. **Examples la excellent:** Bloat factor 49x va 8x rat thuyet phuc
4. **Self-check question:** "Would a senior engineer..." la heuristic tot
5. **YAGNI enforcement:** Ngan chan speculative features hieu qua

## 5. Nhuoc diem

1. **"Impossible scenarios" la dangerous:** LLM co the skip quan trong error handling
2. **Khong co threshold:** Khi nao abstraction la DUNG? (VD: 3 discount types → nen co Strategy)
3. **Khong address existing complexity:** Khi codebase DA co abstractions → lam theo style cu hay simplify?
4. **Subjective "minimum":** "Minimum" cho junior ≠ "minimum" cho senior
5. **Missing refactoring guidance:** "Add later when needed" → nhung HOW? DFD (Design for Deletability)?

## 6. So sanh voi industry practices

| Source | Stance | Alignment |
|--------|--------|-----------|
| YAGNI (XP) | Don't add functionality until needed | ✅ Hoan toan align |
| KISS (Unix) | Keep it simple, stupid | ✅ Align |
| DRY | Don't repeat yourself | ⚠️ Conflict nhe: DRY encourage abstraction, guideline noi "no abstractions for single-use" |
| SOLID | Single responsibility, open/closed... | ⚠️ SOLID encourage interface/abstraction, guideline resist |
| Clean Code (Martin) | Small functions, descriptive names | ⚠️ Clean Code co the dan den over-decomposition |

**Nhan xet:** Guideline nay **CHONG LAI** mot so "best practices" truyen thong (SOLID, Clean Code) — day la **intentional** vi LLMs overuse patterns.

## 7. Tich hop tiem nang

### Voi nguyen tac khac
- **Simplicity + Think:** "Hieu ro scope" → "chi code cho scope do"
- **Simplicity + Surgical:** "Khong add code thua" + "Khong sua code khong lien quan"
- **Simplicity + Goal-Driven:** "Viet minimum code" + "Verify no works" → TDD natural

### Voi tooling
- **Line count alerts:** Warn khi function > X dong
- **Complexity metrics:** Cyclomatic complexity, cognitive complexity
- **Diff review:** Flag khi add nhieu code cho simple request

## 8. Takeaways

1. **#2 principle** (17/20) — giai quyet directly LLM overengineering tendency
2. **7.55/10 weighted score** — good usability, moderate safety concern
3. **Gap lon nhat:** "impossible scenarios" rule can canh bao them; threshold cho abstraction
4. **Strength lon nhat:** Examples cuc ky illustrative (49x bloat factor)
5. **Tension quan trong:** Guideline nay CHONG LAI mot so traditional best practices — intentional cho LLM context
6. **Safety concern:** "No error handling for impossible scenarios" can disclaimer cho security-critical code
