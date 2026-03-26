---
date: 2026-03-25
type: task-worklog
task: karpathy-skills-cxp
title: "Deep Dive: Surgical Changes"
status: open
tags: [deep-dive, surgical-changes, analysis]
---

# Deep Dive: Surgical Changes

## Mo ta task
Nghien cuu sau nguyen tac 3. Phan tich logic core ve minimal edits, orphan cleanup rules, style matching.
Danh gia Anthropic Standards 1-10. Xem 2 examples (Drive-by Refactoring, Style Drift).

## Dependencies
- karpathy-skills-ztk (Xep hang uu tien)

## Ke hoach chi tiet

### Buoc 1: Doc nguon goc (~10 phut)
- SKILL.md section 3, README.md expanded, EXAMPLES.md section 3

### Buoc 2: Phan tich logic core (~20 phut)
- 2 rule sets: editing rules (4 items) + orphan rules (2 items)
- "Every changed line traces to user request" — litmus test nay
- Phan biet: YOUR orphans vs pre-existing dead code

### Buoc 3: Danh gia + Viet analysis.md (~30 phut)

### Output mong doi
- [ ] File self-explores/learn/surgical-changes/analysis.md

## Worklog
*(Chua bat dau)*
