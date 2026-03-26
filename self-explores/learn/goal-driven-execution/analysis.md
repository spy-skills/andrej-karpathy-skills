---
date: 2026-03-26
type: context
task: karpathy-skills-qho
tags: [deep-dive, goal-driven-execution, anthropic-standards]
---

# Deep Dive: Goal-Driven Execution

## 1. Logic Core

### Tagline
> "Define success criteria. Loop until verified."

### 2 Mechanisms

**Mechanism A: Transform imperative → declarative**

| Instead of... | Transform to... |
|--------------|-----------------|
| "Add validation" | "Write tests for invalid inputs, then make them pass" |
| "Fix the bug" | "Write a test that reproduces it, then make it pass" |
| "Refactor X" | "Ensure tests pass before and after" |

**Mechanism B: Multi-step with verification**

```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

### Core Philosophy
- **Declarative > Imperative** — noi WHAT to achieve, khong phai HOW to do it
- **Tests as contracts** — test la bang chung task hoan thanh
- **Verification loop** — lam → kiem tra → sai → sua → kiem tra lai → dung → xong
- **Independence through clarity** — criteria ro → LLM tu loop ma khong can hoi lai

### Nguon goc — Karpathy Quotes
> "LLMs are exceptionally good at looping until they meet specific goals."
> "Don't tell it what to do, give it success criteria and watch it go."

**Phan tich:**
- Day la insight QUAN TRONG NHAT cua Karpathy ve LLM capabilities
- LLMs GIOI viec iterate khi co clear target
- LLMs TE viec define target cho chinh minh
- → Nguoi dung can define criteria, LLM se loop

## 2. Example Analysis

### Example 1: Vague vs Verifiable (EXAMPLES.md dong 372-411)

**Request:** "Fix the authentication system"

**LLM lam sai (vague approach):**
```
1. Reviewing the code
2. Identifying issues
3. Making improvements
4. Testing the changes
```
→ Khong co criteria cu the, khong biet "xong" la gi

**Nen lam (verifiable):**
- First: Xac dinh specific issue → "users stay logged in after password change"
- Plan voi 4 buoc, moi buoc co verify:
  1. Write test (old session invalidated) → Verify: test fails
  2. Implement (invalidate sessions) → Verify: test passes
  3. Edge cases (multiple sessions, concurrent) → Verify: additional tests pass
  4. Regression check → Verify: full suite green

**Insight:**
- "Fix the authentication system" = 0% actionable
- "Write test that old session is invalidated after password change, make it pass" = 100% actionable
- Transformation: Imperative ambiguous → Declarative specific

### Example 2: Multi-Step with Verification (EXAMPLES.md dong 413-452)

**Request:** "Add rate limiting to the API"

**LLM lam sai:** 300-line commit voi Redis, multiple strategies, config, monitoring — tat ca 1 luc

**Nen lam:** 4 incremental steps:
1. Basic in-memory rate limiting (1 endpoint) → Test: 100 requests, first 10 OK, rest 429
2. Extract to middleware (all endpoints) → Test: applies to /users and /posts
3. Add Redis backend → Test: persists across restarts, shared across instances
4. Add configuration → Test: /search 10/min, /users 100/min

**Insight:**
- "Each step is independently verifiable and deployable"
- Moi buoc co **concrete test** (so luong cu the: 10 requests, 429 status)
- Incremental = less risk, easier debug, clearer progress
- **Key pattern:** Start simple → add complexity only when needed (lien ket voi Simplicity First)

### Example 3: Test-First Verification (EXAMPLES.md dong 454-494)

**Request:** "The sorting breaks when there are duplicate scores"

**LLM lam sai:** Fix ngay sort logic ma khong reproduce bug

**Nen lam:**
1. Write test voi duplicate scores → Verify: test fails (reproduces bug)
2. Fix sort: `key=lambda x: (-x['score'], x['name'])` → Verify: test passes consistently

**Insight:**
- Test-first = **bang chung** bug ton tai TRUOC khi fix
- Ngan chan "fix" nhung thuc ra khong fix gi (false confidence)
- 2 buoc don gian nhung methodology SOLID

## 3. Danh gia theo Anthropic Standards

### Completeness: 7/10

**Manh:**
- 3 transform examples bao quat: validation, bug fix, refactoring
- Multi-step format voi verify cho moi buoc
- Test-first approach

**Thieu:**
- Khong address non-testable tasks: design, documentation, research, configuration
- Khong noi ve "soft criteria" (code readability, UX quality) — chi focus "tests pass"
- Khong co huong dan khi KHONG CO test infrastructure (khong co test framework, no CI)
- Khong address performance criteria (VD: "faster" — bao nhieu la du nhanh?)
- Khong noi ve rollback plan khi verification fails repeatedly

### Usability: 6/10

**Manh:**
- Transform table la tool tot: imperative → declarative
- Multi-step format de follow
- Examples cu the va illustrative

**Thieu:**
- **Kho ap dung nhat** trong 4 nguyen tac — can skill decompose task + write AC
- Khong co template/format cho success criteria (Given/When/Then? Checklist? Metric?)
- Junior devs thuong khong biet viet test truoc (TDD khong pho bien)
- "Loop until verified" — loop BAO LAU? Khi nao stop loop va ask for help?
- Khong phan biet task sizes: simple bug fix khong can 4-step plan

### Modularity: 6/10

**Manh:**
- Co the ap dung doc lap
- Khong depend on specific tools

**Thieu:**
- **Phu thuoc ngam vao Think Before Coding** — can "think" de define criteria
- Phu thuoc vao test infrastructure — khong co tests → nguyen tac nay kho ap dung
- Overlap voi Think Before Coding: "define success criteria" ≈ "state assumptions"

### Safety: 8/10

**Manh:**
- Verification loops = catch errors TRUOC khi deploy
- Test-first = bang chung code dung
- Incremental steps = giam blast radius
- "Ensure tests pass before AND after" = regression protection

**Thieu:**
- "Loop until pass" khong co timeout — co the loop vo han
- Khong address: test passes nhung logic sai (wrong test)
- Khong noi ve security testing cu the

### Tong hop diem

| Tieu chi | Diem | Weight | Weighted |
|----------|------|--------|----------|
| Completeness | 7/10 | 25% | 1.75 |
| Usability | 6/10 | 30% | 1.80 |
| Modularity | 6/10 | 20% | 1.20 |
| Safety | 8/10 | 25% | 2.00 |
| **TONG (weighted)** | | | **6.75/10** |

## 4. Uu diem

1. **Unlock autonomous execution:** Khi criteria ro → LLM tu loop → ít can human intervention
2. **Test-first methodology:** Solid engineering practice, khong chi cho LLMs
3. **Incremental delivery:** Moi buoc deployable rieng → less risk
4. **Karpathy's key insight:** "Give it success criteria and watch it go" — leverage LLM's looping strength
5. **Concrete verify steps:** Moi buoc co check cu the (so luong, status code, test pass/fail)

## 5. Nhuoc diem

1. **Kho ap dung nhat:** Can skill decompose tasks, viet AC, design tests
2. **Test-centric bias:** Khong address non-testable work (design, docs, research, infra)
3. **Missing loop bounds:** "Loop until verified" — nhung loop max bao nhieu lan?
4. **Missing escalation:** Khi nao ngung loop va hoi user?
5. **Overkill for simple tasks:** "Fix typo" khong can 4-step verification plan
6. **Assumes test infrastructure:** Nhieu project khong co test framework san

## 6. Advanced Analysis: Karpathy's Core Insight

### "Don't tell it what to do, give it success criteria and watch it go"

Day la insight **transformative** ve cach dung LLMs:

**Truyen thong (imperative):**
```
User: "Fix the sort function"
LLM: [reads code] [changes sort] [hopes it works]
```

**Goal-driven (declarative):**
```
User: "Sort must be stable: same scores → alphabetical order. Test: run 10 times, same output."
LLM: [writes test] [runs test → fails] [fixes sort] [runs test → passes] [runs 10x → consistent] ✓
```

**Tai sao hieu qua:**
1. LLM biet DICH → khong di lac
2. LLM co METRIC → tu do whether → biet khi nao XONG
3. LLM co LOOP condition → tu iterate
4. User khong can babysit

**Tai sao kho:**
1. User phai biet define criteria (khong phai ai cung biet)
2. Criteria phai MEASURABLE (khong phai moi thu deu do duoc)
3. Can infrastructure de verify (test framework, metrics, etc.)

### Relationship voi other principles

```
Think Before Coding  → Define WHAT (requirements)
         ↓
