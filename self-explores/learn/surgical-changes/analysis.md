---
date: 2026-03-26
type: context
task: karpathy-skills-cxp
tags: [deep-dive, surgical-changes, anthropic-standards]
---

# Deep Dive: Surgical Changes

## 1. Logic Core

### Tagline
> "Touch only what you must. Clean up only your own mess."

### 2 Contexts, 6 Rules

**Context A: Editing existing code (4 rules)**

| # | Rule | Anti-pattern |
|---|------|-------------|
| 1 | Don't "improve" adjacent code, comments, or formatting | Drive-by refactoring |
| 2 | Don't refactor things that aren't broken | Unnecessary refactoring |
| 3 | Match existing style, even if you'd do it differently | Style drift |
| 4 | If you notice unrelated dead code, mention it — don't delete it | Unauthorized cleanup |

**Context B: When YOUR changes create orphans (2 rules)**

| # | Rule | Rationale |
|---|------|-----------|
| 5 | Remove imports/variables/functions that YOUR changes made unused | Clean up own mess |
| 6 | Don't remove pre-existing dead code unless asked | Respect existing state |

### Self-check test
> "Every changed line should trace directly to the user's request."

### Core Philosophy
- **Do no harm** — thay doi chi nhung gi duoc yeu cau
- **Respect existing code** — no da duoc viet boi nguoi khac voi ly do cua ho
- **Own your mess, not others'** — clean up chi nhung gi BAN tao ra
- **Signal, don't act** — thay van de → noi ra, khong tu y sua

### Nguon goc — Karpathy Quote
> "They still sometimes change/remove comments and code they don't sufficiently understand as side effects, even if orthogonal to the task."

**Phan tich:**
- "side effects" — thay doi khong lien quan la main complaint
- "orthogonal to the task" — khong nam trong scope
- "don't sufficiently understand" — LLM sua code ma khong hieu context

## 2. Example Analysis

### Example 1: Drive-by Refactoring (EXAMPLES.md dong 227-291)

**Request:** "Fix the bug where empty emails crash the validator"

**LLM lam sai (diff 20+ dong thay doi):**
- Added docstring
- Changed `email` extraction + `.strip()`
- Added `.` validation cho email domain
- Added username validation: `len() < 3`, `isalnum()` check
- Changed comments

**Chi can (diff 6 dong):**
```diff
-     if not user_data.get('email'):
+     email = user_data.get('email', '')
+     if not email or not email.strip():
          raise ValueError("Email required")
-     if '@' not in user_data['email']:
+     if '@' not in email:
```

**Insight:**
- LLM sua 20+ dong, chi 6 dong la cần thiết
- Added username validation = scope creep (khong ai hoi)
- Changed comments = noise
- Added docstring = unsolicited improvement
- **Test:** "Does adding username validation trace to 'fix empty email bug'?" → NO

### Example 2: Style Drift (EXAMPLES.md dong 293-366)

**Request:** "Add logging to the upload function"

**LLM lam sai:**
- Changed `''` to `""` (quote style)
- Added type hints (`file_path: str, destination: str -> bool`)
- Added docstring
- Changed boolean return pattern (`success = response.status_code == 200`)
- Reformatted whitespace

**Nen lam:**
- Matched single quotes (existing style)
- No type hints (existing style)
- No docstring (existing style)
- Kept boolean pattern (existing style)
- Only added: `import logging`, `logger = logging.getLogger(__name__)`, 3 log lines

**Insight:**
- Style drift la "invisible damage" — moi thay doi nho nhung tich luy thanh inconsistency
- **Rule of thumb:** Nhin file hien tai dung gi → dung dung cai do
- LLM thuong "normalize" code ve style cua no → phai resist

## 3. Danh gia theo Anthropic Standards

### Completeness: 9/10

**Manh:**
- 2 contexts ro rang: editing existing vs cleaning up own changes
- 6 rules bao quat ca DO va DON'T
- Self-check test cuc ky concrete: "Every changed line traces to request"
- Rule 4 (mention dead code, don't delete) la nuanced va mature

**Thieu:**
- Khong address: khi user noi "fix AND clean up" → boundary o dau?
- Khong noi ve automated formatting tools (prettier, black) — co duoc chay khong?

### Usability: 9/10

**Manh:**
- Rules cuc ky ro rang — khong mo ho
- Self-check test de ap dung: review moi dong diff
- Examples illustrate "dung" va "sai" cuc ky ro
- De verify: diff chi nen co nhung dong lien quan den request

**Thieu:**
- Doi khi kho phan biet "necessary side effect" vs "drive-by" (VD: rename variable de fix bug)

### Modularity: 9/10

**Manh:**
- Hoan toan doc lap — khong depend on bat ky nguyen tac nao khac
- Ap dung cho moi language, moi codebase
- Tuong thich voi moi CI/CD workflow

**Thieu:**
- Nhe: overlap voi Simplicity First (khong add code thua)

### Safety: 8/10

**Manh:**
- Giam risk "accidental deletion" — rule 6 protect existing code
- "Mention don't delete" = safe signaling mechanism
- Match existing style = giam confusion cho team members
- Clean diffs = de review = de catch security issues

**Thieu:**
- Khong address: khi existing code CO security bug → guideline noi "don't change" nhung should fix?
- Tension: "match existing style" vs "existing style is insecure" (VD: SQL concatenation)

### Tong hop diem

| Tieu chi | Diem | Weight | Weighted |
|----------|------|--------|----------|
| Completeness | 9/10 | 25% | 2.25 |
| Usability | 9/10 | 30% | 2.70 |
| Modularity | 9/10 | 20% | 1.80 |
| Safety | 8/10 | 25% | 2.00 |
| **TONG (weighted)** | | | **8.75/10** |

## 4. Uu diem

1. **Diem cao nhat trong 4 nguyen tac** (8.75) — ro rang, de ap dung, ít mo ho
2. **Self-check test tuyet voi:** "Every changed line traces to request" → de audit
3. **Nuanced orphan rules:** Phan biet "YOUR orphans" vs "pre-existing dead code"
4. **"Mention, don't delete":** Safe signaling — inform user without risk
5. **Immediately verifiable:** Nhin diff la biet co comply khong

## 5. Nhuoc diem

1. **Security tension:** "Match existing style" co the mean "copy insecure patterns"
2. **Scope ambiguity:** User noi "fix bug" — rename variable de fix co duoc khong? Comment out old code?
3. **Formatting tools conflict:** Prettier/Black auto-format CA file — vi pham rule 1?
4. **Khong address urgent fixes:** Thay security hole trong adjacent code → chi "mention"?
5. **Conservative bias:** Co the TOO conservative — user phai explicitly ask moi improvement

## 6. Practical Application

### Checklist truoc khi submit diff:

```
[ ] Moi dong thay doi co trace den user request khong?
[ ] Co sua comments/formatting khong lien quan khong?
[ ] Co refactor code khong thuoc scope khong?
[ ] Style co match existing code khong? (quotes, spacing, naming)
[ ] Co import/variable nao BAN lam orphan khong? → Xoa
[ ] Co dead code TU TRUOC khong? → Mention, khong xoa
```

### Decision tree:

```
Co can thay doi dong nay khong?
├── YES → Trace den user request?
│   ├── YES → Lam
│   └── NO → KHONG lam, mention neu quan trong
└── NO → Khong sua
```

## 7. Takeaways

1. **Diem Anthropic Standards cao nhat: 8.75/10** — nguyen tac ro rang nhat, de audit nhat
2. **De hoc nhat** trong 4 nguyen tac — rule-based, khong can judgment phuc tap
3. **"Every changed line traces to request"** la gold standard cho code review
4. **Tension chinh:** "Don't change existing" vs "existing has problems" → can explicit override mechanism
5. **Best for:** PR reviews, legacy codebases, team development
6. **Least valuable for:** Greenfield projects (khong co "existing code" de protect)