Goal-Driven Execution → Define HOW TO VERIFY (criteria)
         ↓
LLM Loop: implement → test → fail → fix → test → pass ✓
         ↓
Simplicity First     → Keep implementation MINIMAL
Surgical Changes     → Keep changes SCOPED
```

## 7. Tich hop tiem nang

### Voi nguyen tac khac
- **Goal-Driven + Think:** Think dinh nghia scope → Goal-Driven dinh nghia criteria
- **Goal-Driven + Simplicity:** Minimal code du de pass criteria → khong them
- **Goal-Driven + Surgical:** Criteria cu the → chi sua dung cho de pass criteria

### Voi tooling
- **CI/CD integration:** Automated verification after each step
- **Test templates:** Pre-built Given/When/Then for common patterns
- **Loop timeout:** Max N iterations before escalating to user
- **Progress tracking:** Show verification status in real-time

### Voi LLM workflows
- **Agentic loops:** Claude Code + test runner = natural goal-driven execution
- **Cursor/Copilot:** "Run tests after each change" = goal-driven by default
- **System prompts:** "Always write test first, then implement" = inject methodology

## 8. Takeaways

1. **#4 trong ranking (14/20)** nhung co **leverage cao nhat** khi mastered
2. **6.75/10 weighted score** — lowest cua 4 nguyen tac, chu yeu vi KHO AP DUNG
3. **Karpathy's core insight la transformative:** Declarative goals + LLM looping = autonomous coding
4. **Gap lon nhat:** Usability — can template, bounds, escalation rules
5. **Best for:** Complex multi-step tasks, bug fixes, refactoring
6. **Worst for:** Simple changes, non-testable work, no test infrastructure
7. **Mastery path:** Think (foundation) → Surgical (discipline) → Simplicity (taste) → Goal-Driven (autonomy)
